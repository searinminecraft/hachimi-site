---
title: Tungkol
---

# Mga Plugin <!-- markdownlint-disable-line MD025 -->

Sinusuportahan ng Hachimi ang isang plugin system na nagbibigay-daan sa mga developer na palawakin ang functionality nito sa pamamagitan ng mga dynamic library. Maaaring kumonekta ang mga plugin sa laro, baguhin ang pag-uugali, magdagdag ng mga elemento ng GUI, at marami pang iba.

## Ano nga ba ang mga plugin?

Mga dynamic libraries (`.so` file sa Android, `.dll` file sa Windows) ang mga plugin nag-i-integrate sa Hachimi Edge sa pamamagitan ng well-defined na API. Tumatakbo sila sa katulad na process ng laro at mat access sa powerful na features para sa pagbabago ng behavior.

## Ano ang magagawa ng mga plugin?

May access sa komprehensibong API ang mga plugin na nagbibigay-daan sa kanila na:

- **Mag-hook at mag-inspect ng IL2CPP**: Mag-intercept ng functions at i-access ang mga Unity IL2CPP class, method, at fields.
- **Magdagdag ng mga GUI Elements**: Magrehistro ng mga menu items, seksyon, at magpakita ng mga notifications sa built-in GUI ng Hachimi.
- **Android DEX Loading**: Mag-load at execute ng Java/Kotlin code sa Android (API v2+)
- **…at marami pang iba.**

## Mga konsiderasyon sa kaligtasan ⚠️

Huwag mag-install ng mga plugin mula sa di-kilalang sources o hindi pinagkakatiwalaang developer.  
Tumatakbo ang mga plugin na may punong access sa game process. Maaaring gawin ng mga malisyosong plugin ang:

- Pagnanakaw ng mga kredensyal at session token ng game account.
- Baguhin ang game data na maaaring magresulta sa pagka-ban ng account.
- Magresulta sa pag-crash ng laro o pagkasira ng data.
- Ipadala ang game data sa external servers.
- Mag-execute ng arbitrary code sa loob ng pahintulot ng laro.

::: danger ⚠️ KRITIKAL NA BABALA SA SEGURIDAD
**Sa Windows, may BUONG ACCESS SA ADMINISTRATOR ang mga plugin sa iyong system dahil tumatakbo ang laro na may pribilehiyong admin. Maaari silang maka-install ng malware, baguhin ang mga system files, i-access ang lahat ng data mo, atbp.**
:::

Bago mag-install ng plugin, isaalang-alang muna:

- **Pagkatiwala sa pinagmulan**: Gumamit lang ng mga plugin mula sa developer na pinagkakatiwalaan mo.
- **Pagpapatunay ng komunidad**: Tignan kung sinuri ng komunidad ang plugin.
- **Panganib sa pag-update**: Maaaring maging malisyoso ang mga dating pinagkakatiwalaang plugin.

Hindi mapapatunay ng mga Hachimi Edge developer ang mga third-party plugins at hindi sila mananagot sa anumang isyu na maaaring lumitaw sa pagkagamit mo niyan.

## Mga available na plugin
<!-- List plugins here. -->

## Mga susunod na gagawin

- **Sa Mga User**: [Alamin kung paano mag-install ng mga plugin](installation).
- **Sa Mga Developer**: [Matuto kung paano gumawa ng mga plugin](development).
