# Bier op 't Slot Festival — website

Eenpagina-site voor een bierfestival. Statische HTML, gehost op GitHub Pages.
Doelpubliek: bezoekers uit de regio Kennemerland. Taal: Nederlands.

## Het evenement

- Zondag 7 maart 2027, 14:00–22:00 uur
- Slot Assumburg, Heemskerk
- Georganiseerd door Stayokay Heemskerk i.s.m. Brouwerij Zeglis (https://brouwerijzeglis.nl/)
- 10 lokale en nationale brouwerijen; tot nu toe alleen Zeglis met naam en link bevestigd
- Entree: €12,50 (regulier) — inclusief 2 muntjes à €3 per stuk en een proefglaasje (mag je houden)
- De eerste 150 boekingen betalen €10 entree (zelfde inhoud: proefglaasje + 2 muntjes)
- Bij die eerste 150 boekingen: ook 25% korting op een overnachting in het kasteel, alleen geldig op 7 maart 2027 en onder voorbehoud van beschikbaarheid
- Contact: info@bierophetslot.nl
- Beoogd domein: bierophetslot.nl (nog niet geregistreerd)
- Stayokay Heemskerk: https://www.stayokay.com/nl/hostel/heemskerk

## Structuur

Alles staat in de root, geen build-stap, geen dependencies.

- `index.html` — de hele site: HTML + CSS in één bestand, CSS in een `<style>` in de `<head>`
- `hero-wide.{avif,webp,jpg}` — 2000×1333, liggende foto van het slot, voor schermen ≥700px
- `hero-tall.{avif,webp,jpg}` — 1200×1800, staande foto van de toren, voor schermen <700px
- `sfeer-{geverszaal,assendelftzaal,deutzzaal,bar}.{avif,jpg}` — thumbnails, 640px breed
- `sfeer-{…}-groot.{avif,jpg}` — dezelfde foto's op 1800px, voor de lightbox

De hero gebruikt `<picture>` met art direction: brede foto op desktop, staande op mobiel.
Per foto drie formaten, browser kiest zelf (AVIF → WebP → JPEG).

De sfeergalerij gebruikt alleen AVIF → JPEG (geen WebP): `sips` op dit systeem kan geen
WebP schrijven, alleen AVIF en JPEG. Bij nieuwe sfeerfoto's dezelfde tweeslag aanhouden,
tenzij er een tool bij komt die ook WebP kan.

## Lightbox

De zalensectie opent in een lightbox: klik op een thumbnail toont de `-groot`-versie,
met vorige/volgende, pijltjestoetsen, Escape, vegen op mobiel en klik naast de foto.
Onderin `index.html` staat daarvoor het enige stukje JavaScript van de site (vanilla,
geen dependencies). Aandachtspunten bij wijzigen:

- De thumbnails zijn `<a href="…-groot.jpg">`, geen knoppen. Zonder JavaScript opent de
  foto daardoor alsnog; de `href` is tegelijk de JPEG-bron voor de lightbox. Blijf die
  twee gelijk houden, en laat de `e.preventDefault()` in de klik-handler staan.

- Het `<picture>`-element wordt bij elke navigatie opnieuw opgebouwd; alleen de `srcset`
  van een bestaande `<source>` aanpassen wordt niet in elke browser opnieuw geëvalueerd.
- Sluiten op achtergrondklik kijkt óók naar `pointerdown`. Zonder die controle sluit de
  klik waarmee de lightbox net geopend werd 'm meteen weer, en sluit slepen vanaf de foto.
- Op schermen ≤640px staan vorige/volgende onder de foto (`position:static`) in plaats van
  eroverheen; daarboven zweven ze absoluut aan de zijkanten.

## Conventies

- Geen frameworks, geen build-tools, geen npm. Platte HTML/CSS, met één klein
  vanilla-JS-blok onderin `index.html` voor de lightbox.
- Kleuren en maten via CSS-variabelen in `:root`. Geen losse hex-waarden in regels.
- Lettertypes: Fraunces (koppen, variabele assen SOFT/WONK) + Inter (tekst), via Google Fonts `<link>`, niet via `@import`.
- Palet: steen (`--stone-*`), koper (`--copper*`) als accent, mos (`--moss`) voor de Stayokay-sectie.
- Vorm: `--radius`/`--radius-sm` voor afronding, `--shadow` voor kaartschaduw — geen losse waarden per regel.
- Nieuwe afbeeldingen altijd comprimeren voordat ze in de repo gaan. Originelen zijn 3–45 MB; richtlijn is onder 600 KB per variant.
- Animaties respecteren `prefers-reduced-motion`.
- Mobiel-eerst controleren; het grootste deel van het publiek komt via de telefoon.

## Rechten

Foto's van Taco van der Werf, via Stayokay Heemskerk. Gebruik is toegestaan.
Vermelding staat in de footer en rechtsonder in de hero — laat die staan.

## Nog te doen

- Sectie over de 10 brouwerijen (namen, logo's, links) — nu alleen een korte vermelding van Zeglis (met link) in de tekst; overige 9 volgen zodra bekend
- Echte ticketlink; nu een `mailto:`
- Domein registreren en koppelen aan GitHub Pages
- Exact adres van Slot Assumburg verifiëren voor een eventuele routebeschrijving

## Valkuilen

- `index.html` moet in de root van de repo staan, anders vindt GitHub Pages 'm niet.
- Plak HTML nooit via de browser-editor van GitHub; dat heeft eerder het bestand
  afgekapt en gaf een blanco pagina. Committen vanaf lokaal of via bestandsupload.
