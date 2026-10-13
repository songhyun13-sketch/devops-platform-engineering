# S6 separate fault: OpenTelemetry Collector

This is a **separate S6 exercise**, not a component failure caused by the GitOps Canary experiment.

| Signal | Before | During fault | After restore |
|---|---|---|---|
| Collector replica count | **1** | **0** | **1** |
| Prometheus probe_success | **1** | **0** | **1** |
| Direct connectivity check | Baseline observed | **curl rc=7** | **HTTP 200** |
| Telemetry submissions | **HTTP 200** OTLP pre-submission | No continuous delivery claim | **HTTP 200** OTLP post-submission |
| Log/trace markers | Loki and Tempo markers present | No signal preservation claim | Loki and Tempo markers present |

**Result scope:** recorded status PASS for the isolated scale-down/restore checks, verified **2026-09-21 23:21:50 UTC**.

**Wrapper caveat:** recorded remote command exit **1** while EXIT trap returned **0**. This was classified in the evidence as a wrapper-status artifact, not functional evidence of Collector failure beyond the deliberately introduced scale-down.

Source type/name: observability-fault-runtime.json; observation: verified 2026-09-21 23:21:50 UTC; SHA-256 prefix: a6e8ab35997d.

[Limitations](../docs/LIMITATIONS.md).
