# Guide sa pag-install (Windows)

::: warning Babala
Ginagamit ang DotLocal DLL redirection ang proseso ng pag-install sa DMM.
Di ito compatible sa ilang anti-cheats (hal. Vanguard, na ginagamit sa LoL/Valorant) at kailangang i-disable sa tuwing gusto mong laruin ang mga naapektuhang laro. Maaari mong gamitin ang [DotLocalToggle](https://github.com/LeadRDRK/DotLocalToggle/releases) para madali itong i-toggle. Hindi inaapektuhan ang Steam.
:::

::: details Legacy: Paglipat mula sa deprecated na shimming method (Shinmy)
Kailangan mo munang buong i-uninstall ang Shinmy; siguraduhin na hindi ito tumatakbo kapag binubura mo ito dahil nananatili ito hanggang 30 segundo pagkatapos magsara ang DMM at maaaring mabalik sa sarili. **Ang pinakamadaling paraan ay ang paggamit ng mismong installer** (na uninstaller din), lilinisin nito ang lahat para sa iyo.

Pagkatapos niyan, maaari mo nang i-uninstall ang Hachimi gaya ng dati.
:::

## Gamit ang installer (inirerekomenda)

1. I-download ang pinakabagong [Installer](https://github.com/kairusds/Hachimi-Edge/releases/latest/download/hachimi_installer.exe) at patakbuhin ito.
1. Kapag ginamit mo dati ang hindi edge na Hachimi, pindutin muna ang "Uninstall".
1. Piliin ang game version sa lower box.
1. Siguraduhin na tama ang [install directory](faqs#paano-ko-hanapin-ang-install-folder-ng-laro) at baguhin ito kung kailangan
    - Partikular na hindi nade-detect nang awtomatiko ang Global.
1. Pindutin ang "Install".
    - Sa unang beses, baka tatanungin ka na i-enable ang DotLocal DLL redirection. Pindutin ang OK at ie-enable ito para sa iyo. **Kailang mong I-RESTART (hindi shutdown) ang iyong  computer pagkatapos.**

⚠️ Baka kailangan mong ulitin ang pag-install pagkatapos mag-update ang laro (hindi magbubukas ang laro) dahiil sa pagpalit ng mga modified files.

➡ Magpatuloy sa [First Time Setup](getting-started#first-time-setup).

## Manual na pag-install

:::tip
Dagdagan lamang ang file extensions kapag nakikita mo sila sa kanilang orihinal. Itatago nito ng Windows, at ang iyong pagpalit ng pangalan ay maaring magkasira sa laro. Hindi naga-apply sa mga folder.
:::

### Steam

1. I-download ang `hachimi.dll` mula sa [Releases page](https://github.com/kairusds/Hachimi-Edge/releases).
1. I-rename ito sa `cri_mana_vpx.dll` at ilagay ito sa [install folder ng laro](faqs#paano-ko-hanapin-ang-install-folder-ng-laro).
1. Kapag nag-i-install sa JP (laktawan sa Global):
    1. I-download ang [`FunnyHoney.exe` ni Fern](https://gitlab.com/LeadRDRK/FunnyHoney).
    1. Palitan ang pangalan nito sa `UmamusumePrettyDerby_Jpn.exe` at ilagay ito sa install folder ng laro, na nag-o-overwrite ng orihinal.
    1. ⚠️ Panatilihin ang file na ito, baka kailangan mo ito sa tuwing mag-update ang laro (hindi magbubukas ang laro) dahil sa pag-restore ng orihinal na .exe file.

➡ Magpatuloy sa [First Time Setup](getting-started#first-time-setup).

### DMM

1. Sumangguni sa "Configure the registry" na seksyon [sa artikulong ito](https://learn.microsoft.com/en-us/windows/win32/dlls/dynamic-link-library-redirection#optional-configure-the-registry) para i-enable ang DLL redirection. **I-restart** ang iyong computer pagkatapos.
1. I-download ang `hachimi.dll` mula sa [Releases page](https://github.com/kairusds/Hachimi-Edge/releases).
1. Sa [install folder ng laro](faqs#paano-ko-hanapin-ang-install-folder-ng-laro), gumawa ng bagong folder na may pangalan na `umamusume.exe.local` at ilipat ang na-download na DLL file doon. Palitan ang pangalan nito sa `UnityPlayer.dll`.
1. I-download ang `cellar.dll` muka sa [Releases page ng Cellar](https://github.com/Hachimi-Hachimi/Cellar/releases).
1. Ilipat ito sa `umamusume.exe.local` at palitan ang pangalan nito sa `apphelp.dll`.

➡ Magpatuloy sa [First Time Setup](getting-started#first-time-setup).

### KomoeGame

1. I-download ang `hachimi.dll` mula sa [Releases page](https://github.com/kairusds/Hachimi-Edge/releases).
1. I-rename ito sa `winhttp.dll` at ilagay ito sa [install folder ng laro](faqs#paano-ko-hanapin-ang-install-folder-ng-laro).

➡ Magpatuloy sa [First Time Setup](getting-started#first-time-setup).
