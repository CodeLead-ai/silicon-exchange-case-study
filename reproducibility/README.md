# Reproducibility

Everything needed to rerun the experiment or audit the published runs:

1. **[challenge-prompt.md](challenge-prompt.md)** — the exact prompt the pipeline received,
   verbatim, with the one declared stack adaptation (Next.js → Vite) stated.
2. **[model-and-hardware.md](model-and-hardware.md)** — model, quantization, runtime, serving
   settings, machine spec.
3. **[run-summary.json](run-summary.json)** — the machine-readable summary of the run the case
   study leads with (2026-09-14): timings per stage, increments, tests, verification, and the
   development-run honesty block.
4. **[run-log.sanitized.md](run-log.sanitized.md)** — that run's stage-by-stage timeline with
   gate outcomes, linked to the receipts repository
   [CodeLead-ai/silicon-exchange-app](https://github.com/CodeLead-ai/silicon-exchange-app).
5. **[run-2026-09-02.md](run-2026-09-02.md)** — the first published run (3h 46m, 17/17), with
   its own [summary](run-summary-2026-09-02.json), [log](run-log-2026-09-02.sanitized.md) and
   receipts ([CodeLead-ai/silicon-exchange](https://github.com/CodeLead-ai/silicon-exchange)).
6. **[control-arms.md](control-arms.md)** — the same model with the pipeline removed: opencode
   (the tool from the video) and our own plain loop, settings and outcomes.
7. **[other-models.md](other-models.md)** — ten models on the same prompt and pipeline, six on
   the laptop and four remote, with the hand review of each finished app and screenshots.
8. **[tally-request.md](tally-request.md)** and **[pocket-ledger-request.md](pocket-ledger-request.md)**
   — the two off-domain requests, verbatim, that check the pipeline was not tuned to this prompt.

To audit rather than rerun: clone a receipts repo, run `npm install && npm test && npm run
build`, and walk the git history — one commit per verified stage, in order.

If you rerun the prompt (with any agent, any model, any hardware), please file a
[reproduction report](../../../issues/new?template=reproduction-report.md) — deviations and
failures are as valuable as successes.
