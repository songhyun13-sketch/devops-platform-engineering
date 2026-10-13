# S6: Argo CD state normalization

- **Observed states:** one refresh annotation and one Application patch, then four state checks: checks **1-3** **OutOfSync / Degraded**, check **4** **Synced / Healthy**.
- **Context:** healthy local Git source reference **44cf25f** was in place; bad configuration was injected through kubectl patch.
- **Observation time:** S6 run dated **2026-09-22 UTC**; runtime summary verified **2026-09-22 00:34:33 UTC**.
- **AnalysisRun records:** one successful canary AnalysisRun before the abort (2026-09-22 00:30:18~00:30:38 UTC); none after the abort.

Source type/name: argocd-normalize-after-abort.txt; observation: S6 dated 2026-09-22 UTC; per-check timestamps not recorded; SHA-256 prefix: 5cea6c9f7a59.
Source type/name: gitops-canary-runtime.json; observation: verified 2026-09-22 00:34:33 UTC; SHA-256 prefix: a7c859bdf84d.

[Verification overview](07-verification-summary.md).
