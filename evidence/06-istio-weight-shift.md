# P2 separate routing fixture: measured sample distribution

| VirtualService configured weights (v1:v2) | Observed replies (v1:v2) | Request sample |
|---|---|---|
| **80:20** | **75:25** | **n=100** |
| **30:70** (after kubectl patch) | **30:70** | **n=100** |

- **Measurement:** counts of responses from in-mesh clients to the service in the separate P2 fixture, **not** Argo Rollouts setWeight observations from S6.
- **Date:** **2026-09-21**; original narrative does not supply an exact wall-clock sample time.
- **Interpretation:** **75:25** for n=100 against an **80:20** configured split is plausible sampling variability; it is not a failed routing configuration.
- **Boundary:** two bounded response samples do not establish a sustained traffic proportion or cross-environment wiring.

Source type/name: P2_ISTIO_EVIDENCE_20260921.md; observation: P2 evidence dated 2026-09-21; exact sample clock not recorded; SHA-256 prefix: 5addd910ca87.

[Environment separation](../docs/ARCHITECTURE.md).
