# DINO LAB — Project Guide for Claude

## Architecture
This is a **single-file HTML game** (`index.html`, ~4000 lines).
All HTML, CSS, and JavaScript are inline. Do NOT suggest splitting into modules — it would break the game.

## File structure inside index.html
1. `<style>` — All CSS, including CSS custom properties (`:root` dark, `html.light` overrides)
2. HTML body — Header, nav tabs, all page divs (`page-coleccion`, `page-trabajos`, `page-lab`, `page-excavacion`, `page-arena`, `page-jardin`, `page-tienda`, `page-progreso`)
3. `<script>` — All JS: constants, Dino class, render functions, game loop, save/load

## Navigation pages (nav-tab → page-{name})
- `coleccion` — Dino list + selected dino detail + breed panel
- `trabajos` — Work tabs: Mina / Exploración / Arena / Granja
- `lab` — 3-column: Sujetos list | Apply panel | Inventory
- `excavacion` — Fossil excavation grid
- `arena` — Boss Arena fights
- `jardin` — Botanical Garden (8×8 grid)
- `tienda` — Shop: Eggs/Incubator/Upgrades | Altar+Training | Prestige | Deco | Info
- `progreso` — Achievements + Encyclopedia

## Key JS globals
- `misDinos[]` — array of Dino instances
- `dinero`, `esencias`, `ascTokens` — currencies
- `workSlots` — `{mina:{}, exploracion:{}, arena_trab:{}, granja:[]}`
- `incomeAccum` — passive income accumulator (replaces old ranchoAccum)
- `currentLang` — `'es'` or `'en'`; use `T(key)` for translations, `currentLang==='en'?...:...` for inline

## i18n system
- `TRANSLATIONS = {es:{...}, en:{...}}` — both objects must have the same keys
- `T(key, ...args)` — returns translated string; falls back to `es`
- `applyLang()` — walks `[data-i18n]` elements and updates text
- `n_*` keys — notification message keys (e.g. `T('n_no_funds', cost)`)

## Dino class getters
- `d.rc` — rarity of color
- `d.isWorking` — true if in any workSlot
- `d.passiveEarn` — `this.sal / 10` (passive income per second)
- `d.combatPower` — `(sal + fue) × DNA bonus`

## CSS variables (dark/light mode)
- `:root` — dark theme defaults
- `html.light` — light theme overrides
- Key vars: `--bg`, `--p1`, `--p2`, `--tx`, `--a`, `--a2`, `--hdr-bg`, `--grn`, `--rar`, `--leg`, `--ocu`

## Conventions
- Do NOT re-read the file unless strictly necessary — prefer targeted edits with known line context
- Prefer `Edit` tool over `Write` for all changes (sends only the diff)
- All user-visible strings must support both `es` and `en`
- Do NOT add new nav pages without updating both `switchPage()` overrides in the JS
- Save/load via `saveGame()` / `loadGame()` using `localStorage['dinolab_v3']`
