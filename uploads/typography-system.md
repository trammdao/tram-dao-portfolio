# Typography system — tramdao.com

Single source of truth for type across all pages. Replaces the range-based values
("26–34px depending on...") with fixed steps so every page pulls from the same scale.

## Font

- **Montserrat** for headlines/titles: Display H1, Case-study hero title, H2 only.
- **Inter** for everything else: H3, Stat number, Body, Small (captions, eyebrow/labels,
  nav links).
- Load weights: **400, 700**, plus **800 reserved for stat numbers only** — for both
  families, since Montserrat covers H1/H2 (which are always 700) and Inter covers
  everything from H3 down to the 800-weight stat number.
- ⚠ The letter-spacing values below (−0.02em, −0.015em) were tuned against Plus Jakarta
  Sans's letterforms. Montserrat sits differently — treat those numbers as a starting
  point and eyeball them again once the swap is live; they may need to loosen slightly.

## Color tokens

| Token | Hex | Used for | Contrast check |
|---|---|---|---|
| `textPrimary` | `#1A1A1A` | Headings, nav, buttons | ~19:1 on white — no concern anywhere on site |
| `textSecondary` | `#4A4A45` | Body copy, captions, meta text | ~8.7:1 on white/cream, ~7:1+ on lavender band — passes AAA |
| `accentColor` | `#7C3AED` | Stat numbers, links, tag/eyebrow labels | ~5.7:1 on white — passes AA, but has little headroom. Don't lighten without rechecking, especially for the 13–15px uppercase labels, which fall *under* the WCAG "large text" threshold and so need the full 4.5:1, not the relaxed 3:1. |

## Type scale

Fixed sizes only — no more "depending on section weight." Styling (weight, case, italic,
tracking) is layered on top as **modifiers**, the same size can serve more than one role.
Where I collapsed an existing range to one number, it's flagged so you can confirm or
override.

### Sizes

| Role | Font | Size | Line-height | Letter-spacing | Default color |
|---|---|---|---|---|---|
| Display / page hero H1 | Montserrat | 56px | 1.12 | −0.02em ⚠ | textPrimary |
| Case-study hero title | Montserrat | 46px | 1.15 | −0.02em ⚠ | textPrimary |
| H2 (page sections + card/project name) | Montserrat | 32px ⚠ | 1.2 | −0.015em ⚠ | textPrimary |
| Stat number | Inter | 32px | 1.1 | −0.02em | accentColor |
| H3 (sections within a card, mobile menu items) | Inter | 24px ⚠ | 1.25 | −0.01em | textPrimary |
| Body (all paragraph text, incl. hero subhead) | Inter | 16px ⚠ | 1.7 | 0 | textSecondary |
| Small (captions, eyebrow/labels, nav links) | Inter | 14px ⚠ | 1.4 | 0 | textSecondary |

### Weight + style modifiers

| Role | Normal use | Bold/emphasis use |
|---|---|---|
| H1 / Case-study title / H2 / H3 | 700, always — headings have no "normal" variant | — |
| Stat number | 800, always — the one intentionally louder weight on the site | — |
| Body | 400, normal case — standard paragraphs and the hero subhead | 700, normal case — inline emphasis (e.g. "0% interest") |
| Small | 400, normal case, no tracking → **caption**: add italic, centered under images | 700, uppercase, 0.06em tracking, accentColor → **eyebrow/tag label** |
| Small (nav link) | 400 → resting nav link | 700 → active/current-page nav link |

⚠ = collapsed from a range in your current spec. Your original ranges and why I picked
this value:

- **Section H2 was 26–34px, card/project name was a separate 22–30px range.** Merged
  into one H2 role at **32px**: a card/project name (e.g. "Recommending flexible payment
  options early on") carries the same weight as a top-level page section header (e.g.
  "The situation," "Identifying the opportunity"), so they should render identically
  rather than as two competing scales that overlapped in the middle (22–30 vs 26–34).
- **H3 is now specifically "sections within a card"** — e.g. "Turning research insight
  into minimal viable test," "Scaling the successful email" — set to **24px**, clearly
  below the new H2 (32px) and clearly above body text (17px). Nothing outside a card
  should use H3; a subsection at the page level (not nested inside a card/project) stays
  H2.
- **Body was two sizes (19px hero subhead / 17px paragraph, weights 400–500)** → merged
  into **one 16px Body role**, styled with just normal (400) and bold (700). The hero
  subhead now gets its distinction from weight, not size — if it still feels too quiet
  next to the H1, the lever to pull is a wider/narrower measure (line length) or color,
  not a new size token. Note: 16px is also your accessibility floor (see rule 1 below),
  so this sits right at the minimum rather than above it — fine on its own, but it means
  no component should ever render body text smaller than this.
- **Small/meta label was 13px, caption was 14px** → merged into **one 14px Small role**.
  Caption and eyebrow/label are now the same base size, differentiated purely by
  modifier (italic vs. uppercase+bold+tracking+color) — same pattern as body.
- **Nav link (14px/500) had its own pairing** → folded into the Small role, using the
  same normal/bold split as everything else instead of a bespoke 500 weight.
- **Mobile menu item (24px/600) had its own pairing** → reuses **H3** (24px/700)
  outright. A mobile nav item is structurally a list of destinations, which is already
  what H3 means elsewhere — no reason for it to carry a unique weight.

Net effect: five weights (400/500/600/700/800) down to essentially **two** — 400 and
700 — plus 800 kept deliberately as the one louder weight, reserved for stat numbers.

## Accessibility rules to hold going forward

1. **16px is the floor for body text.** Never let a paragraph size dip below it, even in
   dense UI or card contexts.
2. **Uppercase + wide letter-spacing (labels) and italic (captions) are for short strings
   only** — a few words, never a full sentence. Both treatments measurably slow reading
   for low-vision and dyslexic readers; keeping them short is what makes them safe to use
   at all.
3. **Any new use of `accentColor` as text needs a contrast check**, not just a visual
   gut-check — it's currently sitting at 5.7:1 on white, which passes but isn't padded.
   This matters most for the 14px Small role's label/eyebrow modifier, since that size
   falls under the WCAG "large text" threshold and needs the full 4.5:1. If a future
   background is introduced that's darker than your cream/lavender range, re-verify
   before shipping.
4. **Line-height minimums:** body ≥1.5 (yours is 1.6–1.7, good), headings can go tighter
   (1.1–1.25) since large text tolerates it — don't apply heading line-heights to body
   text or vice versa.
5. **Don't go below 700 weight for anything set in `accentColor` at small sizes.** Regular-
   weight purple text under ~16px starts to feel thin against the cream background even
   though it technically clears contrast minimums — bold weight keeps it feeling
   intentional, not washed out.

## CSS custom properties (for handoff)

```css
/* Load both families with the weights actually used */
/* Montserrat: 700 (headings only need bold) */
/* Inter: 400, 700, 800 (800 for stat numbers only) */

:root {
  --font-display: 'Montserrat', sans-serif; /* H1, case-study hero title, H2 */
  --font-sans: 'Inter', sans-serif;          /* H3, stat number, body, small */

  --text-primary: #1A1A1A;
  --text-secondary: #4A4A45;
  --accent: #7C3AED;

  --fs-display: 56px;
  --fs-case-hero: 46px;
  --fs-h2: 32px;   /* page sections + card/project name */
  --fs-stat: 32px;
  --fs-h3: 24px;   /* sections within a card + mobile menu items */
  --fs-body: 16px; /* all paragraph text, incl. hero subhead — accessibility floor */
  --fs-small: 14px; /* captions, eyebrow/labels, nav links */

  --fw-normal: 400;
  --fw-bold: 700;
  --fw-stat: 800; /* stat numbers only */

  --lh-tight: 1.12;
  --lh-heading: 1.2;
  --lh-body: 1.7;
  --lh-small: 1.4;

  --ls-tight: -0.02em;
  --ls-label: 0.06em;
}
```
