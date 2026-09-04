# Reproducibility

Everything needed to rerun the experiment or audit the published one:

1. **[challenge-prompt.md](challenge-prompt.md)** — the exact prompt the pipeline
   received, verbatim, with the one declared stack adaptation (Next.js → Vite) stated.
2. **[model-and-hardware.md](model-and-hardware.md)** — model, quantization, runtime,
   serving settings, machine spec.
3. **[run-summary.json](run-summary.json)** — the machine-readable summary: timings,
   increments, tests, commands, model calls (including the one governor-cut call), the
   post-run repair, and the full development-run honesty block.
4. **[run-log.sanitized.md](run-log.sanitized.md)** — the increment-by-increment timeline
   with per-gate outcomes, each linked to its commit in the
   [receipts repository](https://github.com/CodeLead-ai/silicon-exchange).

To audit rather than rerun: clone the receipts repo, run `npm install && npm test &&
npm run build`, and walk the git history — one commit per verified increment, in order,
with the evidence ledger alongside.

If you rerun the prompt (with any agent, any model, any hardware), please file a
[reproduction report](../../../issues/new?template=reproduction-report.md) — deviations
and failures are as valuable as successes.
