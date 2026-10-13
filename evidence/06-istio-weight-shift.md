# P2 Istio routing: measured sample distribution

| VirtualService configured weights (v1:v2) | Observed replies (v1:v2) | Request sample |
|---|---|---|
| **80:20** | **75:25** | **n=100** |
| **30:70** (after kubectl patch) | **30:70** | **n=100** |

- **Measurement:** counts of responses to requests from one sidecar-injected client pod to the service in the P2 Istio experiment.
- **Date:** **2026-09-21**; sample clock time not recorded.
- **Interpretation:** for Binomial(n=100, p=0.8) the standard deviation is 4; **75** is 1.25 standard deviations from the mean of 80.
- **Samples:** two runs, n=100 each.

Source type/name: P2_ISTIO_EVIDENCE_20260921.md; observation: P2 evidence dated 2026-09-21; exact sample clock not recorded; SHA-256 prefix: 5addd910ca87.

[Architecture map](../docs/ARCHITECTURE.md).
