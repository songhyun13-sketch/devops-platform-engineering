# DevOps / Platform Engineering 포트폴리오 (한국어)

GitOps, Canary 자동 중단, Collector 장애 주입, 운영 기록에 관한 **서로 독립된 검증 사례**를 정리한 포트폴리오입니다.

**역할:** 실험 설계, 증거 범위 설정, 결과 검수와 검증 판단을 직접 수행했습니다. 구현 일부는 AI 도구의 보조를 받았습니다.

## 핵심 구현·검증 결과

- Argo CD·Argo Rollouts·Prometheus/Blackbox 기반 Kubernetes 점진 배포 실험을 구성하고 오류 설정을 주입해, 지표 기반 자동 중단과 중단 후 안정 ReplicaSet **4/4 Ready**, HTTP **200**을 확인했습니다.
- Istio 가중치 라우팅을 각 **100회 요청**으로 측정하고, OpenTelemetry Collector를 **1→0→1**로 장애 주입/복구하여 probe 및 텔레메트리 복구를 확인했습니다.
- PowerShell/SSH·systemd 기반 개인 EC2 자동화 서비스 운영 기록을 정리하되, 프로덕션 SLO나 가용성 개선 수치로 과장하지 않았습니다.

**사례 표기:** S6 = Kind GitOps/Canary 및 별도 Collector 실험, P2 = 별도 Istio/관측성 실험.

## 상세 검증 사실

| 구분 | 실제 확인된 값과 범위 | 시각 |
|---|---|---|
| S6 / Kind | 로컬 Git 정상 기준 44cf25f → kubectl patch 오류 설정 주입. 오류 Git 커밋 배포가 아님 | 2026-09-22 UTC |
| S6 / Canary | setWeight 설정값 **25%**(실제 요청 비중 미측정), AnalysisRun **Failed**, 측정 **[1, 0.5, 0]**, 총 **3회** / **10초** 간격 / failureLimit **1** | 2026-09-22 00:32:14~00:32:34 UTC |
| S6 / 중단 | RolloutAborted, 오류 RS **0/0**, 안정 RS **4/4 Ready**, 안정 서비스 HTTP **200** | 2026-09-22 00:32:34 UTC, 확인 00:34:33 UTC |
| S6 / Argo CD | 첫 **3회** OutOfSync/Degraded, **4회차** Synced/Healthy | 2026-09-22 UTC |
| S6 / 한정 평가 | 평가 **12/12 PASS**, 번들 SHA **8/8** 일치, 기존 소스 검사 **7개 파일 / 탐지 0건** | 2026-09-22 UTC, 번들 00:48:46 UTC |
| Collector 장애 (별도) | replica **1→0→1**, probe **1→0→1**, curl rc **7** → HTTP **200** | 2026-09-21 23:21:50 UTC |
| Istio P2 (별도) | 설정 **80:20 → 관측 75:25**, 설정 **30:70 → 관측 30:70**; 각 **100회 요청** | 2026-09-21 날짜 기록 |
| EC2 개인 서비스 | PowerShell/SSH 배포 스크립트, systemd 재시작 설정, 원격 운영기록 동기화 | 2026-04-22~05-02 기록 |

## 분리된 실험 지도

EC2 개인 서비스 운영 / Kind GitOps·Canary(S6) / Istio 트래픽 분산(P2) / Collector 장애 주입(S6 별도) / 관측 스냅샷(P2 별도) — 서로 항시 연결되어 운영된 통합 배포 환경이 아니며 **AWS EKS 운영 사례가 아닙니다.**

대표 S6 시퀀스는 정상 Git → kubectl patch → AnalysisRun Failed → RolloutAborted → 안정 RS 유지 및 HTTP 200 → Argo CD 재동기화로 한정합니다. 모든 클라이언트에 대한 무영향성이나 복구 후 AnalysisRun 재성공은 증명하지 않았습니다.

**한정된 실험에서 관측·구성:** Kubernetes(Kind), Docker, 로컬 GitOps, Argo CD, Argo Rollouts, Istio, Prometheus/Blackbox, OpenTelemetry Collector, Loki, Tempo, Grafana, Thanos(현재 데이터 연속성 범위). **과거 운영·설정 증빙:** systemd, PowerShell/SSH.

**설계·검토 범위:** 대규모 고가용성, 프로덕션 다중 클러스터, 장기 보관, 별도 작업 제어 원칙 및 기타 미검증 요소.

[아키텍처](docs/ARCHITECTURE.md) · [실패/중단 과정](docs/PROGRESSIVE_DELIVERY.md) · [의사결정](docs/DESIGN_DECISIONS.md) · [증거](evidence/07-verification-summary.md) · **[한계](docs/LIMITATIONS.md)**
