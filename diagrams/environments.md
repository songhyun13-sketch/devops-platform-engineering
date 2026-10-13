# Distinct environments: no implied interconnect

~~~mermaid
flowchart TB
  subgraph A["A: EC2 personal workload (historical)"]
    A1["PowerShell / SSH"] --> A2["systemd service"] --> A3["Operations records"]
  end
  subgraph B["B: Kind GitOps / Canary (S6)"]
    B1["Local bare Git"] --> B2["Argo CD"] --> B3["Rollouts + probe"]
  end
  subgraph C["C: Istio fixture (P2)"]
    C1["VirtualService weights"] --> C2["Observed response counts"]
  end
  subgraph D["D: Collector fault (S6 separate)"]
    D1["Replica scale-down/up"] --> D2["Probe + telemetry markers"]
  end
  subgraph E["E: P2 observability snapshots"]
    E1["Recorded metrics/logs/traces"] --> E2["Snapshot assessment"]
  end
~~~

The diagram intentionally has no edges between distinct subgraphs; it does not represent one production pipeline or an AWS EKS deployment.
