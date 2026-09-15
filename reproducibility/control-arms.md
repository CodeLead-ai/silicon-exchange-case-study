# The control arms — the same model with CodeLead removed

Everything here was run on the same laptop, the same LM Studio endpoint and the same
loaded model instance as the governed runs (`qwen/qwen3.8-27b`, 8-bit, 131,072 context,
one request at a time), from an empty directory, with the same challenge prompt.

## What "raw" means

The stock model, the public one-command scaffold (`npm create vite@latest app -- --template
react-ts`), and an agent loop: the model reads files, writes files and runs shell commands
in a cycle until it declares itself done. No plan, no per-step gates, no verification other
than whatever the model chooses to run itself. Two such loops were run.

## 1. opencode — the tool used in the video

opencode 1.18.30, pointed at the LM Studio endpoint (`lmstudio/qwen/qwen3.8-27b`), run
non-interactively with the challenge prompt as its single instruction.

| Attempt | Reasoning effort | Output cap | Outcome |
|---|---|---|---|
| 1 | medium (requested; never reached the server — the client did not forward it) | 32,000 | 32 min, nothing written beyond the scaffold. Not counted: the setting was not on the wire. |
| 2 | medium, verified on the wire | 44,000 | 45 min, **zero files**. Two of three steps ended on the output cap: the model reasoned past 44,000 tokens without emitting a tool call. |
| 3 | medium, verified | 110,000 | **zero files**. |
| 4 | low, verified | 110,000 | **19 files**, the business logic (filters, holds, overlap, pricing, time) and **40 passing tests**, **no pages**; ended at 107 min on a streaming read timeout in the client (`SSE read timed out`). |

The verification of "on the wire" reads LM Studio's request log and checks the
`reasoning_effort`, temperature and `max_tokens` the server actually received. Attempt 4 is
the best opencode outcome and is reported as run; the timeout is a client limit, not a
model decision.

## 2. Our plain loop — published so that it can be judged

A deliberately generous minimal loop: the same per-call time budget as CodeLead's most
generous, a forgiving output parser, build and test output fed back every turn, retries on
cut calls, 4,000 characters of command output per turn (twice what the governed arm sees),
a 200-turn cap, and a six-hour clock. The system prompt is published below in full. The
one deliberate tilt is named: validation hygiene ("run the build and fix errors") is
allowed; anything about planning, decomposition, increments or per-feature verification is
not, because that is the thing being tested.

| Run | Reasoning effort | Outcome |
|---|---|---|
| medium | medium | **0 files** — 4 calls, each cut at ~1,500 s with ~15,900 reasoning tokens and no content. |
| **low** | low | **Complete.** 25 turns, **75.5 min**, 30 files, all six routes, 64 tests passing, build and preview green; all 62 items of a static checklist derived from the request present. |

### What the low-effort run got wrong

Against a hand-written set of 26 business-rule checks derived from the request's own
sentences (the same set used on every governed run), the plain loop's app fails 2: a
reservation that runs a few minutes past 24 hours is discounted as if a whole extra hour
had been used. Its own unit test asserts that wrong number. Every build, test and render
check passed, and the run declared itself finished.

The same defect class appeared in CodeLead's own whole-app build on 2026-09-14. There, the
independent rule check caught it at verification, a scoped fix was scheduled for the pricing
module, the model corrected the code and its tests in ten minutes, and the check confirmed
the fix. That difference — not speed, not looks — is the case study.

### Effort inversion, disclosed

At medium reasoning effort both raw tools produced nothing: the model thought past its
output budget before acting. At low effort both did real work. For the governed pipeline it
is the other way round: planning and review run at medium, the build at low, and each call
is bounded so a deliberation that runs away is cut and retried with full context rather
than waited out.
