# Incident and recovery evidence

## Personal EC2 service: historical operations record

A personal automation workload ran on a single cloud VM and was deployed by a PowerShell orchestration script invoking SSH/SCP. The service unit specified systemd Restart=on-failure, and the deployment script included two recording/health-cycle monitoring units. An operations-log synchronization record covers **2026-04-22 through 2026-05-02**. Later log entries contain remote command failures with exit **255**; this is not proof of successful remote recovery or a measured outage duration.

Two incidents are present in a historical **backfill script** dated **2026-03-21 KST**, rather than a continuous online incident-management measurement:
- Incident A: from **14:39:16** to estimated upper-bound **16:04:00 KST**, or approximately **84m 44s**. Completion time is an inference, not a fully logged recovery timestamp.
- Incident B: approximated **18:15 to 18:35 KST**, about **20 minutes**; both ends are partially inferred.

No mean restoration-time improvement, service availability percentage or operational objective is established. The script is evidence of the incident-recording approach, not proof that automated recovery succeeded in these incidents.

## Separate laboratory fault: Collector

An independent S6 exercise took a Collector deployment from **1 → 0 → 1** replicas. The Prometheus observation went **1 → 0 → 1**; during the gap direct curl returned **rc=7**, and after restoration the HTTP check returned **200**. The pre/post pipeline test observed ingestion markers in Loki and Tempo.

The recorded remote command wrapper had an exit-status anomaly; see [limitations](LIMITATIONS.md). This is a distinct laboratory fault test, not the EC2 incident or the S6 bad-Canary scenario.

[Collector source summary](../evidence/05-observability-fault-recovery.md).
