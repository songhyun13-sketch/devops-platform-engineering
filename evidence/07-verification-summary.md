# Verification summary: evidence index

| Check | Observed state and scope | Evidence |
|---|---|---|
| S6 GitOps healthy baseline | local Git commit **44cf25f**, 2026-09-22 run | [01](01-baseline-state.md) |
| S6 patch and probe failure | setWeight **25% configured**; samples **[1, 0.5, 0]**; AnalysisRun **Failed**, 2026-09-22 UTC | [02](02-bad-config-injection.md) |
| S6 automatic abort | RolloutAborted, bad RS **0/0**, stable RS **4/4 Ready**, stable HTTP **200** (one check), 2026-09-22 UTC | [03](03-auto-abort.md) |
| S6 GitOps final reconciliation | after annotate + patch, state checks **1–3 OutOfSync/Degraded**, **4 Synced/Healthy**, 2026-09-22 UTC | [04](04-gitops-normalization.md) |
| Collector fault | replicas **1→0→1**, probe **1→0→1**, curl rc **7** → HTTP **200**, 2026-09-21 23:19:10~23:19:49 UTC | [05](05-observability-fault-recovery.md) |
| P2 Istio routing | **80:20→75:25** and **30:70→30:70**, each **100 requests**, P2 2026-09-21 | [06](06-istio-weight-shift.md) |
| S6 evidence bundle hashes | **8/8 SHA-256 matches**, verified 2026-09-22 **00:48:46 UTC** | Manifest verification summary |
| S6 Thanos continuity | **PASS in cycle 2** for current-data query continuity, verified 2026-09-21 **23:15:39 UTC**; cycle 1 **FAILED** (sidecar exit: Prometheus `external_labels` absent) | Thanos runtime summary |

Source type/name: bundle-manifest-verification.json; observation: verified 2026-09-22 00:48:46 UTC; SHA-256 prefix: 5f21bdd5cd7e.
Source type/name: thanos-runtime.json; observation: verified 2026-09-21 23:15:39 UTC; SHA-256 prefix: 3293cf98eff9.
