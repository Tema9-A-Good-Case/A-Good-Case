# A Good Case - Teknisk Dokumentation

## Projektoversigt

Dette projekt er en Astro-baseret webshop re-design for brandet "A Good Case" efter vores figma prototype.
Løsningen er bygget komponentbaseret med fokus på:

1. Genbrugelige UI-komponenter (header, footer, kort, filter, sortering, hero, carousel)
2. Datahentning fra Supabase REST API
3. Responsivt layout for desktop og mobil
4. Enkle, scripts til UI-adfærd (menu, filter, sortering, carousel)

## Teknisk

1. HTML/CSS i `.astro` komponenter
2. Supabase REST endpoint som datakilde

### Layout

`Layout.astro` er den overordnede side-wrapper. Den inkluderer:

1. Global CSS fra `styles/main.css`
2. `Header` komponent
3. `<slot />` til sideindhold
4. `Footer` komponent

Det betyder, at alle sider automatisk far samme top og bund struktur.

### Dataflow

Produktdata hentes fra Supabase via `fetch` i sidefilerne:

1. Forside: henter alle produkter og splitter i to sektioner (`nyheder`, `mest solgte`)
2. Produktliste: henter alle produkter til kortgrid
3. Detaljeside: bruger `getStaticPaths()` til at pre-render hver produktside ud fra `Handle`

## Mappestruktur

src/
components/
header.astro
footer.astro
Menu.astro
hero.astro
Carousel.astro
Marquee.astro
Nyhedsbrev.astro
ProductFilter.astro
sort.astro
card.astro
button.astro
layouts/
Layout.astro
pages/
index.astro
productlist.astro
details/[id].astro

public/
Imgs/

## Side beskrivelser

### 1) Forside (`index.astro`)

Hovedfunktioner:

1. Hero-sektion med stor kampagneoverskrift og CTA
2. Produktsektion "NYHEDER" (de første 4 produkter)
3. Produktsektion "MEST SOLGTE" (de næste 4 produkter)
4. Marquees med budskabstekst
5. Nyhedsbrev-sektion
6. Logo-carousel med auto-scroll og dots
7. Service-ikoner og "Om os" sektion

Teknisk:

1. Data hentes server-side i Astro frontmatter
2. Produktkort renderes via `map`
3. Rabatbadge vises betinget, hvis `product.discount` findes

### 2) Produktliste (`productlist.astro`)

Hovedfunktioner:

1. Intro med overskrift + beskrivende tekst
2. Kontrolrad med filter (venstre) og sortering (hojre)
3. Produktgrid med `Card` komponent

Teknisk:

1. Filter komponent styrer synlighed via `data-type` og `hide` class
2. Sortering komponent reordner DOM-kort ud fra `data-price`
3. Produktcontainer har `id="PList"` til sorteringsscript

### 3) Produktdetalje (`details/[id].astro`)

Hovedfunktioner:

1. Dynamisk route pr. produkt (`/details/[handle]`)
2. Viser brand, produktnavn og produktbillede

Teknisk:

1. `getStaticPaths()` henter alle produkter fra Supabase
2. Astro genererer statiske sider for hver `Handle`

## Komponenter

### `header.astro`

1. Indeholder burger-menu (`Menu` komponent), logo og action-ikoner
2. Viser cart-badge med count
3. Responsivt skalerede ikon og logo-storrelser

### `Menu.astro`

1. Mobilmenu med overlay
2. Under-menu flow for "SPECIALØL"
3. Script styrer open/close/submenu med class toggles

### `footer.astro`

1. Kontakt, abningstider, links og sociale ikoner
2. Grid-layout der kollapser til en kolonne pa mindre skærme

### `hero.astro`

1. Kampagnesektion med billeder, stor typografi og CTA
2. Mobiltilpasning: omorganiserer rækkefolge og skalerer elementer

### `card.astro`

1. Reusable produktkort brugt i produktlisten
2. Indeholder image, rabatbadge, brand, navn, pris og CTA
3. Eksporterer metadata via `data-type` og `data-price` til filter/sort scripts

### `ProductFilter.astro`

1. Viser filterlabels i toplinje
2. "Øltype" åbner dropdown med filterknapper
3. Script filtrerer `.Pcard` elementer via `data-type`

### `sort.astro`

1. Knapper til pris-sortering op/ned
2. Script sorterer kort i `#PList` ved at flytte DOM-elementer
3. Aktiv knap markeres med `is-active`

### `Carousel.astro`

1. Dobbelt logo-track (`trackLogos`) for smooth loop
2. Dots viser aktiv position
3. Auto-advance hvert 2. sekund
4. `transitionend` resetter index for seamless infinit scrolling

### `Nyhedsbrev.astro`

1. Formularlayout med email-input + submit
2. Dekorativ cirkelgrafik overlapper sektionen
3. Mobilversion skifter til vertikal formstruktur

### `Marquee.astro`

1. Kørende tekstbjælke med CSS keyframes
2. Repeated text giver kontinuerlig horizontal bevægelse

### `button.astro`

1. Reusable CTA-knap
2. Kan rendere som `<a>` eller `<button>` afhængigt af `href`
3. Hover-styling med farve og shadow-skift

## Stylingstrategi

1. Global base i `styles/main.css` (reset, font-import, CSS variables)
2. Scoped styles i hver komponent for lokal kapsling
3. Design tokens via CSS variables:
   - `--the-green`
   - `--the-beige`
   - `--the-background`
   - `--font`

## Responsivt Design

1. `1000px` (tablet)
2. `700px` `480px` (mobil-layout)

Eksempler:

1. Produktgrids skifter kolonneantal efter viewport
2. Header-ikoner/logo skaleres ned på mobil
3. Menu og filter/sort kontroller tilpasses touch-visning

## Scriptoversigt

### Filter

1. Lytter på klik i filterknapper
2. Matcher `data-type` mod kortenes `data-type`
3. Toggler `hide` class for visning/skjul

### Sortering

1. Lytter på klik i sorteringsknapper
2. Sorterer kort ud fra numerisk `data-price`
3. Re-append kort i container for ny visuel rækkefolge

### Carousel

1. Holder styr på nuværende index
2. Flytter track med `translateX`
3. Opdaterer aktiv dot
4. Kører autoplay med `setInterval`

## Kørsel og Build i Astro

Kommandoer fra projektroden:

1. `npm install`
2. `npm run dev`
3. `npm run build`

## Bemærkninger

1. Der findes enkelte scripts hvor TypeScript kan advare om `dataset`/`null` checks.
2. De waves vi har i baggrunden af vores prototype i productlisten skulle vi droppe fordi når vi filtreret produkterne ændret det størrelsen af siden og så så det ik godt.
