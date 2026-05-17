# Config

Nililista ng pahinang ito ang mga config options at nilalarawan kung saan sila ginagamit.

Mahahanap ang config file sa isa sa mga sumusunod na folder:

- Windows: `[Install folder ng laro]\hachimi\config.json`
- Android: `/sdcard/Android/media/jp.co.cygames.umamusume/hachimi/config.json`

::: warning Babala
Maaaring magresulta sa file corruption ang pag-edit ng file na ito gamit ang isang basic na text editor. Gumamit ng proper na text editor tulad ng Acode/QuickEdit (Android) o Notepad++ (Windows).
:::

**Tandaan:** Ang ilang mga opsyong ito ay hindi available sa Config Editor at dapat manwal na idagdag.

- `debug_mode`: Kapag papaganahin ang debug mode o hindi. Kasalukuyan lamang nitong pinapagana/hindi pinapagana ang debug logging.
- `translator_mode`: Para sa mga translator. Nilo-log ang mga untranslated na UI strings sa console at ine-enable ang ilang features na pakipakinabang sa mga translator
- `disable_gui`: Dini-disable ang built-in GUI. Pakitandaan na dini-disable din nito ang pagtingin ng update sa translations
- `localized_data_dir`: Ang directory na naglalaman ng localized data. Dapat may localized data configuration file
- `target_fps`: Ang target FPS sa laro. Kapag hindi nakatakda, hindi susubukang i-override ang FPS ng laro. Walang epekto kapag nakatakda ang `vsync_count`
- `open_browser_url`: Ang URL na bubuksan kapag bunubuksan ang in-game browser mula sa GUI. Default: `https://www.google.com/`
- `virtual_res_mult`: Ang virtual resolution multiplier. Kung kaya ito ng iyong device, magandang halaga ang 1.5 o 2; ang anumang mas mataas pa riyan ay overkill. Maaaring gamitin nang hindi isinasara ang laro sa pamamagitan ng paggawa ng "Soft restart".
- `gui_scale`: Binabago ang laki ng GUI. Default: `1.0`.
- `render_scale`: Ang internal render resolution scale multiplier. Ang mas matataas na value ay nagpapabuti sa sharpness ngunit binabawasan ang performance ng laro. Default: `1.0`.
- `msaa`: Kinokontrol ang antas ng MSAA (anti-aliasing) na ginagamit ng game renderer. Ang mas matataas na halaga ay nagpapakinis ng mga edges ngunit maaaring makabawas sa performance.
- `aniso_level`: Kinokontrol ang ang anisotropic filtering level para sa textures. Nagpapabuti ng texture clarity kapalit ng GPU usage.
- `translation_repo_index`: Ang index URL ng translation repo. Ginagamit ng translation updater.
- `skip_first_time_setup`: Kung lalaktawan ang first time setup sa startup o hindi. Awtomatikong nakatakda sa `true` kapag nasara na ang first time setup dialog.
- `disable_auto_update_check`: Dini-disable ang auto update checks sa startup.
- `disable_translations`: Dini-disable ang mga translation features.
- `disable_skill_name_translation`: Dini-disable ang pag-translate ng skill names habang pinapanatiling naka-enable ang ibang translations.
- `meta_index_url`: Ang meta index URL na ginagamit para kunin ang mga available na repository.
- `ui_scale`: Ang UI scale factor. Default: `1.0` (walang scaling).
- `hide_ingame_ui_hotkey`: Ine-enable ang internal hotkey na makakatago ng UI ng laro. Kapag naka-enable ito sa Android, maaari mo ring i-triple-tap ang top right para sa katulad na epekto.
- `graphics_quality`: Mga posibleng value: `Default`, `Toon1280`, `Toon1280x2`, `Toon1280x4`, `ToonFull`, `Max`.
- `story_choice_auto_select_delay`: Delay sa segundo para sa pagpili ng choice kapag ginagamit ang auto mode sa kwento. Default: `0.75` (segundo)
- `story_tcps_multiplier`: Multiplier ng bilis ng text sa kwento ("typewriting count per second"). Default: `1.0`
- `enable_ipc`: Ine-enable ang HTTP interprocess communication server na nagbibigay-daan sa mga ibang program na kontrolin ang laro. Nilalayong gamitin ng mga translation tools.
- `ipc_listen_all`: Tinatanggap ang mga IPC command mula sa anumang device sa network. **HUWAG ito i-enable kapag hindi mo ito kailangan.**
- `force_allow_dynamic_camera`: Pinipilit ang laro na pumili ng dynamic camera (kilala rin bilang POV camera, jockey camera, atbp.) sa anumang uri ng karera.
- `live_theater_allow_same_chara`: Pinipilit ang laro na payagan kang pumili ng katulad na character nang maraming beses para sa pormasyon ng live concert. Dini-disable din ang automatikong pag-save ng pormasyon. **HUWAG SUBUKANG MANWAL NA I-SAVE ANG MGA DUPLICATED NA PORMASYON**
- `physics_update_mode`: Kinokontrol kung paano ina-update ng laro ang physics. Mga posibleng value: `ModeNormal`, `Mode60FPS`, `SkipFrame` and `SkipFramePostAlways`.
- `ui_animation_scale`: Multiplier para sa bilis ng UI animation. Default: `1.0`.
- `sugoi_url`: Ang URL sa Sugoi Offline Translator o compatible na translation server para sa automatic translations. Hindi mo kailangang itakda ang opsyong ito kapag gumagamit ka ng tipikal na Sugoi setup. Default: `http://127.0.0.1:14366`
- `auto_translate_stories`: Pinapayagan ang pag-translate ng mga kwento gamit ang auto translator.
- `auto_translate_localize`: Pinapayagan ang pag-translate ng UI text gamit ang auto translator. Karaniwan itong HINDI inirerekomenda dahil hindi karaniwang pinapanatili ng mga translator ang line breaks o formatting tags.
- `disabled_hooks`: Manwal na i-disable ang mga hook. Huwag itong pakialaman, dahil ginagamit ito bilang debug tool para sa pag-troubleshoot ng mga isyu sa compatibility.
- `enable_file_logging`: Ine-enable ang pag-log sa `hachimi.log` sa directory ng laro. Kapag naka-disable o hindi magawa ang file, babalik lamang ang logging sa debug output lamang. Default: `false`.
- `lazy_translation_updates`: Lalaktawan ang ang pag-redownload ng translation files na hindi pa nagbago sa repository, kahit na outdated o corrupted ang local files. Pinapabilis ang mga update ngunit iiwanan ang mga sirang file. Default: `false`.
- `shadow_resolution`: Kinokontrol ang kalidad ng shadows. Pinapahusay ng mas-mataas na value ang shadow detail ngunit maaaring ipababa ang performance.
- `skill_info_dialog`: Ine-enable ang extended na skill information dialog na nagpapakita ng karagdagang detalye tungkol sa skill kapag tinitignan sila sa laro.

## Sa Windows lang

- `vsync_count`: Ang VSync count. Itakda ito sa 1 para tumugma sa refresh rate ng iyong monitor. Sumangguni sa [Unity docs](https://docs.unity3d.com/ScriptReference/QualitySettings-vSyncCount.html) para sa higit pang impormasyon.
- `load_libraries`: Listahan ng libraries na ilo-load sa startup. Maaaring gamitin upang mag-load ng ibang mods. Halimbawa: `["applejuicer.dll", "banana.dll"]`
- `menu_open_key`: Ang Windows VK code para sa key sa pagbukas ng menu. Default: `39` (Right arrow key). Tignan [ang pahinang ito](https://cherrytree.at/misc/vk.htm) para sa listahan ng lahat ng keycode.
- `auto_full_screen`: Awtomatikong i-full screen ang laro tuwing tumutugma ang orientation ng laro sa orientation ng screen. Sa ngayon, walang suporta ang Hachimi para sa iba pang aspect ratio at palaging naka-scale ang full screen resolution sa 16:9. Pakitakda nang maayos ang mga value na `full_screen_mode` at `resolution_scaling` kapag ginagamit ang opsyong ito.
- `full_screen_mode`: Ang fullscreen mode na gagamitin. Mga posibleng value: `ExclusiveFullScreen`, `FullScreenWindow` (Borderless full screen). Pakigamit ang `ExclusiveFullScreen` kapag hindi 16:9 ang aspect ratio ng iyong display, kung hindi, hindi masusukat nang maayos ang contents ng laro.
- `resolution_scaling`: Mode ng resolution scaling. Mga posibleng value: `Default`, `ScaleToScreenSize`, `ScaleToWindowSize`. Pakigamit ang `ScaleToScreenSize` (inirerekomenda) o `ScaleToWindowSize` kapag ginagamit ang `auto_full_screen` sa screen na may resolution na mas-mataas kaysa sa 1080p, kung hindi, hindi masusukat nang maayos ang contents ng laro.
- `block_minimize_in_full_screen`: Iba-block ang pag-minimize kapag full screen. Gamitin lamang kasama ang `FullScreenWindow`.
- `window_always_on_top`: Panatilihin ang game window sa itaas ng ibang window.
- `disable_gui_once`: Dini-disable ang built-in GUI hanggang sa susunod na pag-launch. Awtomatikong ire-reset sa `false` pagkatapos ng startup at pinipilit ang `disable_gui: true` nang isang beses. Pinapayagan ka nitong gumawa ng pagbili sa Steam store.
- `discord_rpc`: Ine-enable ang Discord rich presence integration sa mga platform na hindi sinusuportahan (DMM), na nagpapakita ng kasalukuyan mong aktibidad sa Umamusume sa iyong Discord profile.
