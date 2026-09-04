# The challenge prompt (as run)

Functional requirements from the public "Silicon Exchange" challenge featured in Alex
Ziskind's video ["I Gave Local AI and the Cloud the Exact Same Job"](https://www.youtube.com/watch?v=ujs0_cpAnaw).
One declared adaptation: the stack was changed from Next.js to Vite + React + TypeScript
(the stack the pipeline currently supports); every functional rule, route, test and
quality-bar requirement was kept. The text below was passed to the pipeline verbatim via
`--request-file`.

```
Build a production-ready, visually stunning front-end web app for a fictional
company: "SILICON EXCHANGE" — a marketplace where people rent out idle GPUs and
AI accelerators by the hour. Renters browse listings, inspect utilization
charts, and reserve time blocks.

FRONT END ONLY. No backend, no database, no auth. All data is mock data defined
in code, but the app must behave like the real thing — the reservation logic,
the pricing math, and the filter state all have to actually work.

TECH STACK: Vite + React + TypeScript (strict). react-router-dom for routes.
Plain CSS with design tokens (no CSS framework). Charts are hand-rolled inline
SVG. React context + reducer for shared state. Vitest for unit tests. Zero paid
services and zero external network requests at runtime.

MOCK DATA (typed, deterministic, defined in src/data):
- 24 listings across at least 5 regions, with realistic chips (H100, RTX 5090,
  M3 Ultra, MI300X, RTX Pro 6000, etc.), memory in GB, TFLOPS, hourly rate in
  integer cents, and a status of "available" | "maintenance" | "retired".
- Hourly utilization samples per listing (0-100%), generated from a seeded
  pseudo-random function so charts look organic but render identically on every
  reload. Do NOT use Math.random() at render time.
- A handful of pre-existing reservations, including at least one that a naive
  overlap check would wrongly allow.

THE HARD PART — these rules must be pure, tested TypeScript functions:
1. Overlap detection. A listing cannot hold two reservations whose time ranges
   overlap. Ranges are half-open: a reservation ending at 14:00 and one starting
   at 14:00 do NOT overlap. Cancelled reservations don't count.
2. Pricing. Billed in 15-minute increments, always rounded UP. Minimum billable
   block is 1 hour. Any reservation longer than 24 continuous hours gets 10% off
   every hour beyond the 24th — not off the whole booking. All money is integer
   cents. Never use floating point for money.
3. Holds expire. A reservation held for more than 10 minutes without being
   confirmed flips to "expired" and frees its slot. Drive this off a real timer
   in the UI with a visible countdown.
4. Maintenance blocks new reservations but leaves existing confirmed ones alone.
5. Reservations persist to localStorage and survive a full page reload.

PAGES / ROUTES:
1) "/" Home — hero, a "GPUs online" counter computed from the real mock data,
   three feature cards, CTA into browse.
2) "/browse" — grid of listing cards with working search, region filter, memory
   range filter, status filter, and sort (price, memory, TFLOPS, utilization).
   Filter state lives in the URL query string and must survive a refresh AND the
   browser back button. Show an empty state when filters match nothing.
3) "/listings/:slug" — spec sheet, a 24-hour utilization line chart with a
   working hover tooltip, a 7-day availability calendar that visually blocks out
   taken slots, and a reservation form with a live price quote that recalculates
   as the user adjusts the time range. Show the pricing breakdown — base hours,
   rounding applied, discount applied.
4) "/dashboard" — the user's reservations from localStorage, with countdown
   timers on held ones, cancel buttons, and a running total spend.
5) "/compare" — pick up to 3 listings and diff their specs side by side.
   Selection persists across navigation.
6) A custom, on-brand not-found page (not the default).

DESIGN DIRECTION: Dark-first with a real light mode toggle that persists and
does not flash on reload. Technical-industrial: near-black surfaces, thin
hairline borders, monospace for all numbers and IDs, one electric accent color
used sparingly. Data-dense but calm — a trading terminal, not a SaaS template.
Hover lift on cards, visible focus states, smooth transitions. Responsive from
375px to 4K; tables collapse into cards on mobile; charts stay readable on a
phone. Accessibility: keyboard navigation, semantic headings, labelled form
controls, aria labels on icon buttons, and a text summary alternative for every
chart. No stock photography and no external image or font requests — every
visual is inline SVG or CSS; the app renders perfectly with the network off.

TESTS (must actually pass — npm test): Vitest unit tests covering at minimum
overlap detection including the half-open boundary case, the 15-minute
round-up, the 1-hour minimum, the over-24-hour discount applied only to the
excess hours, hold expiry at the 10-minute boundary, and the filter/sort logic
returning correct results for a combined query.

QUALITY BAR: Production-ready. No TODO comments, no stubbed functions, no any
types, no Lorem Ipsum, no dead buttons. Every control does something. npm
install, npm test, npm run build, npm run dev must all succeed.
```
