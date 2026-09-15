# Model, runtime, and hardware

Everything below is what the published run (2026-09-14) actually used. If a setting is not
listed, it was the runtime's default. The 2026-09-02 run used the same model, machine and
serving settings.

## Model

| | |
|---|---|
| Model | `qwen/qwen3.8-27b` (stock published weights, no fine-tuning) |
| Parameters | 27B |
| Quantization | 8-bit (MLX variant; ~29.5 GB on disk, ~27.5 GiB resident when loaded) |
| Observed generation speed | ≈18 tokens/second on this machine |

## Runtime

| | |
|---|---|
| Server | LM Studio, MLX engine, OpenAI-compatible endpoint on `localhost:1234` |
| Load settings | single model loaded, `--parallel 1`, context length 131,072 |
| Reasoning effort | `medium` for the planning and review calls, `low` for the build (standard endpoint parameters, set per call by the pipeline; the server's own per-model default is never relied on) |
| Concurrency | the model server was used exclusively by this run |

## Hardware

| | |
|---|---|
| Machine | MacBook Pro (`Mac17,6`) |
| Chip | Apple M5 Max |
| Memory | 64 GB unified |
| OS | macOS 26.6.2 |
| Sleep | disabled for the duration of the run (a sleeping model server mid-run is the most common cause of fake "timeout" failures we saw in development) |

## Reproduction notes

- Any machine that can serve this model at 8-bit (≈28 GB of weights plus headroom) is in
  range.
- Smaller models degrade honestly rather than fail silently — see
  [`other-models.md`](other-models.md).
- The control arms (the same model with the pipeline removed) and their exact settings are
  in [`control-arms.md`](control-arms.md).
- The driver, gates and evidence pipeline are part of the CodeLead beta (coming);
  reproduction today means running your own agent against the published prompt and
  comparing against the receipts, or waiting for the beta's one-command rerun.
