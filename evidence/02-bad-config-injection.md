# S6: direct bad-config injection

- **Observed action:** kubectl patch updated the Rollout template in the Kind cluster.
- **Configured value:** Argo Rollouts **setWeight=25**, a configured Canary weight of **25%**. In the P2 run, setWeight 25 with 4 replicas gave canary 1 / stable 3 pods (`kubectl get rs`); request share was not measured.
- **Metric definition:** avg(avg_over_time(probe_success{job="blackbox-canary"}[20s])); successCondition result[0] >= 0.99; count **3**; interval **10s**; failureLimit **1**.
- **Observed measurements:** **[1, 0.5, 0]**, resulting in **AnalysisRun Failed**.
- **Time:** **2026-09-22 00:32:14–00:32:34 UTC** for AnalysisRun measurements.
- **Injection method:** in the P2 run on **2026-09-21**, the bad configuration was deployed as Git commit **f926d6b** and recorded the same measurements **[1, 0.5, 0]**, **Failed** and **RolloutAborted**. In S6 (2026-09-22) it was injected with kubectl patch.

Source type/name: bad-canary-patch.txt; observation: S6 run 2026-09-22 UTC, action time not recorded in patch output; SHA-256 prefix: 6b677a175c7e.
Source type/name: bad-abort-analysisrun.json; observation: completed 2026-09-22 00:32:34 UTC; SHA-256 prefix: 4843259e22a4.
Source type/name: bad-abort-rollout.json; observation: aborted 2026-09-22 00:32:34 UTC; SHA-256 prefix: 18f151a99102.

[Next: Abort evidence](03-auto-abort.md).
