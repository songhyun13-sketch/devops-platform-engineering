# S6 observed GitOps/Canary sequence

~~~mermaid
flowchart TB
    A["Healthy local Git baseline 44cf25f"] --> B["kubectl patch: bad config"]
    B --> C["Configured Rollouts weight 25 percent"]
    C --> D["AnalysisRun: Failed, [1, 0.5, 0]"]
    D --> E["RolloutAborted"]
    E --> F["Bad RS 0/0; Stable RS 4/4; HTTP 200 probe"]
    F --> G["Argo CD refresh and normalization"]
    G --> H["Try 4: Synced / Healthy"]
~~~

The diagram does not assert measured Canary traffic or a separate successful analysis after recovery.
