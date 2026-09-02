# Bier op 't Slot Festival — website

Eenpagina-site voor een bierfestival. Statische HTML, gehost op GitHub Pages.
Doelpubliek: bezoekers uit de regio Kennemerland. Taal: Nederlands.

## Het evenement

- Zondag 7 maart 2027, 14:00–22:00 uur
- Slot Assumburg, Heemskerk
- Georganiseerd door Stayokay Heemskerk i.s.m. Brouwerij Zeglis (https://brouwerijzeglis.nl/)
- 10 lokale en nationale brouwerijen; tot nu toe alleen Zeglis met naam bevestigd
  (bewust zonder hyperlink in de lopende tekst)
- Entree: €12,50 (regulier) — inclusief 2 muntjes à €3 per stuk en een proefglaasje (mag je houden)
- De eerste 150 boekingen betalen €10 entree (zelfde inhoud: proefglaasje + 2 muntjes)
- Bij die eerste 150 boekingen: ook 25% korting op een overnachting in het kasteel, alleen geldig op 7 maart 2027 en onder voorbehoud van beschikbaarheid
- Adres: Tolweg 9, 1967 NG Heemskerk (geverifieerd via stayokay.com en het
  rijksmonumentenregister, dat het kasteel op hetzelfde adres vermeldt)
- Contact: info@bierophetslot.nl
- Domein: bierophetslot.nl, geregistreerd bij mijn.host op naam van NedBytes
  (tenaamstelling nog over te zetten naar Stichting Stayokay als zij dat willen)
- E-mail: info@bierophetslot.nl, mailbox in het pakket Personal bij mijn.host
- Stayokay Heemskerk: https://www.stayokay.com/nl/hostel/heemskerk

## Structuur

Alles staat in de root, geen build-stap, geen dependencies.

- `index.html` — de hele site: HTML + CSS in één bestand, CSS in een `<style>` in de `<head>`
- `hero-wide.{avif,webp,jpg}` — 2000×1333, liggende foto van het slot, voor schermen ≥700px
- `hero-tall.{avif,webp,jpg}` — 1200×1800, staande foto van de toren, voor schermen <700px
- `sfeer-{geverszaal,assendelftzaal,deutzzaal,bar}.{avif,jpg}` — thumbnails, 640px breed
- `sfeer-{…}-groot.{avif,jpg}` — dezelfde foto's op 1800px, voor de lightbox
- `stayokay-logo.png` — 400px breed, met transparantie; staat linksboven over de hero,
  op dezelfde plek als in de header van stayokay.com. Bewust géén link, omdat de hero
  verder linkvrij is gehouden.
- `luchtfoto.{avif,jpg}` — 1600px, volle band tussen de tickets en de zalensectie;
  laat het terrein zien voordat de pagina naar binnen gaat.

De hero gebruikt `<picture>` met art direction: brede foto op desktop, staande op mobiel.
Per foto drie formaten, browser kiest zelf (AVIF → WebP → JPEG).

De sfeergalerij gebruikt alleen AVIF → JPEG (geen WebP): `sips` op dit systeem kan geen
WebP schrijven, alleen AVIF en JPEG. Bij nieuwe sfeerfoto's dezelfde tweeslag aanhouden,
tenzij er een tool bij komt die ook WebP kan.

Let op bij `sips -s format avif`: zonder `-s formatOptions` gebruikt het de hoogste
kwaliteit, wat bij detailrijke foto's een AVIF oplevert die groter is dan de JPEG —
precies het omgekeerde van wat de `<source>` moet doen. Geef dus altijd een niveau mee
(`high`, of een getal 0–100) en controleer dat de AVIF kleiner uitvalt dan de JPEG.

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

## Ouderdom van het kasteel

Op de site staat bewust "een plek waar al bijna 700 jaar een kasteel staat", niet
"ons 700 jaar oude kasteel". Volgens het rijksmonumentenregister dateert de oudste
bouwfase van het huidige kasteel uit de 15e eeuw en kreeg het zijn vierkante vorm pas
in 1708–1719; de plek zelf wordt al in 1335 genoemd. De formulering slaat dus op de
locatie, niet op de muren. Stayokay zelf spreekt van een "13e-eeuws kasteel" — dat
wordt door het register niet gedekt. Meta-description en lopende tekst moeten hetzelfde
verhaal vertellen; pas ze samen aan.

## Rechten

Foto's van Taco van der Werf, via Stayokay Heemskerk. Gebruik is toegestaan.
Vermelding staat in de footer en rechtsonder in de hero — laat die staan.

Het Stayokay-logo is merkmateriaal van Stayokay; aangeleverd door de organisatie.
Niet uitrekken of verkleuren, en de verhouding intact laten.

Van de luchtfoto (`luchtfoto.*`) is de maker nog niet bekend — die staat daarom
zonder vermelding op de pagina. Navragen bij de organisatie voordat de site live gaat.

## Nog te doen

- Sectie over de 10 brouwerijen (namen, logo's, links) — nu alleen een korte vermelding van Zeglis in de tekst; overige 9 volgen zodra bekend
- Echte ticketlink; nu een `mailto:`
- Tenaamstelling domein overzetten naar Stichting Stayokay, als zij dat willen
- DMARC terug naar `p=reject` zodra de mail bewezen goed doorkomt; staat nu op `p=none`
- E-mailpakket opzeggen vóór augustus 2027 (gaat dan van €0,99 naar €1,99 per maand)
- Maker van de luchtfoto achterhalen en zo nodig een vermelding toevoegen

## DNS

Het domein staat bij mijn.host; DNS-beheer daar. De zone bevat naast elkaar:

- vier A- en vier AAAA-records op `@` naar GitHub Pages
- `www` als CNAME naar `albertosturkenboom.github.io`
- MX naar `mx1`/`mx2.mijn.host`, plus SPF, DKIM (selector `x`) en DMARC voor de mail
- `webmail`, `mail`, `autodiscover` en `autoconfig` wijzen naar `217.180.14.67`
  (h67.mijn.host) — dat hoort zo, dat is hun webmail en de autoconfiguratie voor
  mailclients. Alleen op `@` mocht dat IP niet staan.

Valkuil bij dit domein: mijn.host koppelde het domein aanvankelijk aan hun eigen
webserver en zette `217.180.14.67` steeds terug op `@`, ook na verwijderen. Hun paneel
weigert bovendien dubbele records binnen één RRset, dus je kunt het record ook niet naar
een GitHub-adres wijzigen. Support heeft die koppeling verwijderd — maar nam daarbij ook
de acht GitHub-records mee. Komt dat IP ooit terug op `@`, dan is de koppeling opnieuw
gelegd en moet support er weer aan te pas komen.

## Valkuilen

- `index.html` moet in de root van de repo staan, anders vindt GitHub Pages 'm niet.
- `CNAME` bevat `bierophetslot.nl` en stuurt GitHub Pages aan; niet weghalen of hernoemen.
- Plak HTML nooit via de browser-editor van GitHub; dat heeft eerder het bestand
  afgekapt en gaf een blanco pagina. Committen vanaf lokaal of via bestandsupload.
