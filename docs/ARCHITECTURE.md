# Architecture map: deliberately separated environments

See [environment diagram](../diagrams/environments.md). This is a **map of distinct exercises**, not a data-flow graph connecting their systems.

| Exercise | Components observed or configured | Boundary |
|---|---|---|
| A: EC2 personal automation workload | PowerShell, SSH/SCP, systemd, operations record synchronization | Service deployment configuration and historical logs; no availability measurement |
| B: Kind GitOps/Canary S6 | Local bare Git, Argo CD, Argo Rollouts, Kubernetes, Prometheus/Blackbox exporter | Bad configuration introduced with kubectl patch after healthy Git baseline |
| C: Istio P2 fixture | VirtualService, destination subsets, sidecar-routed sample requests | Independent routing distribution experiment |
| D: Collector fault S6 | Kubernetes scale, OpenTelemetry Collector, Prometheus, Loki, Tempo | Separate fault injection and restoration, not the S6 Canary request path |
| E: Observability snapshots P2 | Recorded metrics, logs, traces and dashboards | Separate dated snapshots, not S6 fault measurements |

The cases were **not continuously interconnected**, are not evidence of AWS EKS operations and do not establish a commercial environment. Any policy, validation, pause-on-uncertainty or checkpoint framework is documented exclusively as a [design principle](OPERATION_PRINCIPLES.md), **not** as a wired part of Argo CD or Argo Rollouts.

**Thanos boundary:** S6 current-data query continuity passed after a correction; remote object storage, retention duration and disaster recovery are not proven.
