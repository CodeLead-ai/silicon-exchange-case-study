# Run log — stage by stage (sanitized)

Derived from the run's console log and journal (2026-09-14, `qwen/qwen3.8-27b` 8-bit, one
Mac laptop). Per the repository's IP posture, prompt and response bodies are withheld; every
stage, gate outcome, timestamp and commit below is as recorded. Commits link into the
receipts repository, **CodeLead-ai/silicon-exchange-app**.

Gate keys: **build** = tsc + vite build · **tests** = full unit-test suite · **browser** =
the planned capabilities exercised in a real browser · **rules** = every business rule in
the request checked independently of the model's own tests.

| Stage | Started (UTC) | Duration | Build | Tests | Browser | Rules | Receipts commit |
|---|---|---|---|---|---|---|---|
| Planning: 18 increments, reviewed against the request (6 criteria added) | 06:33:10 | 17m 28s | — | — | — | — | — |
| 1 · Runnable app shell (no model call) | 06:50:38 | 9 s | passed | — | — | — | `increment inc-1` |
| 2 · Whole application — all 18 planned increments, built and committed | 06:50:47 | 52m 38s | passed | 40/40 | — | — | `increment build-all` |
| 3 · Verification — capabilities in the browser, rules against the request | 07:43:25 | ~23m | — | — | 8 observed, 1 not observed, 8 not observable | **1 failure**: pricing discount on a partial hour past 24 h | — |
| 4 · Governed fix — pricing rule and its tests, scoped to the pricing module | 08:06:28 | 10m 29s | passed | 40/40 | — | re-checked | `increment rule-pricing` |
| 5 · Visual polish and responsiveness | 08:16:57 | 22m 47s | passed | 40/40 | passed | — | `increment inc-18` |
| 6 · Final verification | 08:39:44 | ~20m | passed | 40/40 | — | 26/26 by hand; the earlier failure gone | — |
| Exit `all_increments_done` | 08:59:47 | **2h 26m 37s total** | | | | | |

"Not observable" means the browser check could not perform a meaningful interaction on
that capability (for example a pure-logic module renders nothing to click). An unobservable
check never counts as a pass; verification then rests on the build, tests and rules gates,
which is recorded as such.

**The one failure, in full:** a reservation of 24 hours and 1 minute rounds up to 24 hours
15 minutes; the 10% discount applies to the 15-minute excess only. Expected 24,225 cents at
a 1,000 c/h rate; the first build returned 24,250 (it had discounted a whole hour). The fix
stage rewrote the discount to apply to the excess quarter-hours, brought the model's own
tests to the request's numbers, and passed build, tests and the re-check.

**No post-run repairs.** The application is published exactly as the pipeline left it.
