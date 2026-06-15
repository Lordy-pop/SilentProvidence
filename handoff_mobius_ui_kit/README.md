# Handoff — Project Mobius UI Kit + Main Menu (MOB-108 / MOB-109)

A reusable button/focus/panel kit and a skinned Main Menu for **"Silent Providence"** — a cold, quiet sci-fi co-op runtime. Target engine: **Unity UI Toolkit (UXML + USS)**.

## About these files
The visual source of truth is an **HTML/CSS prototype** (`Mobius Kit.dc.html`, in the project root) — a high-fidelity design reference, not production code. This folder translates that reference into Unity-ready building blocks: 9-slice sprites, a complete USS stylesheet, and two example UXML screens. **Recreate the look in Unity UI Toolkit using the patterns below** — do not try to port HTML/CSS literally (UI Toolkit has no clip-path, box-shadow, or text-transform; see Caveats).

Fidelity: **hi-fi.** Colours, type, spacing, and states are final. Match them.

## File manifest
```
handoff_mobius_ui_kit/
├── README.md            ← this file
├── MobiusKit.uss        ← tokens + every component, state and theme
├── MainMenu.uxml        ← MOB-109 screen, wired class names
├── Settings.uxml        ← orange-accent theme demo (toggle/meter/segmented)
├── Lobby.uxml           ← crew muster screen (reuses the kit + .mob-crew-row)
└── Sprites/
    ├── btn-frame.svg     ← 9-slice button frame (top-right notch)        — WHITE, tint per state
    ├── ctrl-frame.svg    ← smaller-notch frame for toggle/segments       — WHITE, tint per state
    ├── panel-frame.svg   ← 9-slice panel (notch + baked gradient/vignette)— WHITE, tint to panel colour
    ├── mark-triangle.svg ← THE signature inverted triangle               — WHITE, tint red/orange
    └── mark-cross.svg    ← registration / coordinate mark                — WHITE, tintable
```

## Prerequisites / setup
1. **Unity 2022 LTS or newer**, UI Toolkit runtime (`com.unity.ui` is built in).
2. **Vector Graphics package** (`com.unity.vectorgraphics`) to import the SVGs — or pre-rasterise them to PNG. Import each SVG as **Sprite (2D and UI)**. For the framed sprites set **Mesh Type = Full Rect** and the 9-slice **Border** to the values in the table below (Sprite Editor).
3. **Fonts** — import **Orbitron** (SemiBold) and **JetBrains Mono** (Regular + Bold) as **Font Assets** (TTF → right-click → Create → Text → Font Asset, or the legacy TTF). Place them so the USS `url()` paths resolve (`Fonts/Orbitron-SemiBold.ttf`, `Fonts/JetBrainsMono-Regular.ttf`) or edit the two `-unity-font-definition` lines in `MobiusKit.uss`.
4. Put `MobiusKit.uss` and the `Sprites/` + `Fonts/` folders together; the USS uses **relative `url()`** paths.
5. The root element of every screen carries **`class="mobius-root"`** — it defines all the design tokens (custom properties). Children read them via `var(--mob-…)`.

### 9-slice border values
| Sprite | left | top | right | bottom | used by |
|---|---|---|---|---|---|
| `btn-frame.svg`   | 8  | 16 | 16 | 8  | all `.mob-btn` |
| `ctrl-frame.svg`  | 6  | 8  | 8  | 6  | `.mob-toggle`, `.mob-seg` |
| `panel-frame.svg` | 10 | 20 | 20 | 10 | `.mob-panel` |

> The notch always lives in the **top-right corner cell**, so it stays a constant pixel size at any control width/height. Don't let the slice shrink below these values.

---

## How the notch works (read this first)
UI Toolkit cannot clip arbitrary shapes. The signature top-right corner notch is baked into a **white 9-slice sprite**. Each control draws that sprite as its `background-image` and recolours it with **`-unity-background-image-tint-color`**. So:

- **One sprite → ~24 button states**, each just a different tint. No redrawing.
- The **panel** reuses the same grammar at a larger scale (its sprite also bakes a subtle top-light gradient + vignette for depth — tint it with the dark panel colour).
- **Toggle / segmented** controls use the smaller-notch `ctrl-frame.svg`.

### Caveats vs. the HTML reference
- **No `text-transform`** → author all labels UPPERCASE in the UXML text (already done in the examples).
- **No `box-shadow` / glow** → the soft glows in the mock are dropped. Focus is a crisp border ring (below). If you want the glow, add a faint blurred sprite behind the control; treat it as optional polish.
- **`letter-spacing` is px** in USS (the HTML used `em`). Values here are pre-converted.
- **`clip-path` doesn't exist** → all notches come from sprites, never CSS.

---

## Design tokens
All defined on `.mobius-root` in `MobiusKit.uss`. Use the `var()` names, not raw hex, in your own code.

### Neutrals (cool, desaturated blue-grey)
| Token | Hex | Use |
|---|---|---|
| `--mob-base` | `#0B0F16` | screen background |
| `--mob-base-deep` | `#080B11` | menu stage behind starfield |
| `--mob-panel` | `#1E2735` | panel tint (top of gradient) |
| `--mob-panel-deep` | `#161D29` | deep panel / matrix bg |
| `--mob-card` | `#121823` | cards, sub-panels |
| `--mob-line` | `#303C4C` | visible rules/borders |
| `--mob-line-dim` | `#1B2330` | inner dividers |
| `--mob-disabled` | `#222C3A` | disabled fill |

### Text
| Token | Hex | Use |
|---|---|---|
| `--mob-text` | `#EEF3F8` | titles, primary |
| `--mob-text-2` | `#AEBBCB` | subtitles |
| `--mob-text-3` | `#93A6BB` | eyebrows, quiet labels |
| `--mob-muted` | `#5A6A7E` | captions, meta |

### Accents & signals
| Token | Hex | Use |
|---|---|---|
| `--mob-red` / `-hi` / `-lo` | `#ED1F3D` / `#FF3A55` / `#C50E28` | **Primary** (go/active/error family). `-hi` hover, `-lo` pressed |
| `--mob-red-ink` | `#190406` | dark label on red |
| `--mob-destruct` / `-hi` / `-lo` | `#8E2030` / `#AB2A3D` / `#66131F` | **Destructive** (deep blood) |
| `--mob-destruct-ink` | `#F0C3C9` | label on destructive |
| `--mob-blue` / `-hi` / `-lo` | `#84BAD9` / `#9ECCE7` / `#6699BB` | **Secondary** (light steel blue) |
| `--mob-blue-ink` | `#0B121A` | dark label on blue |
| `--mob-quiet` / `-hi` / `-lo` | `#19212D` / `#232E3D` / `#11161F` | **Quiet** fill |
| `--mob-orange` / `-hi` / `-lo` | `#F0820F` / `#FF9A2B` / `#C7610A` | **Orange flavour / Settings accent** |
| `--mob-orange-ink` | `#190E02` | dark label on orange |
| `--mob-focus` | `#FFFFFF` | keyboard focus ring |
| `--mob-active` | `#9FD0F2` | active info / join code |
| `--mob-success` | `#84D6BE` | success status |
| `--mob-error` | `#FF6E6E` | error status/labels |
| `--mob-input` / `--mob-input-line` | `#0E141D` / `#46566A` | input bg / border |
| `--mob-accent` | `#ED1F3D` (→ `#F0820F` under `.theme-orange`) | **themeable** accent: panel edge + mark |

### Colour rules of engagement
- **Red is dominant and oppressive.** It owns "go", active, error, and run-critical/destructive moments.
- **Orange is sparing flavour** — one warm registration mark per screen, a meter head, the occasional accent — and the calmer accent for **non-destructive menus (Settings)** where it *replaces* red. Red and orange never compete on one screen.
- **Focus is always white** — never coloured, so it can't be mistaken for a state.
- **Dark labels ride bright fills** (red, blue, orange). Quiet and Destructive keep light labels.

## Typography
| Role | Family | Size | letter-spacing | Notes |
|---|---|---|---|---|
| Title / wordmark | **Orbitron** SemiBold | 26–29px | 1px | `.mob-display`, `.mob-title`. Titles ONLY — it is not monospaced. |
| Everything else | **JetBrains Mono** | 10–13px | 1.4–3px | labels, codes, buttons, status, body |
| Eyebrow | JetBrains Mono | 10px | 3px | `.mob-eyebrow` |

### Type roles (MOB-197) — completed scale
Orbitron carries titles only; **JetBrains Mono carries everything else** (true monospace → codes and number columns lock to a grid). Author UPPERCASE labels in the UXML (no `text-transform` in USS). Smallest readable size stays **10px** (a11y floor).

| Role | class | Family | Size | letter-spacing | Use |
|---|---|---|---|---|---|
| Title 1 | `.mob-title-1` (= `.mob-title`) | Orbitron SemiBold | 29px | 1px | Primary panel title / screen name |
| Title 2 | `.mob-title-2` | Orbitron SemiBold | 22px | 1px | Section header |
| Title 3 | `.mob-title-3` | Orbitron SemiBold | 17px | 1.4px | Sub-section / card header |
| Subtitle | `.mob-subtitle` | JetBrains Mono | 11px | 0.5px | Supporting line under a title |
| Body | `.mob-body` | JetBrains Mono | 13px | 0.5px | Default reading text |
| Body small | `.mob-body-sm` | JetBrains Mono | 11px | 0.5px | Dense body / secondary copy |
| Label | `.mob-label` | JetBrains Mono | 10px | 2px | Field labels, row labels (UPPERCASE) |
| Eyebrow | `.mob-eyebrow` | JetBrains Mono | 10px | 3px | Kicker above a title |
| Caption | `.mob-caption` | JetBrains Mono | 9px | 1px | Meta / footnotes (≥9px, never smaller) |
| Button | `.mob-button` | JetBrains Mono Bold | 12px | 2px | Button labels (matches `.mob-btn`) |
| Stat / number | `.mob-stat` | JetBrains Mono Bold | 26px | 1px | Readouts, counts, codes (digits align) |
| Mono / debug | `.mob-mono` | JetBrains Mono | 10px | 0.5px | Debug overlays, coordinates, code |

> Caption (9px) is meta-only and may fall below AA for body reading — keep it for non-essential text. Anything a player must read at distance uses Label (10px) or larger.

---

## Gameplay & data tokens (MOB-196)

The menu palette above governs flat, clean menus. These tokens govern **in-world / HUD data**, where the rule is **cool = us / safe, red = danger**. Every status/data hue also carries a **non-colour cue** (icon / shape / placement / motion) so colour is never the only signal. Ready to map to Unity (USS `var()` + a future ScriptableObject token set).

### Player / ally / friendly — LOCKED to steel blue
The player faction hue is the existing `--mob-blue` family. The **player / ally layer never uses decorative red.**

| Token | Value | Use | Non-colour cue |
|---|---|---|---|
| `--mob-ally` | `#84BAD9` (= `--mob-blue`) | Friendly/player markers, ally outlines, own-team UI | Up-chevron / triangle pip, bracket frame |
| `--mob-ally-hi` / `-lo` | `#9ECCE7` / `#6699BB` | hover / pressed on ally controls | — |
| `--mob-ally-ink` | `#0B121A` | dark label on an ally fill | — |

- **Use for:** local player, allies, "safe/us" affordances.
- **Forbidden:** danger, enemy, or destructive meaning — never tint a threat marker with ally blue.

### Data colours — distinct cool hues at a shared lightness
Picked so no single bar shouts over another; disambiguated by icon, not just hue.

| Token | Hex | Use | Forbidden | Non-colour cue |
|---|---|---|---|---|
| `--mob-shield` | `#74C8D6` | shield / armour value | health, ally identity | Hex / shield glyph |
| `--mob-energy` | `#A9D86B` | stamina / energy | success status (use `--mob-success`) | Bolt / spark icon |
| `--mob-xp` | `#B4A6E6` | XP / progression / level | any live combat state | Diamond / level pip |
| `--mob-resource` | `#D9B45C` | resource / currency | warning (warmer; see state table) | Coin / diamond glyph |
| `--mob-data-neutral` | `#8090A4` | inert / no-data / offline | active or filled data | Dashed / empty pattern |

### Health — NO health-bar hue (diegetic), + temp dev bar
The shipping HUD has **no health bar**: damage is shown diegetically (visor crack at escalating intensity + critical-low red edge vignette + warning signs — built in session C). Critical-low and world-threat share the **one danger-red**. The only health *tokens* are for the **temporary development bar** (bottom of screen, slightly curved as if on the visor, plain white fill, with a short-lingering red damage-delta segment marking the chunk just lost, which fades).

| Token | Hex | Use |
|---|---|---|
| `--mob-hp-fill` | `#EEF3F8` | dev bar fill — plain white |
| `--mob-hp-loss` | `#ED1F3D` | dev bar damage-delta (= the one danger-red), brief & lingering |
| `--mob-hp-track` | `#161D29` | dev bar empty track |

> Dev-only. Once the diegetic damage model (session C) is final, this bar becomes a debug toggle. Non-colour cue for the delta: it animates (slides + fades), so it reads even without colour.

### Threat / enemy — the red family, doing double duty
Red is **brand CTA in menus** *and* **threat / danger / critical / enemy in gameplay**. There is **one danger-red**: "an enemy is near" and "you're dying" use the *same* red — leaning into the oppressive tone — disambiguated by **context + non-colour cue**, never by hue.

| Token | Value | Use | Non-colour cue |
|---|---|---|---|
| `--mob-threat` | `#ED1F3D` (= `--mob-red`) | enemy markers, danger, critical | Inverted-triangle warning, edge placement |
| `--mob-threat-hi` | `#FF3A55` (= `--mob-red-hi`) | escalation / pulse peak | faster pulse rate |
| `--mob-danger` | `#ED1F3D` (alias) | critical-low / destructive intent | — |

**Brand red vs threat red — how they don't collide:** they never share a surface. Brand red lives only on **interactive CTAs in flat menus** (a static fill on a notched button). Threat red lives only on the **diegetic HUD/world layer**, which is sparse, carries no CTAs, and always pairs the red with motion (pulse) + threat iconography. A menu never shows a threat marker; the HUD never shows a brand button. Within one screen, only one of the two roles is ever present.

## State colours (MOB-196)

The kit had `success` / `error` / `active` — adding **warning, info, locked**. Each status pairs with a non-colour cue.

| Token | Hex | Use | Text pairing | Non-colour cue |
|---|---|---|---|---|
| `--mob-success` | `#84D6BE` | success / ready / linked | dark ink or own colour | Check / triangle-up |
| `--mob-warning` | `#E7B23F` | caution / attention | `--mob-warning-ink` `#1A1303` | Triangle `!` |
| `--mob-danger` / `--mob-error` | `#ED1F3D` / `#FF6E6E` | error / failure (red family) | `--mob-red-ink` / own colour | Octagon / `✕` |
| `--mob-info` | `#9FD0F2` (= `--mob-active`) | informational note | own colour | `i` in circle |
| `--mob-locked` | `#5A6A7E` (= `--mob-muted`) | unavailable / disabled | own colour | Lock / dash, reduced opacity |

- **Warning vs brand orange:** `--mob-warning` (`#E7B23F`) is yellower and calmer than the brand `--mob-orange` (`#F0820F`). Warning = a status; orange = a deliberate accent/theme. Don't use orange to mean "caution".
- **Contrast:** on `--mob-base`/panels, the status hues meet AA for large text and status labels (≥10px). For body-size status text prefer the lighter members (`--mob-error`, `--mob-active`). `--mob-locked` on dark is intentionally low-contrast (it means *unavailable*) — always back it with the lock/dash cue.

## Surfaces (MOB-196)

The kit had base / panel / card; adding the elevated, modal, and diegetic layers.

| Token | Value | Use |
|---|---|---|
| `--mob-panel-2` | `#26303F` | elevated panel / popover sitting above `--mob-panel` |
| `--mob-scrim` | `rgba(6,9,14,0.72)` | modal overlay / scrim behind dialogs |
| `--mob-screen` | `#0A1418` | in-world device / terminal screen surface (diegetic) |
| `--mob-screen-line` | `rgba(159,208,242,0.07)` | faint phosphor scanline tint on device screens |

---

## Non-colour token scales (MOB-197)

The kit hard-coded these inline; they are now named tokens on `.mobius-root`. Each is ready to map to Unity (USS `var()` + future ScriptableObject token).

### Spacing scale
| Token | Value | Use |
|---|---|---|
| `--mob-space-2xs` | 4px | icon gaps, tight pips |
| `--mob-space-xs` | 8px | inline label gaps |
| `--mob-space-sm` | 12px | control inner gaps, row padding |
| `--mob-space-md` | 16px | card gap, between related blocks |
| `--mob-space-lg` | 24px | block separation |
| `--mob-space-xl` | 32px | major section gap |
| `--mob-space-2xl` | 48px | screen-section rhythm |
| `--mob-screen-margin` | 48px | outer screen padding (menu stage) |
| `--mob-panel-pad` | 28px | default panel inner padding |
| `--mob-list-gap` | 2px | meter / segmented / tight list gap |
| `--mob-grid-gap` | 16px | card / grid gap |
| `--mob-safe-margin` | 18px | safe-area / corner-mark inset |

*Example:* `.mob-panel { padding: var(--mob-panel-pad); }` · a card grid uses `--mob-grid-gap`; a meter uses `--mob-list-gap`.

### Radius / shape scale
Mostly sharp — the signature corner is a **sprite notch** (`--mob-notch`), not a CSS radius.
| Token | Value | Use |
|---|---|---|
| `--mob-radius-sharp` | 0 | default — panels, buttons |
| `--mob-radius-sm` | 2px | inputs |
| `--mob-radius-md` | 4px | dev chips, small controls |
| `--mob-radius-pill` | 999px | dots, pills, toggle knobs |
| `--mob-radius-screen` | 10px | diegetic device/terminal screen corners |
| `--mob-notch` | 16px | signature top-right cut — drives the 9-slice border, not `border-radius` |

### Stroke scale
| Token | Value | Use |
|---|---|---|
| `--mob-stroke-hair` | 1px | structural borders, dividers (subtle) |
| `--mob-stroke-edge` | 2px | leading accent edge / default emphasis |
| `--mob-stroke-active` | 2px | selected / active control border |
| `--mob-stroke-focus` | 2px | white keyboard-focus ring |
| `--mob-stroke-danger` | 3px | critical-alert / damage edge |

### Elevation / depth scale
No real shadow in UI Toolkit — depth is expressed via **surface tint + glow/scrim recipe**.

| Level | Name | Recipe |
|---|---|---|
| 0 | Flat HUD | no tint, no glow — diegetic, on the world |
| 1 | Card / panel | `--mob-panel` (or `--mob-card`) + `--mob-stroke-hair` border + leading accent edge |
| 2 | Floating window | `--mob-panel-2` + hairline border + faint `--mob-glow-cool` halo sprite |
| 3 | Modal | `--mob-scrim` behind + `--mob-panel-2` + `--mob-glow-accent` halo |
| 4 | Critical alert | `--mob-stroke-danger` red edge + pulsing `--mob-glow-danger` (uses motion) |

**Glow tokens** (faked, since no `box-shadow`/blur): `--mob-glow-focus` `rgba(255,255,255,0.32)`, `--mob-glow-accent` `rgba(237,31,61,0.30)`, `--mob-glow-cool` `rgba(159,208,242,0.14)`, `--mob-glow-danger` `rgba(237,31,61,0.45)`. Implement as a blurred/feathered white sprite or an additive overlay VisualElement behind the element, tinted with the token.

### Motion scale
UI Toolkit supports `transition-duration` / `-property` / `-timing-function`. Glow & pulse are faked (see above). The `.reduce-motion` root class cuts durations toward 0 and disables looping pulse/drift; the visor warp/aberration/crack has its own HUD on/off toggle (session C) — atmospheric by default, fully defeatable.

| Token | Value | Use |
|---|---|---|
| `--mob-dur-instant` | 60ms | press feedback |
| `--mob-dur-fast` | 120ms | hover / press tint (matches existing kit) |
| `--mob-dur-base` | 200ms | toggles, selection |
| `--mob-dur-slow` | 320ms | panel / dialog open-close |
| `--mob-dur-cine` | 600ms | theatrical visor / device-screen transitions |
| `--mob-ease-standard` | `ease-out` | hover / press |
| `--mob-ease-enter` | `cubic-bezier(0.16,1,0.30,1)` | open / reveal |
| `--mob-ease-exit` | `cubic-bezier(0.40,0,1,1)` | close / dismiss |
| `--mob-ease-pulse` | `ease-in-out` | error / threat pulse (loops; off under reduce-motion) |

> Rules of thumb: hover/press → `--mob-dur-fast` + `--mob-ease-standard`; open/close → `--mob-dur-slow` with `--mob-ease-enter`/`-exit`; error-pulse and threat → `--mob-dur-base` ping-pong with `--mob-ease-pulse`; the visor/device cinematic transitions → `--mob-dur-cine`.

> **Visual reference:** `Reference/05-token-board-sessionA.jpg` is a one-page board of all Session-A tokens (data + state + surfaces, the temp dev health bar, the spacing/radius/stroke/motion scales, and the completed type scale) rendered in the kit's grammar. The live source is `Token Board.dc.html` in the project root — open it in a browser for the editable version.

---

## Components

### Button — `.mob-btn` + variant
4 variants × 5 states. UXML: `<ui:Button text="HOST SESSION" class="mob-btn mob-btn--primary" />`.

| Variant | class | idle fill | hover | pressed | label |
|---|---|---|---|---|---|
| Primary | `--primary` | `#ED1F3D` | `#FF3A55` | `#C50E28` | `#190406` (dark) |
| Secondary | `--secondary` | `#84BAD9` | `#9ECCE7` | `#6699BB` | `#0B121A` (dark) |
| Quiet | `--quiet` | `#19212D` | `#232E3D` | `#11161F` | `#93A6BB` → `#C3CEDB` |
| Destructive | `--destructive` | `#8E2030` | `#AB2A3D` | `#66131F` | `#F0C3C9` |
| Orange (Settings) | `--orange` | `#F0820F` | `#FF9A2B` | `#C7610A` | `#190E02` (dark) |

States (all variants):
- **Idle** — tinted fill.
- **Hover** (`:hover`) — fill brightens to `-hi`.
- **Focus** (`:focus`) — **white rectangular ring** (2px border on the button box; the notched fill is the sprite). This is the a11y fix; it reads distinctly from hover. *(Optional offset/glow: wrap the button in a VisualElement with 3px padding and move the border to the wrapper, or add a blurred sprite behind.)*
- **Pressed** (`:active`) — fill darkens to `-lo` + `translate: 0 1px`.
- **Disabled** (`:disabled`) — fill `#222C3A`, label `rgba(185,200,215,0.30)`.

> If your Unity version does not re-evaluate `var()` inside `-unity-background-image-tint-color` on pseudo-states, the state rules already set the value via `var(--mob-red-hi)` etc.; swap to the literal hex if you see no change on hover.

### Input — `.mob-input`
`<ui:TextField class="mob-input" />`. Rectangular (no notch). Idle: bg `#0E141D`, 1px border `#46566A`. **Focus**: border → `#ED1F3D`. Text/caret: value text `#9FD0F2`, caret `--unity-cursor-color: #ED1F3D`, selection `rgba(237,31,61,0.72)`. Style targets the inner `.unity-text-field__input` (see USS).

### Panel — `.mob-panel`
`background-image: panel-frame.svg` tinted `--mob-panel`, 9-slice, plus a 2px **left accent edge** = `--mob-accent` (red, or orange under `.theme-orange`). The gradient/vignette is baked into the sprite. Holds from ~392px (Main Menu) to ~520px+ (Lobby) — same sprite.

### Signature mark — `.mob-mark`
The inverted triangle. `background-image: mark-triangle.svg`, tinted `--mob-accent`. Sizes: `.mob-mark` (13px), `.mob-mark--lg` (16px, header), `.mob-mark--sm` (8px, node rules). **This glyph is the brand — use it for every eyebrow lockup and divider node.**

### Status — `.mob-status`
Row of a 7px dot + label. Add a kind class to recolour both: `.is-active` (`#9FD0F2`), `.is-success` (`#84D6BE`), `.is-error` (`#FF6E6E`); default idle is `#93A6BB`. Drive from C# as connection state changes.

### Settings controls (orange theme)
- **Toggle** `.mob-toggle` (+ `.is-on`) with child `.mob-toggle__knob`. Custom — toggle `.is-on` in C#; knob slides via `translate`.
- **Segmented** `.mob-seg` (+ `.is-selected`) — quality picker; selected = accent fill, dark bold label.
- **Meter** `.mob-meter` of `.mob-meter__seg` (+ `.is-filled`) — build N segments in C#, fill the first k. For a real draggable control use `<ui:Slider>` and tint its dragger `--mob-accent`.

### Crew row — `.mob-crew-row` (roster lists)
Roster list item for the Lobby and any future crew/party/friends screen. One row per player: slot number, status dot, name, optional tag, readiness label. State classes recolour the dot + readiness text: **`.is-ready`** (green `#84D6BE`), **`.is-standby`** (orange `#F0820F`), **`.is-open`** (muted). Add **`.is-you`** to highlight the local player (steel-blue tint + left edge); **`.mob-crew-row__tag--host`** for the HOST badge. Wrap N rows in a bordered container (`border-bottom: 0`) — each row draws its own bottom rule. Rows are `focusable`/clickable to toggle ready in C#. See `Lobby.uxml`.

### Ornaments (the "lines & dots" grammar)
Simple, clean, reusable; monochrome by default, one red/orange node carries the weight. Classes in USS: `.mob-noderule` (divider: mark-sm + `.mob-rule` + `.mob-dot`×3), `.mob-tick`/`--major`/`--accent` (measurement scale), `.mob-bracket--tl/tr/bl/br` (frame corners), `.mob-cross`/`--accent` (registration marks). **Use sparingly — reuse, don't multiply.**

---

## Theming: red default → orange Settings
The only change between a standard screen and a Settings screen is the **`.theme-orange`** class on the panel (or any ancestor). It remaps `--mob-accent`, so the mark, panel edge, toggle, meter, and segments all turn orange automatically. Buttons that must be orange use the explicit `.mob-btn--orange` variant (e.g. `APPLY`). See `Settings.uxml`.

## Interactions & state (Main Menu)
Wire in C# (`MonoBehaviour`/`UIDocument` controller):
- **Host** → generate a 6-char code (alphabet `ABCDEFGHJKLMNPRTUVWXY3479`, no ambiguous chars), show it on `#join-code-display` (active/`#9FD0F2`), set status → "HOSTING · AWAITING LINK" (`.is-active`).
- **Join** → read `#join-code`; if `< 6` chars → `#error-label` = "INVALID LINK CODE — NEEDS 6 CHARS"; else status → "LINKING…" (`.is-success`), then "LINKED · SESSION READY".
- **Quit** → status "SHUTDOWN…" (`.is-error`) → back to "STANDBY · LINK IDLE".
- **Leave Run** → "RUN ABORTED" (`.is-error`) → standby.
- Uppercase + filter the join-code field to `[A-Z0-9]`, max 6.

The Lobby screen reuses the same classes (`.mob-btn--primary`, `.mob-panel`, etc.) — no separate pass needed.

## Screens
Each screen ships as one `.uxml` against the shared `MobiusKit.uss` — no per-screen stylesheet.

| Screen | File | Accent | Notable |
|---|---|---|---|
| Main Menu (MOB-109) | `MainMenu.uxml` | red | Host / Join / Quit + status + destructive Leave |
| Settings | `Settings.uxml` | **orange** (`.theme-orange`) | toggle, volume meter, quality segmented, orange APPLY |
| Lobby | `Lobby.uxml` | red | crew roster (`.mob-crew-row`), readiness meter, START RUN gated on all-ready |

**Adding a screen later** is the common case and it's light: mock it against the reference, then drop **one new `.uxml`** into this folder reusing the existing classes. Only add to `MobiusKit.uss` when a genuinely new, reusable component appears (as `.mob-crew-row` was added for the Lobby) — then every future screen inherits it. New sprites are rare. You never redo the font/sprite setup.

## Implementation checklist
- [ ] Install Vector Graphics; import the 5 SVGs as Sprites with the 9-slice borders in the table.
- [ ] Import Orbitron + JetBrains Mono font assets; confirm the two `-unity-font-definition` paths.
- [ ] Add `MobiusKit.uss` to your `UIDocument`; root element gets `class="mobius-root"`.
- [ ] Build `MainMenu.uxml`; verify all 5 button states (hover/focus/press/disabled) and Tab focus shows the white ring.
- [ ] Build `Settings.uxml`; confirm `.theme-orange` flips the accent and `APPLY` is orange.
- [ ] Wire the Main Menu controller (Host/Join/Quit/Leave + status kinds).
- [ ] Visual-check against `Mobius Kit.dc.html` (open in a browser) — it is the canonical reference for spacing, colour, and the full state matrix.
```
```
Questions or gaps → compare directly against `Mobius Kit.dc.html`; every value here is taken from it.
