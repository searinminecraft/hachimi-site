# FAQs

## Paaano gumamit ng ibang mods kasama ang Hachimi Edge?

Tignan ang `load_libraries` na opsyon sa [Config page](config). Tandaan na hindi gagana ang mga ilang mod sa pamamaraang ito; hindi talaga sila designed para gamitin sa isa't isa.

## Tapos nang mag-install ang update pero hindi pa rin sila translated. Bakit?

Baka tapos na silang mag-install, pero hindi pa nilo-load ng laro ang mga bagong translations. Mag-navigate lang sa isa pang screen (na nagti-trigger ng loading screen) at makikita mo sila.

## Pwede pa ba akong maglaro habang naga-update pa ang mga translations?

Oo, okay lang na magpatuloy sa paglalaro habang ina-update pa ang mga translation data, ngunit pakitandaan na naka-disable ang translations habang naga-update at mawawala sila kapag umalis ka sa kasalukuyang screen sa laro. Babalik din sila kapag tapos na sila mag-update at nilo-load ng laro ang mga ito (tignan ang itaas).

## Hindi ko sinadyang sinara ang laro habang naga-update pa rin ang translations. Ito na ba ang aking katapusan?

Hindi, buksan mo lang ulit ang laro at tatanugin ka ulit na magsimulang i-download ang mga translations sa simula.

## Ano ung `.tl_repo_cache` na file sa aking Hachimi folder? Pwede ko to kainin?

Hindi, ginagamit ito para i-track ang kasalukuyang estado ng mga translation files para sa incremental updates; huwag ito baguhin o burahin.

## Mayroon bang version o ibang mod para sa Apple devices (iOS)?

Hindi. Napaka-locked down ang Apple devices, mahirap gumawa o mag-install ng mga mod para sa mga iyon dahil diyan.

## Paano i-uninstall ang Hachimi?

**Windows**: May uninstall option ang installer. Kung hindi na yan nasa iyo, i-download lang ang pinakabago at gamitin iyan.  
**Android**: Kailangan mong i-uninstall ang patched na version at i-install ang orihinal.

## Maba-ban ba ako kapag gamitin ko ang Hachimi? May mga ban na bang nangyari?

Nilalabag ng Hachimi at mga parehong tool ang TOS. Lagi mong dinadala ang panganib.
Ito ay gayunpaman hindi malamang, at walang naitalang ban na nangyari kaugnay sa Hachimi o iba pang translation tools mula noong 2022.

## Parang na-ban ako, paano ko talaga malalaman?

Ito ang ipapakitang mensahe kapag banned ka:
![Access to your account has been restricted](/assets/banned.jpg)

## Maaari ko bang laruin ang Global at JP version nang sabay-sabay?

Oo, pero may kailangan kang gawing workaround kapag nilalaro mo ang DMM version, sundan ang [mga hakbang na ito](troubleshooting#error-501).

## Ano ang mga pagkakaiba sa mga Mode ng Pag-Update sa Physics?

Ito ang mga mode na internal na tinutukoy ng laro, at hindi ipinatupad ng Hachimi. Kaunti lang ang nalalaman tungkol sa mga ito.

- `ModeNormal` ang default.
- Mukhang naibabalik ng `Mode60FPS` ang ilang physics movements kapag pinapataas ang framerate, ngunit maaari pa ring maging medyong buggy minsan.
- Hindi alam ang parehong `SkipFrame` modes pero mukhang pareho silang sira.

## Paano mag-update sa Android?

::: info Impormasyon
Kinakailangan lang ang pag-uninstall ng laro para sa pinakaunang install, hindi para sa anumang updates.
:::

### Laro

Ang notification sa pag-update mula sa Play Store ay nangangahulugan na kailangan mong i-update ang laro mismo sa pamamagitan ng pag-download ng pinakabagong APK ng laro. Pagkatapos ay kailangan mong i-install ang mga bagong file na iyon sa pamamagitan ng UmaPatcher, na siyang maga-apply din ng pinakabagong available na Hachimi Edge release.

Sundan ang [install guide](installing-android#using-umapatcher-edge-recommended) mula sa step 4.

### Hachimi

Kapag may bagong bersyon ng Hachimi na na-release, maaari mong piliin na i-install para sa mga bagong features at updates. **Hindi mo naman kailangan** na gawin mo ito kaagad. Ang paggawa nito ay nangangailangan ng pag-patch muli.

Sundin ang [install guide](installing-android#using-umapatcher-edge-recommended) mula sa step 5 para i-reinstall ang kasalukuyang game version, or mula sa step 4 kapag hindi na nasa iyo ang mga pinakabagong APK ng laro.

### UmaPatcher

Dahil ang UmaPatcher ang tagatulong para sa pag-patch at pag-install ng laro, kinakalangan lang itong i-update sa ilang mga kaso, tulad na kung nagkakaroon ka ng anumang isyu sa pag-install. Wala itong benefits sa laro. Sa katunayan, basta't wala kang anumang problema, inirerekomenda namin ang paggamit ng iyong kasalukuyang bersyon.

1. Kung di mo pa ito ginagawa, pumunta sa Settings -> Export signing key.
1. I-download at i-install ang [pinakabagong UmaPatcher Edge](https://github.com/kairusds/UmaPatcher-Edge/releases/latest/download/app-release.apk), na naga-update sa app in-place.
1. (*Opsyonal*) Magpatuloy sa sinusubukan mong gawin.

## Pinatay ko ang Hachimi GUI/Overlay, paano ko to buksan ulit?

Una sa lahat, baka ginawa mo ito bilang workaround sa Android. Naayos na ang isyu na ito mula noong v0.15.1, kaya tignan mo kung nag-update ka na sa pinakabagong Hachimi Edge version.

Buksan ang config file ng Hachimi Edge (`config.json`) sa text editor at palitan ang `disable_gui` value mula `true` sa `false`, at i-restart ang laro. Nakalagay ang `hachimi` folder sa loob ng [installation folder ng laro](#paano-ko-hanapin-ang-install-folder-ng-laro).
Kung di ka komportable sa paggawin nito o magkaroon ka ng mga isyu, mas-ligtas na burahin na lang ang config file. Gagawin muli ng Hachimi Edge ang file na may mga default.

## Paano ko hanapin ang install folder ng laro?

**Steam**: I-right-click ang game sa Steam -> Manage -> Browse local files

**DMM**: I-click ang 3 dots sa tabi ng pangalan ng laro sa DMM -> 🛈 icon -> 📁 icon

**Android**: Hindi ito karaniwang naa-access, pero nakalagay ang ilang data sa `Android/media/jp.co.cygames.umamusume` (maaaring mag-iba depende sa phone brand). Para sa full access, kailangan mong i-patch ang laro na may `"Export internal data documents provider"` na patch ng Revanced at gumamit ng file explorer na may document provider selector o naka-root ka at i-access ang `/data/data/jp.co.cygames.umamusume/files`. (Ang mga tutorial kung paano gawin ang mga ito ay wala sa saklaw ng proyekto, kaya gamitin ang Google.)
