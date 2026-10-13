# S6 separate fault: OpenTelemetry Collector

This S6 exercise ran in the local Kind cluster on 2026-09-21, before the S6 GitOps/Canary run (2026-09-22); the fault was a deliberate scale-down of the Collector.

| Signal | Before | During fault | After restore |
|---|---|---|---|
| Collector replica count | **1** | **0** | **1** |
| Prometheus probe_success | **1** | **0** | **1** |
| Direct connectivity check | **HTTP 200** (precondition check) | **curl rc=7** | **HTTP 200** |
| Telemetry submissions | **HTTP 200** OTLP pre-submission | no record | **HTTP 200** OTLP post-submission |
| Log/trace markers | Loki and Tempo markers present | no record | Loki and Tempo markers present |

**Result:** recorded status PASS for the scale-down/restore checks. Sequence on 2026-09-21: scale-down **23:19:10 UTC**, restore start **23:19:28 UTC**, recovery PASS **23:19:49 UTC**; runtime summary verified **23:21:50 UTC**.

**Wrapper caveat:** the remote command wrapper reported exit **1** in the S6 run (observed fault command exit code 1); in a separate isolated reproduction the EXIT trap printed `TRAP_RC=0`. The evidence classifies this as a wrapper-status artifact.

Source type/name: observability-fault-runtime.json; observation: verified 2026-09-21 23:21:50 UTC; SHA-256 prefix: a6e8ab35997d.

[Limitations](../docs/LIMITATIONS.md).
