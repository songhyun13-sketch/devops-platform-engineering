# DevOps / Platform Engineering 포트폴리오

GitOps, Canary 자동 중단, Collector 장애 주입, 운영 기록에 관한 검증 사례를 정리한 포트폴리오입니다. Kind GitOps/Canary·Istio·Collector 실험은 같은 로컬 Kind 클러스터에서 목적과 시점을 달리해 따로 실행했고(S6: 2026-09-21 23:03 ~ 09-22 00:49 UTC, P2: 2026-09-21), EC2 개인 서비스는 별도 VM의 기록입니다.

**역할:** 실험 설계·증거 범위 설정·결과 검수와 검증 판단은 본인이 했고, 구현·명령 실행·증거 수집은 AI 에이전트(Codex 등)를 지휘해 수행했습니다.

## 핵심 구현·검증 결과

- Argo CD·Argo Rollouts·Prometheus/Blackbox 기반 Kubernetes 점진 배포 실험을 구성하고 오류 설정을 주입해, 지표 기반 자동 중단과 중단 후 안정 ReplicaSet **4/4 Ready**, HTTP **200**을 확인했습니다.
- Istio 가중치 라우팅을 각 **100회 요청**으로 측정하고, OpenTelemetry Collector를 **1→0→1**로 장애 주입/복구하여 probe 및 텔레메트리 복구를 확인했습니다.
- EC2 개인 서비스: 배포 스크립트(PowerShell/SSH)·systemd 재시작 설정·운영 기록 동기화(2026-04-22~05-02)를 정리했습니다.

**사례 표기:** S6 = Kind GitOps/Canary 및 Collector 장애 주입 실험(Collector는 Canary보다 먼저 실행), P2 = Argo CD 동기화·정상/오류 canary·Istio 가중치를 포함한 2026-09-21 실험.

## 상세 검증 사실

| 구분 | 실제 확인된 값과 범위 | 시각 |
|---|---|---|
| S6 / Kind | 로컬 Git 정상 기준 44cf25f → kubectl patch로 오류 설정 주입 | 2026-09-22 UTC |
| S6 / Canary | setWeight 설정값 **25%**(P2에서 canary 1 / 안정 3 파드 관측, 요청 비중은 측정하지 않음), AnalysisRun **Failed**, 측정 **[1, 0.5, 0]**, 총 **3회** / **10초** 간격 / failureLimit **1** | 2026-09-22 00:32:14~00:32:34 UTC |
| S6 / 중단 | RolloutAborted, 오류 RS **0/0**, 안정 RS **4/4 Ready**, 안정 서비스 HTTP 확인 1회 **200** | 2026-09-22 00:32:34 UTC, 요약 검증 00:34:33 UTC |
| S6 / Argo CD | annotate·patch 1회 후 상태 조회 4회: 1~3회 OutOfSync/Degraded, 4회 Synced/Healthy | 2026-09-22 UTC |
| S6 / 증거 번들 | 번들 SHA-256 **8/8** 일치 | 2026-09-22 00:48:46 UTC |
| Collector 장애 (S6) | replica **1→0→1**, probe **1→0→1**, curl rc **7** → HTTP **200** | 2026-09-21 23:19:10~23:19:49 UTC, 확인 23:21:50 UTC |
| Istio P2 | 설정 **80:20 → 관측 75:25**, 설정 **30:70 → 관측 30:70**; 각 **100회 요청** | 2026-09-21 날짜 기록 |
| EC2 개인 서비스 | PowerShell/SSH 배포 스크립트, systemd 재시작 설정, 원격 운영기록 동기화 | 2026-04-22~05-02 기록 |

## 실험 지도

EC2 개인 서비스 운영(별도 VM 기록) / Kind GitOps·Canary(S6) / Istio 트래픽 분산(P2) / Collector 장애 주입(S6) — 뒤의 세 실험은 같은 로컬 Kind 클러스터에서 따로 실행했습니다.

대표 S6 시퀀스는 정상 Git → kubectl patch → AnalysisRun Failed → RolloutAborted → 안정 RS 유지 및 HTTP 200 → Argo CD 재동기화입니다. abort 후 확인은 안정 서비스 HTTP 확인 1회 200, 안정 RS 4/4 Ready이고, abort 후 AnalysisRun 기록은 없습니다. 정상 canary AnalysisRun은 abort 전(2026-09-22 00:30:18~00:30:38 UTC)에 성공했습니다.

**실험에서 사용한 구성:** Kubernetes(Kind), Docker, 로컬 GitOps, Argo CD, Argo Rollouts, Istio, Prometheus/Blackbox, OpenTelemetry Collector, Loki, Tempo, Grafana, Thanos(현재 데이터 연속성 범위). **과거 운영·설정 증빙:** systemd, PowerShell/SSH.

[아키텍처](docs/ARCHITECTURE.md) · [실패/중단 과정](docs/PROGRESSIVE_DELIVERY.md) · [의사결정](docs/DESIGN_DECISIONS.md) · [증거](evidence/07-verification-summary.md) · **[한계](docs/LIMITATIONS.md)**
