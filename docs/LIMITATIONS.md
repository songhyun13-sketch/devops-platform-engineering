# Limitations and attribution

**Owner:** design, review and verification judgments by the portfolio author; some implementation was assisted by AI tools.

- Evidence is drawn from isolated Kind experiments, a separate Istio routing fixture, a separate Collector fault test, P2-era observability snapshots, and historical EC2 operational artifacts. These are not one continuously connected environment. No AWS EKS operation is asserted.
- Canary configured weight **25%** is the Rollouts setting, not a measured request fraction. Istio measured distributions are separate bounded samples.
- Stable ReplicaSet **4/4 Ready**, bad ReplicaSet **0/0** and stable HTTP **200** are a narrow stable-service probe result, not a guarantee across all real clients.
- S6 failed analysis and an Argo CD state of Synced/Healthy after refresh were observed. **No independent successful post-abort AnalysisRun** was established. Attempts 1–3 were OutOfSync/Degraded before attempt 4 reached Synced/Healthy.
- The S6 evaluator recorded **12/12 PASS** in its own bounded checklist, with bundle **8/8** digests matching. In a separate earlier evaluation, two bundles remained **HOLD** even though that evaluation's overall state was PASS; there is no systemwide all-green conclusion.
- The S6 Collector test logged a **wrapper exit-status anomaly** (remote command exit **1**, EXIT trap status **0**). The artifact classified this as wrapper-status behavior, not an observed Collector functional failure. No repeat runtime was claimed.
- Thanos was verified for **current-data continuity only**, not object-store retention or a backup-and-restore guarantee. The first apply cycle **failed** (the Thanos sidecar exited because Prometheus `external_labels` were absent); the continuity check passed only in the **second cycle** after that divergence was corrected. The artifact records `object_store_claim: false`.
- Private EC2 historical incident entries are retrospective and their times partly estimated. Monitoring logs do not establish an aggregate uptime or mean restoration-time improvement.
- No large-scale high availability, multi-cluster failover, security certification, public deployment or general end-to-end operational readiness is established.
- Examples are concepts only; the raw operational implementation and private evidence are not included.
