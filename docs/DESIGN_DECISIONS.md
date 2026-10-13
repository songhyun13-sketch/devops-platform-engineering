# Engineering decisions and accepted tradeoffs

The first row's rationale is the author's own stated reason. The other rationales and alternatives are **retrospective interpretations** derived from the experiment evidence, not contemporaneous decision records.

| Decision | Rationale | Alternative | Accepted limitation |
|---|---|---|---|
| Use local bare Git + Argo CD in Kind | Single-user lab: the repository did not need to be shared with anyone, so external hosting was unnecessary (author's stated reason) | External managed Git hosting | Does not validate public repository or enterprise credentials |
| Argo Rollouts as delivery controller | Metric-conditioned change handling | Direct Deployment update | setWeight is not observed request distribution |
| Measure Blackbox probe via Prometheus | Separates readiness from service-response behavior | Readiness probe alone | The stable-service probe cannot represent all client requests |
| Abort failed change before further promotion | Keep stable service available in lab observation | Continue promotion and inspect later | Abort alone does not normalize Argo CD health |
| Inspect Argo CD state after abort and repeat normalization checks | Verify whether the reported desired/live state returned to healthy | Assume a controller repairs every state automatically | Attempts 1–3 remained OutOfSync/Degraded; attempt 4 was Synced/Healthy, but the summarized evidence does not establish the complete restoration mechanism |
| Separate Istio routing fixture | Observe actual sample distributions | Infer routing from configured weights | Bounded n=100 observations, not an ongoing percentage guarantee |
| Separate Collector fault injection | Isolate signal loss/restoration | Combine with deployment-path outage | No correlated end-to-end dependency claim |
| Publish summarized evidence, not full logs | Reduce disclosure and preserve measurement boundaries | Push original manifests and logs | Less immediately reproducible without private fixtures |
| Mark unverified states as held | Prefer honest partial evidence to false success | Aggregate all projects as green | Cross-project readiness remains outside the S6 gate |

The task-control practice (validation, stop-on-uncertainty, checkpoints and evidence review) is a **design principle**, not established deployment wiring.
