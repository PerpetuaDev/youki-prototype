# Handoff: Sunlight — sunrise tracking app

> **Live:** [Prototype](https://perpetuadev.github.io/youki-prototype/) · [Widgets](https://perpetuadev.github.io/youki-prototype/widgets.html) · [Onboarding flow](https://perpetuadev.github.io/youki-prototype/?onboarding)
>
> In this repo, `index.html` = "Sunlight Prototype.dc.html" from the original design bundle, and `widgets.html` holds only the final widget deliverables (7a–7d plus the 6c lock screen) extracted from the design-history file. The only additions are the small view-switcher buttons top-left and a `?onboarding` query flag that starts the prototype on the onboarding flow.

## Overview
**Sunlight** (Japanese brand name: **ようき**) is an iOS app that predicts how visible and colourful the local sunrise will be, up to 7 days ahead. It scores each morning 0–100, renders a faithful simulation of the predicted sky as the app's hero surface, suggests promising mornings via evening notifications, and lets the user opt in to a wake-up alarm per day. Free tier covers today + tomorrow and one location; Pro unlocks the 7-day calendar, multiple locations, one-tap wake-ups, and home-screen widgets.

## About the Design Files
The files in this bundle are **design references created in HTML** — interactive prototypes showing the intended look and behaviour, not production code. The task is to **recreate these designs in the target codebase's environment** (SwiftUI is the natural fit for iOS + WidgetKit; otherwise React Native or the team's established stack) using its patterns and libraries. If no codebase exists yet, choose the most appropriate iOS framework and implement there.

- `index.html` (originally "Sunlight Prototype.dc.html") — the complete app: all screens, light/dark themes, English/Japanese locales, free/trial/pro plan states. **This is the source of truth.**
- `widgets.html` — the final widget deliverables: turn-7 widgets **7a–7d** plus the **6c lock-screen** pieces, extracted from the design-history file (which is not in this repo).
- `support.js` — prototype runtime only; ignore for implementation (needed next to `index.html` to open it in a browser; `widgets.html` is plain HTML).

## Fidelity
**High-fidelity.** Colors, typography, spacing, copy, and interactions are final design intent. Recreate pixel-perfectly. All measurements below are CSS px at a 340×722 iPhone frame (roughly iOS points).

## Design Tokens

### Themes
| Token | Light | Dark |
|---|---|---|
| Background `--bg` | `#f6f1e8` | `#141110` |
| Ink `--ink` | `#1b1712` | `#f3ece1` |
| Secondary text `--sub` | `rgba(27,23,18,.55)` | `rgba(243,236,225,.55)` |
| Hairline `--line` | `rgba(27,23,18,.08)` | `rgba(255,255,255,.09)` |
| Card fill `--card` | `rgba(27,23,18,.05)` | `rgba(255,255,255,.06)` |
| Accent (text) `--accent` | `#c37a2c` | `#f0b968` |
| Bottom bar bg `--barbg` | `rgba(246,241,232,.92)` + blur(10px) | `rgba(20,17,16,.92)` + blur(10px) |

### Fixed brand colors (identical in both themes)
- Interactive accent (buttons, toggles ON, armed dots, PRO badge): `#e0913a`
- Logo disc: `#f0a13f` with inset ring `rgba(255,236,190,.6)` (`box-shadow: inset 0 0 0 2.5px`; ring scales with disc size)
- Sheet scrim: `rgba(10,8,10,.4)` (paywall `.45`)

### Typography
- Family: **Manrope** (300/400/500/600/700/800) with **Zen Kaku Gothic New** fallback for Japanese; single stack `'Manrope','Zen Kaku Gothic New',sans-serif`.
- Scale: score 70px/300 (home) · 44px/300 (small widget) · 42px/300 (medium widget) · sheet titles 17px/700 · full-settings title 22px/700 · row labels 14px/600 · list values 13px tabular-nums · sub/captions 11–11.5px · section headers 10.5px/700, uppercase, letter-spacing .09em · micro-labels 9.5px/700, letter-spacing .06–.09em.
- Numeric times use `font-variant-numeric: tabular-nums`.
- Japanese wordmark tracking: `-0.05em` (EN `-0.01em`); JP brand size 14.5px vs EN 16px in the bottom bar.

### Radii & spacing
- Sheets: 26px top corners; cards/inputs 14–18px; buttons/pills 999px; widget corners 26px.
- Screen padding: 24–30px horizontal. List rows: 13–14px vertical padding with 1px hairline dividers.
- Milestone rows (home): `padding: 11px 10px; margin: 0 -10px; border-radius: 12px` so the selected-state fill extends past the text column.

### Sky gradients (the product's core visual)
Sky is a faithful vertical gradient per condition; the sun is a radial glow cresting the bottom edge (never a disc); overcast/muted hide or dim the glow. Cloud layers are blurred ellipses drifting via a 34–52s `floatCloud` keyframe; glow pulses via 7s `sunGlow`.

Dawn gradients (180deg, top→bottom):
- **vivid**: `#6e7f99 → #8b89a2 16% → #a68aa0 32% → #c28d97 46% → #de8d7d 60% → #f29a72 72% → #f9a55c 83% → #ffbe5e 93% → #ffd27a`
- **clear**: `#2e4157 → #48607a 20% → #748ba0 42% → #a7b6bc 60% → #dcc68f 76% → #f0b656 89% → #f7a844`
- **overcast**: `#9791a6 → #a89cb2 28% → #b6a5b6 52% → #c9adb4 70% → #eebd80 86% → #f4a94f` (no sun glow)
- **pastel**: `#46536e → #687792 30% → #8b96ab 46% → #c2b49f 68% → #ecd39a 85% → #ffd98a`
- **muted**: `#5a6474 → #79808f 30% → #8a8ea0 48% → #a89e9e 68% → #c4ab92 86% → #d9b183` (glow at 55% strength)

Milestone-preview gradients (good weather / bad weather = overcast|muted):
- **first light**: `#17233a → #26344e 30% → #41506a 58% → #7c7484 82% → #c39d68` / `#232733 → #383e4d → #565c6b → #82828c → #a3967f`
- **golden hour (AM)**: `#4a5e7d → #8d7f96 28% → #d98f75 56% → #ffb45e 80% → #ffd98a` / `#8f8ba0 → #a99fae → #c3aaa8 → #e8b87e → #f2a952`
- **daylight**: `#3d7ec4 → #6ba3d8 34% → #a8cbe8 62% → #d8e8f2 84% → #eef2ec` (glow near top; none when bad) / `#8b95a3 → #a5adb8 → #c2c7cd → #d9dbdb → #e6e2d9`
- **golden hour (PM)**: `#5b7396 → #9d86a2 28% → #e0956f 56% → #ffae54 80% → #ffd27a` / `#7d8398 → #9b96a4 → #bfa393 → #dba76c → #e69a4e`
- **sunset**: `#2e3d5c → #5c5a86 28% → #a86a8a 54% → #e87e5f 78% → #ff9e46` / `#3e4457 → #5f6274 → #837a84 → #a88a78 → #c08858`

Predicted-colour ramps (horizontal, 90deg — shown as a 12px bar on home and in widgets):
- vivid: `#6e7f99, #a68aa0, #de8d7d, #f9a55c, #ffd27a`
- clear: `#2e4157, #748ba0, #dcc68f, #f7a844, #ffe9ac`
- overcast: `#9791a6, #b6a5b6, #c9adb4, #eebd80, #f4a94f` (rendered at 55% opacity)
- pastel: `#46536e, #8b96ab, #d9c9a0, #ffd98a`
- muted: `#5a6474, #8a8ea0, #b5a8a2, #d9b183`

### Iconography
- Line icons, 1.6px stroke, round caps, ~19px in the 44×44 bottom-bar buttons: expand (two arrows), calendar, settings (two-slider filter).
- Location marker: **drawn double ring** SVG (concentric circles r=5 and r=1.9 in a 12-viewbox, stroke 1.1, currentColor) — 12px in the app sky header, 10px on widgets. The locations list uses the ◎ glyph at 45% opacity; "Use my location" uses ◉ in accent.
- Toggles: 42×25px pill, knob 20px white; ON = `#e0913a`, OFF = `--line`.

## Screens / Views

### 1. Home
- **Status bar**: clock (driven by app time), white with text-shadow, over the sky.
- **Sky panel** (top, 266px tall, bottom corners 30px): condition gradient + clouds + sun glow. Location label centered near top (12px/600 white .85, double-ring icon). Tap sky → expands to full screen (0.6s cubic-bezier(.4,0,.2,1)) revealing the **colour analysis** overlay.
- **Score block** (left): score 70px/300 (50% opacity when q<50), condition word 15.5px/600 below ("Exceptional", "Clear", "Overcast", "Partly clear", "Vivid", "Poor visibility").
- **Next-event stack** (right, right-aligned, no container): event time 24px/500 · event name 10.5px/600 in accent + countdown in `--sub` ("Sunrise in 53 min" — no separator dot; overcast appends "· behind cloud") · **Wake me** pill button (1.5px `--line` border, accent text; armed state: `#e0913a` fill, white text, "Alarm set ✓").
  - **Event-adjusted logic**: the stack always shows the *next* milestone relative to the current clock — first light → golden hour → sunrise → PM golden hour → sunset, wrapping to tomorrow's first light. Countdown formats: `in N min` under an hour, `in Nh Mm` above.
  - When the previewed/next event is Daylight, PM golden hour, or Sunset, the button reads **"Remind me" / "Reminder set ✓"** instead of Wake me.
- **Predicted colour**: micro-label + 12px ramp bar (per-day gradient above).
- **Milestone list**: 6 tappable rows (First light, Golden hour, Sunrise, Daylight, Golden hour PM, Sunset), each: 9px dot (colour-coded; hollow 1.5px ring for passive rows, `#e0913a` for golden hours, accent for sunrise) + label 13.5px + time 13px tabular right-aligned. Sunrise row is 600-weight, others 500.
  - **Tap a row** → "milestone preview": row gets `--card` fill (300ms), the sky cross-fades (0.7s) to that moment's projected gradient (weather-adjusted, from the preview table above), and the next-event stack recomputes from that moment (e.g. tap Sunrise → "Golden hour in 12h"). Daylight previews 12:30. Tap again to deselect. Sunrise preview keeps the base dawn sky.
- **Bottom bar** (60px, translucent + blur, hairline top): logo (15px disc + wordmark) left; three 44×44 icon buttons right (expand sky, calendar, settings).

### 2. Expanded sky — colour analysis
Full-bleed sky; glass card pinned to bottom (16px margins, `rgba(14,10,18,.6)` + blur(18px), 1px `rgba(255,255,255,.15)` border, radius 26px): grab handle, "Colour analysis" 17px/700 + "96/100" pill (`rgba(255,255,255,.12)`), 1–2 sentence analysis text 13.5px/1.65, 10px ramp bar with first-light/sunrise/blue-hour-end times beneath, then a 3-column footer (Golden hour · Cloud % · UV). Tap sky to collapse.
Analysis copy is generated per-day (see Claude integration); canned fallbacks exist per condition.

### 3. Calendar sheet ("This week")
Bottom sheet. 7 rows: day name (+ 6px selected dot `#e0913a`, + 8px hollow armed ring), date, confidence label (High/Medium/Low — days 1–3 high, 4–5 medium, 6–7 low), colour dot + score 14px/700. **Free plan**: rows 3–7 at 55% opacity with a lock icon; tapping opens the paywall. Selecting a row sets the app's active day and closes. "About our forecasts" info row expands an explainer.

### 4. Locations sheet
Bottom sheet: search field (14px radius outline), "◉ Use my location" row in accent, saved locations (Yokohama, Tokyo, Kamakura, Hakone — JP: 横浜/東京/鎌倉/箱根) with ✓ on the active one, "＋ Add location" (lock icon + paywall when free).

### 5. Settings mini sheet
Plan row (label + "Upgrade" button when not Pro) · Morning suggestions toggle · Sunset alerts toggle · "All settings ›".

### 6. Full settings (full-screen)
Sections: **Account & subscription** (account, plan, manage, restore) · **Alarm** (morning suggestions toggle, suggestion threshold slider at 70 with gradient track `#8a90b0→#f9a55c→#ffd27a`, wake timing "Golden hour start", sound "Dawn chorus", weekend lie-in toggle, sunset alerts) · **Appearance** (theme Light/Dark pills, language English/日本語 pills, app icon light/dark pickers, Home-screen widgets row with PRO badge) · **Data & privacy** (AI analysis credits 42/60 with progress bar, notifications, location access, data controls) · **About** (terms ↗, privacy ↗, version).

### 7. Paywall sheet
Logo disc + "sunlight Pro" 19px/700, subtitle "See the whole week coming.", 4 benefit rows (7-day forecast, one-tap wake-ups, colour analysis, widgets), two plan cards (annual $19.99/yr "2 months free" — default selected with `#e0913a` outline — and $1.99/mo), full-width CTA "Start 7-day free trial" (52px, `#e0913a`), restore/terms footnote. JP pricing: ¥2,800/年 · ¥280/月.

### 8. Onboarding (4 steps, dot progress)
1. Logo 72px + wordmark + "Never miss a beautiful sunrise" + value prop.
2. Location: "Use my location" (accent-outlined) / search field.
3. "We suggest — you choose": mock notification card (logo, 21:30, "Tomorrow looks like an 88 — sunrise 5:51. Wake up for it?", Wake me / Skip buttons).
4. Pro pitch + trial note. CTA advances; final CTA "Start watching the sky"; Skip under every step.

### 9. Widgets (see `widgets.html`)
All widgets: sky or dark ground as full surface, 26px corners, Manrope, no sun glow (legibility), location = double-ring + "YOKOHAMA" 9.5px/700 spaced caps.
- **7a Dawn forecast (2×2, 158px)**: always tomorrow's predicted dawn gradient. Score 44px/300 top (optically outdented −2.5px), condition word, footer stacked: "Golden hour" 12px/700 + countdown 11px/600 .8 opacity.
- **7b Live sky (4×2, 338×158)**: surface = current sky simulation. Score 42px/300 + one-word condition top-left; "◎ YOKOHAMA" bottom-left; next event bottom-right, right-aligned: name 12px/700 + countdown 26px/300. **On the daylight (pale) sky use dark ink `#16354e` with light text-shadow `rgba(255,255,255,.35)`;** on dark skies white with `rgba(0,0,0,.3)` shadow.
- **7c Live sky (2×2)**: small 7b — score top, stacked next event bottom-left.
- **7d Colour bar (2×2 + 4×2, minimal)**: flat `#141110` ground, ink `#f3ece1`, condition word in `#f0b968`. Score top-left; event ("Golden hour / in 31 min") above a full-width 12px ramp bar along the bottom. Medium adds the event top-right and times under the ramp ("First light 5:14 · Golden 5:24 · Sunrise 5:51", 9.5px/600 55%).
- **Lock screen** (6c): monochrome inline "5:51 · 96 Exceptional" and a circular dial with dashed sun-position arc + score.

## Interactions & Behavior
- **Sheets** (calendar, locations, settings mini, paywall): open over a scrim; close ONLY by tapping the backdrop (or swipe-down in native). No close buttons.
- **Sky expand/collapse**: tap anywhere on the sky; height animates 266px ↔ full, 0.6s `cubic-bezier(.4,0,.2,1)`, corner radius 30px → 0.
- **Milestone preview**: see Home. Fill transition 300ms; sky cross-fade 0.7s ease (overlay layer fading over the base dawn sky).
- **Wake/Remind arming**: per-day boolean; button toggles instantly. Armed days show a hollow ring in the calendar.
- **Plan gating**: free → days 3+ locked in calendar, add-location locked, widgets row locked; all route to the paywall. Trial behaves as Pro with "Free trial · 5 days left" label.
- **Theme/language**: instant swap, whole app; brand wordmark swaps sunlight ↔ ようき.
- **Onboarding**: linear, skippable at every step; finishing or skipping lands on Home.

## State Management
- `day` (0–6 selected forecast day) · `armed{}` (per-day alarm/reminder) · `previewEvt` (null | firstLight|golden|sunrise|daylight|goldenPM|sunset) · `view` (home | expanded | settings | onboarding) · sheet booleans (calendar, locations, settings, paywall) · `loc` (active location) · `theme`, `lang`, `plan` · settings toggles (smartAlarm, sunsetAlerts, weekend) · `paySel` (yr|mo) · onboarding step.
- Derived per render: next event from clock time (or previewed moment), countdown string, sky phase/gradient, wake vs remind button label.

## API / Integration Points
1. **Weather & astronomy data**: per location + day — sunrise/first light/golden hour (AM+PM)/sunset/blue-hour-end times, cloud cover %, UV, visibility → drives quality score (0–100), condition class (vivid/clear/pastel/muted/overcast), sky gradient choice, and ramp.
2. **Claude colour-analysis**: the expanded-sky card's sentence is generated per day+language. Prompt pattern used in the prototype: *"You are the colour analyst inside a sunrise-forecast app. Forecast for {day}: quality score {q}/100, condition '{type}', cloud cover {cloud}, UV {uv}, sunrise {time}, golden hour {range}. In {language}, write one short sentence (under 18 words) about the colours to expect. Plain-spoken and calm, no flowery or dramatic language."* Cache per day+locale; fall back to canned per-condition copy. Metered as "AI analysis credits" (60/month) in settings.
3. **Subscription state**: trial/free/pro drives all gating above; StoreKit products = annual ($19.99, "2 months free") + monthly ($1.99); 7-day trial.
4. **Notifications**: evening suggestion (~21:30) when tomorrow ≥ user threshold (default 70); optional sunset alerts; alarm fires only for armed days at "wake timing" (default golden-hour start).
5. **Widgets**: WidgetKit timelines — dawn-forecast (static per evening) vs live-sky (time-driven phase changes).

## Localization
Full EN/JP string tables live in the prototype's logic (objects `en` and `jp`, ~70 keys). Japanese uses the same layout with adjusted wordmark size/tracking. Countdown format JP: 「あとN分」/「あとN時間M分」.

## Assets
No bitmap assets. Everything is code-drawn: gradients, blurred-ellipse clouds, radial sun glow, inline SVG icons, CSS logo disc. Fonts from Google Fonts (Manrope, Zen Kaku Gothic New).
