# Mobile Nav Fix — Brief for Claude Design

## Issue
On mobile and tablet widths, the site header shows only the "Tram Dao" logo — the Work / About / Contact / Resume nav links disappear entirely, with no hamburger menu or other way to access them.

**Scope:**
- Affects: Homepage (`/`) and Resume page (`/Resume.dc`)
- Not affected: Case study page (`/code-yellow`) — its nav renders correctly on mobile, which confirms this is a component-specific bug, not a site-wide limitation. The two broken pages likely share one header component that's missing its small-screen treatment; the working case study page probably uses a different one.
- **Breakpoint range affected: ~320px to ~900px wide.** This covers virtually all phones and most tablets (including iPad portrait at 768px). Nav only appears once the viewport reaches ~900–1024px.

**Technical detail:** the nav links aren't just visually hidden — inspecting the page shows they render at 0×0 pixels in this range, so there's no way to reveal or reach them without scrolling to the footer (which only has Contact/LinkedIn/Resume links, not "Work" or "About").

**Impact:** visitors on phones/tablets — likely most people arriving from LinkedIn — can't jump to sections or reach the Resume link from the header. They have to manually scroll the whole page.

## Solution
1. Add a hamburger menu that appears below the ~900px breakpoint, replacing the horizontal nav.
2. Tapping it should reveal Work / About / Contact / Resume as a stacked list or slide-out menu.
3. Apply the same header/nav component to both the homepage and the Resume page, so behavior is consistent across the site (and matches how the case study page already handles it).

## Reference
- Nav should behave consistently with the working example on `/code-yellow`, which shows all four links inline and legible down to 375px — though below ~900px a hamburger is preferred over shrinking the links further, since they'd start crowding at true phone widths.
