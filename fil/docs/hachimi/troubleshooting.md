# Troubleshooting

Nililista dito ang mga kilalang isyu at kanilang solusyon. Suriin ang "Pangkalahatan" na seksyon muna, kasunod ang sa iyong platform, at sa huli [kung ano ang dapat mong gawin kapag hindi nakalista ang iyong isyu](#hindi-nakalista-dito-ang-aking-isyu) sa ibaba.

[[toc]]

## Pangkalahatan

### "Communication error" kapag sinusubukang simulan ang laro

**Hindi kailangan** ng karamihan ng mga user ang VPN para kumonekta sa laro (DMM lang, gumagawa ng Steam account), at maaari itong magresulta sa mga isyu. Tignan kung nakapatay ang iyong VPN, o gamitin ang split tunneling kapag sinusuportahan ng iyong VPN.
***Kailangan* ang VPN sa ilang mga region o ISP para makakonekta sa laro**.

Para sa JP version, maaari mong tignan sa pamamagita ng pagbisita ng [API](https://api.games.umamusume.jp/). Kung makakuha ka ng `404 Not Found`, **hindi** mo kailgan ng VPN. Ang `Access Denied` ay nangangahulugan na kailangan mo. Ang ibang resulta ay nagpapahiwatig ng problema sa iyong network o ISP at hindi ka namin matutulungan diyan.

Tignan ang [guide ng GameTora](https://gametora.com/umamusume/playing-on-dmm) para makapagsimula sa paggamit ng VPN, at ang [OpenVPN Google Doc](https://docs.google.com/document/d/18m9wHT4_AIh5ePKSo_ZYH9nSgNh492YQx76bIxmgqyc/edit?tab=t.0#heading=h.7cq4imx1gkqf) para sa alternatibong solusyon.

::: details Kailangan ko ng VPN para maglaro.
Kung nagpapasya kang gumamit ng OpenVPN (gamit ang UmaVPN.top), inirerekomenda na gamitin ang v2.7 client dahil sinusuportahan nito ang split tunneling (piliin iyan kapag nagda-download mula sa UmaVPN). Ang mga lumang bersyon ay nangangailangan ng pinning script sa guide sa itaas, na madaling mag-update ng mga isyu at hindi kasama ang mga mas bagong domain.

Sinusuportahan lang ng mga VPN (lalo na ang VPN Gate, na ginagamit ng UmaVPN) ang IPv4. Maaari mong subukang [i-unbind ang IPv6 mula sa adapter na ginagamit mo](https://networking.grok.lsu.edu/article.aspx?articleid=17573), pero **pakitandaan na maaari itong magdulot ng ibang isyu**.

Ang isang mas ligtas na paraan ay dapat na [i-prefer ang IPv4](https://learn.microsoft.com/en-us/troubleshoot/windows-server/networking/configure-ipv6-in-windows#use-registry-key-to-configure-ipv6), ngunit ito ay mas kasangkot at ang pagiging matulungin nito ay kasalukuyang hindi pa nasubok.

Ang iyong client o configuration ay maaari ding magkaroon ng mga pangkalahatang isyu sa split tunneling. Subukang i-off ito sa kasong iyon.
:::

::: details Note sa split tunneling (DMM o general-use na VPN).
Depende sa kung paano mo na-configure ang iyong VPN, alinman i-exclude ang laro o i-include lang ang DMM.

Kapag gumagamit ng UmaVPN.top, inaapektuhan lamang ng pinning script ang laro (walang kwenta para sa DMM) at kasama ang pareho sa `split-tunneling` na opsyon. Para sa huli, maaari mong buksan ang profile gamit ang text editor, mag-scroll sa `Route` na seksyon, at tanggalin ang hindi DMM na domains. Sa Android, maaari mo rin itong i-edit sa pamamagitan ng OpenVPN GUI.
:::

Kung naka-install ang **Global Steam** at **Japanese DMM** version ng laro, [subukan ang mga hakbang para sa Error 501 na isyu](#error-501).

### Corrupted/pakalatkalat na textures o text

::: tip
Kapag nilalaro mo ang **Global** version, baka na-install mo ang mga translations **na hindi para sa Global**.
Para ayusin ito, buksan ang `First time setup` ulit para pumili ng compatible na source o wala, at i-restart ang laro.
:::

Nangyayari ito dahil may mismatch sa sprite textures ng laro at naka-translate. Ang malamang na dahilan ay ang pag-update ng laro at binago ang ilang mga sprite, na karaniwang inaapektuhan ang `atlas` na type.

1. I-update ang translations mula sa menu. Kung may update na nahanap, **i-restart ang laro pagkatapos itong matapos**.
    - Sa Android o ilang mga device, *baka* kailangan mong burahin ang [mga naapektuhang folder](faqs#paano-ko-hanapin-ang-install-folder-ng-laro) (`atlas` ang pinakakaraniwan) para hayaan itong mag-update nang tama.
1. Kung walang update na nahanap, outdated ang iyong translation source. Maghintay o suriin ang source, pagpapaalam sa kanila kung kinakailangan.
    - <small>Kung malapit sa pag-update ng laro, malamang na inaayos na ito ng mga source maintainer. Pakisuri muna kung alam na nila ito bago sila abalahin.</small>

::: details Iba pang dahilan sa walang updates <!-- markdownlint-disable-next-line MD032 -->
1. Hindi na aktibo ang iyong translation source. Maaaring ipahiwatig nito na gumagamit ka pa rin ng lumang source mula sa orihinal na Hachimi..
Siguraduhin na ginagamit mo ang Hachimi Edge, at buksan mo ang menu at patakbuhin ang `First time setup` ulit.
1. Baka outdated ang mismong sources list, lalo na kung nag-upgrade ka agad mula sa lumang Hachimi. Sa kasong ito, Maaari mong gamitin ang `Restore Defaults` para i-reset ito sa pinakabagong kasama.
    - ⚠️ Babala: Ire-reset nito ang lahat ng mga setting.
:::

Kung wala sa mga translation sources ang aktibo, o kung gusto mong pansamantalang linisin ang UI, i-enable ang `Menu` -> `Config Editor` -> `Apply TL Atlas Workaround`. Huwag i-update ang iyong translations hangga't sa alam mo na updated na sila.

### Hindi nakakakuha ng mga translation updates

Una sa lahat, baka hindi naman sila updates. Ito ay ipapakita sa pamamagitan ng "No updates found" na mensahe.
Kung hindi ito nagpapakita at gumagamit ka ng VPN para maglaro, patayin ito habang naga-update

Kapag ginamit mo ang Hachimi dati bago ang Edge version, baka out-of-date ang iyong translation sources. Baguhin ang Meta URL sa first-time setup sa `https://gitlab.com/umatl/hachimi-meta/-/raw/main/meta.json` o i-reset ang iyong settings, at kumpletuhin ang setup na may bagong source.

### Mali ang bonus stats o nagsisimula sa 0

Ito ay isang bug sa laro na dulot ng pagtatakda ng iyong FPS nang masyadong mataas. Ibaba mo ito.

### Matigas ang physics (buhok, damit, atbp.) kapag tumatakbo sa 60+ FPS

Palitan ang "Physics update mode" sa `Mode60FPS`. Available ang setting na ito sa Config editor sa "Gameplay" na tab.

### Hindi naglo-load ang laro lagpas sa splash/start screen

Kapag nasa **Steam Global** ka, gamitin ang `alt` + `enter` para mag-toggle between sa fullscreen at windowed.
Mukha itong isang bug sa laro, na dinudulot ng Hachimi na madaling ma-trigger. Baka may paparating na opisyal na solusyon.

Kung **ma-stuck** ito sa splash screen, tignan ang [Error 501](#error-501).

Kung nakikta mo ang splash screen pero **nagca-crash** pagkatapos, tignan ang [Ayaw mag-start ang laro pagkatapos i-install ang Hachimi](#ayaw-mag-start-ang-laro-pagkatapos-i-install-ang-hachimi).

### Bina-block ng Hachimi UI ang pag-interact sa laro

Maaaring mangyari ito sa ilang pambihirang kombinasyon na pagkakataon. Subukan ang [panic button](built-in-gui#panic-button).

### Maliit ang in-game background / Puting border

Buksan ang Hachimi Menu -> Config Editor at i-reset ang `virtual resolution multiplier` sa 1.
Kapag hindi ito tumutulong, subukan itong i-adjust hanggang sa okay na ito.

### First time setup: Stuck sa loading ang repo selection o nagpapakita ng error

Baka gumagamit ka ng VPN para i-access ang laro. Pansamantala itong patayin hanggang sa tapos na ang setup at na-download na ang translations.

**OS Error 103 (Android)**: Subukang i-disable ang battery optimizations para sa laro o [i-reset ang iyong network settings](https://youtu.be/ah99wYYtUqU).

Tignan rin ang [itong kaugnay na isyu](#hindi-nakakakuha-ng-mga-translation-updates).

### Nagiiba minsan ang wika sa lyrics

Naayos na ang isyung ito noong Hachimi Edge v0.15.1. Mag-update sa pinakabagong version.

### May hindi naka-translate

Ang mga translations ay ibinibigay ng mga community volunteers na nag-aalok ng kanilang oras. May marami pang bagay na hindi pa tapos. Makipag-ugnayan sa iyong napiling [translation source](/credits) para sa progreso at sikaping suportahan ang mga translators nito.

### "Account restricted" na mensahe

Banned ka kung ganoon

## Windows

### Runtime error sa pag-launch

Nangangahulugan ito na gumagamit ka ng lumang version ng Hachimi, nasira pagkatapos ng update sa laro noong 2025/09/24 (JP) at 2025/11/11 (Global).
[I-install ang Hachimi Edge](getting-started).

Kapag ginagamit mo na ang Edge, subukang i-reinstall ang pinakabagong version.

### Ayaw mag-start ang laro pagkatapos i-install ang Hachimi

::: warning Babala
Maaaring pigilan ng ilang kernel anti-cheats (hal. Vanguard) ang pag-start ng laro gamit ang Hachimi Edge.
Siguraduhin na hindi sila tumatakbo, at subukan ulit.
:::

- Siguraduhin na ginagamit mo ang [Hachimi Edge](getting-started).
- Steam: Minsan pinapalit ng updates sa laro ang mga modified files. I-reinstall ang Hachimi gamit ang installer..
- DMM: I-restart (**hindi** shutdown + start!) ang iyong computer pagkatapos i-enable ng installer ang DotLocal DLL redirection.  
- DMM: Subukang i-restart ang DMM Launcher o pilitin ito na tumakbo ito palagi bilang Administrator.
- Pumuta sa installation folder ng laro, i-right click ang .exe ng laro, piliin at **Properties**, subukan ang **isa o higit pang** mga sumusunod sa ayos:
  - I-enable ang `Disable fullscreen optimizations` sa ilalim ng Compatibility tab.
  - Buksan ang `Change high DPI settings`, i-enable ang `High DPI scaling override`, at itakda ito sa `Application`.
- Buksan ang `Windows Settings → Display → Graphics`, idagdag ang .exe ng laro doon, at i-check ang `Don't use optimizations for windowed games` sa mga opsyon nito.

<!-- 
    TODO: add more details about weird edge cases like old unsupported versions of CarrotJuicer?
-->

### Ayaw nang mag-start ang laro

Kung gumagana ang Hachimi Edge dati, karaniwan na nag-update ang laro at na-replace ang ilang mga modified files. I-reinstall lang ang Hachimi Edge.

### Nagrerehistro ang input sa maling lugar / Stretched ang laro sa full screen mode

::: warning Babala
Sa Global client, maaaring sirain ng `Resolution scaling` ang rendering at input kahit sa 1080p resolution.
Labis naming inirerekomenda na **panatilihin ang `Resolution scaling` sa default value** sa Global version.
:::

::: info Impormasyon
Kapag nangyayari ito pagkatapos i-resize ang window sa DMM, naayos na ang isyung ito sa Hachimi Edge v0.14.3. Mag-update sa pinakabagong version.
:::

- Siguraduhin na nakatakda ang `Full screen mode` at `Resolution scaling` na opsyon nang mabuti.  
- Kapag mas-mataas kaysa sa **1080p** ang iyong screen resolution, subukang pumili ng ibang `Resolution scaling` value.  
- Kapag hindi **16:9** ang aspect ratio ng iyong monitor, itakda ang `Full screen mode` sa **Exclusive** sa halip.

### Nagsa-stutter ang laro

Siguraduhin na hindi mo pinagana ang auto-translate sa Hachimi settings. Gagana lang ito kapag may naka-set up nang mabuti na translation server, at maaaring magdulot sa problema sa performance.

### Steam: Mga isyu sa GUI/overlay

Minsan ay maaaring makaabala ang Steam overlay sa overlay ni Hachimi. I-disable ang isa sa mga ito (inirerekomenda ang Steam).

Para i-disable ang overlay ng Hachimi: buksan ang Hachimi menu at i-check ang "Disable overlay (GUI)" sa "General" tab, pindutin ang "Save", at i-restart ang laro.
Kapag gusto mong i-enable ulit ang Hachimi overlay, buksan ang config file ng Hachimi (`config.json`) sa isang text editor at baguhin ang `disable_gui` na value mula `true` sa `false`, at i-restart ang laro. Nakalagay ang file na ito sa `hachimi` folder sa installation folder ng laro.

### DMM: Hindi makalaro ng ilang laro pagkatapos i-install ang Hachimi

DotLocal DLL redirection ang paraan na ginagamit ng Hachimi para sa pag-load ng **DMM** version ng laro. na nagdudulot sa mga isyu sa ilang anti-cheat (hal. Vanguard).
Kailangan mong i-disable ang DLL redirection sa tuwing gusto mong laruin ang naapektong laro.  
Isang maliit na program ang [DotLocalToggle](https://github.com/LeadRDRK/DotLocalToggle/releases/) na nagbibigay-daan sa iyo na madali itong i-toggle.  
Bilang alternatibo, laruin ang **JP Steam** version.

### Installer: "Code execution cannot proceed / VCRUNTIME" na error

I-install ang pinakabagong [VC++ redistributable](https://learn.microsoft.com/en-us/cpp/windows/latest-supported-vc-redist?view=msvc-170) na tumutugma sa iyong device architecture. Kung hindi ka sigurado, Malamang na ito ay `x64`.

### Installer: I/O error: The system cannot find the file specified (os error 2)

Maaari itong mangyari sa global dahil sa ilang pagkakaiba sa pangalan ng file na hindi pa natutugunan. Hindi ito dapat makaapekto sa Hachimi at maaaring ligtas na balewalain.

### Installer: I/O error: Access is denied (os error 5)

May gumagamit ng (mga) file na sinusubkan nitong baguhin. Baka ito ay dahil nakabukas pa rin ang laro habang ini-install o ina-uninstall ang Hachimi.

### Mga isyu sa sound

Isa itong bug sa laro, hindi sa Hachimi. Maaaring i-enable ng mga user ang Windows Sonic nang walang anumang epekto para ayusin ito.

### Error 501

Ginagamit ng parehong version ang katulad na pangalan ng download directory pero may ibang capitalization.
Kailangang i-enable ang case sensitivity sa directory na ito para gumana sila
::: tip
Kung kailangan/gusto mong ilipat nang manwal: Para direktang pumunta sa data directory ng laro, gamitin ang `WinKey + R` at ilagay ang `%localappdata%low\Cygames` sa dialog.
Ginagamit ng Global ang "Umamusume" habang ginagamit ng JP ang "umamusume".
:::

1. Isara ang laro.
1. Buksan ang `Start Menu`, hanapin ang `PowerShell`, piliin ang "Run as Admin".
1. Patakbuhin ang sumusunod na command: `fsutil.exe file setCaseSensitiveInfo $env:USERPROFILE\AppData\LocalLow\Cygames enable`.
    - Kung makakuha ka ng `Error: Unsupported action` or katulad, patakbuhin ang sumusunod na command: `Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux`, at subukan ulit.
    - Kapag makakuha ka ng `Error: The directory is not empty`, pansamantalang ilipat ang lahat ng nilalaman sa  `Cygames` folder, at subukan muli:
    ```powershell
    New-Item -ItemType Directory "$env:USERPROFILE\AppData\LocalLow\CygamesTEMP"
    Move-Item "$env:USERPROFILE\AppData\LocalLow\Cygames\*" "$env:USERPROFILE\AppData\LocalLow\CygamesTEMP"
    ```
1. Kung kailangan mong i-empty ang Cygames folder, ilipat ang lahat pabalik:
    ```powershell
    Move-Item "$env:USERPROFILE\AppData\LocalLow\CygamesTEMP\*" "$env:USERPROFILE\AppData\LocalLow\Cygames"
    Remove-Item "$env:USERPROFILE\AppData\LocalLow\CygamesTEMP"
    ```

### Patuloy na humihiling ang Global Steam at JP DMM na mag-redownload ng data

Tignan ang [Error 501](#error-501).

## Android

### Nabigo ang pag-patch

- Siguraduhin na pinili mo ang **base at split APK** files, o ang **isang XAPK** file.
  I-tap and hold para pumili ng maraming file.
  - Inirerekomenda namin ang pag-download sa [Qoopy](https://qoopy.leadrdrk.com/) (Gamitin ang ID **6172**).
- Isara ang UmaPatcher Edge at i-clear ang app cache nito sa `App info → Storage`.
- I-redownload at i-reinstall ang UmaPatcher Edge at **i-import ang iyong signing key** ulit.
- Kung may makita kang `kotlinx` sa installer log, Gamitin ang `Save patched file` method at i-install ang nagreresultang file gamit ang [SAI](https://github.com/aefyr/sai/releases).
- Kung gumagamit ka ng **Xiaomi/POCO** device na tumatakbo ng **MIUI** (hindi **HyperOS**), subukan ang [Shizuku install method](installing-android#using-umapatcher-edge-recommended) o i-disable ang *MIUI Optimizations* mula sa Developer Options. Minsan itong makikialam sa pag-install.
    ::: warning Babala
    Ire-reset ang **lahat ng pahintulot ng mga app** ang pag-disable ng **MIUI Optimizations** at maaaring magresulta sa pagkawalan ng access (storage, notifications, atbp.)
    :::

### "App not installed as app isn't compatible" na error

::: info Impormasyon
Kinakailangan ang mga hakbang na ito para sa ilang Samsung devices at kinabibilangan ng pagkonekta ng iyong telepono sa PC. **Maaaring** gumana rin ang mga ito para sa iba pang Android devices.
:::

Maaari itong mangyari kung *na-uninstall* ang laro, pero nananatili pa rin sa **Secure Folder**. Sundan ang mga hakbang ito para kumpletong burahin ang laro:

1. **I-enable ang USB Debugging** sa iyong device sa Developer Options.  
   Kung hindi mo alam kung paano, sundan ang [YouTube Short Guide](https://www.youtube.com/shorts/p7DDuq56suU)
1. **I-download and i-extract** ang [Android Platform Tools (ADB)](https://developer.android.com/tools/releases/platform-tools#downloads) ZIP file sa iyong computer.
1. **Magbukas ng Terminal** sa pamamagitan ng pag-right click sa empty area sa extracted na ADB folder (lung saan nakalagay ang `adb.exe`) at pagpili ng **Open in Terminal** (o katulad).
   - Ang pagpindot ng **Shift** habang nagra-right click sa Windows 10 ay magpapakita ng **"Open PowerShell window here"** na opsyon.
1. **I-connect ang device** sa computer gamit ang USB (USB-C o anumang compatible na cable).
1. sa Terminal window, i-type `adb.exe` at pidutin ang **Enter** para siguraduhin na recognized ito.
1. I-type ang `adb devices` at pindutin ang **Enter**.  
   Tignan ang iyong device at **payagan ang USB debugging permission** kapag tinanong, at patakbuhin ulit ang command para i-verify ang connection.  
   May ipapakita itong `"ABCD1234EFGH" device` o katulad sa Terminal.
   Kung hindi, tignan ang ibaba.
1. I-type ang `adb uninstall jp.co.cygames.umamusume` at pindutin ang **Enter** para i-uninstall ang laro.

#### Unauthorized device troubleshooting

Kapag ang pag-type ng `adb devices` at pag-pindot ng **Enter** ay nagpapakita ng **"unauthorized"** sa halip ng **"device"**:

1. Sa iyong device, **i-disable ang USB debugging**, at **ire-enable** ito.
1. I-reconnect ang device at **payagan ang USB debugging permission** ulit kapag tinanong.
1. Ulitin ang mga kaugnay na hakbang sa itaas (karaniwang steps 5–7).

### I/O error: Permission denied (os error 13)

Dahil sa na-introduce na scoped storage sa Android 10, Maaaring mabigo ang Hachimi sa paggawa ng data directory nito.

1. Isara ang laro.
1. Buksan ang iyong file manager at mag-navigate sa `Android/media`.
1. Gumawa ng folder tawwag `jp.co.cygames.umamusume` kung kailangan.
1. Sa loob ng bagong ginawang folder, gumawa ng foler tawag `hachimi`.
1. Buksan ulit ang laro.

### I/O error: File exists (os error 17)

I-reboot ang iyong device at subukang buksan ang laro muli. Kung nagpapatuloy pa rin ang error, humingi ng tulong sa Discord server.

### Nagca-crash pagkatapos buksan (mga ilang device)

:::warning Babala
**HINDI** ito kaugnay sa crashing issue na nangyayari sa lumang Hachimi version (v0.14.1).  
Tignan ang [guide na ito](faqs.md#paano-mag-update-sa-android) para i-updte ang Hachimi.
:::

Baka kailangan ito para sa mga emulator at ilang Samsung devices.

1. Sundan muna ang [os error 13](#io-error-permission-denied-os-error-13) pero huwag mo muna buksan ang laro.
1. I-download [ang config file na ito](https://files.leadrdrk.com/hachimi/android-compat/config.json) at ilagay ito sa loob ng `hachimi` folder (siguraduhin na tawag itong `config.json`).

### Nawawalang translation selection habang nasa first time setuo

Tignan ang [os error 13](#io-error-permission-denied-os-error-13).

### Offset ang mga pagpindot

Buksan ang menu ng Hachimi -> Config Editor and i-adjust ang virtual resolution multiplier para hanapin kung ano ang okay na value.

### Hindi nagrerehistro ang mga taps, o nagdudulot sa pag-crash o pag-freeze ng laro

Naayos ang isyu na ito sa Hachimi Edge v0.15.1. Siguraduhin na [nag-update ka](faqs.md#how-do-i-update-on-android).

<details>
<summary class="collapsible-header-sub">Nararanasan ko ito sa isang version mas bago kaysa sa 0.15.1</summary>

:::tip
Idi-disable din ang mga translation updates ang pag-disable ng GUI. Kailangan mo itong paminsan-minsang i-enable at i-disable ulit para gawin ito.
:::

1. Siguraduhin na up-to-date ang iyong translations. Hayaang i-update ng Hachimi ang mga ito at huwag magpindot hanggang tapos na ito
1. Buksan ang menu ng Hachimi -> Config Editor at piliin ang Disable Overlay (GUI).
    - Para i-enable ito ulit, buksan ang `config.json` file sa isang text o JSON editor at palitan ang `disable_gui` na value mula `true` sa `false`, at i-restart ang laro. Nakalagay ito sa `android/media/jp.co.cygames.umamusume/hachimi` (maaaring mag-iba depende sa phone brand).
1. Pakiulat ang isyu na ito sa Hachimi Edge Devs sa Discord o GitHub.

</details>

### Matagumpay na na-patch pero walang translations

Patakbuhin ang first time setup ulit mula sa Hachimi Edge menu. Siguraduhin na naka-download at up to date ang mga translations.
Suriin ang impormasyon ng iyong translation source upang matiyak na isinasalin nito ang iyong tinitingnan.

Kapag habang nagpa-patch may binanggit na `libmain.so` sa logs, maaari mong subukan ang mga sumusunod hanggang may isa na gumagana:

1. I-force redownload ang Hachimi Edge sa UmaPatcher Edge settings, at mag-patch ulit.
1. I-clear ang cache at data ng UmaPatcher Edge sa `App info → Storage`.
1. I-reinstall ang UmaPatcher Edge, at mag-patch ulit.
1. (Advanced!) I-restart ang iyong device, i-wipe ang cache, at mag-patch muli.
<!-- Todo: How safe is the last one...? -->

### Hindi maka-log in gamit ang Google Play account

Hindi ka maaaring mag-log in sa patched version gamit ang iyong Google account at dapat kang gumamit ng Data Link password sa halip.
Kung mayroon ka nang Data Link password, mag-log in sa account sa title screen (☰ > Data Link).

Kung *wala* ka pang Data Link password, kailangan mong i-uninstall ang patched version ng laro, i-reinstall ang unpatched na version, mag-log in gamit ng Google account, at gumawa ng Data Link password.
Pagkatapos, maaari mong ulitin ang proseso ng pag-patch at mag-log in gamit ng iyong Data Link password.
Bilang alternatibo, maaari kang mag-log in sa Cygames ID para i-link ang iyong account data.

### Hindi ka pinapayagang maglaro sa device na ito (この端末でのプレイは許可されていません) na error

#### Naka-root ang iyong device

Siguraduhing stable ang iyong connection at ang device ay nakakapasa nang hindi bababa sa **DEVICE_INTEGRITY** sa Play Integrity servers (maaari mo itong i-verify gamit ang [Play Integrity API Checker](https://play.google.com/store/apps/details?id=gr.nikolasspyr.integritycheck) app). Kung pumapasa ito, baka gagana ang pag-enable ng **Magisk DenyList** (I-enable ang *Enforce DenyList* kung hindi ito gumagana). Gagana din ang paggamit ng mga tool tulad ng **Shamiko**.

#### Hindi naka-root ang iyong device

Kapag patuloy na pinapakita ang error na ito sa iyong device, maaaring ipahiwatig nito ang isang hindi matatag na koneksyon sa mga server ng Play Integrity, o kailangan mo ng **VPN** kapag sinisimulan ang laro. Tignan ang [Communication error](#communication-error-kapag-sinusubukang-simulan-ang-laro) na seksyon para sa higit pang mga detalye.

## Mga emulator (kasama ang Google Play Games)

Hindi sinusuportahan ng Hachimi o laro ang mga emulator. Maaari mo silang paganahin, pero bahala ka na diyan. Para maglaro sa PC, gamitin ang DMM o Steam client.

## Hindi nakalista dito ang aking isyu

I-uninstall ang Hachimi gamit ang installer. Subukang gamitin ang version na ginamit mo sa pag-install ng kasalukuyan mong version, ngunit dapat ay gumana nang maayos ang pinakabago.
Kung may marami kang version ng laro na naka-install, siguraduhin na nag-uninstall ka sa tamang path. at i-reinstall ang pinakabagong Hachimi Edge.
Kung hindi ito gumana, maaari kang magtanong sa `help/support` channel sa [Hachimi Project Discord](https://discord.gg/hachimimod). Pakisabi ang iyong game server, device platform at model, at malinaw na ipaliwanag ang iyong isyu at kung ano ang iyong sinubukan.
