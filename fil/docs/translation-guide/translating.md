---
outline: [2,3]
---

# Pag-translate

::: tip
Kapag direkta kang na-link dito, inirerekomenda namin na [basahin ang guide sa simula](welcome).
:::

Ngayong mas naiintindihan mo na ang mga pangunahing kaalaman ng mga asset ng laro at kung paano ginagamit ng Hachimi ang mga ito, tingnan natin kung paano aktwal na simulan ang pagsasalin.

## Mga Kagamitan

Mayroong ilang iba't ibang mga tool na magagamit upang lumikha at mag-edit ng mga translations.

### ZokuZoku

Binuo ng lumikha ng Hachimi, ito ang kasalukuyang pinakamadaling simulan ang paggamit at sinusuportahan ang karamihan sa mga format ng Hachimi.

Gayunpaman, hindi na ito makakapag-edit ng mga kwento pagkatapos ng malaking update sa Japanese server noong Setyembre 24, 2025, at marami itong hindi nalutas na bug at mga isyu sa paggamit. Ito ay kasalukuyang hindi pinapanatili.

Tingnan ang [ZokuZoku guide](using-zokuzoku).

### ZokuZoku Edge

Ito ay isang aktibong community fork ng orihinal na ZokuZoku extension. Ang layunin ay ibalik ang mga functionality sa pag-eedit, magdagdag ng mga pagpapabuti sa kalidad ng buhay, at lutasin ang mga isyung iniwan ng pangunahing developer ng ZokuZoku.

Bagama't gumagana ang fork na ito, maaaring manatiling hindi matatag ang ilang feature depende sa usecase.

Mga Link: [Source Code](https://github.com/Mario0051/ZokuZoku), [Extension Builder at Release Repository](https://github.com/Tenshou170/ZokuZoku-Edge)

### UmaTL Legacy Tools

::: info
May paparating nang update para masuportahan nang maayos ng mga ito ang lahat ng bahagi ng laro, gamit ang mga bagong feature. Kapag nakumpleto na, ito na ang magiging pangunahing toolset.
:::

Ang mga ito ay bahagi ng pinakamaagang patch ng pag-translate ng laro, at dahil dito ay hindi direktang sumusuporta sa mga format ng Hachimi. Medyo hindi rin gaanong madaling gamitin ang mga ito.

Gayunpaman, maaari pa rin itong gamitin sa tulong ng ilang karagdagang script, at partikular na madaling gamitin para sa mga kuwento, na nagbibigay ng mas maraming feature at mas maayos na karanasan.

Tingnan ang [Gabay sa UmaTL](using-umatl).

### Kagamitan sa Hachimi (Hachimi Tools)

Ito ay isang maliit na karagdagang toolset na pangunahing nakatuon sa pagharap sa mga `texture` at `uianimation` assets. Nagbibigay din ito ng ilang mga kapaki-pakinabang na tampok.

Medyo teknikal ang mga ito, kaya ipinapalagay nila na ang gumagamit ay may sapat na kaalaman at isinasama lamang ang mga pangunahing dokumentasyon sa kanilang readme.

Tulad ng ZokuZoku, ang mga ito ay hindi pinapanatili at humihinto sa paggana pagkatapos ng isang pag-update ng laro.

Gamitin ang [pinapanatiling fork na ito](https://github.com/noccu/hachimi-tools) na may nakapirming suporta at mga bagong tampok.

### Carotene Tools

Bahagi ng mas lumang Carotene patch. Ang mga ito ay hindi pinapanatili at hindi na gumagana nang tama.

## Pagharap sa mga partikular na pagsasalin

### UI (Localize / Hashed dicts)

Ang `localize_dict` ay kasalukuyang sinusuportahan lamang ng [ZokuZoku](#zokuzoku).

Ang mga hashed dicts ay hindi sinusuportahan ng anumang mga tool at dapat manu-manong i-edit. Kakailanganin mo rin ng isang paraan ng pagbuo ng tamang hash. Ang [tool na ito](https://github.com/Hidden-is-fun/UmamusumeTextHashCalc/releases/tag/v0.1) ay isang ganitong paraan.

Dapat mong i-dump ang `localize_dict.json` file mula sa laro gamit ang menu ni Hachimi pagkatapos ng mga pag-update ng laro. Paganahin ang `translator mode` upang makita ang opsyon.

### MDB

Sa karamihan ng mga kaso, dapat mo pa ring gamitin ang [ZokuZoku](#zokuzoku). Magagamit ang mga tool ng UmaTL kung alam mo ang iyong ginagawa, at paminsan-minsan ay nagbibigay ng mga bentahe.

Bigyang-pansin ang `text_data`, maraming kategorya at kung minsan ang mga ito ay nadoble nang buo o bahagyang. Mayroon ding mga kombinasyon.

### Dialogue (Story / Race / Lyrics asset dicts)

These are likely the easiest to deal with. They are straightforward, all tools support audio preview and choice branching (which isn't complex in the vast majority of cases), and there are no real gotchas. Just don't forget to translate the titles.

The game update on 2025/09/24 encrypted both assets and the `meta` database indexing them. ZokuZoku doesn't support this.

Your best choice is to use [Legacy UmaTL tools](#umatl-legacy-tools), which also has a few relevant features not found in other tools.

### Textures (kasama ang atlases)

Gamitin ang [espesyal na toolset](#kagamitan-sa-hachimi-hachimi-tools) para kumuha ng mga textures at lumikha ng `diff.png` pagkatapos i-edit.

Kapag nagawa na ang isang diff, maaari ka ring magtrabaho mula roon para sa mga update ng laro gamit ang mga kagamitan.

Kakailanganin mo ng isang editor ng imahe para i-edit ang mga imahe. Asikasuhin ang mga limitasyon sa laki sa laro, sa kasalukuyan ay hindi sinusuportahan ng Hachimi ang pagpapalit ng mga iyon.

Kasama sa mga kagamitan ang pangunahing dokumentasyon.

### UIAnimation

Gamitin ang [espesyal na toolset](#kagamitan-sa-hachimi-hachimi-tools) para mag-extract at mag-update.
Kasama sa mga tool ang pangunahing dokumentasyon.

Marami sa mga ito ay maglilista lamang ng metadata para sa kanilang mga katugmang texture (tingnan sa itaas). Sa kasalukuyan, iyon ang asset hash ng pinagmulan para sa sistema ng proteksyon.

Kapag may kasamang text ang mga ito, i-edit lamang ang text sa nakuha na file. Maaari mo ring ayusin ang laki at posisyon nito, na maaaring maging lubhang kapaki-pakinabang.
I-extract ang mga ito sa alinman sa `flash` o `flashcombine`. Siguraduhing suriin ang pareho.
Dapat mong alisin ang anumang bahagi ([JSON objects](https://www.w3schools.com/js/js_json.asp)) na hindi na-edit.

Mas makakatulong sa iyo ang pagkakaroon ng paraan para tingnan ang `meta` file (ito ay isang SQLite DB) at mag-browse o maghanap ng mga potensyal na pangalan ng asset.
Kapag ang text ay hindi matatagpuan sa ibang lugar, malamang na "nakatago" ito sa isa sa mga file na ito. Karaniwan para sa ilang bahagi ng UI ng isang senaryo.

### Movies

::: info
Nag-eksperimento ka ba o mayroon ka pang karagdagang impormasyon? Pakisabi sa amin!
:::

Ang mga movie ng laro ay nasa USM format at naka-encrypt. Hindi ito ang pinakamadaling gawin at may kasamang maraming mga babala. Maraming mga opsyon ang hindi pa nasusubukan. Inirerekomenda ang mahusay na teknikal na kasanayan at oras para sa pananaliksik at eksperimento.

Maaari kang gumamit ng iba't ibang mga tool upang i-decrypt at muling i-encrypt ang mga ito, gamit ang key na `75923756697503`/`(0000)450D608C479F`.

- [USMBreak](https://github.com/beer-psi/usmbreak)
- [WannaCRI](https://github.com/donmai-me/WannaCRI)
- [CRID USM Demux Tool](https://mega.nz/file/TJQniYwL#Dp_D-KvzVlVgTwqzVJc1n3vslBZsHdy8pdDqzhRtsOI)
- [VGMToolbox](https://sourceforge.net/projects/vgmtoolbox/)

Sa pagitan, maaaring i-edit ang video sa anumang karaniwang paraan, bagama't ang pangunahing layunin namin ay ang pagdaragdag ng mga subtitle. Tandaan na ang orihinal na video ay mayroon nang "hard-subbed" sa wikang Japanese.

::: info
Sa hinaharap, maaaring suportahan ng Hachimi Edge ang mga soft-subtitle upang maiwasan ang pangangailangang manggulo sa mga video file.
:::

Malamang na dapat manatili ang mga video sa parehong format at resolution. Para sa mga simpleng kaso, dapat mong magamit ang kaugnay na encoder para sa format nito (MPEG1/H264/VP9?). Maaari mo ring subukan ang [opisyal na SDK](https://archive.org/details/new-criware-sdk) na *dapat* makatulong din sa mas advanced na mga kaso. Mag-ingat, ang ilang mga file ay naglalaman ng alpha channel bilang pangalawang video stream. Karamihan sa mga tool ay hindi mahusay na nakikitungo dito.

Ang audio ay hindi dapat hawakan at mainam na kopyahin nang direkta, nang walang de-/encryption. Maaaring kailanganin itong iproseso nang hiwalay. Tingnan ang mga dokumento para sa iyong mga tool.

## Iba pang mga konsiderasyon

Gumagana ang Hachimi sa Windows at Android. Kakailanganin mo ng access sa isang updated na Android `meta` file upang makabuo ng mga hash para sa luma na asset protection system. Hindi na ito kinakailangan para sa anumang iba pa.

Hindi mo na kailangan ng mga tool, siyempre. Minsan mas madaling gumawa ng pag-edit nang direkta sa isang `.json` file. Ang paghahanap sa mga ito ay maaari ring maging isang madaling paraan upang makahanap ng isang bagay.
