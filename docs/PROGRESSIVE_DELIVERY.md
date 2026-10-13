# Progressive delivery: S6 case record

## Control objective

Use a working local Git baseline and metrics-based Rollouts analysis to demonstrate an aborted change, then record the exact controller and GitOps states. Rollouts setWeight does not independently measure real request distribution.

## Observed S6 chronology

1. Baseline: local Git revision **44cf25f** provided the healthy desired state in the Kind test.
2. Injection: **kubectl patch** changed the Rollout template to the bad configuration. No bad Git revision was introduced by the S6 step.
3. Canary step: configured weight **25%** means the Rollouts **setWeight field**, not a measured request share.
4. Analysis: Prometheus query avg(avg_over_time(probe_success{job="blackbox-canary"}[20s])); threshold result[0] >= 0.99; count **3**, interval **10s**, failureLimit **1**. Values **[1, 0.5, 0]** yielded phase **Failed**.
5. Abort: Rollout condition **RolloutAborted**, recorded at **2026-09-22 00:32:34 UTC**. Bad RS **0/0** and stable RS **4/4 Ready**, with stable-service probe HTTP **200**.
6. Post-abort state checks: three consecutive Argo CD observations were **OutOfSync/Degraded**, and the **fourth** was **Synced/Healthy** after refresh/normalization attempts. The summarized evidence establishes those observed states, not the complete controller-action sequence that produced the final state.

These observations show **post-abort stable-service availability at the recorded probe** and bounded release-risk containment; no assertion about uninterrupted availability or all client sessions can be inferred. There is no independently evidenced successful analysis after the abort.

**Separate P2 event:** on **2026-09-21**, a different experiment used Git bad revision **f926d6b**. That P2 Git step must not be presented as the S6 cause.

## Evidence chain

[Healthy baseline](../evidence/01-baseline-state.md) → [bad patch](../evidence/02-bad-config-injection.md) → [abort](../evidence/03-auto-abort.md) → [Argo CD normalization](../evidence/04-gitops-normalization.md).

Source type/name: bad-abort-analysisrun.json; observation: 2026-09-22 00:32:14–00:32:34 UTC; SHA-256 prefix: 4843259e22a4.
Source type/name: bad-abort-rollout.json; observation: 2026-09-22 00:32:34 UTC; SHA-256 prefix: 18f151a99102.
