# Mobile train row layout design

Date: 2026-01-21

## Context
The mobile view renders the train rows with a single-line flex layout. On narrow screens, the destination column is heavily truncated, making the station name hard to read even though desktop looks good.

## Goals
- Preserve the existing data and markup while improving destination readability on small screens.
- Keep all fields visible (time, platform, destination, stops, arrival, status).
- Avoid device-width breakpoints and let layout respond to the actual row width.

## Non-goals
- No changes to the API, rendering logic, or route selection UX.
- No field removal, abbreviations, or conditional hiding.

## Approach options
1) CSS Grid with named areas and a container query to reflow rows when narrow.
2) Flexbox with wrap and reordering via `order`.
3) Alternate templates using `@container` with more complex markup changes.

## Decision
Use CSS Grid with named areas and a container query. This keeps the DOM unchanged, makes alignment explicit, and avoids global breakpoints.

## Layout details
Default layout uses a single grid row:
- Columns: time, platform, destination (flexible), stops, arrival, status.
- Destination uses `minmax(0, 1fr)` and truncation, so it expands first and truncates last.

Compact layout uses a container query on `.train`:
- Row 1: time, platform, status (scan-friendly header line).
- Row 2: destination spans full width and can wrap to multiple lines.
- Row 3: stops and arrival remain visible to the right side.

## Data flow and components
No JS changes. The existing `.train` markup remains unchanged; only CSS governs layout and responsiveness.

## Risks and mitigations
- Container queries may not be supported in older browsers. Default grid layout remains single-line, matching current behavior.
- Multi-line destination may slightly increase row height. This is acceptable given the goal of improved readability.

## Testing
- Verify on a desktop browser with the devtools mobile emulation at narrow widths.
- Confirm destination names are readable and rows return to single-line layout when width increases.
