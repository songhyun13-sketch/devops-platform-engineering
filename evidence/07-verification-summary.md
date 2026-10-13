# Verification summary: bounded evidence index

| Bounded check | Observed state and scope | Evidence |
|---|---|---|
| S6 GitOps healthy baseline | local Git commit **44cf25f**, 2026-09-22 run | [01](01-baseline-state.md) |
| S6 patch and probe failure | setWeight **25% configured**; samples **[1, 0.5, 0]**; AnalysisRun **Failed**, 2026-09-22 UTC | [02](02-bad-config-injection.md) |
| S6 automatic abort | RolloutAborted, bad RS **0/0**, stable RS **4/4 Ready**, stable HTTP **200**, 2026-09-22 UTC | [03](03-auto-abort.md) |
| S6 GitOps final reconciliation | attempts **1–3 OutOfSync/Degraded**, **4 Synced/Healthy**, 2026-09-22 UTC | [04](04-gitops-normalization.md) |
| Collector independent fault | replicas **1→0→1**, probe **1→0→1**, curl rc **7** → HTTP **200**, 2026-09-21 23:21:50 UTC | [05](05-observability-fault-recovery.md) |
| P2 Istio routing fixture | **80:20→75:25** and **30:70→30:70**, each **100 requests**, P2 2026-09-21 | [06](06-istio-weight-shift.md) |
| S6 evaluator | **12/12 PASS**, S6 evaluator scope only; exact evaluator clock not recorded | Private bounded-checklist evaluator summary |
| S6 evidence bundle hashes | **8/8 SHA-256 matches**, verified 2026-09-22 **00:48:46 UTC** | Manifest verification summary |
| Source hygiene check at S6 | **7** files scanned, **0** hits in that private S6 check; not equivalent to a scan of this public repository | Source scanner summary |
| S6 Thanos continuity | **PASS in cycle 2** for current-data query continuity, verified 2026-09-21 **23:15:39 UTC**; cycle 1 **FAILED** (sidecar exit: Prometheus `external_labels` absent) | Current-data scope only; no object-store claim |

## Attribution and safeguards

S6 evaluator 12/12 is not a judgment about all related work: in a separate earlier evaluation, two bundles remained HOLD although its overall state was PASS. The source scan of seven files is independent of any redaction scan performed on this public repository. No further post-abort AnalysisRun success is established.

Source type/name: s6-evaluator-result.json; observation: S6 evaluation dated 2026-09-22 UTC; exact check clock absent; SHA-256 prefix: ec613cf82603.
Source type/name: bundle-manifest-verification.json; observation: verified 2026-09-22 00:48:46 UTC; SHA-256 prefix: 5f21bdd5cd7e.
Source type/name: secret-scan.json; observation: S6 artifact dated 2026-09-22 UTC; exact scan clock absent; SHA-256 prefix: cfb401817dea.
Source type/name: thanos-runtime.json; observation: verified 2026-09-21 23:15:39 UTC; SHA-256 prefix: 3293cf98eff9.
