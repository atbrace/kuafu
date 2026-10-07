# kuafu

Fork of [jpezzulli/sglang-rtxpro6000](https://github.com/jpezzulli/sglang-rtxpro6000), an sglang-based inference serving stack for a single NVIDIA RTX Pro 6000 GPU.

## What this is

A shared measurement project run by two independent home labs. The goal is to find how far one RTX Pro 6000 can be pushed for agentic LLM workloads — its concurrency and throughput ceilings — and to publish measured progress against those numbers.

## How it works

We use an incremental ("ratchet") method. Each experiment window raises one measured limit by one step, then restores the last known stable configuration. Unstable results are rolled back; stable gains carry forward. The two labs run experiments alternately and share measurement data. Performance improvements that generalize are contributed to the upstream open-source components.

## Baseline & goals

Recorded 2026-09-30 during a 128-node workflow fan-out. Both labs consider these ceilings provisional, not final.

| Metric | Day-zero baseline | Goal |
| --- | --- | --- |
| Concurrent sglang requests | 10 served (15 queued at peak) | TBD |
| Concurrent agents (reference recipe) | 6 | TBD |

## Milestones

- 2026-10-01 — Baseline recorded: 10 concurrent / 15 queued / 6-agent recipe cap. Fork initialized as placeholder.
- 2026-10-07 — `sys-kchw1o` ratchet window: forked the pinned Pennyroyal v2.5.3 Flash-Next FR-Spec recipe. The trial shape is native 262,144 context (no YaRN override), `extra_buffer_lazy` with eight Mamba states per path, automatic KV-pool sizing, no NIXL L3 cache, and NVMe SSD-streamed PLE. Acceptance is live argv, SSD-stream boot identity, and a real 16-request admission/scaling measurement; rollback is the digest-pinned upstream recipe.

## Fork contents

`configs/pennyroyal/` is the reviewable recipe overlay mounted by the serving job. It is copied from `jpezzulli/sglang-rtxpro6000` Pennyroyal v2.5.3 image digest `sha256:81d4d3d19b21e30b895f57e3628a8dd82c02a52c17e6234f5f3ea7836fd31fb3`, then changed only where a measured ratchet requires it. The image remains digest-pinned; kuafu carries the divergence.

## Upstream contributions

TBD. No upstreamed improvements recorded yet; each will be listed with a link when it lands.

## License

TBD. Contributing rules and release cadence: TBD.
