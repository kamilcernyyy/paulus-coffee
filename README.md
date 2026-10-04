# Paulus Coffee — web

Statický web bez build kroku: nahraj obsah této složky na hosting a je to. Funguje na Vercelu
(`vercel.json`) i na Wedosu / Apache (`.htaccess`).

## Výsledky (Lighthouse, finální build)
| | Výkon | Přístupnost | Osvědčené postupy | SEO |
|---|---|---|---|---|
| Mobil   | 100 | 100 | 100 | 100 |
| Desktop | 100 | 100 | 100 | 100 |

Mobil: LCP 1,7 s · CLS 0,002 · TBT 0 ms · 521 kB. Původní verze: výkon 0, LCP 18,4 s, 3,4 MB.
SEO/AEO + kontrola integrity: 35/35.

## Nasazení
- **Wedos:** přes FTP nahraj *celý obsah* složky (včetně skrytého `.htaccess`) do `www/`.
  Až poběží SSL, odkomentuj v `.htaccess` přesměrování na HTTPS.
- **Vercel:** nahraj složku do repa / přetáhni do Vercelu. `vercel.json` nastaví cache a bezpečnostní hlavičky.

## Co je potřeba doplnit (hledej v `index.html`)
- `Ulice a č.p.`, `+420 000 000 000` — v sekci Návštěva **i** v JSON-LD v `<head>`.
- Fotky: bloky `class="photo"` (interiér 3:2, galerie 16:9, mapa/vchod 4:3) nahraď `<img>` —
  ideálně AVIF/WebP, s `width`/`height`, `loading="lazy"` a `alt`.
- Odkazy na Instagram/Facebook (`sameAs` v JSON-LD + patička).
- Doména: všude je `https://pauluscoffee.cz/` (canonical, OG, JSON-LD, robots, sitemap).

## Co web dělá
- **Hero:** animace jako video. AV1 (520 kB) s H.264 zálohou, na mobilu menší 960px verze.
  Snímky jsou vyčištěné a zvětšené na 1440×1080, smyčka bez cuknutí.
  Bílé pozadí mizí přes `mix-blend-mode: multiply`. Nejdřív se ukáže plakátový snímek (10–20 kB AVIF),
  video se pustí po načtení a mimo obrazovku se pauzne.
- **Ilustrace (vektor, GSAP):** káva nateče do hrnku (hladina, kapky, pára) · sendvič, ze kterého
  padají drobečky · muž s croissantem pomalým krokem · skákající postavička v Rezervaci ·
  nová scéna „stůl u okna“ (Sloup Nejsvětější Trojice, radniční věž, mraky, ptáci, kočka, kniha, pára).
- **Ambientní pohyb** (bubliny, vlnitý text, video) se plynule rozjede ~1,6 s po načtení — první
  vykreslení je klidné a procesor je při načítání volný. Vše mimo obrazovku je pozastavené.
- **Omezení pohybu** (`prefers-reduced-motion`): bez animací, místo videa klidný snímek. Bez JS je vše viditelné.
- **Otevírací doba:** zvýraznění dneška a „Právě otevřeno / Zavřeno“ podle pražského času.
  Při změně hodin uprav HTML (`#hours`), JSON-LD i čísla v `assets/js/site.*.js`
  (`450`/`1140` = 7:30/19:00, `510`/`1080` = 8:30/18:00, v minutách).

## Technické poznámky
- Fonty hostované lokálně, ořezané na češtinu (Bricolage 31 kB, Instrument Sans 25 kB, Space Mono 7 kB),
  s metricky sladěnou zálohou → text při načtení neposkakuje.
- Soubory v `assets/` mají v názvu otisk obsahu → cachují se natrvalo. Když soubor vyměníš,
  dej mu nový název a uprav odkaz v `index.html`.
- Barvy textu jsou dopočítané na kontrast WCAG AA (terakotový text #A83F11 = 5,2 : 1).
