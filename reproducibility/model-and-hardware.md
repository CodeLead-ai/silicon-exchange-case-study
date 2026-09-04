# Model, runtime, and hardware

Everything below is what the published run (2026-09-02) actually used. If a setting is not
listed, it was the runtime's default.

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
| Server | LM Studio, MLX engine (CLI commit `07b7252`), OpenAI-compatible endpoint on `localhost:1234` |
| Load settings | single model loaded, `--parallel 1` |
| Served context length | 131,072 tokens (as reported by the server for this load; the pipeline keeps its own prompts far below this) |
| Reasoning effort | `medium` for the planning and analysis calls (a standard endpoint parameter) |
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
  range. The 4-bit variant (≈16 GB) completed 17/17 in a development run one day earlier,
  on the same machine.
- Smaller models degrade honestly rather than fail silently: a 4B model on a mid-range
  Windows desktop completed 4/14 increments with every failure recorded — see the FAQ's
  "why not a frontier model" answer for the model-size ladder.
- The driver, gates and evidence pipeline are part of the CodeLead beta (coming);
  reproduction today means running your own agent against the published prompt and
  comparing against the receipts, or waiting for the beta's one-command rerun.
