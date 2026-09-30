# Model, runtime, and hardware

Two servings of the same model on the same machine. The run the case study leads with
(2026-09-29) used TensorFold; the run whose receipts are published (2026-09-14) and the
2026-09-02 run used LM Studio. If a setting is not listed, it was the runtime's default.

## The 2026-09-29 run: TensorFold with speculative decoding

| | |
|---|---|
| Model | `Qwen3.8-27B-MLX-8bit`, the same checkpoint file as below |
| Server | `tensorfold serve … --context 89600 --reasoning-effort medium` |
| Speculative decoding | drafter `z-lab/Qwen3.8-27B-DFlash2` |
| Observed decode rate | mean 59.3 tokens/second, median 55.5, range 45–100 across 52 requests (serve log) |
| Machine | the same MacBook Pro as below |

The serving configuration and serve log will be published with the run's artifacts; see
[`run-2026-09-29.md`](run-2026-09-29.md).

## The 2026-09-14 and 2026-09-02 runs: LM Studio

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
