---
title: Pag-develop
---

# Guide sa pag-develop ng plugin <!-- markdownlint-disable-line MD025 -->

Tutulungan ka ng guide na ito sa paggawa ng mga plugins para sa Hachimi Edge. Ang mga plugin ay isang dynamic library na nagpapalawak ng functionality ng Hachimi sa pamamagitan ng well-defined na C-compaible API

## Choice sa programming language

Maaaring gawin ang mga plugin gamit ang **anumang language** na makaka-produce ng C-compatible dynamic library (`.so` sa Android, `.dll` sa Windows). Kasama dito ang:

- **Rust** (inirerekomenda - ginagamit dito ang Rust sa mga example)
- **C/C++**
- **Zig**
- **Go** (gamit ang cgo)
- **Anumang language na may support sa C FFI**
- **Assembly** (Kung isa ka talagang masokista)

Ginagamit ng guide na ito ang Rust dahil ginawa mismo ang Hachimi Edge gamit ang Rust. Gayunpaman, ang API ay C compatible, kaya maaari mong gamitin ang anumang programming language na gusto mo. Siguraduhin lamang na ang iyong `hachimi_init` o `hachimi_init_v3` na function ay nai-export gamit ang C calling convention.

## Mga kinakailangan

Bago ka makapagsimula, dapat may:

- May experience ka sa pinili mong programming language.
- May kaalaman ka sa istraktura ng laro.
- Mahy naka-install na development toolchain para sa target platform.

::: warning Babala
Huwag lumikha ng mga malisyosong plugin na nagnanakaw ng data o nakakasama sa ibang players.
:::

## Istraktura ng plugin

### Entry point

Dapat mag-export ng `hachimi_init` o `hachimi_init_v3` na function ang bawat plugin. Gumawa ng `Cargo.toml`:

```toml
[package]
name = "hachimi_myplugin"
version = "0.1.0"
edition = "2021"

[lib]
crate-type = ["cdylib"]

[dependencies]
```

At sa `src/lib.rs`:

```rust
use std::ffi::{c_char, c_void};

#[repr(C)]
pub struct Vtable {
    // Function pointers (tignan ang API Reference sa ibaba)
}

#[repr(i32)]
pub enum InitResult {
    Error = 0,
    Ok = 1,
}

static mut VTABLE: Option<&'static Vtable> = None;

#[no_mangle]
pub extern "C" fn hachimi_init(vtable: *const Vtable, version: i32) -> InitResult {
    if vtable.is_null() {
        return InitResult::Error;
    }
    if version < 2 {
        return InitResult::Error;
    }

    unsafe {
        VTABLE = Some(&*vtable);
    }

    // I-initialize ang iyong plugin dito

    InitResult::Ok
}
```

**Kasalukuyang API Version: 3** <!-- markdownlint-disable-line MD036 -->

### Pag-initialize (api v3+)

Dagdag pa sa `hachimi_init` na nakabase sa vtables, nagdadagdag ng bagong
paraan ng pag-initialize ang API version 3: ang `hachimi_init_v3` na export.
Sa mode na ito, hindi ibinibigay ng host ang isang `Vtable` pointer. Sa halip
nagbibigay ito ng `hachimi_get_api` na function na nagre-resolve ng kani-kanilang
API entrypoints sa kanilang pangalan.

```rust
use std::ffi::{c_char, c_void, CString};

pub type HachimiGetApiFn = extern "C" fn(name: *const c_char) -> *mut c_void;

#[no_mangle]
pub extern "C" fn hachimi_init_v3(get_api: HachimiGetApiFn, version: i32) -> InitResult {
    if version < 3 {
        return InitResult::Error;
    }

    unsafe {
        // Resolve only the symbols you actually need. Unknown names return null.
        let log_name = CString::new("log").unwrap();
        let log_ptr = get_api(log_name.as_ptr());
        if log_ptr.is_null() {
            return InitResult::Error;
        }
        // Store log_ptr somewhere accessible, typically a static.
    }

    InitResult::Ok
}
```

Ang tinatanggap na symbol names ay tumutugma sa field names ng Vtable
struct na ginagamit sa v2 init path (hal. `hachimi_instance`,
`interceptor_hook`, `gui_ui_combo_menu`, `hachimi_get_data_path`).
Kapag maghiling ka ng di-kilalang pangalan, magbibigay ito ng null
pointer, na nagbibigay-daan sa iyo na suriin para sa mga opsyonal
na feature nang hindi itigil ang pag-initialize.

::: tip
Gamitin ang v2 na `hachimi_init` kung gusto mong gumamit ng single-typed
na struct. Gamitin ang `hachimi_init_v3` kung gusto mong dinamikong
gumamit ng mga bagong symbol at manatiling forward-compatible sa
hinaharap na karagdagan sa API nang hindi pag-redeclare sa buong `Vtable`.
:::


### Ang vtable

Ang vtable ay isang structure na naglalaman ng function pointers sa APi ng Hachimi. Nakukuha mo ito sa `hachimi_init` at iimbak ito para sa paggamit sa plugin.

### Pag-initialize (api v2)

```rust
#[no_mangle]
pub extern "C" fn hachimi_init(vtable: *const Vtable, version: i32) -> InitResult {
    if vtable.is_null() {
        return InitResult::Error;
    }
    if version < 2 {
        // API version too old
        return InitResult::Error;
    }

    // Store vtable safely
    unsafe {
        VTABLE = Some(&*vtable);
    }

    InitResult::Ok
}
```

## API reference

### Hachimi instance

```rust
use std::ffi::c_void;

unsafe fn get_hachimi_and_interceptor() -> (*const c_void, *const c_void) {
    let vtable = VTABLE.unwrap();
    let hachimi = (vtable.hachimi_instance)();
    let interceptor = (vtable.hachimi_get_interceptor)(hachimi);
    (hachimi, interceptor)
}
```

### Interceptor (pag-hook ng function)

Nagbibigay-daan sa iyo ang interceptor na mag-hook at i-modify ang mga function sa laro:

```rust
use std::ffi::c_void;

unsafe fn hook_function(
    interceptor: *const c_void,
    original_addr: *mut c_void,
    hook_addr: *mut c_void
) -> *mut c_void {
    let vtable = VTABLE.unwrap();
    (vtable.interceptor_hook)(interceptor, original_addr, hook_addr)
}

unsafe fn hook_vtable_entry(
    interceptor: *const c_void,
    vtable_ptr: *mut *mut c_void,
    index: usize,
    hook_addr: *mut c_void
) -> *mut c_void {
    let vtable = VTABLE.unwrap();
    (vtable.interceptor_hook_vtable)(interceptor, vtable_ptr, index, hook_addr)
}

unsafe fn get_trampoline(
    interceptor: *const c_void,
    hook_addr: *mut c_void
) -> *mut c_void {
    let vtable = VTABLE.unwrap();
    (vtable.interceptor_get_trampoline_addr)(interceptor, hook_addr)
}

unsafe fn unhook_function(
    interceptor: *const c_void,
    hook_addr: *mut c_void
) -> *mut c_void {
    let vtable = VTABLE.unwrap();
    (vtable.interceptor_unhook)(interceptor, hook_addr)
}
```

### IL2CPP functions

I-access ang IL2CPP runtime ng Unity:

```rust
use std::ffi::{c_char, c_void, CStr, CString};

unsafe fn resolve_il2cpp_symbol(name: &str) -> *mut c_void {
    let vtable = VTABLE.unwrap();
    let name_cstr = CString::new(name).unwrap();
    (vtable.il2cpp_resolve_symbol)(name_cstr.as_ptr())
}

unsafe fn resolve_il2cpp_icall(name: &str) -> *mut c_void {
    let vtable = VTABLE.unwrap();
    let name_cstr = CString::new(name).unwrap();
    (vtable.il2cpp_resolve_icall)(name_cstr.as_ptr())
}

unsafe fn get_assembly_image(name: &str) -> *const c_void {
    let vtable = VTABLE.unwrap();
    let name_cstr = CString::new(name).unwrap();
    (vtable.il2cpp_get_assembly_image)(name_cstr.as_ptr())
}

unsafe fn get_class(
    image: *const c_void,
    namespace: &str,
    class_name: &str
) -> *mut c_void {
    let vtable = VTABLE.unwrap();
    let ns_cstr = CString::new(namespace).unwrap();
    let class_cstr = CString::new(class_name).unwrap();
    (vtable.il2cpp_get_class)(image, ns_cstr.as_ptr(), class_cstr.as_ptr())
}

unsafe fn get_method(
    klass: *mut c_void,
    method_name: &str,
    arg_count: i32
) -> *const c_void {
    let vtable = VTABLE.unwrap();
    let method_cstr = CString::new(method_name).unwrap();
    (vtable.il2cpp_get_method)(klass, method_cstr.as_ptr(), arg_count)
}

unsafe fn get_methods(klass: *mut c_void) -> impl Iterator<Item = *const c_void> {
    let mut iter: *mut c_void = std::ptr::null_mut();

    std::iter::from_fn(move || {
        let vtable = VTABLE.unwrap();
        let method = (vtable.il2cpp_class_get_methods)(klass, &mut iter);

        if method.is_null() {
            None
        } else {
            Some(method)
        }
    })
}

unsafe fn get_method_addr(
    klass: *mut c_void,
    method_name: &str,
    arg_count: i32
) -> *mut c_void {
    let vtable = VTABLE.unwrap();
    let method_cstr = CString::new(method_name).unwrap();
    (vtable.il2cpp_get_method_addr)(klass, method_cstr.as_ptr(), arg_count)
}

unsafe fn object_new(klass: *const c_void) -> *mut c_void {
    let vtable = VTABLE.unwrap();
    (vtable.il2cpp_object_new)(klass)
}

unsafe fn get_field(klass: *mut c_void, field_name: &str) -> *mut c_void {
    let vtable = VTABLE.unwrap();
    let field_cstr = CString::new(field_name).unwrap();
    (vtable.il2cpp_get_field_from_name)(klass, field_cstr.as_ptr())
}

unsafe fn get_field_value<T>(
    object: *mut c_void,
    field: *mut c_void,
    out_value: *mut T
) {
    let vtable = VTABLE.unwrap();
    (vtable.il2cpp_get_field_value)(object, field, out_value as *mut c_void);
}

unsafe fn set_field_value<T>(
    object: *mut c_void,
    field: *mut c_void,
    value: *const T
) {
    let vtable = VTABLE.unwrap();
    (vtable.il2cpp_set_field_value)(object, field, value as *const c_void);
}

unsafe fn unbox(object: *mut c_void) -> *mut c_void {
    let vtable = VTABLE.unwrap();
    (vtable.il2cpp_unbox)(object)
}

unsafe fn get_main_thread() -> *mut c_void {
    let vtable = VTABLE.unwrap();
    (vtable.il2cpp_get_main_thread)()
}

unsafe fn create_array(element_class: *mut c_void, length: usize) -> *mut c_void {
    let vtable = VTABLE.unwrap();
    (vtable.il2cpp_create_array)(element_class, length)
}

unsafe fn get_singleton_instance(klass: *mut c_void) -> *mut c_void {
    let vtable = VTABLE.unwrap();
    (vtable.il2cpp_get_singleton_like_instance)(klass)
}

unsafe fn get_method_overload(
    klass: *mut c_void,
    method_name: &str,
    param_types: &[i32] // Il2CppTypeEnum values
) -> *const c_void {
    let vtable = VTABLE.unwrap();
    let method_cstr = CString::new(method_name).unwrap();
    (vtable.il2cpp_get_method_overload)(
        klass,
        method_cstr.as_ptr(),
        param_types.as_ptr() as *const c_void,
        param_types.len()
    )
}

unsafe fn get_method_overload_addr(
    klass: *mut c_void,
    method_name: &str,
    param_types: &[i32]
) -> *mut c_void {
    let vtable = VTABLE.unwrap();
    let method_cstr = CString::new(method_name).unwrap();
    (vtable.il2cpp_get_method_overload_addr)(
        klass,
        method_cstr.as_ptr(),
        param_types.as_ptr() as *const c_void,
        param_types.len()
    )
}

unsafe fn get_method_cached(
    klass: *mut c_void,
    method_name: &str,
    arg_count: i32
) -> *const c_void {
    let vtable = VTABLE.unwrap();
    let method_cstr = CString::new(method_name).unwrap();
    (vtable.il2cpp_get_method_cached)(klass, method_cstr.as_ptr(), arg_count)
}

unsafe fn get_method_addr_cached(
    klass: *mut c_void,
    method_name: &str,
    arg_count: i32
) -> *mut c_void {
    let vtable = VTABLE.unwrap();
    let method_cstr = CString::new(method_name).unwrap();
    (vtable.il2cpp_get_method_addr_cached)(klass, method_cstr.as_ptr(), arg_count)
}

unsafe fn find_nested_class(parent: *mut c_void, name: &str) -> *mut c_void {
    let vtable = VTABLE.unwrap();
    let name_cstr = CString::new(name).unwrap();
    (vtable.il2cpp_find_nested_class)(parent, name_cstr.as_ptr())
}

unsafe fn get_static_field_value<T>(field: *mut c_void, out_value: *mut T) {
    let vtable = VTABLE.unwrap();
    (vtable.il2cpp_get_static_field_value)(field, out_value as *mut c_void);
}

unsafe fn set_static_field_value<T>(field: *mut c_void, value: *const T) {
    let vtable = VTABLE.unwrap();
    (vtable.il2cpp_set_static_field_value)(field, value as *const c_void);
}

unsafe fn runtime_object_init(object: *mut c_void) {
    let vtable = VTABLE.unwrap();
    (vtable.il2cpp_runtime_object_init)(object);
}

unsafe fn string_new(text: &str) -> *mut c_void {
    let vtable = VTABLE.unwrap();
    let text_cstr = CString::new(text).unwrap();
    (vtable.il2cpp_string_new)(text_cstr.as_ptr())
}

unsafe fn string_chars(s: *mut c_void) -> *mut u16 {
    let vtable = VTABLE.unwrap();
    (vtable.il2cpp_string_chars)(s)
}

unsafe fn string_length(s: *mut c_void) -> i32 {
    let vtable = VTABLE.unwrap();
    (vtable.il2cpp_string_length)(s)
}

unsafe fn get_attached_threads(out_size: &mut usize) -> *mut *mut c_void {
    let vtable = VTABLE.unwrap();
    (vtable.il2cpp_get_attached_threads)(out_size as *mut usize)
}

unsafe fn schedule_on_thread(thread: *mut c_void, callback: unsafe extern "C" fn()) {
    let vtable = VTABLE.unwrap();
    (vtable.il2cpp_schedule_on_thread)(thread, callback);
}

```

### Logging

```rust
use std::ffi::CString;

// Mga log level: 1=Error, 2=Warn, 3=Info, 4=Debug, 5=Trace
unsafe fn log(level: i32, tag: &str, message: &str) {
    let vtable = VTABLE.unwrap();
    let tag_cstr = CString::new(tag).unwrap();
    let msg_cstr = CString::new(message).unwrap();
    (vtable.log)(level, tag_cstr.as_ptr(), msg_cstr.as_ptr());
}

// Helper functions
unsafe fn log_info(tag: &str, message: &str) {
    log(3, tag, message);
}

unsafe fn log_error(tag: &str, message: &str) {
    log(1, tag, message);
}

unsafe fn log_warn(tag: &str, message: &str) {
    log(2, tag, message);
}
```

### Gui integration

#### Menu items

```rust
use std::ffi::{c_char, c_void, CString};

// Mag-load ng PNG icon
const ICON_BYTES: &[u8] = include_bytes!("../icon.png");

// Callback type
type MenuCallback = extern "C" fn(userdata: *mut c_void);

extern "C" fn my_menu_callback(_userdata: *mut c_void) {
    unsafe {
        log_info("MyPlugin", "Menu item clicked!");
    }
}

unsafe fn register_menu_item(name: &str, callback: MenuCallback) -> bool {
    let vtable = VTABLE.unwrap();
    let name_cstr = CString::new(name).unwrap();
    (vtable.gui_register_menu_item)(
        name_cstr.as_ptr(),
        Some(callback),
        std::ptr::null_mut()
    )
}

unsafe fn register_menu_item_with_icon(
    name: &str,
    icon_data: &[u8],
    callback: MenuCallback
) -> bool {
    let vtable = VTABLE.unwrap();
    let name_cstr = CString::new(name).unwrap();
    if !(vtable.gui_register_menu_item)(
        name_cstr.as_ptr(),
        Some(callback),
        std::ptr::null_mut()
    ) {
        return false;
    }
    (vtable.gui_register_menu_item_icon)(
        name_cstr.as_ptr(),
        std::ptr::null(),
        icon_data.as_ptr(),
        icon_data.len()
    )
}

// Usage:
// register_menu_item_with_icon("My Plugin", ICON_BYTES, my_menu_callback);
```

#### Menu sections

```rust
type MenuSectionCallback = extern "C" fn(ui: *mut c_void, userdata: *mut c_void);

extern "C" fn my_section_callback(ui: *mut c_void, _userdata: *mut c_void) {
    unsafe {
        let vtable = VTABLE.unwrap();

        // Build your UI
        let heading = CString::new("My Plugin Settings").unwrap();
        (vtable.gui_ui_heading)(ui, heading.as_ptr());

        (vtable.gui_ui_separator)(ui);

        // Add widgets...
    }
}

unsafe fn register_menu_section(callback: MenuSectionCallback) -> bool {
    let vtable = VTABLE.unwrap();
    (vtable.gui_register_menu_section)(Some(callback), std::ptr::null_mut())
}

unsafe fn register_menu_section_with_icon(
    title: &str,
    icon_data: &[u8],
    callback: MenuSectionCallback
) -> bool {
    let vtable = VTABLE.unwrap();
    let title_cstr = CString::new(title).unwrap();
    (vtable.gui_register_menu_section_with_icon)(
        title_cstr.as_ptr(),
        std::ptr::null(),
        icon_data.as_ptr(),
        icon_data.len(),
        Some(callback),
        std::ptr::null_mut()
    )
}

// Usage:
// register_menu_section_with_icon("My Settings", ICON_BYTES, my_section_callback);
```

#### Mga widget sa UI

Mga available na function para sa pag-build ng iyong GUI:

```rust
use std::ffi::CString;

unsafe fn ui_heading(ui: *mut c_void, text: &str) {
    let vtable = VTABLE.unwrap();
    let text_cstr = CString::new(text).unwrap();
    (vtable.gui_ui_heading)(ui, text_cstr.as_ptr());
}

unsafe fn ui_label(ui: *mut c_void, text: &str) {
    let vtable = VTABLE.unwrap();
    let text_cstr = CString::new(text).unwrap();
    (vtable.gui_ui_label)(ui, text_cstr.as_ptr());
}

unsafe fn ui_small(ui: *mut c_void, text: &str) {
    let vtable = VTABLE.unwrap();
    let text_cstr = CString::new(text).unwrap();
    (vtable.gui_ui_small)(ui, text_cstr.as_ptr());
}

unsafe fn ui_colored_label(ui: *mut c_void, r: u8, g: u8, b: u8, a: u8, text: &str) {
    let vtable = VTABLE.unwrap();
    let text_cstr = CString::new(text).unwrap();
    (vtable.gui_ui_colored_label)(ui, r, g, b, a, text_cstr.as_ptr());
}

unsafe fn ui_separator(ui: *mut c_void) {
    let vtable = VTABLE.unwrap();
    (vtable.gui_ui_separator)(ui);
}

unsafe fn ui_button(ui: *mut c_void, text: &str) -> bool {
    let vtable = VTABLE.unwrap();
    let text_cstr = CString::new(text).unwrap();
    (vtable.gui_ui_button)(ui, text_cstr.as_ptr())
}

unsafe fn ui_small_button(ui: *mut c_void, text: &str) -> bool {
    let vtable = VTABLE.unwrap();
    let text_cstr = CString::new(text).unwrap();
    (vtable.gui_ui_small_button)(ui, text_cstr.as_ptr())
}

unsafe fn ui_checkbox(ui: *mut c_void, label: &str, value: &mut bool) -> bool {
    let vtable = VTABLE.unwrap();
    let label_cstr = CString::new(label).unwrap();
    (vtable.gui_ui_checkbox)(ui, label_cstr.as_ptr(), value as *mut bool)
}

unsafe fn ui_text_edit(ui: *mut c_void, buffer: &mut [u8]) -> bool {
    let vtable = VTABLE.unwrap();
    (vtable.gui_ui_text_edit_singleline)(
        ui,
        buffer.as_mut_ptr() as *mut c_char,
        buffer.len()
    )
}

unsafe fn ui_horizontal(
    ui: *mut c_void,
    callback: Option<extern "C" fn(*mut c_void, *mut c_void)>,
    userdata: *mut c_void
) -> bool {
    let vtable = VTABLE.unwrap();
    (vtable.gui_ui_horizontal)(ui, callback, userdata)
}

unsafe fn ui_grid(
    ui: *mut c_void,
    id: &str,
    columns: usize,
    spacing_x: f32,
    spacing_y: f32,
    callback: Option<extern "C" fn(*mut c_void, *mut c_void)>,
    userdata: *mut c_void
) -> bool {
    let vtable = VTABLE.unwrap();
    let id_cstr = CString::new(id).unwrap();
    (vtable.gui_ui_grid)(
        ui,
        id_cstr.as_ptr(),
        columns,
        spacing_x,
        spacing_y,
        callback,
        userdata
    )
}

unsafe fn ui_end_row(ui: *mut c_void) -> bool {
    let vtable = VTABLE.unwrap();
    (vtable.gui_ui_end_row)(ui)
}

unsafe fn ui_combo_menu(
    ui: *mut c_void,
    id: &str,
    selected_index: &mut i32,
    items: &[&str],
    search_term: Option<&mut [u8]>
) -> bool {
    let vtable = VTABLE.unwrap();
    let id_cstr = CString::new(id).unwrap();

    // Convert &[&str] into a *const *const c_char array.
    let cstrs: Vec<CString> = items.iter().map(|s| CString::new(*s).unwrap()).collect();
    let ptrs: Vec<*const c_char> = cstrs.iter().map(|c| c.as_ptr()).collect();

    let (search_ptr, search_len) = match search_term {
        Some(buf) => (buf.as_mut_ptr() as *mut c_char, buf.len()),
        None => (std::ptr::null_mut(), 0),
    };

    (vtable.gui_ui_combo_menu)(
        ui,
        id_cstr.as_ptr(),
        selected_index as *mut i32,
        ptrs.as_ptr(),
        ptrs.len(),
        search_ptr,
        search_len
    )
}

```

#### Mga abiso

```rust
unsafe fn show_notification(message: &str) -> bool {
    let vtable = VTABLE.unwrap();
    let msg_cstr = CString::new(message).unwrap();
    (vtable.gui_show_notification)(msg_cstr.as_ptr())
}
```

#### Menu dimensions

Maaaring basihin at i-override ng mga plugin ang width ng Hachimi menu.
Paki-pakinabang ito kapag nagre-render ng malaking widget na pipilitin
ang menu na mag-expand lagpas sa inaasahang width. Halimbawa, kapag hindi
naka-wrap ang nilalaman o hindi binigyan ng explicit na width.

```rust
unsafe fn get_menu_width() -> f32 {
    let vtable = VTABLE.unwrap();
    (vtable.gui_get_menu_width)()
}

unsafe fn set_menu_width(width: f32) {
    let vtable = VTABLE.unwrap();
    (vtable.gui_set_menu_width)(width);
}
```

::: warning Babala
Globally na binabago ng `gui_set_menu_width` ang menu width, kasama dito
ang lahat ng section ng plugin, hindi lamang sa iyo. Hindi nag-e-expose
ng wrapping layout helper ang plugin API, kaya kapag masyadong wide ang
iyong nilalaman, mas-mainam na i-restructure ito gamit ang `gui_ui_grid`
at `gui_ui_end_row` sa halip ng pag-expand ng menu.
:::

#### Windows (api v3+)

Maaaring gumawa at ipamahala ng mga plugin ang sarili nilang GUI window.
Ang bawat window ay may scrollable na contents area at isang opsyonal na
fixed bottom area.

```rust
use std::sync::atomic::AtomicI32;

static MY_WINDOW_ID: std::sync::Mutex<i32> = std::sync::Mutex::new(-1);
static MY_COUNTER: AtomicI32 = AtomicI32::new(0);

type WindowCallback = extern "C" fn(ui: *mut c_void, userdata: *mut c_void);

// Get a unique window ID, call this once per window you want to create
unsafe fn new_window_id() -> i32 {
    let vtable = VTABLE.unwrap();
    (vtable.gui_new_window_id)()
}

// Show a window with the given ID, title, and callbacks
unsafe fn show_window(
    id: i32,
    title: &str,
    contents_callback: Option<WindowCallback>,
    bottom_callback: Option<WindowCallback>,
    userdata: *mut c_void
) -> bool {
    let vtable = VTABLE.unwrap();
    let title_cstr = CString::new(title).unwrap();
    (vtable.gui_show_window)(
        id,
        title_cstr.as_ptr(),
        contents_callback,
        bottom_callback,
        userdata
    )
}

// Close a window by its ID
unsafe fn close_window(id: i32) {
    let vtable = VTABLE.unwrap();
    (vtable.gui_close_window)(id)
}

// Example: open a window from a menu item callback
extern "C" fn on_open_window_click(_userdata: *mut c_void) {
    unsafe {
        let mut id_guard = MY_WINDOW_ID.lock().unwrap();

        // If window is already open, close it first
        if *id_guard >= 0 {
            close_window(*id_guard);
        }

        let id = new_window_id();
        *id_guard = id;

        show_window(
            id,
            "My Plugin Window",
            Some(on_window_contents),
            Some(on_window_bottom),
            &MY_COUNTER as *const _ as *mut c_void,
        );
    }
}

// Contents callback, rendered inside a scrollable area
extern "C" fn on_window_contents(ui: *mut c_void, userdata: *mut c_void) {
    unsafe {
        let vtable = VTABLE.unwrap();

        let heading = CString::new("My Window").unwrap();
        (vtable.gui_ui_heading)(ui, heading.as_ptr());
        (vtable.gui_ui_separator)(ui);

        // Read counter from userdata
        let counter = {
            let counter_ptr = userdata as *const AtomicI32;
            (*counter_ptr).load(std::sync::atomic::Ordering::Relaxed)
        };
        let counter_text = CString::new(format!("Counter: {}", counter)).unwrap();
        (vtable.gui_ui_label)(ui, counter_text.as_ptr());
    }
}

// Bottom callback, rendered in a fixed area below the scrollable contents
extern "C" fn on_window_bottom(ui: *mut c_void, userdata: *mut c_void) {
    unsafe {
        let vtable = VTABLE.unwrap();
        (vtable.gui_ui_separator)(ui);

        let btn_text = CString::new("Increment").unwrap();
        if (vtable.gui_ui_button)(ui, btn_text.as_ptr()) {
            let counter_ptr = userdata as *const AtomicI32;
            (*counter_ptr).fetch_add(1, std::sync::atomic::Ordering::Relaxed);
        }

        let close_text = CString::new("Close").unwrap();
        if (vtable.gui_ui_button)(ui, close_text.as_ptr()) {
            let id_guard = MY_WINDOW_ID.lock().unwrap();
            close_window(*id_guard);
        }
    }
}
```

::: warning Babala
Dapat unique ang window ID sa bawat plugin. Palaging gamitin ang
`gui_new_window_id` para makakuha ng non-colliding ID sa halip
ng pag-hardcode ng sarili mo.
:::

### Lifecycle callbacks (api v3+)

Maaaring magrehistro ng callbacks ang mga plugin na ini-invoke sa
mga punto ng lifecycle ng host. Nagtatanggap ng isang `userdata` pointer
ang parehong callback, na fino-forward kapag i-invoke ito ng host.

#### Game initialized

Ini-invoke kapag nag-initialize na ang il2cpp at managed assemblies ng laro.
Ito ang pinakaligtas na lugar para mag-resolve ng mga class, mag-hook ng
methods, o mag-interact sa managed state.

```rust
type GameInitializedCallback = unsafe extern "C" fn(userdata: *mut c_void);

unsafe extern "C" fn on_game_initialized(_userdata: *mut c_void) {
    // Safe to call il2cpp_get_class / hook game functions here.
    log_info("MyPlugin", "Game initialized");
}

unsafe fn register_on_game_initialized(callback: GameInitializedCallback) -> bool {
    let vtable = VTABLE.unwrap();
    (vtable.hachimi_register_on_game_initialized)(Some(callback), std::ptr::null_mut())
}
```

#### Present callback (Windows only)

Ini-invoke sa bawat frame, bago naka-present ang swap chain. Gamitin ito
para mag-render ng mga overlay gamit ng sarili mong graphics backend,
mag-drive ng animations, o mag-sample sa bawat frame. Sa mga host na
hindi Windows, ito ay isang no-op na function at magbibigay lang ng
`false`.

```rust
type PresentCallback =
    unsafe extern "C" fn(swapchain: *mut c_void, userdata: *mut c_void);

unsafe extern "C" fn on_present(_swapchain: *mut c_void, _userdata: *mut c_void) {
    // Called every frame on Windows. Keep this fast.
}

unsafe fn register_present_callback(callback: PresentCallback) -> bool {
    let vtable = VTABLE.unwrap();
    (vtable.hachimi_register_present_callback)(Some(callback), std::ptr::null_mut())
}
```

::: warning Babala
Available lang ang `hachimi_register_present_callback` sa Windows builds.
Sa Android, palagi ito magbibigay ng `false`. Harangin ang anumang
platform-specific na code sa likod ng `#[cfg(target_os = "windows")]
na block para iwasan ang dead code sa ibang targets.
:::

### Mga path at directory (api v3+)

Nagbibigay ng dalawang path helper ang Hachimi para mahanap ng mga
laro ang data ng laro at data directory ng Hachimi nang hindi
mag-hardcode ng platform-specific na path.

```rust
use std::ffi::CStr;

unsafe fn get_base_dir() -> &'static str {
    let vtable = VTABLE.unwrap();
    let ptr = (vtable.hachimi_get_base_dir)();
    if ptr.is_null() {
        return "";
    }
    CStr::from_ptr(ptr).to_str().unwrap_or("")
}

unsafe fn get_data_path() -> &'static str {
    let vtable = VTABLE.unwrap();
    let ptr = (vtable.hachimi_get_data_path)();
    if ptr.is_null() {
        return "";
    }
    CStr::from_ptr(ptr).to_str().unwrap_or("")
}
```

- Binibigay ng `hachimi_get_base_dir` ang directory kung saan nakalagay ang
  data ng laro.
- Binibigay ng `hachimi_get_data_path` ang directory na ginagamit ng Hachimi
  para sa sarili nitong data (config, mods, plugins, atbp.). Gamitin ito
  para mahanap ang shared assets o panatilihin ang plugin state sa tabi ng
  data ng Hachimi.

Minamay-ari ng host ang parehong pointer at nananatiling valid sa lifetime
ng process, kaya ligtas itong i-cache sa isang `static` pagkatapos ng unang
call.

### Pag-load ng DEX sa Android (api v2+)

Mag-load at mag-execute ng Java/Kotlin code sa Android:

```rust
unsafe fn load_dex(dex_data: &[u8], class_name: &str) -> u64 {
    let vtable = VTABLE.unwrap();
    let class_cstr = CString::new(class_name).unwrap();
    (vtable.android_dex_load)(
        dex_data.as_ptr(),
        dex_data.len(),
        class_cstr.as_ptr()
    )
}

unsafe fn dex_call_static_noargs(handle: u64, method: &str, signature: &str) -> bool {
    let vtable = VTABLE.unwrap();
    let method_cstr = CString::new(method).unwrap();
    let sig_cstr = CString::new(signature).unwrap();
    (vtable.android_dex_call_static_noargs)(
        handle,
        method_cstr.as_ptr(),
        sig_cstr.as_ptr()
    )
}

unsafe fn dex_call_static_string(
    handle: u64,
    method: &str,
    signature: &str,
    arg: &str
) -> bool {
    let vtable = VTABLE.unwrap();
    let method_cstr = CString::new(method).unwrap();
    let sig_cstr = CString::new(signature).unwrap();
    let arg_cstr = CString::new(arg).unwrap();
    (vtable.android_dex_call_static_string)(
        handle,
        method_cstr.as_ptr(),
        sig_cstr.as_ptr(),
        arg_cstr.as_ptr()
    )
}

unsafe fn unload_dex(handle: u64) -> bool {
    let vtable = VTABLE.unwrap();
    (vtable.android_dex_unload)(handle)
}
```

#### Java example (DEX side)

Dapat mag-expose ng **static** methods ang iyong Java/Kotlin code na tumtugma sa signatures na kino-call mo mula sa Rust. Ito ay isang minimal na Java example na maaari mong i-compile sa isang DEX at i-load nang may mga helper sa itaas:

```java
package dev.hachimi;

import android.util.Log;

public class DexExample {
        private static final String TAG = "HachimiDexExample";

        public static void hello() {
                Log.i(TAG, "Hello from DEX");
        }

        public static void setVisibleString(String value) {
                Log.i(TAG, "setVisibleString: " + value);
        }
}
```

Kapag ilo-load, gamitin ang fully-qualified class name:

- `class_name`: `"dev.hachimi.DexExample"`
- `hello()` signature: `"()V"`
- `setVisibleString(String)` signature: `"(Ljava/lang/String;)V"`

#### I-build ang DEX

```bash
ANDROID_JAR=~/Android/Sdk/platforms/android-34/android.jar

rm -rf /tmp/hachimi_dex
mkdir -p /tmp/hachimi_dex/classes

javac -source 1.8 -target 1.8 \
    -classpath "$ANDROID_JAR" \
    -d /tmp/hachimi_dex/classes \
    java/DexExample.java

jar cf /tmp/hachimi_dex/classes.jar -C /tmp/hachimi_dex/classes .

d8 --lib "$ANDROID_JAR" \
    --output /tmp/hachimi_dex/out \
    /tmp/hachimi_dex/classes.jar

cp /tmp/hachimi_dex/out/classes.dex assets/dex_example.dex
```

At i-load ito mula sa Rust:

```rust
let dex_bytes = include_bytes!("../assets/dex_example.dex");
let handle = load_dex(dex_bytes, "dev.hachimi.DexExample");
dex_call_static_noargs(handle, "hello", "()V");
dex_call_static_string(handle, "setVisibleString", "(Ljava/lang/String;)V", "true");
```

## Kumpletong example plugin (v2)

Ito ang gumagana at kumpletong halimbawa:

```rust
use std::ffi::{c_char, c_void, CString};
use std::sync::Once;

#[repr(i32)]
#[derive(Debug, Copy, Clone, Eq, PartialEq)]
pub enum InitResult {
    Error = 0,
    Ok = 1,
}

#[repr(C)]
#[derive(Clone, Copy)]
pub struct Vtable {
    pub hachimi_instance: unsafe extern "C" fn() -> *const c_void,
    pub hachimi_get_interceptor: unsafe extern "C" fn(this: *const c_void) -> *const c_void,

    pub interceptor_hook: unsafe extern "C" fn(
        this: *const c_void,
        orig_addr: *mut c_void,
        hook_addr: *mut c_void,
    ) -> *mut c_void,
    pub interceptor_hook_vtable: unsafe extern "C" fn(
        this: *const c_void,
        vtable: *mut *mut c_void,
        vtable_index: usize,
        hook_addr: *mut c_void,
    ) -> *mut c_void,
    pub interceptor_get_trampoline_addr:
        unsafe extern "C" fn(this: *const c_void, hook_addr: *mut c_void) -> *mut c_void,
    pub interceptor_unhook:
        unsafe extern "C" fn(this: *const c_void, hook_addr: *mut c_void) -> *mut c_void,

    pub il2cpp_resolve_symbol: unsafe extern "C" fn(name: *const c_char) -> *mut c_void,
    pub il2cpp_get_assembly_image:
        unsafe extern "C" fn(assembly_name: *const c_char) -> *const c_void,
    pub il2cpp_get_class: unsafe extern "C" fn(
        image: *const c_void,
        namespace: *const c_char,
        class_name: *const c_char,
    ) -> *mut c_void,
    pub il2cpp_get_method: unsafe extern "C" fn(
        class: *mut c_void,
        name: *const c_char,
        args_count: i32,
    ) -> *const c_void,
    pub il2cpp_get_method_overload: unsafe extern "C" fn(
        class: *mut c_void,
        name: *const c_char,
        params: *const c_void,
        param_count: usize,
    ) -> *const c_void,
    pub il2cpp_get_method_addr: unsafe extern "C" fn(
        class: *mut c_void,
        name: *const c_char,
        args_count: i32,
    ) -> *mut c_void,
    pub il2cpp_get_method_overload_addr: unsafe extern "C" fn(
        class: *mut c_void,
        name: *const c_char,
        params: *const c_void,
        param_count: usize,
    ) -> *mut c_void,
    pub il2cpp_get_method_cached: unsafe extern "C" fn(
        class: *mut c_void,
        name: *const c_char,
        args_count: i32,
    ) -> *const c_void,
    pub il2cpp_get_method_addr_cached: unsafe extern "C" fn(
        class: *mut c_void,
        name: *const c_char,
        args_count: i32,
    ) -> *mut c_void,
    pub il2cpp_find_nested_class:
        unsafe extern "C" fn(class: *mut c_void, name: *const c_char) -> *mut c_void,
    pub il2cpp_resolve_icall:
        unsafe extern "C" fn(name: *const c_char) -> *mut c_void,
    pub il2cpp_class_get_methods:
        unsafe extern "C" fn(klass: *mut c_void, iter: *mut *mut c_void) -> *const c_void,
    pub il2cpp_get_field_from_name:
        unsafe extern "C" fn(class: *mut c_void, name: *const c_char) -> *mut c_void,
    pub il2cpp_get_field_value: unsafe extern "C" fn(
        obj: *mut c_void,
        field: *mut c_void,
        out_value: *mut c_void,
    ),
    pub il2cpp_set_field_value: unsafe extern "C" fn(
        obj: *mut c_void,
        field: *mut c_void,
        value: *const c_void,
    ),
    pub il2cpp_get_static_field_value:
        unsafe extern "C" fn(field: *mut c_void, out_value: *mut c_void),
    pub il2cpp_set_static_field_value:
        unsafe extern "C" fn(field: *mut c_void, value: *const c_void),
    pub il2cpp_object_new: unsafe extern "C" fn(klass: *const c_void) -> *mut c_void,
    pub il2cpp_unbox: unsafe extern "C" fn(obj: *mut c_void) -> *mut c_void,
    pub il2cpp_get_main_thread: unsafe extern "C" fn() -> *mut c_void,
    pub il2cpp_get_attached_threads:
        unsafe extern "C" fn(out_size: *mut usize) -> *mut *mut c_void,
    pub il2cpp_schedule_on_thread:
        unsafe extern "C" fn(thread: *mut c_void, callback: unsafe extern "C" fn()),
    pub il2cpp_create_array:
        unsafe extern "C" fn(element_type: *mut c_void, length: usize) -> *mut c_void,
    pub il2cpp_get_singleton_like_instance:
        unsafe extern "C" fn(class: *mut c_void) -> *mut c_void,

    pub log: unsafe extern "C" fn(level: i32, target: *const c_char, message: *const c_char),
    pub gui_register_menu_item: unsafe extern "C" fn(
        label: *const c_char,
        callback: Option<extern "C" fn(*mut c_void)>,
        userdata: *mut c_void,
    ) -> bool,
    pub gui_register_menu_section: unsafe extern "C" fn(
        callback: Option<extern "C" fn(*mut c_void, *mut c_void)>,
        userdata: *mut c_void,
    ) -> bool,
    pub gui_show_notification: unsafe extern "C" fn(message: *const c_char) -> bool,
    pub gui_ui_heading: unsafe extern "C" fn(ui: *mut c_void, text: *const c_char) -> bool,
    pub gui_ui_label: unsafe extern "C" fn(ui: *mut c_void, text: *const c_char) -> bool,
    pub gui_ui_small: unsafe extern "C" fn(ui: *mut c_void, text: *const c_char) -> bool,
    pub gui_ui_separator: unsafe extern "C" fn(ui: *mut c_void) -> bool,
    pub gui_ui_button: unsafe extern "C" fn(ui: *mut c_void, text: *const c_char) -> bool,
    pub gui_ui_small_button: unsafe extern "C" fn(ui: *mut c_void, text: *const c_char) -> bool,
    pub gui_ui_checkbox:
        unsafe extern "C" fn(ui: *mut c_void, text: *const c_char, value: *mut bool) -> bool,
    pub gui_ui_text_edit_singleline: unsafe extern "C" fn(
        ui: *mut c_void,
        buffer: *mut c_char,
        buffer_len: usize,
    ) -> bool,
    pub gui_ui_horizontal: unsafe extern "C" fn(
        ui: *mut c_void,
        callback: Option<extern "C" fn(*mut c_void, *mut c_void)>,
        userdata: *mut c_void,
    ) -> bool,
    pub gui_ui_grid: unsafe extern "C" fn(
        ui: *mut c_void,
        id: *const c_char,
        columns: usize,
        spacing_x: f32,
        spacing_y: f32,
        callback: Option<extern "C" fn(*mut c_void, *mut c_void)>,
        userdata: *mut c_void,
    ) -> bool,
    pub gui_ui_end_row: unsafe extern "C" fn(ui: *mut c_void) -> bool,
    pub gui_ui_colored_label: unsafe extern "C" fn(
        ui: *mut c_void,
        r: u8,
        g: u8,
        b: u8,
        a: u8,
        text: *const c_char,
    ) -> bool,
    pub gui_register_menu_item_icon: unsafe extern "C" fn(
        label: *const c_char,
        icon_uri: *const c_char,
        icon_ptr: *const u8,
        icon_len: usize,
    ) -> bool,
    pub gui_register_menu_section_with_icon: unsafe extern "C" fn(
        title: *const c_char,
        icon_uri: *const c_char,
        icon_ptr: *const u8,
        icon_len: usize,
        callback: Option<extern "C" fn(*mut c_void, *mut c_void)>,
        userdata: *mut c_void,
    ) -> bool,
`
    pub android_dex_load: unsafe extern "C" fn(
        dex_ptr: *const u8,
        dex_len: usize,
        class_name: *const c_char,
    ) -> u64,
    pub android_dex_unload: unsafe extern "C" fn(handle: u64) -> bool,
    pub android_dex_call_static_noargs:
        unsafe extern "C" fn(handle: u64, method: *const c_char, sig: *const c_char) -> bool,
    pub android_dex_call_static_string: unsafe extern "C" fn(
        handle: u64,
        method: *const c_char,
        sig: *const c_char,
        arg: *const c_char,
    ) -> bool,
    pub il2cpp_runtime_object_init:
        unsafe extern "C" fn(object: *mut c_void),
    pub il2cpp_string_new:
        unsafe extern "C" fn(text: *const c_char) -> *mut c_void,
    pub il2cpp_string_chars:
        unsafe extern "C" fn(s: *mut c_void) -> *mut u16,
    pub il2cpp_string_length:
        unsafe extern "C" fn(s: *mut c_void) -> i32,
    pub gui_ui_combo_menu: unsafe extern "C" fn(
        ui: *mut c_void,
        id: *const c_char,
        selected_index: *mut i32,
        items: *const *const c_char,
        item_count: usize,
        search_term: *mut c_char,
        search_term_len: usize,
    ) -> bool,
    pub hachimi_register_on_game_initialized: unsafe extern "C" fn(
        callback: Option<unsafe extern "C" fn(*mut c_void)>,
        userdata: *mut c_void,
    ) -> bool,
    pub hachimi_register_present_callback: unsafe extern "C" fn(
        callback: Option<unsafe extern "C" fn(*mut c_void, *mut c_void)>,
        userdata: *mut c_void,
    ) -> bool,
    pub gui_get_menu_width: unsafe extern "C" fn() -> f32,
    pub gui_set_menu_width: unsafe extern "C" fn(width: f32),
    pub hachimi_get_base_dir: unsafe extern "C" fn() -> *const c_char,
    pub hachimi_get_data_path: unsafe extern "C" fn() -> *const c_char,
}

static INIT: Once = Once::new();
static mut VTABLE_PTR: *const Vtable = std::ptr::null();
static mut API_VERSION: i32 = 0;

extern "C" fn on_menu_click(_userdata: *mut c_void) {
    unsafe {
        let vtable = VTABLE_PTR.as_ref();
        if let Some(vtable) = vtable {
            let message = CString::new("Hello from the example plugin!").unwrap();
            (vtable.gui_show_notification)(message.as_ptr());
        }
    }
}

extern "C" fn on_menu_section(ui: *mut c_void, _userdata: *mut c_void) {
    unsafe {
        let vtable = VTABLE_PTR.as_ref();
        if let Some(vtable) = vtable {
            let heading = CString::new("Example Plugin").unwrap();
            (vtable.gui_ui_heading)(ui, heading.as_ptr());
            (vtable.gui_ui_separator)(ui);
            let label = CString::new("This section is rendered by the plugin.").unwrap();
            (vtable.gui_ui_label)(ui, label.as_ptr());
        }
    }
}

#[unsafe(no_mangle)]
pub extern "C" fn hachimi_init(vtable: *const Vtable, version: i32) -> InitResult {
    if vtable.is_null() || version < 2 {
        return InitResult::Error;
    }

    unsafe {
        VTABLE_PTR = vtable;
        API_VERSION = version;
    }

    INIT.call_once(|| unsafe {
        let vtable = &*vtable;
        let title = CString::new("Example Plugin").unwrap();
        (vtable.gui_register_menu_item)(title.as_ptr(), Some(on_menu_click), std::ptr::null_mut());
        (vtable.gui_register_menu_section)(Some(on_menu_section), std::ptr::null_mut());
    });

    InitResult::Ok
}
```

## Best practices

1. **Pagsusuri ng version**: Palaging suriin ang API version sa `hachimi_init`
1. **Pag-handle ng error**: I-return ang `InitResult::Error` kapag mabigo ang initialization
1. **Logging**: Gamitin ang logging API para sa pag-debug at user feedback.

## Mga halimbawa sa pag-hook at il2cpp

### Pag-hook ng game functions

Halimbawa ng pag-hook ng mga game function:

```rust
type UpdateFunc = unsafe extern "C" fn(*mut c_void);

unsafe extern "C" fn my_update_hook(this: *mut c_void) {
    if let Some(vtable) = VTABLE {
        let (_, interceptor) = get_hachimi_and_interceptor();

        // Get original function
        let trampoline = (vtable.interceptor_get_trampoline_addr)(
            interceptor,
            my_update_hook as *mut c_void
        );
        let original: UpdateFunc = std::mem::transmute(trampoline);

        // Do something before
        let tag = CString::new("MyPlugin").unwrap();
        let msg = CString::new("Update called").unwrap();
        (vtable.log)(4, tag.as_ptr(), msg.as_ptr());

        // Call original
        original(this);

        // Do something after
    }
}

// In hachimi_init (real example used in Hachimi):
// UnityEngine.Texture2D.ReadPixels (args = 3)
unsafe {
    let vtable = VTABLE.unwrap();
    let (_, interceptor) = get_hachimi_and_interceptor();

    let image_name = CString::new("UnityEngine.CoreModule").unwrap();
    let image = (vtable.il2cpp_get_assembly_image)(image_name.as_ptr());
    let namespace = CString::new("UnityEngine").unwrap();
    let class_name = CString::new("Texture2D").unwrap();
    let klass = (vtable.il2cpp_get_class)(image, namespace.as_ptr(), class_name.as_ptr());

    let method_name = CString::new("ReadPixels").unwrap();
    let readpixels_addr = (vtable.il2cpp_get_method_addr)(klass, method_name.as_ptr(), 3);

    (vtable.interceptor_hook)(
        interceptor,
        readpixels_addr,
        my_update_hook as *mut c_void
    );
}
```

### Paggalaw sa mga il2cpp objects

```rust
unsafe fn example_il2cpp_usage() {
    if let Some(vtable) = VTABLE {
        // Get a class
        let image_name = CString::new("UnityEngine.CoreModule").unwrap();
        let image = (vtable.il2cpp_get_assembly_image)(image_name.as_ptr());

        let namespace = CString::new("UnityEngine").unwrap();
        let class_name = CString::new("Object").unwrap();
        let klass = (vtable.il2cpp_get_class)(image, namespace.as_ptr(), class_name.as_ptr());

        if klass.is_null() {
            return;
        }

        // Example: get a singleton-like instance (if available)
        let instance = (vtable.il2cpp_get_singleton_like_instance)(klass);
        if instance.is_null() {
            return;
        }

        // Example: instantiate a new object
        let new_obj = object_new(klass);

        // Example: resolve a method address
        let method_name = CString::new("get_name").unwrap();
        let method_addr = (vtable.il2cpp_get_method_addr)(klass, method_name.as_ptr(), 0);
        if method_addr.is_null() {
            return;
        }

        // Example: find a specific method overload by index
        // This is useful when multiple methods have the same name
        if let Some(method_info) = get_methods(klass).nth(2) {
            // The first field of MethodInfo is the actual function pointer
            let specific_addr = *(method_info as *const *mut c_void);
        }

        // Cast method_addr to a function pointer if you intend to call it
    }
}
```
