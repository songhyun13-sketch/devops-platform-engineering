# Limitations and attribution

**Owner:** the portfolio author designed the experiments, set the evidence scope, reviewed the results and made the verification judgments; implementation, command execution and evidence collection were carried out by directing AI agents (Codex and others).

- Evidence comes from experiments run separately on the same local Kind cluster (S6 GitOps/Canary, S6 Collector fault, P2 Istio) and from historical operational artifacts of a separate EC2 VM.
- Canary configured weight **25%** is the Rollouts setting. In P2, setWeight 25 with 4 replicas gave 1 canary / 3 stable pods (`kubectl get rs`); request share was not measured. Istio distributions are two n=100 samples.
- After abort, stable ReplicaSet **4/4 Ready**, bad ReplicaSet **0/0** and one stable-service HTTP check returning **200** were recorded.
- S6 recorded a Failed AnalysisRun; after one refresh annotation and one Application patch, four Argo CD state checks gave OutOfSync/Degraded (checks 1-3) and **Synced/Healthy** (check 4). No AnalysisRun was recorded after the abort; a successful canary AnalysisRun was recorded before it (2026-09-22 00:30:18 to 00:30:38 UTC).
- The S6 evidence bundle matched **8/8** SHA-256 digests at 2026-09-22 00:48:46 UTC.
- In the S6 Collector run the remote command wrapper reported exit **1**; in a separate isolated reproduction the EXIT trap printed `TRAP_RC=0`. The artifact classified this as a wrapper-status artifact. The fault log contains one scale-down/restore cycle.
- Thanos was verified for **current-data continuity**; object-store retention and backup/restore were not measured. The first apply cycle **failed** (the Thanos sidecar exited because Prometheus `external_labels` were absent); the continuity check passed in the **second cycle** after that divergence was corrected.
- EC2 incident entries come from a backfill script: Incident A's end time is an upper-bound estimate, Incident B's start is the time the local cause was confirmed. Uptime percentage and mean restoration time were not measured.
- Environments: one local Kind cluster (lab runs) and one EC2 VM (historical records).
- Examples under `examples/` are concept illustrations; the raw operational implementation and private evidence are kept private.
