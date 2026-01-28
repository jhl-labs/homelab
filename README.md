# 🏠 Homelab 인프라 소개

> **“집에서 만드는 개발 클러스터, 자유로운 실험과 연구를 위한 개인 인프라”**

---

## 🎯 Homelab의 목적

- **자유로운 개발 및 실험 환경**을 구축하여, 기술 탐구, 프로토타이핑, 연구 활동을 집에서도 가능하게 함
- **실무에 적용 가능한 인프라 경험**을 쌓기 위한 테스트베드 역할
- **개인 기술 스택 확장 및 스타트업 준비**를 위한 풀스택 기술 연습장

---

## 🌐 네트워크 구성

- **Synology 라우터** 기반의 엔터프라이즈급 네트워크 서비스 제공
  - AD 인증, 트래픽 모니터링, VLAN 관리 등 기업 수준의 네트워크 제어
- **하이브리드 스위칭 환경** (10G / 2.5G / 1G)으로 고부하 트래픽 처리 가능
  - 고속 데이터 전송 및 저지연 환경 구현

---

## 💾 스토리지 아키텍처

### 1. **Proxmox 기반 Ceph 클러스터**
- **클러스터 구성**: 3 MON, 3 MGR, 11 OSD
- **스토리지 타입**: NVMe (RWO/RWX) + HDD (RWO/RWX) 혼합
- **스토리지 용량**: NVMe 2TB × 3, HDD 12TB × 1 → 총 18TB 이상의 클러스터 풀
- **기능 지원**: S3 호환 객체 저장소, 블록/파일 스토리지, 고가용성

### 2. **Synology NAS 기반 iSCSI 스토리지**
- **SSD 기반 (10G)**: SATA SSD 2TB × 3 → 고속 블록 스토리지
- **HDD 기반 (1G)**: 4TB × 8 → 대용량 백업/아카이브 용도

---

## 🐳 Kubernetes 클러스터

### 1. **Dev 클러스터 (RKE2 기반)**
- **CPU**: 106코어
- **RAM**: 373GB
- **GPU**: 4개 (총 104GB VRAM) — RTX 5090, 4090, 3090×2 (NVLink)
- **용도**: 개발, 테스트, AI 모델 학습 및 실험

### 2. **Prod 클러스터 (RKE2 기반)**
- **CPU**: 32코어
- **RAM**: 47GB
- **용도**: 안정적인 운영 환경, CI/CD 배포 대상

---

## 🤖 주요 로컬 AI 모델

- **Ollama 기반 모델**
  - `gpt-oss:120b` — 오픈소스 기반 대형 언어 모델
  - `qwen3-vl:235b` — 멀티모달 지원, 시각-언어 통합 모델

---

## 💳 구독 중인 상용 AI 도구 및 IDE

| 도구 | 구독 플랜 | 용도 |
|------|-----------|------|
| **Claude Code** | MAX+ ($200/월) | 다중 세션 개발, 코드 생성 및 리팩토링 |
| **Antigravity** | Google AI Pro (1년 구독) | AI 기반 개발 보조, 코드 분석 및 제안 |
| **Cursor** | PRO+ | AI 통합 IDE, 로컬/원격 개발 환경 |

---

## 💻 주요 개발 환경

### 🐧 Linux 환경 (Ubuntu 22.04)

- **메인 워크스테이션**
  - CPU: AMD Ryzen Threadripper 7960X (24C/48T, 4.2~5.3GHz)
  - RAM: Samsung 128GB DDR5 ECC (32GB × 4)
  - GPU: RTX 5090 (32GB) + RTX 4090 (24GB) + RTX 3090×2 (24GB, NVLink)
- **보조 노드**
  - AMD 5600X, 5800X, 7735HS, 5560U, 5600H, Intel i5-12600, i5-12400 등
  - *전기세 문제로 100% 가동은 아직 미흡 😅*

### 🪟 Windows 환경

- **Dell Workstation 3680T**
  - CPU: i7-14700K, RAM: 96GB DDR5, GPU: RTX 3080 (10GB)
  - 용도: 원격 데스크톱 개발 환경
- **조립 PC**
  - CPU: AMD 7800X3D, RAM: 128GB DDR5, GPU: RTX 3080 Ti (12GB)
  - 용도: Windows Hosted Runner, 자동화 및 운영 환경
- **Galaxy Book5 Pro 16인치 (226V, 32GB)**
  - 용도: 침대에서의 “바이브 코딩” — 원격 접속용

### 🍏 Mac 환경

- **MacBook Pro M4 (24GB)**
  - 용도: 외부 컨퍼런스/카페/세미나 — **메인 이동용**
- **MacBook Pro M1 Max (32GB)**
  - 용도: 책상에서의 시즈모드 — **M4에서 원격 접속용**

---

## 🛠️ 기타 인프라

- **GitHub Team + Copilot 라이선스** → 1인 GitHub Organization 운영
- **Jira Server / Confluence Server** 라이선스 보유 → 내부 프로젝트 관리
- **Self-hosted GitHub Actions Runner** → 10~50개 노드로 CI/CD 자동화

---

## 🧩 주요 개발 방식

- **Linux 워크스테이션에서 SSH 기반 개발** 또는 **Windows 원격 데스크톱** 활용
- **Claude Code**를 통한 **10~15개 동시 세션** 운영 → `tmux/screen`보다 편리한 멀티세션 경험
- **Cursor / Antigravity**를 활용한 애플리케이션 개발 → 로컬 빌드/테스트 후 GitHub Actions로 CI/CD 자동화
- **Web Service 개발 프로세스**
  1. `docker-compose`로 로컬 개발
  2. `Helm + ArgoCD`로 GitOps 방식으로 Dev 클러스터 배포
  3. 안정화 후 Prod 클러스터로 동일 방식 배포
- **Claude Code**를 통한 개발/운영 자동화 → [참고: SDLG History](https://jhl-labs.github.io/sdlc-history/)

---

## 🚀 주요 개발 프로젝트

| 프로젝트 | 공개 여부 | 설명 |
|----------|-----------|------|
| **sepilot desktop** | 공개 (사내 프로젝트: `dsdn-desktop`) | GUI 기반 AI Agent 애플리케이션 — Claude Desktop과 유사 |
| **euno.news** | 부분공개 | GitHub Actions 기반 외부 커뮤니티 글 수집 → 번역/요약 → GitHub Pages 공개 |
| **sepilot cli** | 비공개 | Python 기반 CLI Agent — Claude Code 비용 절감을 위한 로컬 운영용 |
| **sepilot wiki** | 비공개 | Git repo 스캔 + 외부 지식 탐색 + K8s 변화 감지 → 문서화 (DeepWiki 참고) |
| **sepilot ssh** | 비공개 | AI 협력 기반 웹 SSH 클라이언트 — Warp Terminal을 목표로 개발 |
| **jhl-space SaaS / IDP** | 비공개 | S3, 인증, 네트워크, 스토리지, 보안, GitOps 관리용 SaaS |
| **jtube** | 비공개 | YouTube 미러 서비스 — AI 자막 번역/요약 → 자녀 콘텐츠 통제 및 큐레이션 |
| **the pics** | 비공개 | 인스타그램 컨셉 사진 공유 플랫폼 — Google Photos의 AI 분류 부족 보완 |
| **jhl-search** | 비공개 | 내부/외부 개발 리소스 크롤링 → OpenSearch 기반 시맨틱 검색 (RAG + MCP 연동) |
| **jhl-tables** | 비공개 | 웹 기반 Excel — Python 함수 + AI Agent 기능 통합 → 사내 엑셀 대체 |

---

## 🤔 왜 이렇게 하는가?

- **직무 변화에 대응하기 위한 기술 확장**
  - 네트워크 → IoT → SE 품질 → 인프라 → DevOps → 플랫폼 → DevRel
- **회사 내 부족한 환경에 대한 ‘분노의 개발’**
  - “이런 것도 못하면서 무슨 SW 개발을 해?”라는 마음으로 만들어 조직 내 공유
- **(전) SE 엔지니어로서의 사명감**
  - 선진 기술 도입 및 확산을 위한 ‘기술 에반젤리즘’
- **스타트업 창업을 위한 기술 준비**
  - 닫힌 기업 기술이 아닌, **열린 기술 스택**을 지속적으로 학습

---

## 📈 앞으로의 계획

1. **sepilot desktop & sepilot cli 오픈소스 공개** → 그 외 도구들의 공개 여부 결정
2. **바이브코딩으로 만든 프로젝트의 지식/스킬을 완전히 내 것으로 흡수**
3. **제2의 부가가치 창출** → 성공적인 자녀 교육을 위한 AI Agent 개발 및 적용

---

> ✨ **“기술은 끝이 없고, 호기심은 끝이 없다. 집에서 시작하는 인프라가, 내일의 혁신이 된다.”**

---
