# Architecture map: experiments on one local Kind cluster and a separate EC2 record

See [environment diagram](../diagrams/environments.md). The Kind GitOps/Canary (S6), Istio (P2) and Collector (S6) experiments ran separately, at different times and for different purposes, on the same local Kind cluster (S6: 2026-09-21 23:03 UTC to 2026-09-22 00:49 UTC; P2: 2026-09-21). The EC2 record comes from a separate VM. The map groups the experiments by purpose.

| Experiment | Components observed or configured | Scope |
|---|---|---|
| A: EC2 personal automation workload | PowerShell, SSH/SCP, systemd, operations record synchronization | Service deployment configuration and historical logs (2026-04-22 to 2026-05-02) |
| B: Kind GitOps/Canary S6 | Local bare Git, Argo CD, Argo Rollouts, Kubernetes, Prometheus/Blackbox exporter | Bad configuration introduced with kubectl patch after healthy Git baseline (2026-09-22) |
| C: Istio P2 | VirtualService, destination subsets, sidecar-routed sample requests | Routing distribution experiment, n=100 requests per weight setting (2026-09-21) |
| D: Collector fault S6 | Kubernetes scale, OpenTelemetry Collector, Prometheus, Loki, Tempo | Fault injection and restoration, 2026-09-21 23:19:10 to 23:19:49 UTC, before the S6 Canary run |

Policy, validation, pause-on-uncertainty and checkpoint practices are documented as a [design principle](OPERATION_PRINCIPLES.md).

**Thanos scope:** S6 current-data query continuity passed in the second apply cycle after a correction.
