# Run log — increment by increment (sanitized)

Derived from the run's console and session logs. Per the repository's IP posture, prompt
and response bodies are withheld; every event name, gate outcome, timestamp and commit
below is as recorded. Commits link into the receipts repository.

Gate keys: **build** = tsc + vite build · **tests** = full unit-test suite · **probe** =
headless-browser acceptance probe on the increment's own route · **regression** =
cumulative probe over all previously verified increments. Multiple values (e.g.
`failed/passed`) show attempt 1 then attempt 2.

| # | Increment | Attempts | Build | Tests | Probe | Regression | Committed | Receipts commit |
|---|---|---|---|---|---|---|---|---|
| 1 | App shell | 1 | passed | — | skipped: | — | 05:19:08 UTC | [`e9726a3`](https://github.com/CodeLead-ai/silicon-exchange/commit/e9726a3) |
| 2 | Typed mock data | 1 | passed | passed | unavailable*: | passed | 05:31:35 UTC | [`981538f`](https://github.com/CodeLead-ai/silicon-exchange/commit/981538f) |
| 3 | Overlap detection | 1 | passed | passed | unavailable*: | passed | 05:41:56 UTC | [`2368389`](https://github.com/CodeLead-ai/silicon-exchange/commit/2368389) |
| 4 | Pricing math | 1 | passed | passed | skipped: | passed | 05:54:42 UTC | [`297f3f0`](https://github.com/CodeLead-ai/silicon-exchange/commit/297f3f0) |
| 5 | Hold expiry | 1 | passed | passed | skipped: | passed | 06:04:23 UTC | [`31a79af`](https://github.com/CodeLead-ai/silicon-exchange/commit/31a79af) |
| 6 | Maintenance blocking | 1 | passed | passed | skipped: | passed | 06:11:35 UTC | [`ac36048`](https://github.com/CodeLead-ai/silicon-exchange/commit/ac36048) |
| 7 | Filter and sort logic | 1 | passed | passed | skipped: | passed | 06:22:43 UTC | [`d8d4011`](https://github.com/CodeLead-ai/silicon-exchange/commit/d8d4011) |
| 8 | Shared state and persistence | 1 | passed | passed | unavailable*: | passed | 06:33:58 UTC | [`8496951`](https://github.com/CodeLead-ai/silicon-exchange/commit/8496951) |
| 9 | Home page | 2 | failed/passed | passed | unavailable*: | passed | 06:47:46 UTC | [`3458b5e`](https://github.com/CodeLead-ai/silicon-exchange/commit/3458b5e) |
| 10 | Browse page | 1 | passed | passed | unavailable*: | passed | 07:00:43 UTC | [`b10edab`](https://github.com/CodeLead-ai/silicon-exchange/commit/b10edab) |
| 11 | Spec sheet and utilization chart | 1 | passed | passed | passed | passed | 07:14:34 UTC | [`7dcdc58`](https://github.com/CodeLead-ai/silicon-exchange/commit/7dcdc58) |
| 12 | Availability calendar | 1 | passed | passed | unavailable*: | passed | 07:27:06 UTC | [`a536d62`](https://github.com/CodeLead-ai/silicon-exchange/commit/a536d62) |
| 13 | Reservation form with live pricing | 1 | passed | passed | unavailable*: | passed | 07:40:03 UTC | [`fd67614`](https://github.com/CodeLead-ai/silicon-exchange/commit/fd67614) |
| 14 | Dashboard | 1 | passed | passed | unavailable*: | passed | 07:53:50 UTC | [`5d1a895`](https://github.com/CodeLead-ai/silicon-exchange/commit/5d1a895) |
| 15 | Compare page | 1 | passed | passed | passed | passed | 08:08:17 UTC | [`83bc313`](https://github.com/CodeLead-ai/silicon-exchange/commit/83bc313) |
| 16 | Not-found page | 1 | passed | passed | unavailable*: | passed | 08:16:46 UTC | [`232c3f1`](https://github.com/CodeLead-ai/silicon-exchange/commit/232c3f1) |
| 17 | Polish | 4 | passed/failed/passed | passed/passed | skipped: | passed | 08:58:03 UTC | [`d657d7d`](https://github.com/CodeLead-ai/silicon-exchange/commit/d657d7d), [`570fae2`](https://github.com/CodeLead-ai/silicon-exchange/commit/570fae2) |

\* `unavailable` means the probe could not perform a meaningful interaction on that
surface (for example, a pure-logic increment renders nothing to probe). An unavailable
probe never counts as a pass — verification then rests on the build, test and regression
gates, which is recorded as such in the ledger.

**Post-run governed repair (included in the receipts):** a 404/not-found module was also
rendered on the home page; one `/implement` pass through the same gates removed that
render (the catch-all 404 route was untouched) — [`5adcf1d`](https://github.com/CodeLead-ai/silicon-exchange/commit/5adcf1d).

**Evidence files in the receipts repo:** `.codeleadsessions/increment-ledger.json` (the
per-increment evidence ledger) and per-session `summary.json`. Model I/O transcripts are
withheld (see FAQ, "What is patented?").
