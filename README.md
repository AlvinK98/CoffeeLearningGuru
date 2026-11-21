# ☕ Cherry to Cup — Specialty Coffee Atlas & Lab

An interactive, single-file learning website that follows coffee from cherry to cup: explore where coffee is grown, log the beans you buy, learn how processing and roasting shape flavor, and brew with a guided ratio calculator and timer.

Everything lives in one HTML file (`specialty_coffee_guide.html`). There is no build step, no backend and no account system.

---

## Features

### 01 — Origin Map & Bean Passport
- **Interactive world map** (D3 + TopoJSON) with the Bean Belt highlighted. Countries are colored by status: in your passport, coffee origin, or non-producing.
- **17 origin profiles** with region, altitude, key varietals, harvest season and typical cup profile: Ethiopia, Colombia, Brazil, Kenya, Indonesia, Guatemala, Costa Rica, Panama, Honduras, Peru, Vietnam, Rwanda, Tanzania, India, Malaysia, Taiwan, Mexico and Yemen.
- **Bean Passport**: record, edit and delete coffees you have bought.
  - Bean/lot name, origin (including custom countries), variety, altitude
  - Process (Washed, Natural, Honey, Anaerobic, Carbonic Maceration, Wet-Hulled)
  - Roast level, purchase date, 1–5 star rating
  - Bag weight (g / kg / oz / lb) and price in 13 currencies (USD, EUR, GBP, MYR, SGD, HKD, NTD, IDR, JPY, CAD, AUD, CHF, THB)
  - Bag photo, automatically resized and stored in the browser
  - Tasting notes picker based on the **SCAA / WCR Coffee Taster's Flavor Wheel (2016)**
- Search and process filters, quick-jump country dropdown, popular-origin pills, and count badges on the map.
- **Undo / Redo** (up to 30 steps) for passport changes.

### 02 — Fermentation & Roasting
- **Post-harvest processing explorer**: Washed, Natural, Honey, Anaerobic & Carbonic Maceration, and Wet-Hulled, each with a step-by-step breakdown, microbial biochemistry notes and a sensory profile (acidity, sweetness, body, clarity).
- **Roast profile explorer**: seven thermal stages from Green Bean through Drying, Maillard, First Crack, Light, Medium and Second Crack/Dark, with bean temperature, chemistry, cup impact and flavor-balance bars.

### 03 — Brewing Lab & Timer
- **Six built-in methods**: V60, Espresso, AeroPress, French Press, Chemex and Moka Pot.
- **Ratio calculator**: dose slider (10–60 g) and ratio slider (1:1.5–1:18) calculate total water, with grind size and water temperature guidance.
- **Live guided timer** with per-step progress bars and target weights that scale with your dose and ratio.
- **Custom recipes**: create, edit and delete your own recipes (name, barista, brewer, grind, temperature, notes and timed pour steps, expressed as × coffee weight or % of total water).
- **Extraction Dial-In Compass**: quick troubleshooting for under-extracted, balanced and over-extracted cups.

### Languages
English, Simplified Chinese (简体中文) and Traditional Chinese (繁體中文), switchable from the header. The choice is remembered.

---

## Getting Started

1. Download `specialty_coffee_guide.html`.
2. Open it in any modern browser (Chrome, Edge, Firefox, Safari).
3. That's it. An internet connection is needed on first load for the CDN libraries, fonts and map data (see below).

To host it, upload the file to any static host (GitHub Pages, Netlify, Cloudflare Pages, or a plain web server).

---

## Tech Stack

| Purpose | Library / Source |
|---|---|
| Styling | [Tailwind CSS](https://tailwindcss.com) (Play CDN) with a custom cream / espresso / terracotta / gold palette |
| Map rendering | [D3 7.9.0](https://d3js.org) (cdnjs) |
| Map geometry | [TopoJSON 3.0.2](https://github.com/topojson/topojson) (cdnjs) and `world-atlas@2` `countries-110m.json` (jsDelivr) |
| Fonts | Fraunces (display) and Inter (body) via Google Fonts |
| Logic | Vanilla JavaScript, no framework |

If the map data fails to load, the app falls back to an offline origin directory (dropdown and pills) so you can still use the passport.

---

## Data & Privacy

All data stays in your browser's `localStorage`. There are no accounts, cookies, analytics or servers.

| Key | Contents |
|---|---|
| `terroir_roast_beans_v1` | Bean Passport entries (including resized photos) |
| `cherry_to_cup_recipes_v1` | Custom brewing recipes |
| `cherry_to_cup_lang` | Selected language |

- First launch seeds three sample beans (Ethiopia, Colombia, Taiwan) so the passport is not empty.
- Clearing your browser's site data permanently deletes everything. Data is not backed up and does not sync between devices or browsers.
- Photos are scaled to a maximum of 720 px and saved as JPEG (quality 0.78) to conserve storage. If storage fills up, the app shows a warning.
- The in-app Privacy Policy and Copyright & Credits are available from the footer.

---

## Project Structure

```
specialty_coffee_guide.html   # The entire app: markup, styles, data, logic, i18n
README.md
```

Inside the HTML file, the main script is organized into numbered sections:

1. **Knowledge datasets**: `COFFEE_ORIGINS`, `TASTING_WHEEL`, `DEFAULT_BEANS`, `FERMENTATION_METHODS`, `ROAST_STAGES`, `BREW_METHODS`
2. **State & storage**
3. **Section navigation**
4. **Origin map & passport**
5. **Currency formatting, modal, form logic, undo/redo**
6. **Fermentation & roasting explorers**
7. **Brewing calculator, recipes & timer**
8. **Initialization**

A second script block at the bottom contains the translation layer (`I18N`).

---

## Customizing

### Add a coffee origin
Add an entry to `COFFEE_ORIGINS`:

```js
"Burundi": {
  region: "East Africa • Kayanza & Ngozi",
  altitude: "1,700 – 2,000 masl",
  varieties: "Red Bourbon",
  harvest: "March – July",
  profile: "Red berry, floral, brown sugar"
}
```

If the country's name in the map data differs from yours, add a mapping to `COUNTRY_NAME_ALIASES`.

### Add a built-in brew method
Add an entry to `BREW_METHODS` with `name`, `defaultDose`, `defaultRatio`, `grind`, `temp`, `guide` and a `steps` array. Each step uses `time` (seconds), `label` (`"m:ss — Name"`), `waterMult`, an optional `isFraction` (true = fraction of total water, otherwise × coffee weight) and `note`.

### Add or change translations
The i18n layer matches English text against dictionaries in `I18N["zh-CN"]` and `I18N["zh-TW"]`, and a MutationObserver translates the page as it re-renders. Any new user-facing string needs an entry in both dictionaries, using the **exact** English text. Pattern-based strings use the `rules` array (regular expressions).

To add a new language, add a new key to `I18N` and a matching `<option>` in the `#lang-select` dropdown.

### Theme
Colors and fonts are defined in the `tailwind.config` block near the top of the file.

---

## Known Limitations

- Requires an internet connection on first load (CDNs, Google Fonts, map data).
- Per-browser storage only: no sync, export or import yet.
- Educational content is general guidance. Flavors, altitudes and harvest seasons vary by farm, year and roaster.

## Ideas for Future Work

- Export / import passport data (JSON or CSV)
- Offline support via a service worker and bundled libraries
- Brew logging linked to passport beans
- Additional origins and a country-detail page for each producer

---

## Credits

- Tasting notes follow the SCAA / WCR Coffee Taster's Flavor Wheel (2016), which belongs to its respective owners.
- World map data: [Natural Earth](https://www.naturalearthdata.com) via [world-atlas](https://github.com/topojson/world-atlas) (public domain).
- Built with D3, TopoJSON and Tailwind CSS. Fonts: Inter and Fraunces (SIL Open Font License).
- All other trademarks and product names belong to their respective owners.

## License

© 2026 Cherry to Cup. All rights reserved. The text, design and code of this site may not be copied or redistributed without permission.
