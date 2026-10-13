# Engineering decisions and accepted tradeoffs

The first row's rationale is the author's stated reason. The other rationales and alternatives were written retrospectively from the experiment evidence.

| Decision | Rationale | Alternative | Accepted limitation |
|---|---|---|---|
| Use local bare Git + Argo CD in Kind | Single-user lab: the repository did not need to be shared with anyone, so external hosting was unnecessary (author's stated reason) | External managed Git hosting | External hosting and credentials were outside the experiment |
| Argo Rollouts as delivery controller | Metric-conditioned change handling | Direct Deployment update | setWeight 25 is the configured value; P2 observed canary 1 / stable 3 pods (replicas 4); request share was not measured |
| Measure Blackbox probe via Prometheus | Separates readiness from service-response behavior | Readiness probe alone | The analysis query uses probe_success of the canary endpoint (job=blackbox-canary) |
| Abort failed change before further promotion | Keep stable service available in lab observation | Continue promotion and inspect later | After abort, a refresh annotation and an Application patch were applied; checks 1-3 showed OutOfSync/Degraded and check 4 showed Synced/Healthy |
| Inspect Argo CD state after abort and repeat normalization checks | Verify whether the reported desired/live state returned to healthy | Assume a controller repairs every state automatically | One refresh annotation and one Application patch, then four state checks: checks 1-3 OutOfSync/Degraded, check 4 Synced/Healthy |
| Separate Istio routing fixture | Observe actual sample distributions | Infer routing from configured weights | Two n=100 samples (80:20 → 75:25, 30:70 → 30:70) |
| Separate Collector fault injection | Isolate signal loss/restoration | Combine with deployment-path outage | The Collector fault ran as its own experiment (2026-09-21) |
| Publish summarized evidence, not full logs | Reduce disclosure and preserve measurement boundaries | Push original manifests and logs | Less immediately reproducible without private fixtures |
| Evaluator returns HOLD for a check with missing input | A missing input yields HOLD instead of PASS | Aggregate all items as green | Status is per check |

The task-control practice (validation, stop-on-uncertainty, checkpoints and evidence review) is documented as a design principle in [OPERATION_PRINCIPLES.md](OPERATION_PRINCIPLES.md).
