# S6: metric-conditioned automatic abort

- **AnalysisRun:** **Failed**, with measurements **[1, 0.5, 0]**, **3** measurements at **10s** intervals and failureLimit **1**.
- **Rollout:** **RolloutAborted** condition at **2026-09-22 00:32:34 UTC**.
- **ReplicaSets:** bad **0/0**, stable **4/4 Ready** after abort, as recorded by the bounded runtime evidence.
- **Service probe:** stable-service HTTP **200** after abort (one check).
- **Follow-on:** after abort, a refresh annotation and an Application patch were applied; state checks 1-3 showed OutOfSync/Degraded and check 4 showed Synced/Healthy (see next page).

Source type/name: bad-abort-analysisrun.json; observation: completed 2026-09-22 00:32:34 UTC; SHA-256 prefix: 4843259e22a4.
Source type/name: bad-abort-rollout.json; observation: aborted 2026-09-22 00:32:34 UTC; SHA-256 prefix: 18f151a99102.
Source type/name: gitops-canary-runtime.json; observation: verified 2026-09-22 00:34:33 UTC; SHA-256 prefix: a7c859bdf84d.
Source type/name: stable-after-abort-http.txt; observation: S6 runtime evidence dated 2026-09-22 UTC; SHA-256 prefix: 27badc983df1.

[Next: Argo CD normalization](04-gitops-normalization.md).
