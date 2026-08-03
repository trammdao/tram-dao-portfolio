# Mobile Image Overflow Fix — Brief for Claude Design

## Issue
On the Code Yellow case study page (`/code-yellow`), several images and graphics get **cropped/clipped on mobile** instead of scaling down to fit the screen. Content is cut off at the edges rather than shrinking proportionally.

**Affected elements:**
- **Experiment 01 section** ("Recommending flexible payment options early on") — two overlapping phone mockups. The right-hand phone's content (payment options, Apple Pay/Google Pay section) is cut off at the right edge.
- **Experiment 02 section** ("Converting partial payers into payment plan users") — same two-phone-mockup layout, same clipping pattern.
- **Three-image row** (magazine cover + "Income" bar chart + "Priorities when new medical expenses grow" list) — the magazine cover is cropped on the left edge, and the "Priorities" list is cropped on the right edge. Only the middle "Income" chart displays fully.
- **"Suggest a payment plan to partial payer" annotated callout** — cropped at the top (heading text sliced) and right edge (annotation labels truncated to fragments like "Cl / mon / an" instead of "Close to monthly plan amounts").
- **Engagement funnel chart** — rightmost bar ("Payment plan," labeled "21") appears clipped at the edge of its card.

**Not affected:** the two side-by-side research-board screenshots (brainstorm boards) scale down correctly — they just become too small to read, which is a separate, lower-priority legibility issue, not a cropping bug.

## Root cause (confirmed in the live DOM)
The affected images have **hardcoded fixed pixel widths in CSS** instead of responsive sizing. Examples pulled directly from the page:

| Image | Fixed CSS width | Actual image dimensions |
|---|---|---|
| `alice-slide-4.png` (magazine cover) | `348px` | 2050×1148 |
| `alice-slide-6.png` (Priorities list) | `221px` | 1160×1148 |
| Phone mockup (right-hand phone, Experiment 01/02) | `232px` rendered, positioned at `left: 455px` | 1160×1148 |
| `alice-concepts-tested.png` (payment plan callout) | `436px` | 4390×1696 |

These widths are static — they don't respond to viewport size. Meanwhile, the surrounding card container has **fixed padding (32px each side)**, so as the screen narrows, the space available for images shrinks — but the images themselves don't shrink with it. The container clips whatever spills past its edge (`overflow: hidden`), producing the cropped look rather than a scrollbar or visible overflow.

At a 500px-wide test window (the narrowest this could be verified live), one phone mockup image already extended 187px past the visible edge before being clipped. On real phone widths (375–430px), that gap is larger — matching the cropping seen in on-device screenshots.

## Solution
Replace fixed pixel widths on these images with responsive sizing, e.g.:
```css
max-width: 100%;
height: auto;
```
(or `width: 100%` inside an appropriately sized container), so images scale down to fit their container instead of overflowing and getting clipped.

**Apply to:**
1. Two-phone mockup pairs (Experiment 01 and Experiment 02 sections)
2. Three-image row (magazine cover / Income chart / Priorities list)
3. "Suggest a payment plan" annotated callout graphic
4. Engagement funnel chart — worth re-checking specifically at true phone widths (~375–390px) since it wasn't fully reproducible at the 500px test width

**Secondary, lower-priority issue:** the two research-board screenshots (side-by-side) scale correctly but shrink to the point of being illegible on phones. Consider stacking them full-width on mobile instead of keeping them side-by-side, or making them tap-to-zoom.

## Reference
Cross-check against the homepage and other pages once fixed — the same fixed-width image pattern may exist elsewhere on the site and should get the same responsive treatment.
