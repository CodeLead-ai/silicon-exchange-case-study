# FAQ — the questions we expect, answered plainly

**"The cloud agent did it in 15 minutes — so what?"**
Yes, it did, and we say so in the comparison table. The claim is not speed. It is that a
fixed-cost laptop, with no code leaving it, produced a fully machine-verified result
unattended. If your constraint is privacy, cost predictability, or air-gapped
infrastructure, the 15-minute cloud run is not an option — the 2h 27m local run is.

**"Why did Kimi K3 take four hours on four Mac Studios?"**
Our reading, not a measurement of their setup. The video states 238 tokens/second of prompt
processing and 14.7 tokens/second of generation; at that rate four hours is at most about
210,000 generated tokens. The local run used opencode, an open-ended agent loop that sends
the whole growing conversation back to the model before every tool call, so every step pays
prompt processing again, and Kimi K3 is a reasoning model that thinks at length before it
acts. That is what an unstructured loop costs on a very large model. CodeLead gives a small
model one bounded job per call, verifies in the browser and the test runner instead of
asking the model to think harder, and sends what is wrong back as a scoped fix.

**"What do you mean by raw, and why not just run the model raw?"**
Raw is the stock model, the public one-command Vite scaffold, and a minimal agent loop —
read a file, write a file, run a command, until the model says it is done — with no plan,
no gates and no verification beyond what the model chooses to run. We ran two such loops
on the same model, laptop and settings: opencode (the tool from the video) and our own
plain loop. The plain loop built a complete, good-looking marketplace in 75 minutes with 64
passing tests — and the pricing rule wrong, with its own test asserting the wrong number.
Nothing in the loop could know. That is why not raw: "finished" is the model's word for
it. In CodeLead, finished means checked, and the same defect in CodeLead's own build was
caught and fixed in ten minutes. Details: [`reproducibility/control-arms.md`](reproducibility/control-arms.md).

**"You ran opencode? That is what the video used."**
Yes — opencode 1.18 against the same LM Studio endpoint, same model, same prompt. At the
reasoning effort we use for planning it produced zero files in 45 minutes: two of its three
steps ended on the output cap with the model still thinking, and raising the cap to 110,000
tokens changed nothing. At low effort it wrote 19 files with the business logic and 40
passing tests, no pages, and died at 107 minutes on a streaming timeout in the client. We
report it as run; the settings are in the control-arms document.

**"Is this cherry-picked? How many runs?"**
The first published run (2026-09-02, 3h 46m, 17/17) was the first run performed on its
configuration at the time. In the following week we ran the challenge about twenty more
times while changing one thing at a time in the pipeline — context handling, step size,
build-then-verify, the independent rule check — scoring every run the same way. The run
shown here is the first performed on the current configuration. Run-to-run wall clock on
identical configurations varies by about ±20 minutes.

**"Why is this run faster than your first one?"**
Because the pipeline changed, not the model. The first run made 38 model calls with a
per-step analysis before each patch; the current one gives the model a bounded workspace to
build in, verifies the whole product afterwards, and sends only what failed back as a
scoped fix. Same model, same laptop, same serving; 3h 46m became 2h 27m with more
verification, not less.

**"Did you tune CodeLead to this prompt?"**
No, and we checked. The same morning we wrote two requests in unrelated domains — a team
task manager and a personal expense ledger, ~250 words and five business rules each — and
ran them through the same pipeline with nothing changed: 8/8 and 7/7 increments, 2h 02m and
1h 54m, every business rule verified against its request. Both requests are published in
[`reproducibility/`](reproducibility/). Both apps lacked a way to add a new item — and
neither request asked for one.

**"The app is simple."**
Fair. It is a front-end product with mock data — that is what the public challenge
specifies. The harder result class is brownfield: the same governance applied to a
17-year-old C# library, where the pipeline discovered and satisfied an undocumented
co-change obligation before its patch was allowed to land. That story is next; this page
is the greenfield one.

**"How much did the harness do versus the model?"**
Exactly this. The project scaffold — an empty runnable shell of the kind a project
template gives you, with its styling baseline, brand mark, navigation and test setup —
came from the pipeline, with no model involvement. Everything that makes this Silicon
Exchange rather than an empty app was written by the model: the data layer, all five
business-rule implementations, every page and component, and all 40 unit tests. The
harness planned the work, scoped each step, ran the gates, checked the rules, reverted
what failed, and recorded the evidence. It never wrote application code, and no human
wrote or edited any.

**"Can I reproduce it on my machine?"**
Yes. The prompt, model, quantization, runtime and machine spec are in
[`reproducibility/`](reproducibility/). Any Mac that can serve a 27B at 8-bit (≈28 GB of
weights) is in range. The installable beta will add the one-command rerun. If you try it,
file a reproduction report — including the failures; those are the interesting part.

**"Was the model fine-tuned or given the solution?"**
No fine-tuning; the stock published weights were served locally. At the category level,
the model received: the challenge prompt, the increment plan and the current step's
objective and acceptance criteria, a ledger summary of what already exists (and what
failed), relevant file contents, and — on fixes — the machine-generated failure report.
It never received reference solutions, and no human edited its output.

**"Why not just use a frontier model?"**
If you can, and your code may leave the building, a frontier model is faster and stronger
— the video shows exactly that. The point of this result is different: governance makes
small local models viable, and the weaker the model, the more the governance matters. On
the same pipeline a 4B model reached 4 of 14 increments and stopped honestly, a 28B reached
13 of 15, and the 27B completed everything. Same gates at every size; only the model
changed. See [`reproducibility/other-models.md`](reproducibility/other-models.md).

**"What does a partial run actually produce?"**
A smaller app that works, not a broken one. Running the same challenge on a 4B model
(google/gemma-4-e4b, on a mid-range Windows desktop) verified 4 of 14 increments in 6.2
hours and stopped honestly: the routing, the detail pages, the filters and the test suite
were never built, and the ledger records each of those increments as failed with its
cause. What *did* land is real and runs — the pricing math on screen with its rounding and
over-24-hour discount, a sortable listing grid, and a live hold countdown that expires:

![The single surface a 4B model produced: dark theme, pricing breakdown, sortable listing grid, live hold countdown](assets/screenshots/gemma-4b-partial.png)

That is the governance floor doing its job at the small end: every increment that shipped
was machine-verified, every one that didn't is recorded as not built. Nothing was
half-applied and nothing was claimed.

**"What is patented?"**
The underlying methods are the subject of three provisional patent applications. We
describe the mechanism at the level of the six steps in the README and no further — not
the prompts, protocols, gate implementations, or how the independent rule check is
produced. That is also why the published receipts exclude model I/O transcripts; they keep
every code commit and the run summaries.
