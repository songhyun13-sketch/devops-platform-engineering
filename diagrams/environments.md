# Experiment map: one local Kind cluster plus an EC2 record

~~~mermaid
flowchart TB
  subgraph A["A: EC2 personal workload (separate VM, historical)"]
    A1["PowerShell / SSH"] --> A2["systemd service"] --> A3["Operations records"]
  end
  subgraph K["Local Kind cluster: experiments run separately"]
    subgraph B["B: GitOps / Canary (S6)"]
      B1["Local bare Git"] --> B2["Argo CD"] --> B3["Rollouts + probe"]
    end
    subgraph C["C: Istio (P2)"]
      C1["VirtualService weights"] --> C2["Observed response counts"]
    end
    subgraph D["D: Collector fault (S6)"]
      D1["Replica scale-down/up"] --> D2["Probe + telemetry markers"]
    end
  end
~~~

The experiments B, C and D ran at different times on the same local Kind cluster; A is a record from a separate VM.
