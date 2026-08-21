# Magsimula

::: warning Babala
Likas na nilalabag ng proyekto ang TOS ng laro.
Gamitin sa iyong sariling peligro, at magbahagi ng mga pangalan at link sa isang responsableng paraan, para mabawasan ang atensyon mula sa mga developer.

Umiiral na Hachimi user? Dapat kang lumipat sa Hachimi Edge dahil sa malaking game update noong 2025/09/24 (JP) at 2025/11/11 (Global).
:::

::: tip Wika 🗣🌍
Maaari mong palitan ang wika sa top right.
:::

Pakisundan ang buong proseso kahit na dati mong ginamit ang Hachimi, dahil may ilang pagbabago ang Hachimi Edge.
Kung magkaroon ka ng anumang problema, tignan ang [Troubleshooting](troubleshooting).

## Compatibility

Sinusuportahan ng Hachimi Edge ang Steam, DMM, at Android versions para sa Japanese server, Steam version para sa Global server, at ang KomoeGame version para sa TW server.

::: details Mga detalye

### Windows

| Version | Sinusuportahan |
| --- | :---: |
| JP (DMM) | ✅ |
| JP (Steam) | ✅ |
| TW (KomoeGame) | ✅ |
| KR | ❌ |
| Global | ✅ |
| Emulators (anumang region) | ❌ |

### Android

| Version | Normal install | Direct install | Zygisk |
| --- | :---: | :---: | :---: |
| JP | ✅ | ✅ | ✅ |
| KR | ❌ | ❌ | ❌ |
| TW GP | ⚠️ | ⚠️ | ✅ |
| TW MC | ⚠️ | ⚠️ | ✅ |
| CN | ⚠️ | ⚠️ | ✅ |
| Global | ❌️ | ❌️ | ❌ |

<small>

✅ - Sinusuportahan.  
⚠️ - Gumagana, ngunit humahatong sa pag-fail ng laro dahil sa external na factors.  
❔ - Untested, baka gagana, pero huwag kang umasa.  
❌ - Hindi sinusuportahan.  

</small>

:::

## Mga Guide sa Pag-install

May installer ang Hachimi Edge para sa Windows at Android. Magkaiba sila!
Pakitignan ang mga specific na guide at magpatuloy sa pahinang ito pagkatapos i-install

[Para sa Windows](installing-windows)  
[Para sa Android](installing-android)

## First Time Setup

Sa unang pag-launch ng laro pagkatapos i-install ang Hachimi Edge, ipapakita sa iyo ang dialog na ito:

![First Time Setup](/assets/fil/first-time-setup.webp)

::: details Di ko 'to nakikita!
**Kapag lumilipat ka sa Edge**:  
Baka hindi ka nag-uninstall muna. Iiwanan nito ang mga outdated values sa iyong configuration. Buksan ang setup na ito sa Hachimi Edge menu at siguraduhin na ang Meta URL ay `https://gitlab.com/umatl/hachimi-meta/-/raw/main/meta.json` (Maliban kung **alam mo** na gumagamit ka ng iba). Ang hindi paggawa nito ay maaaring magresulta sa outdated na translations at sirang textures.

**Kung hindi**:  
Hindi na-install nang mabuti ang Hachimi Edge. Pakibasa ang install guide at subukan ulit, o tignan ang [Troubleshooting](troubleshooting).
:::

Pindutin ang "Susunod" at piliin gusto mong translation source, at pindutin ang "Tapos na" para i-save ang iyong config at simulan ang pagsusuri ng update.

Kapag pinili, Ipo-prompt ka ng Hachimi Edge na i-download ang bagong translation update, pindutin ang "Oo" para simulang i-download ang mga files.

Maaari kang bumalik sa dialog mamaya para palitan ang iyong translation source sa pamamagitan ng Hachimi menu

::: tip
Kung may mga isyu ka sa translations, maaari mo silang i-disable sa Config Editor > Paglalaro, at i-restart ang laro.
:::

## Paggamit

Ginagawa ang configuration gamit ang [in-game menu](built-in-gui) o sa pamamagitan ng [raw config file](config).  
Sinusuportahan rin ng Hachimi Edge ang [Mga Plugin](../plugins/about).
