# S6 observed GitOps/Canary sequence

~~~mermaid
flowchart TB
    A["Healthy local Git baseline 44cf25f"] --> B["kubectl patch: bad config"]
    B --> C["Configured Rollouts weight 25 percent"]
    C --> D["AnalysisRun: Failed, [1, 0.5, 0]"]
    D --> E["RolloutAborted"]
    E --> F["Bad RS 0/0; Stable RS 4/4; stable-service HTTP check (1 run) 200"]
    F --> G["Refresh annotation + Application patch, 4 state checks"]
    G --> H["Checks 1-3 OutOfSync/Degraded; check 4 Synced/Healthy"]
~~~

Weight 25 is the configured Rollouts value; the successful canary AnalysisRun (2026-09-22 00:30:18 to 00:30:38 UTC) was recorded before the abort.
