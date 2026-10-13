# DevOps / Platform Engineering — evidence-led portfolio

A set of **separate** case studies on GitOps, progressive delivery, observability failure injection, and operations, with explicit observed-versus-inferred boundaries.

**Owner:** I designed the experiments, set evidence boundaries, reviewed results, and made the verification judgments. Some implementation was assisted by AI tools.

## What I demonstrated

- Built a Kubernetes progressive-delivery lab with Argo CD, Argo Rollouts and Prometheus/Blackbox, injected a bad configuration, and observed metric-driven abort with post-abort stable ReplicaSet **4/4 Ready** and HTTP **200**.
- Tested Istio weighted routing with bounded **100-request** samples and an isolated OpenTelemetry Collector scale-down/restore, observing probe and telemetry recovery.
- Operated a personal EC2 automation workload with PowerShell/SSH and systemd; historical incident records are presented without claiming production SLOs or measured availability improvement.

**Case labels:** S6 = Kind GitOps/Canary and separate Collector exercises; P2 = separate Istio/observability fixtures.

## Detailed verification

| Case (isolated environment) | Observed value and precise scope | Observation time |
|---|---|---|
| S6, Kind GitOps/Canary | Baseline local Git revision 44cf25f; bad configuration applied by kubectl patch, not by a bad Git revision | 2026-09-22 UTC, run S6 |
| S6, Argo Rollouts | Canary configured weight **25%** (setWeight configuration, not measured request share); AnalysisRun **Failed**, measurements **[1, 0.5, 0]**, **3** samples at **10s** intervals, failureLimit **1** | 2026-09-22 00:32:14–00:32:34 UTC |
| S6, abort | RolloutAborted; bad ReplicaSet **0/0**, stable ReplicaSet **4/4 Ready**; stable-service HTTP probe **200** | Abort at 2026-09-22 00:32:34 UTC; runtime checked at 00:34:33 UTC |
| S6, Argo CD normalization | Attempts **1–3**: OutOfSync/Degraded; attempt **4**: Synced/Healthy after refresh | 2026-09-22 UTC, S6 normalization record |
| S6, scoped verification | S6 evaluator **12/12 PASS**, evidence bundle digest match **8/8**, private source secret scan **7** files / **0** hits | S6 artifacts dated 2026-09-22 UTC; bundle verified 00:48:46 UTC |
| S6, separate Collector fault | Replicas **1→0→1**; Prometheus probe **1→0→1**; direct curl rc **7** during interruption, then HTTP **200** | 2026-09-21 23:21:50 UTC |
| P2, independent Istio routing fixture | Configured **80:20** yielded **75:25**; configured **30:70** yielded **30:70**; **n=100 requests per configuration** | Evidence dated 2026-09-21, exact sample clock not recorded |
| EC2, personal automation workload | PowerShell/SSH deployment and systemd restart configuration, with remote operations-log synchronization records | Script/config inspection; logs dated 2026-04-22–2026-05-02 |

## Five distinct case studies, not a single live pipeline

| Area | Environment and evidence | Follow-up |
|---|---|---|
| EC2 operations | Personal service workload, script and recorded synchronization | [Incident and recovery](docs/INCIDENT_AND_RECOVERY.md) |
| Kind GitOps/Canary (S6) | Disposable local bare Git, Argo CD, Argo Rollouts, Blackbox/Prometheus | [Progressive delivery](docs/PROGRESSIVE_DELIVERY.md) |
| Istio weighted routing (P2) | Separate in-mesh fixture and bounded response samples | [Istio summary](evidence/06-istio-weight-shift.md) |
| OpenTelemetry Collector fault (S6 separate test) | Scale-down and scale-up with probe/log/trace markers | [Collector evidence](evidence/05-observability-fault-recovery.md) |
| Observability snapshot (P2 separate run) | P2-era snapshots, not joined to S6 | [Architecture boundaries](docs/ARCHITECTURE.md) |

**These were not continuously integrated into one production topology; no AWS EKS operations are claimed.** The S6 abort experiment and the independent P2 Git bad-revision exercise are not conflated.

## Representative sequence: S6 only

Healthy local Git baseline (44cf25f) → direct kubectl patch to inject bad config → Prometheus-based AnalysisRun **Failed** ([1, 0.5, 0]) → **RolloutAborted** → bad RS **0/0**, stable RS **4/4 Ready**, stable HTTP **200** → Argo CD refresh → **Synced/Healthy on fourth attempt**.

This is evidence of **deployment-risk containment at the stable-service probe**, not proof of zero application impact across real clients. A separate successful post-abort AnalysisRun is not established.

## Technology, with evidence scope

**Observed or configured in bounded exercises:** Kubernetes (Kind), Docker, local bare Git/GitOps, Argo CD, Argo Rollouts, Istio, Prometheus with Blackbox exporter, OpenTelemetry Collector, Loki, Tempo, Grafana and Thanos (current-data continuity only). **Historical operations/configuration artifacts:** systemd and PowerShell/SSH.

**Designed or reviewed rather than independently proven here:** other platform components, large-scale high availability, production multi-cluster operations, extended storage retention, and general-purpose policy/checkpoint coordination.

## Read the evidence

[Architecture](docs/ARCHITECTURE.md) · [Delivery experiment](docs/PROGRESSIVE_DELIVERY.md) · [Operating decisions](docs/DESIGN_DECISIONS.md) · [Control principles](docs/OPERATION_PRINCIPLES.md) · [Evidence index](evidence/07-verification-summary.md) · [Security](SECURITY.md) · **[Limitations](docs/LIMITATIONS.md)**

This repository presents laboratory case studies and historical personal-service operations; it does not establish a production deployment.
