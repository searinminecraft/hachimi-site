# Auto translation

Mata-translate ng Hachimi ang mga bahagi ng laro sa pamamagitan ng Sugoi Offline Translator o anumang compatible na translation server na nag-i-implement ng katulad na API (tulad ng [py3translationServer](https://github.com/gdiaz384/py3translationServer)).

Functionality:

- Makakakuha ka ng mga machine translation para sa anuman na hindi pa naka-translate.
- Papalitan ang machine translations sa aktual na translations kapag available na sila mula sa translation repo.

Mga Babala:

- Synchronous ang proseso na ito; mag-ha-hang ang laro habang hinihintay ang translations na matapos. Mababawasan ito sa paglipas ng panahon habang mas maraming bagay ang naisasalin at kayang i-reload ng Hachimi ang mga naisalin nang datos.
- Ang mga kagamitang nabanggit sa itaas ay nilayong gamitin sa isang PC; sa Android, maaaring kailanganin mo lang i-adjust ang `sugoi_url` sa address ng iyong PC na nagpapatakbo ng translation software.

## Paano gamitin

Buksan ang Config Editor at i-enable ang isa sa mga auto translation options (Awtomatikong i-translate ang mga kwento/UI).

::: warning Babala
Hindi karaniwang inirerekomenda ang "Awtomatikong i-translate ang UI", lalo na sa mga translator na walang internal cache.
:::

Kailangan mo ring patakbuhin ang translation program kasama ang laro

### Para sa Sugoi Offline Translator

I-launch ang "Offline Translator" .bat sa Sugoi Toolkit. Iwanang nakabukas ang translation program/command prompt para magamit ng Hachimi ang translator (ngunit wala itong ipapakita sa anumang window).

**Hindi inirerekomenda ang "Awtomatikong i-translate ang UI" sa translator na ito.**

### Para sa py3translationServer

Kung ipagpapalagay na na-set up mo na nang tama ang mga model, hindi na kailangan ng karagdagang configuration. Simulan lang ang server at baka gagana ito.

## Custom na URL

Kapag tumatakbo ang server sa ibang port o ibang machine, baka gusto mong ipa-connect ang Hachimi sa ibang address

Para magawa ito, itakda ang `sugoi_url` na opsyon sa config file. Halimbawa:

```json
"sugoi_url": "http://10.0.170.16:14366"
```

Bilang default, baka nawawala ang value na ito nakatakda sa `null` sa config file. Normal ito, at kokonekta pa rin ang Hachimi sa `http://localhost:14366` kapag hindi ito manwal na nakatakda.
