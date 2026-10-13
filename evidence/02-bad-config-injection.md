# S6: direct bad-config injection

- **Observed action:** kubectl patch updated the Rollout template in the Kind cluster.
- **Configured value:** Argo Rollouts **setWeight=25**, a configured Canary weight of **25%**. It is **not** a measurement of request distribution.
- **Metric definition:** avg(avg_over_time(probe_success{job="blackbox-canary"}[20s])); successCondition result[0] >= 0.99; count **3**; interval **10s**; failureLimit **1**.
- **Observed measurements:** **[1, 0.5, 0]**, resulting in **AnalysisRun Failed**.
- **Time:** **2026-09-22 00:32:14–00:32:34 UTC** for AnalysisRun measurements.
- **Causal boundary:** This was not a bad Git revision pushed into the S6 workflow. A **separate P2** run dated **2026-09-21** did record bad Git commit **f926d6b**; it is not merged into this representative sequence.

Source type/name: bad-canary-patch.txt; observation: S6 run 2026-09-22 UTC, action time not recorded in patch output; SHA-256 prefix: 6b677a175c7e.
Source type/name: bad-abort-analysisrun.json; observation: completed 2026-09-22 00:32:34 UTC; SHA-256 prefix: 4843259e22a4.
Source type/name: bad-abort-rollout.json; observation: aborted 2026-09-22 00:32:34 UTC; SHA-256 prefix: 18f151a99102.

[Next: Abort evidence](03-auto-abort.md).
