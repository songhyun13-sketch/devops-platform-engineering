# Isolated S6 Collector failure/restore

~~~mermaid
flowchart LR
    P["Before: Collector 1, probe 1"] --> G["Injected gap: Collector 0, probe 0, curl rc 7"]
    G --> Q["Restored: Collector 1, probe 1, HTTP 200"]
    Q --> M["Pre/post Loki and Tempo markers confirmed"]
~~~

This Collector exercise is separate from the S6 GitOps/Canary and from the P2 snapshot.
