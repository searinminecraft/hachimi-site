# Built-in GUI

Maaaring gamitin ang built-in GUI para i-activate ang ilang in-game functions at baguhin ang configuration.

![GUI screenshot](/assets/fil/built-in-gui.webp)

## Pagbukas ng menu

Maaaring buksan ang menu gamit ang ilang paraan, depende sa platform:

- **Android:** `Volume Up + Volume Down` o `I-triple tap ang top left`
- **Windows:** `Right arrow key`(\*)

**Tandaan:** Habang nakabukas ang menu o isang dialog mula sa Hachimi, iba-block nito ang lahat ng input mula sa system papunta sa laro. Kailangan mo silang isara bago mo makuha muli ang kontrol.

(\*) Maaaring i-rebind ang key. Tignan ang `menu_open_key` na opsyon sa [Config page](config).

## Panic Button

Kung makakuha ka ng anumang isyu dahil sa Hachimi Edge GUI, gamitin ang Panic Button key para pwersahang isara ang lahat ng mga overlay

- Android: Pindutin nang mabilis ang `Volume Down` nang 4 beses.
- Windows: Pindutin ang `K + Menu Open Hotkey`.

## Config

- **Buksan ang config editor:** Dito mo ma-e-edit ang config file sa laro. Tignan ang [Config page](config) para sa mga detalye tungkol sa bawat opsyon
- **I-reload ang config:** I-reload ang config file mula sa disk. Ginagamit kapag nabago ang file sa labas ng editor.
- **Buksan ang setup:** Binubuksan ang first time setup wizard. Maaari mong palitan ang iyong translation repo doon.

## Graphics

Dito mo mababago ang graphics sa laro nang real time.

Pakitandaan na hindi mananatili ang mga opsyong ito at magre-reset sa susunod na pagkasimula. Para permanenteng baguhin ang mga opsyon na ito, i-edit sila sa Config Editor.

## Translation

- **I-reload ang localized data:** Para sa mga translator. Ire-reload nito ang mga translation files mula sa disk.
- **Tignan kung may update sa translations**: Tinitignan nang mabilis kung may update. Efficient ito (gamit ang cache), at karaniwan itong sapat.
- **Tignan kung may update sa translations (pedantic)**: Magsagawa ng punong pagtingin kung may update. Sinusuri nito ang lahat ng mga file para sa posibleng isyu.

## Danger Zone

Hindi naman sila mapanganib kapag gamitin mo sila nang mabuti; pero ito talaga ang dahilan kung bakit sila nandito. **Huwag mo silang gamitin maliban kung alam mo ang ginagawa mo.**

- **I-soft restart:** Nagti-trigger ng error sa laro at pinipilit ang user na kumpirmahin ang restart, na nagre-restart nito pabalik sa title screen. Ito ang pinakamabilis na paraan para i-apply ang mga setting sa graphics na kung di man hindi mag-a-apply hanggang sa isara mo ang laro at binuksan muli. **Huwag ito gamitin habang naglalaro, hindi mo ito makakansela.**
- **Buksan ang in-game browser:** (Android lang) Ligtas ito gamitin, binubuksan lang nito ang browser sa laro, na maaaring gamitin para mag-browse sa web (~~o maglaro ng DOOM~~) nang hindi umaalis sa laro. Binubuksan ang Google bilang default, maaaring i-configure. Nakalagay ito dito dahil maaari itong makialam sa dialog system ng laro. Huwag lang ito buksan habang sinusubukan ng laro na i-prompt ka para sa ibang dahilan.
- **I-toggle ang game UI:** I-enable/i-disable ang lahat ng mga kasalukuyang aktibong game UI objects. Hindi maapektuhan ang anumang objects na nagawa pagkatapos i-activate. Maaaring magkamali sa pag-restore ng UI active state (hal. pag-enable ng mga object na hindi nararapat) pero hindi magreresulta sa problema sa karamihan ng mga kaso. Sa Android, maaari din ito magawa sa pamamagitan ng pag-triple-tap sa top right.
