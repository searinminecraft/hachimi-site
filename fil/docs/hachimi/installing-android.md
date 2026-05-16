# Guide sa pag-install (Android)

::: danger
Madaling magkamali sa pag-install sa Android. Basahing mabuti ang pahinang ito!
:::

Ang paraan nito ay ang pag-patch ng Hachimi Edge sa APK ng laro.
Gumagawa nito at nag-i-install ng **bagong, hiwalay na app**, na maaaring magresulta sa karamihang isyu.

Bukod pa rito, maraming bagay ang maaaring magkamali depende sa iyong device o version ng Android. Palaging isama ang mga ito kapag humihingi ng tulong.

**Hindi sinusuportahan** ang Global version sa Android!

## ⚠️ Babala ⚠️

Kapag may umiiral kang save data, mag-set up ng **Data Link** o **Cygames ID** bago i-install ang patched na version para i-save ang progress. Naka-disable ang pag-sign in gamit ang Google account.

Kapag naka-install ang *unpatched* na version ng laro, **kailangan mo muna itong i-uninstall**. Maaaring i-update ang laro mamaya nang hindi pag-uninstall.

**Hindi mo magagamit ang Google Play Store** sa laro pagkatapos mag-patch, kasama ang pagbili. Gagana ang Cygames Store.

## Gamit ang UmaPatcher Edge (inirerekomenda)

::: tip
Sa mga Xiaomi device na hindi HyperOS, inirerekomenda namin ang Shizuku na opsyon.  
Maaari mo rin subukang i-disable ang `MIUI Optimizations` bago mag-install, pero ire-reset nito ang **lahat ng mga pahintulot ng mga app**.
:::

Ang UmaPatcher Edge ay isang installer tool na naga-automate ng pag-patch ng APK. Dina-download nito ang pinakabagong Hachimi Edge para sa iyo kapag nagpa-patch

1. (*Isang beses lang*) Kapag lumilipat ka mula sa hindi Edge na UmaPatcher, buksan ang settings page nito at **i-export ang signing key**. Panatilihin mo ito nang ligtas, kailangan mo ito mamaya.
1. (*Isang beses lang*) I-uninstall ang **orihinal at hindi naka-patch** na laro. Laktawan kapag na-patch mo na ito.
1. I-download at i-install ang pinakabagong version ng [UmaPatcher Edge](https://github.com/kairusds/UmaPatcher-Edge/releases/latest/download/app-release.apk).
1. I-download ang (mga) game APK file, ang mga sinusuportahang format ay:
    - **Split APK files:** Isang base APK kasama ang mga modular APK na nakadepende sa device.
    - **Single APK file**: Punong APk na kasama ang lahat ng mga split APK. Deprecated.
    - **XAPK file**: Naka-rename na ZIP archive na may mga split APK files.
    ::: warning Babala
    Kilala sa pagdudulot ng mga problema ang APKPure. Inirerekomenda ang [Qoopy](https://qoopy.leadrdrk.com/) bilang source, gamitin ang ID 6172.
    :::
1. Buksan ang UmaPatcher.
1. (*Isang beses lang*) Kapag lumilipat o in-uninstall ang UmaPatcher, i-import ang signing key na na-export mo sa step 1.
1. Piliin ang **Normal install**. Piliin ang (mga) game APK file na na-downlaod mo.
1. Pindutin ang **Patch** para simulan ang proseso ng pag-patch at pag-install.
1. (*Isang beses lang*) Kapag tagumpay itong natapos, kapag hindi mo pa na-save ang key, i-export ang iyong signing key mula sa settings ng UmaPatcher Edge.
   Panatilihin itong ligtas! Baka kailangan mo ito ulit para sa troubleshooting.

⚠️ Kailangan mo itong ulitin mula sa step 4 sa tuwing mag-update ang laro. **Hindi mo kailangang** i-uninstall ang laro para mag-update. [Higit pang impormasyon sa pag-update](faqs#paano-mag-update-sa-android).

::: warning Huwag ibahagi ang mga signing key o gamitin ang mga pre-patched na APK ng ibang tao!
Ito ay kakaiba sa bawat device at inilalagay sa APK. **Magdudulot ito ng mga isyu sa pag-update**.
:::

➡ Magpatuloy sa [First Time Setup](getting-started#first-time-setup).

::: details Paggamit ng Shizuku (alternative, baka papaganahin ang store)
Maaaring i-install ang UmaPatcher Edge gamit ang [Shizuku](https://github.com/RikkaApps/Shizuku/releases).
Gumagana ito na parang "rootless direct install" at *maaaring* maiwasan ang *ilang* isyu sa pag-install.  
Kung hindi mo makita ang opsyong ito, i-update ang UmaPatcher sa pinakabagong bersyon.

1. I-install ang [Shizuku](https://github.com/RikkaApps/Shizuku/releases).
1. Sundan ang [Shizuku activation guide](https://shizuku.rikka.app/guide/setup/) para simulan ang Shizuku.
1. Sundan ang normal na UmaPatcher Edge install guide sa itaas, pero piliin ang **Shizuku Method** sa step 7, dapat itong lumabas bilang 'available' ngayon.
1. Kapag tapos na, inirerekomenda na itigil ang Shizuku at i-disable ang wireless debugging sa Developer Options.
:::

::: details Mag-patch nang hindi pag-uninstall + mga update sa store (kailangan ang root)
Kasama ang rooted install na opsyon sa UmaPatcher Edge na hindi nangangailangan na i-uninstall ang laro o makitungo sa mga APK, na nagbibigay-daan sa iyo na mag-update sa anumang app store.

Kapag naka-install na ang laro, i-tap ang card sa itaas ng home screen ng patcher para piliin ang app na gusto mong i-patch (kung kinakailangan). Pagkatapos, piliin ang "Direct install" bilang paraan ng pag-install at i-tap ang "Patch". Hindi kailangan ng input files.

**Kailangan mo pa ring** i-patch ulit ang laro gamit ang UmaPatcher sa tuwing mag-update ang laro.
:::

## Manual na pag-install (di inirerekomenda)

1. I-build o i-download ang mga pre-built libraries sa [Releases page](https://github.com/kairusds/Hachimi-Edge/releases).
1. I-extract ang APK file ng laro. Baka kailangan mo gamitin ang [apktool](https://apktool.org/) para dito.
1. Palitan ang pangalan ng `libmain.so` file sa bawat folder sa loob ng `lib` sa `libmain_orig.so`.
1. Kopyahin ang mga proxy libraries sa kanikanilang folder (hal. ilalagay ang `libmain-arm64-v8a.so` sa `lib/arm64-v8a`). Palitan ang pangalan nito sa `libmain.so`.
1. I-build ang APK at i-install ito.

➡ Magpatuloy sa [First Time Setup](getting-started#first-time-setup).
