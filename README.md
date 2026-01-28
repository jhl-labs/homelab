# Homelab 인프라 소개

## Homelab의 목적
- 집 환경에서 개발 클러스터를 구성하여 자유로운 개발 및 실험, 연구를 진행

## 네트워크 소개
- Synology Router를 통한 Enterprise 서비스 지원
  - AD 인증, 트래픽 모니터링, VLAN 관리
- 10G / 2.5G / 1G Switch 의 하이브리드 사용으로 고부하 네트워크 트래픽 처리

## 스토리지 구성
- Proxmox 를 이용한 Ceph 클러스터 규송
  - 3 MON, 3 MGR, 11 OSD 를 통한 클러스터 스토리지 제공 (nvme / hdd 기반 RWO, RWX 지원), s3 지원
- Synology NAS 기반 iscsi 지원 (SATA SSD 기반 10G (2TB x 3),   HDD 기반 1G (4TB x 8)
  
## k8s 클러스터 소개
  - rke2 기반 Dev 클러스터 (106 cores, 373GB RAM,  4GPU[104GB VRAM]  )
  - rke2 기반 Prod 클러스터 (32 cores, 47GB RAM)

## 주 사용 로컬 AI 모델
  - ollama
    - gpt-oss:120b
    - qwen3-vl:235b

## 현재 구독하는 상용 AI 모델 및 Agent, IDE
  - claude-code  (MAX+ $200 Plan)
  - antigravity  (Google AI Pro 1년 구독)
  - cursor (PRO+)

## 주요 개발 환경
### Linux 환경
  - AMD Ryzen Threadripper 7960X (24C / 48T) 4.2GHz~5.3GHz
  - Sasmsung 128GB DDR5 ECC (32gb x 4)
  - RTX5090 (32GB) + RTX4090 (24GB) + RTX3090x2(24GB with nvlink)
  - Ubuntu 22.04 

### Windows 환경
  - Dell Workstaiton 3680T --> Intel i7-14700k, 96GB DDR5 RAM, 10GB VRAM(RTX 3080), Windows 11 Pro, 원격 데스크톱용
  - 조립 PC --> AMD 7800X3D, 128GB DDR5 RAM, 12GB VRAM(RTX 3080ti), Windows 11 Pro, Windows Hosted Runner 및 자동화용, 운영 환경으로 적용 예정

### 기타 인프라
  - github team license +copilot license 를 통해 1인 GitHub Org 운영중
  - Jira Server / Confluence Server 라이선스 보유 중
  - github actions를 위한 self hosted runner를 10-50개 등록하여 GitHub Org 운영. 

## 주요 개발 방식
  - linux workstation에 ssh 환경 내에서 개발 또는 윈도우 원격 데스크톱으로 개발 진행
  - claude code를 통하여 다중 세션 (10~15개)를 윈도우 원격 환경에서 띄운 후 개발 진행  <-  세션이 안끊어짐. 개인적으로 tmux/screen 보다 더 편한 사용성..
  - application 개발은 cursor/antigravity를 이용하여 개발.  로컬 빌드 및 테스트를 수행하나 CI/CD 적용으로 백그라운드로 github actions로 release 패키지 생성
  - web service 개발은 claude code를 통해 리눅스 환경에서 진행.
      - docker-compose 기반으로 1차 개발 수행 -> helm/argocd를 통해 gitops로 내부 개발 클러스터에 배포 -> 안정적 운영 시 같은 방법으로 운영 클러스터 배포
      - claude code의 적극적인 개발/운영에 활용 (by github actions); 참고: https://jhl-labs.github.io/sdlc-history/

## 주요 개발 프로젝트
  - (공개) sepilot desktop (claude desktop과 같은 다목적 gui 기반 ai agent 어플리케이션)
  - (비공개) sepilot cli (python 기반의 cli agent, claude code가 비싸서 로컬 운영에서 사용하려고 개발)
  - (비공개) sepilot wiki (내부 지식 문서에 대해 git repo 스캔 및 외부 지식 자동 탐색, k8s 인프라 변화 감지를 문서화, deepwiki를 참고)
  - (비공개) sepilot ssh (warp 터미널을 꿈꾸고, 만든 ai 협력 기반의 웹 ssh client 서비스)
  - (비공개) jhl-space 용 SaaS / IDP 서비스 (s3, 인증, 네트워크, 스토리지, 노드, 리소스, 보안, gitops 관리용 SaaS 서비스들)
  - (비공개) jtube (youtube mirror 서비스, AI 자막 번역, 요약을 위해 제작, 개발 목적은 자녀의 동영상 스트리밍 통제, 적절한 큐레이션 제공 목적)
  - (비공개) the pics (인스타그램 컨셉 자체 사진 공유 플랫폼,  개발 목적은 Google Photos의 ai 분류 기능이 너무 보급형이라 가족 사진들(특히 어린이집 사진)을 제대로 인물 분류 / 장소 분류 시키게 하려고
  - (비공개) jhl-search (내부 개발 리소스[github, confluence] 및 외부 개발 정보(hackersnews, google, meta 등)에 대한 크롤링 정보에 대한 opensearch 기반의 시맨틱 웹 검색 환경 제공.  자체적인 google 검색엔진 생태계를 만들고자 개발함 (RAG 연동 및 MCP 활용 목적)
  - (비공개) jhl-tables (회사 내부의 excel 환경에 불만을 품고, 웹 excel 을 바이브로 구현. python 기반의 엑셀 함수 및 ai agent 기능을 심어서 사내에서 엑셀 안쓰고 데이터 취합에 활용하려고 개발함)

## 왜 이렇게 하는가?
- 회사 내 커리어 변천사에 맞춰 변화하는 직무/기술에 대응하는 삶
  - 네트워크(WiFi 장비, 스위치 장비) 엔지니어 -> IoT 엔지니어 -> SE 품질 엔지니어 -> SE 인프라 엔지니어 -> DevOps 엔지니어 -> DevOps 에반젤리즘 -> 플랫폼 엔지니어 -> DevRel + 플랫폼 엔지니어
- 회사 내 부족한 환경에 대해 분노의 개발 (이런것도 못하면서 무슨 SW 개발을 해?? 라는 마음으로 만들어놓고 조직 내 공유하는 편)
- (전)SE 앤지니어로서 선진 문물(?) 배포에 대한 사명감 ( 부문의 SE 엔지니어였던지라...)
- 언젠가 꿈꾸는 스타트업 창업을 위한 풀스택 기술 능력 확보. 회사의 닫힌 기술이 아닌 열린 기술(?)을 계속 공부하여 언제든 나갈 기술력 확보

