# Progressive delivery: S6 case record

## Control objective

Use a working local Git baseline and metrics-based Rollouts analysis to demonstrate an aborted change, then record the exact controller and GitOps states. setWeight is a configured value; request share was not measured.

## Observed S6 chronology

1. Baseline: local Git revision **44cf25f** provided the healthy desired state in the Kind test.
2. Injection: **kubectl patch** changed the Rollout template to the bad configuration (2026-09-22).
3. Canary step: configured weight **25%** is the Rollouts **setWeight field**.
4. Analysis: Prometheus query avg(avg_over_time(probe_success{job="blackbox-canary"}[20s])); threshold result[0] >= 0.99; count **3**, interval **10s**, failureLimit **1**. Values **[1, 0.5, 0]** yielded phase **Failed**.
5. Abort: Rollout condition **RolloutAborted**, recorded at **2026-09-22 00:32:34 UTC**. Bad RS **0/0** and stable RS **4/4 Ready**, with a stable-service HTTP check (1 run) of **200**.
6. Post-abort Argo CD: one refresh annotation and one Application patch were applied, then four state checks were made: checks 1-3 **OutOfSync/Degraded**, check 4 **Synced/Healthy**.

After the abort, one HTTP check of the stable service returned **200** and the stable ReplicaSet was **4/4 Ready**. A successful canary AnalysisRun was recorded before the abort (2026-09-22 00:30:18 to 00:30:38 UTC); no AnalysisRun was recorded after it.

**Related P2 event:** on **2026-09-21**, the P2 experiment deployed the bad configuration as Git commit **f926d6b** and recorded the same measurements **[1, 0.5, 0]**, phase **Failed** and **RolloutAborted**. S6 (2026-09-22) injected the bad configuration with kubectl patch.

## Evidence chain

[Healthy baseline](../evidence/01-baseline-state.md) → [bad patch](../evidence/02-bad-config-injection.md) → [abort](../evidence/03-auto-abort.md) → [Argo CD normalization](../evidence/04-gitops-normalization.md).

Source type/name: bad-abort-analysisrun.json; observation: 2026-09-22 00:32:14–00:32:34 UTC; SHA-256 prefix: 4843259e22a4.
Source type/name: bad-abort-rollout.json; observation: 2026-09-22 00:32:34 UTC; SHA-256 prefix: 18f151a99102.
