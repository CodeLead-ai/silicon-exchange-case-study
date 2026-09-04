# FAQ — the questions we expect, answered plainly

**"The cloud agent did it in 15 minutes — so what?"**
Yes, it did, and we say so in the comparison table. The claim is not speed. It is that a
fixed-cost laptop, with no code leaving it, produced a fully machine-verified result
unattended. If your constraint is privacy, cost predictability, or air-gapped
infrastructure, the 15-minute cloud run is not an option — the 3h46m local run is.

**"Is this cherry-picked? How many runs?"**
8 full-challenge runs were performed during development, across two quantizations of the
same model. 5 stopped early — every stop was CodeLead's own gates catching a defect
(ours, not the model's, in most cases), and each produced a documented fix. 1 completed
with a single failed increment, repaired afterward under the same governance. 2 completed
17/17: the 4-bit quantization on 2026-09-01, and the published 8-bit run on 2026-09-02 —
which was the first run performed on its final configuration. We did not run the final
configuration repeatedly and pick a winner.

**"The app is simple."**
Fair. It is a front-end product with mock data — that is what the public challenge
specifies. The harder result class is brownfield: the same governance applied to a
17-year-old C# library, where the pipeline discovered and satisfied an undocumented
co-change obligation before its patch was allowed to land. That story is next; this page
is the greenfield one.

**"How much did the harness do versus the model?"**
Exactly this: the deterministic skeleton (app shell, routing, design tokens, test harness)
was produced with zero model calls. Every line of application logic and every one of the
53 unit tests was model-written. The harness planned, scoped, gated, verified, reverted
failures, and recorded evidence — it never wrote application code.

**"Can I reproduce it on my machine?"**
Yes. The prompt, model, quantization, runtime and machine spec are in
[`reproducibility/`](reproducibility/). Any Mac that can serve a 27B at 8-bit (≈28 GB of
weights) is in range; smaller quantizations run on less and completed 13/14 in our
development runs. The installable beta will add the one-command rerun. If you try it, file
a reproduction report — including the failures; those are the interesting part.

**"Was the model fine-tuned or given the solution?"**
No fine-tuning; the stock published weights were served locally. At the category level,
the model received: the challenge prompt, the increment plan and the current increment's
objective and acceptance criteria, a ledger summary of what already exists (and what
failed), relevant file contents, and — on retries — the machine-generated failure report.
It never received reference solutions, and no human edited its output.

**"Why not just use a frontier model?"**
If you can, and your code may leave the building, a frontier model is faster and stronger
— the video shows exactly that. The point of this result is different: governance makes
small local models viable, and the weaker the model, the more the governance matters. In
our development runs the same pipeline took a 4B model to a 4/14 partial with honest
failure records, a 27B@4-bit to 13/14 and then 17/17, and the 27B@8-bit to the published
clean run. Same gates at every size; only the model changed.

**"What is patented?"**
The underlying methods are the subject of three provisional patent applications. We
describe the mechanism at the level of the six steps above and no further — not the
prompts, protocols, or gate implementations. That is also why the published session
evidence excludes model I/O transcripts; the receipts keep every code commit, the
evidence ledger, and the run summaries.
