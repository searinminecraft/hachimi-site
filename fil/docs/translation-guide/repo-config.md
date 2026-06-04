---
outline: [2,3]
---

# Pag-configure ng translation repo

May mga kani-kanilang configuration na hiwalay sa user config ng Hachimi ang mga repo para ayusin ang kanilang behavior. Tinatakda ng mga maintainer ng repo ang config na ito. Kung ikaw ito, pakibasa ang pahinang ito para maintindihan ang mga opsyon

JSON format ang configuration file at ilalagay ito sa `localized_data` folder.
Relative ang lahat ng mga path sa config sa folder na iyan.

## Mga opsyon

Hindi kumpleto ang pahinang ito. Ang punong internal na representasyon ng lahat ng mga available na opsyon ay mahahanap sa [source code](https://github.com/kairusds/Hachimi-Edge/blob/main/src/core/hachimi.rs#L574).

### Localization file paths

``` json
"localize_dict": "path"
"hashed_dict": "path"

"text_data_dict": "path"
"character_system_text_dict": "path"
"race_jikkyo_comment_dict": "path"
"race_jikkyo_message_dict": "path"

"assets_dir": "path"
```

Ito ang mga relatibong path sa mga files sa bawat bahagi ng translation system.

- Ang unang dalawa ay para sa [pangkalahatang UI](translation-system#localize-dict-hashed-dict).
- Ang sumusunod na apat ay ang mga [MDB tables](translation-system#mdb-dicts).
- Ang huli ang folder kung saan [nakalagay ang ibang localization files](translation-system#asset-dicts). Tulad ng mga kwento, liriko, larawan, atbp. Di tulad ng ibang dict, ang mga file doon ay isinasaayos ayon sa internal na path ng laro.

### Karagdagang localization assets

``` json
"extra_asset_bundle": {
    "windows": "path",
    "android": "path"
}
```

Isang opsyonal na dict na nagbibigay-daan sa iyo na mag-include ng custom na Unity AssetBundle na maaaring i-load sa laro. Ang mga key nito ang mga sinusuportahan na platform, at ang kanilang value ang path sa bundle.  
Maaaring maglaman ng anumang file ang bundle na ito, ngunit kulang sa ngayon ang mga eksaktong detalye sa kung paano ito gumagana. Karaniwan itong ginagamit para mag-include ng custom na font para gagamitin sa localization, kung kinakailangan. Ang paggawa ng AssetBundle ay kailangan na mayroon kang naka-install na compatible na version ng Unity Editor, o maghanap ka ng third party na tool.

``` json
"replacement_font_name": "path"
```

Ang internal path sa loob ng `extra_asset_bundle` para maghanap ng custom na font, kung kinakailangan.

### Localized text functions

``` json
"plural_form": "n == 1 ? 0 : 1",
"ordinal_form": "(((n+9)%10)<3 && ((n+90)%100)>10) ? ((n+9)%10) : 3",
"ordinal_types": ["$st", "$nd", "$rd", "$th", "etc"]
```

Ang unang dalawa ay mga custom na expression na susuriin upang matukoy ang mga anyong plural at ordinal, para magamit sa mga template expression na `$ordinal(n)` at `$plural(n, t0, t1)`. Sa mga expression na ito, ang `n` ay karaniwang isang game text-variable tulad ng `{0}`. Ang expression ang nagpapasya kung alin sa mga `tx` argument ang gagamitin para sa plural, at alin sa mga `ordinal_type` para sa ordinal, batay sa value ng `n`.  
Parehong opsyonal ang dalawa. Ang halimbawa ay ang default para sa Ingles. Ang mga detalye ay nasa [source code](https://github.com/kairusds/Hachimi-Edge/blob/main/src/core/plurals.rs).

``` json
"months": ["month 1", "month 2", "etc"],
"month_text_format": "$(half) $(month)"
```

Kung pano ipo-format ang buwan.
Ang una ay isang array kung saan ang bawat entry ay kumakatawan sa isang buwan.
Ang pangalawa ay ang string na ginagamit sa ilang bahagi ng laro na nagpapakita ng kalahating buwan, tulad ng para sa mga "turn" ng Career. Hindi ako sigurado kung ginagamit talaga ito.

### Pag-wrap ng text

``` json
"use_text_wrapper": true
```

Boolean para i-enable ang custom na text wrapping o hayaan ang laro na ipamahala ito.
Hindi magagamit ang iba pang mga opsyon sa seksyong ito kapag nakatakda ito sa `false`.

``` json
"wrapper_penalties": {
    "nline_penalty": 0,
    "overflow_penalty": 0,
    "short_last_line_fraction": 0,
    "short_last_line_penalty": 0,
    "hyphen_penalty": 0
}
```

Isang opsyonal na dict na nagsasaad ng mga penalty na ginamit sa optimal-fit wrapping algorithm. Kapag ginamit, dapat i-configure ang lahat ng penalty.
Tignan ang [halimbawa](https://docs.rs/textwrap/latest/textwrap/wrap_algorithms/fn.wrap_optimal_fit.html#optimal-fit-algorithm) para sa isang pangunahing ideya kung paano ito gumagana, at ang [Penalties](https://docs.rs/textwrap/latest/textwrap/wrap_algorithms/struct.Penalties.html#fields) para sa higit pang detalye at settings.

``` json
"line_width_multiplier": 2.0
```

Ang panloob na lapad ng linya ng laro ay nakatakda sa mga karakter na CJK unicode, kung saan ang bawat isa ay karaniwang humigit-kumulang 2 karakter na ASCII.  
Ang setting na ito ay global na ina-apply upang i-offset ito, kung saan kinakailangan para sa mga kalkulasyon ng custom wrapping (hindi inilalapat sa lahat ng dako).

``` json
"text_frame_line_spacing_multiplier": 0.72,
"text_frame_font_size_multiplier": 0.96
```

Mga multiplyer na ina-apply sa text frame/box settings. Karaniwan itong ginagamit para sa story dialog.

``` json
"systext_cue_lines": {
    "type": 1,
    "...": 2
}
```

Opsyonal na dict na nagpapahiwatig ng pinakamataas na bilang ng linya bawat uri ng systext.  
Ang mga key nito ay ang "type" na nahahanap sa `cue_sheet` column ng MDB: `snd_vo_*TYPE*_`. Ang mga value nito ang max lines para sa uri na iyan.
Maaari ding i-specify ang `"default"` key, na gagamitin kapag walang anumang uri ang tumutugma. Kapag hindi nakatakda, `4` ang default.

``` json
"skill_formatting": {
    "name_length": 18,
    "desc_length": 18,
    "name_short_lines": 1,

    "name_short_mult": 1.0,
    "name_sp_mult": 1.0
}
```

Mga custom na haba ng linya para sa mga skills. Ginagamait kung saanman pinapakita ang skills.  
Mga value ibinibigay bilang game-internal (pre-multiply). Opsyonal ang bawat value, gayundin ang mismong dict.

- `Short`: tumutukoy sa mga kasanayang ipinapakita sa isang dobleng listahan na walang paglalarawan, tulad ng screen ng impormasyon ng Umamusume.
- `SP`: tumutukoy sa pag-render ng skill points sa tabi ng pangalan, tulad ng pag-upgrade.

### Mga karagdagang functions

``` json
"auto_adjust_story_clip_length": true
```

Pinapayagan ang pag-adjust ng internal values para tumugma sa haba ng localized text. Inaapektuhan ang delay/oras ng pagbasa ng diyalogo sa `auto` mode.

``` json
"now_loading_comic_title_ellipsis": true,
```

Putulin nang may ellipsis ang mga pamagat ng komik sa halip ng pag-resize nito.

``` json
"remove_ruby": true
```

Tanggalin ang furigana ng laro.

``` json
"character_note_top_gallery_button": {
    "text": "Career Event\n     Gallery",
    "font_size": 38,
    "line_spacing": 0.6
},
"character_note_top_talk_gallery_button": {
    "text": "     Chat\n   Gallery",
    "font_size": 44,
    "line_spacing": 0.55
}
```

Mga espesyal na setting para sa mga non-standard na element na nagdudulot ng mga isyu. Maaaring hilingin ang mga karagdagan kapag may nahanap na element na ganito.

``` json
"news_url": "https://hachimi.leadrdrk.com/PakaNews/"
```

URL kung saan kukunin ang balita ng laro. Kinakailangan ang espesyal na naka-set up na source para magbigay ng localized na balita mula sa opisyal na website.
