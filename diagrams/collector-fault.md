# S6 Collector failure/restore

~~~mermaid
flowchart LR
    P["Before: Collector 1, probe 1"] --> G["Injected gap: Collector 0, probe 0, curl rc 7"]
    G --> Q["Restored: Collector 1, probe 1, HTTP 200"]
    Q --> M["Pre/post Loki and Tempo markers confirmed"]
~~~

The Collector exercise ran on 2026-09-21 (23:19:10 to 23:19:49 UTC) in the same local Kind cluster as the S6 GitOps/Canary and P2 experiments, at a different time.
