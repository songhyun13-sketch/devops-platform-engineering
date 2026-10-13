# Incident and recovery evidence

## Personal EC2 service: historical operations record

A personal automation workload ran on a single EC2 VM and was deployed by a PowerShell script that calls SSH/SCP. The service unit sets systemd Restart=on-failure with RestartSec=30, and the deployment script installs two monitoring units (a record monitor and a main-cycle monitor). The operations-log synchronization log covers **2026-04-22 through 2026-05-02** and records remote command failures with exit **255** on 2026-04-22, 2026-04-27 and 2026-05-02.

Two incidents dated **2026-03-21** (KST) are recorded in a **backfill script** (times in KST):
- Incident A: from **14:39:16** to **16:04:00 KST**, which is **84 min 44 s**. The script marks the end time as an estimated upper bound.
- Incident B: from **18:15:00** to **18:35:00 KST**, which is **20 minutes**. The script takes the start from the time the local cause was confirmed and the end from the first post-fix row in the test operations log.

## Laboratory fault: Collector

The S6 Collector exercise (2026-09-21 23:19:10 to 23:19:49 UTC) took a Collector deployment from **1 → 0 → 1** replicas. The Prometheus observation went **1 → 0 → 1**; during the gap direct curl returned **rc=7**, and after restoration the HTTP check returned **200**. The pre/post pipeline test observed ingestion markers in Loki and Tempo.

The recorded remote command wrapper had an exit-status anomaly; see [limitations](LIMITATIONS.md). This laboratory fault test ran in the local Kind cluster; the EC2 incidents and the S6 bad-Canary scenario are separate records.

[Collector source summary](../evidence/05-observability-fault-recovery.md).
