---
title: Pag-install
---

# Pag-install ng mga plugin <!-- markdownlint-disable-line MD025 -->

Ipinapaliwanag ng guide na ito kung paano i-install at i-configure ang mga plugin para sa Hachimi Edge.

::: danger ⚠️ BABALA SA SEGURIDAD
Mag-install lamang ng mga plugin mula sa mga source at developer na iyong pinagkakatiwalaan! Hindi mananagot ang mga developer ng Hachimi Edge para sa mga isyu o pinsala mula sa mga third party plugins.

Maaaring nakawin ng mga malisyosong na plugin ang mga kredensyal ng account sa laro, magresulta sa pagka-ban ng account, mag-execute ng arbitraryong code na parang galing sa laro, atbp.

🚨 **Sa Windows, may BUONG ACCESS SA ADMINISTRATOR ang mga plugin sa iyong system. Maaari silang maka-install ng malware, baguhin ang mga system files, i-access ang lahat ng data mo, atbp.**
:::

## Pag-install sa Windows

Sa Windows, ang mga plugin ay dapat i-configure sa config file.

1. **Hanapin ang plugin file**: Dapat may `.dll` file ka (e.g. `hachimi_myplugin.dll`).
1. **Ilagay ang plugin sa `hachimi` folder**: Ilagay ang plugin sa [`hachimi` folder](../hachimi/faqs.md#paano-ko-hanapin-ang-install-folder-ng-laro).
1. **I-edit ang config**: Buksan ang `config.json` sa `hachimi` folder idagdag ang plugin name:
   ```json
   {
      "load_libraries": [
        "hachimi\\hachimi_myplugin.dll"
      ]
   }
   ```
1. **I-save at i-restart**: I-save ang config file at i-restart ang laro

## Pag-install sa Android 

Sa Android, dapat idagdag ang mga plugin sa UmaPatcher Edge bago i-patch ang laro

1. **Ihanda ang plugin file**: Dapay may `.so` file ka (hal. `libmyplugin.so`).
1. **Buksan ang UmaPatcher Edge**: Buksan ang UmaPatcher Edge app.
1. **Idagdag ang plugin**:
   - Sa home screen mag-scroll pababa at hanapin ang "Plugins" section.
   - I-tap ang "Add Plugin" button.
   - Piliin ang iyong `.so` file mula sa iyong device.
   - Idadagdag ang plugin sa listahan at naka-enable bilang default.
1. **Ipamahala ang mga plugin** (opsyonal):
   - Maaari mong i-enable/i-disable ang mga plugin gamit ang checkbox sa tabi ng bawat plugin.
   - I-tap ang "Remove" para burahin ang plugin mula sa listahan.
1. **I-patch ang laro**: Sundan ang normal na [guide sa pag-install gamit ang UmaPatcher Edge](../hachimi/installing-android#gamit-ang-umapatcher-edge-inirerekomenda) para i-patch at i-install ang laro.
1. **I-verify**: Buksan ang laro. Kapag matagumpay na mag-load ang mga plugin, makikita mo ang mga epekto nito o menu items nito sa built-in GUI ng Hachimi.

::: tip
Kapag i-update mo ang laro, pinapanatili sa UmaPatcher Edge ang iyong mga plugin. I-patch lang ang bagong version at awtomatiko silang isasama.
:::

## Pag-disable ng mga plugin

### Windows

- Tanggalin ang plugin mula sa `load_libraries` array sa `config.json`.

### Android

- Buksan ang UmaPatcher Edge.
- I-uncheck ang plugin sa Plugins section.
- I-repatch ang laro.

## Pag-troubleshoot ng plugins

### Hindi naglo-load ang plugin

Sa Windows:

- Siguraduhin na tama ang path sa `config.json`.
- Siguraduhin na compatible sa iyong Hachimi version ang plugin.

Sa Android:

- Siguraduhin na nakadagdag sa UmaPatcher Edge ang plugin bago i-patch.
- Tignan kung compatible sa iyong Hachimi version ang plugin..
- Subukang i-patch ulit ang laro nang naka-enable ang plugin.

### Hindi gumagana o nagka-crash ang plugin

Kapag nagdudulot sa pag-crash o hindi gumagana ang plugin:

- **Makipagugnayan sa plugin author**: Dapat iulat sa plugin developer ang mga isyung kaugnay sa plugin.

## Mga susunod na gagawin

- Matuto kung [paano gumawa ng sarili mong plugin](development).
