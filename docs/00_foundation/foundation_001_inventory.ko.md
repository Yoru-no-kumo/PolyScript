# FOUNDATION-001 Source Inventory & Coverage Matrix

## 0. 문서 정보

- Document ID: `FOUNDATION-001-INVENTORY`
- Parent Document: `FOUNDATION-001 — PolyScript Project Foundation`
- Version: `0.1`
- Status: `DRAFT`
- Purpose: FOUNDATION-001 작성 전 기존 PolyScript 설계를 재분류하고 누락, OPEN 사항, 후속 상세설계, Invariant 후보, ADR 후보를 식별한다.
- Intended Readers: Project Owner, Architect, Coding Agent
- Canonical Status: 내부 작업 문서. 정식 Foundation 문서의 Canonical Source가 아님.

### 상태 정의

- `CONFIRMED`: 현재까지 설계상 합의된 내용
- `OPEN`: 아직 결정하지 않은 내용
- `BENCHMARK`: 실험/벤치마크 이후 결정할 내용
- `PROPOSED`: 현재 권장하지만 아직 정식 확정하지 않은 내용
- `ADR`: Architecture Decision Record 작성 가치가 있는 결정
- `INVARIANT`: Engineering Contract의 강제 규칙 후보

---

# PART I — PROJECT

## 1. Project Overview

### CONFIRMED

PolyScript AI는 미디어의 음성을 분석하여 다음 기능을 하나의 프로젝트 흐름으로 통합하는 시스템을 목표로 한다.

- 미디어 입력 및 음성 추출
- 음성 전처리 및 향상
- 화자 분리
- 중첩 음성 처리
- 음성 인식
- 타임스탬프 정렬
- 화자 식별 및 관리
- 화자 라이브러리
- 캐릭터 관리
- 원문 스크립트 구축 및 수정
- 다국어 번역
- 자막 생성
- 검토 및 사용자 수정
- 결과물 내보내기
- 데이터 품질 검증
- 향후 학습 데이터 구성
- 모델 학습/평가/승격 관리

초기 중심 언어는 일본어이며 한국어 번역의 우선순위가 높다. 장기적으로 일본어, 한국어, 영어, 중국어를 주요 입출력 언어 범위로 고려한다.

초기 운영 방향은 Local-first이다.

### Foundation 포함

프로젝트의 한 문단 정의와 주요 Capability를 포함한다.

### 후속 상세 문서

- `CONTEXT-001`
- `ARCH-001`
- `PROCESS-001`

### Invariant 후보

- PolyScript는 특정 단일 AI 모델의 Wrapper가 아니다.
- AI Processing Engine은 교체 가능해야 한다.

---

## 2. Background & Problem Statement

### CONFIRMED

현재 미디어 음성 처리에서는 여러 도구를 개별적으로 조합해야 한다.

주요 문제:

- 배경음/노이즈로 인해 특정 화자의 음성이 잘 인식되지 않음
- 여러 화자 및 중첩 발화 처리의 어려움
- ASR만으로는 화자 identity를 지속적으로 관리할 수 없음
- 프로젝트가 바뀌면 동일 화자를 다시 식별해야 함
- 화자와 캐릭터의 개념이 혼동되기 쉬움
- 원문 수정과 번역 수정이 후속 산출물에 어떻게 영향을 주는지 추적하기 어려움
- 자막, 대본, 번역, 화자 정보가 서로 분리되어 관리됨
- 사용자의 수정 결과가 다음 프로젝트에 체계적으로 활용되지 않음
- AI 결과의 신뢰성과 provenance가 불명확할 수 있음

### Foundation 포함

문제 자체를 설명한다. 특정 솔루션 구현은 상세 문서로 넘긴다.

---

## 3. Project Vision

### CONFIRMED

PolyScript는 단순한 Speech-to-Text 또는 Subtitle Generator보다 넓은 시스템을 지향한다.

장기적으로:

`Media → Understanding → Human Review → Canonical Knowledge → Trusted Data → Learning → Improved Processing`

의 순환 구조를 갖는다.

### Foundation 포함

장기 비전과 인간 검토가 포함된 개선 루프.

### Invariant 후보

- AI 출력은 최종 진실이 아니라 검증 대상이다.
- 학습 데이터는 자동 생성되지 않는다.

---

## 4. Project Goals

### CONFIRMED

핵심 목표:

1. 다양한 미디어에서 음성을 안정적으로 추출한다.
2. 화자를 구분하고 중첩 발화를 다룬다.
3. 높은 품질의 원문 ASR을 생성한다.
4. 발화 구간과 텍스트를 시간축에 정렬한다.
5. 프로젝트 화자와 장기 화자 프로필을 분리해 관리한다.
6. 화자와 캐릭터를 독립적으로 관리한다.
7. 사용자 검토와 수정을 정식 데이터 흐름으로 취급한다.
8. 원문 스크립트와 번역을 버전/관계 기반으로 관리한다.
9. JP/KR 및 향후 EN/ZH 번역을 지원한다.
10. SRT/VTT/ASS 및 통합 자막을 생성한다.
11. Artifact와 Dependency를 추적한다.
12. 부분 재실행 및 실패 복구를 지원한다.
13. 검증된 데이터를 장기 지식 및 학습 후보로 활용한다.
14. 학습/평가/모델 Registry를 별도 Learning 환경에서 관리한다.
15. 전체 처리 내역을 추적 가능하고 재현 가능하게 만든다.

---

## 5. Non-Goals

### CONFIRMED에 가까운 항목

다음은 PolyScript의 목표가 아니다.

- AI가 사용자의 검토 없이 모든 identity를 절대적 사실로 확정하는 시스템
- 모든 사용자 수정 내용을 자동으로 Training Data로 사용하는 시스템
- 학습된 모델을 평가/승인 없이 자동으로 Production에 배포하는 시스템
- 특정 ASR, diarization, translation 모델 하나에 영구적으로 종속되는 시스템
- 대용량 Media/Audio/Embedding 파일을 PostgreSQL 자체에 집중 저장하는 구조

### OPEN / PROPOSED

- 실시간 Streaming ASR/방송 자막을 초기 목표에서 제외할지
- 초기 버전에서 완전한 SaaS multi-tenant 운영을 제외할지

이 두 항목은 Foundation v1.0 전에 별도 확인 필요.

---

## 6. Target Users

### CONFIRMED

시스템 레벨:

- `ADMIN`
- `USER`

Project 레벨:

- `OWNER`
- `EDITOR`
- `REVIEWER`
- `VIEWER`

초기 사용 시나리오에는 개인 사용자가 포함된다.

### PROPOSED

향후 `System Operator` 또는 운영 전용 역할 필요 여부.

### 후속 상세 문서

- `DOMAIN-USER-001`
- `SECURITY-001`

---

## 7. Core Use Cases

### CONFIRMED

- Project 생성
- Media 입력
- Pipeline 실행
- 진행상태 확인
- 화자 후보 확인
- Project Speaker 이름 지정
- Persistent Speaker 연결
- 화자 Merge/Split
- Character 생성/연결
- 원문 Script 검토/수정
- Translation 생성/검토
- Subtitle 생성/수정/재출력
- Narrator 별도 처리
- Speaker Library 관리
- Data Integrity Issue 검토
- Review Queue 처리
- Dataset 후보 검토
- Admin의 Dataset 구성
- Training 요청
- Model Evaluation
- Model 승인 및 Registry 관리
- 결과물 Export

### 후속 문서

각 Domain / UI / API 문서.

---

## 8. System Scope

### CONFIRMED — PolyScript 내부 책임

- 프로젝트 관리
- AI Processing orchestration
- Processing result management
- Speaker/Character/Script/Translation 관리
- Artifact/Dependency 관리
- Review/Trust/Integrity
- Learning dataset lifecycle
- Training orchestration
- Model Registry
- Export
- 사용자/권한

### External Dependency 후보

- FFmpeg
- AI Models
- GPU/CUDA runtime
- OS
- PostgreSQL
- File/Object Storage
- 외부 Translation API가 선택될 경우 해당 서비스

### OPEN

외부 Cloud Provider와 Managed Service 선택.

---

# PART II — CONCEPT

## 9. Core Concepts

### CONFIRMED

핵심 Entity/Concept:

- Project
- Media
- Audio Track
- Artifact
- Artifact Dependency
- Job
- Project Speaker
- Persistent Speaker
- Speaker Sample
- Speaker Embedding
- Speaker Embedding Profile
- Speaker Association
- Character
- Character Assignment
- Script Segment
- Translation
- Subtitle / Output
- Review
- Confidence / Evidence
- Canonical Data
- Trusted Data
- Training Candidate
- Dataset
- Dataset Version
- Training Run
- Model Version
- Model Evaluation
- Global Knowledge
- Audit Event
- Data Quality Issue

### Invariant 후보

`Project Speaker ≠ Persistent Speaker ≠ Character`

---

## 10. Conceptual System Flow

### CONFIRMED

Core Processing:

`Media → Audio → Speech Analysis → Diarization → ASR → Alignment → Speaker Analysis → Script → Review → Translation → Subtitle/Output`

Learning:

`AI Result → User Correction → Canonical Data → Trust Evaluation → Training Candidate → Validation → Dataset → Training → Evaluation → Model Candidate → Admin Approval → Model Registry → Processing`

### Foundation 포함

27단계 전체를 나열하기보다 이 수준의 Conceptual Flow를 사용.

---

# PART III — PRINCIPLES

## 11. Design Philosophy

### CONFIRMED

- Local-first
- Accuracy-first
- Human-in-the-loop
- AI result as candidate
- Traceability
- Reproducibility
- Replaceable engines
- Explicit trust
- Data integrity
- Versioned evolution
- Processing/Learning separation
- Extensibility over premature optimization

### ADR 후보

- Local-first 기본 방향
- Core/Learning separation
- Replaceable Engine Architecture

---

## 12. Data Principles

### CONFIRMED

- PostgreSQL은 관계형 metadata/application state의 Source of Truth.
- 대용량/바이너리 Artifact는 File/Object Storage에 저장.
- Cache는 재생성 가능하며 authoritative source가 아님.
- Derived Artifact는 입력과 dependency를 추적할 수 있어야 함.
- 원본/과거 Artifact를 기본적으로 즉시 덮어쓰지 않음.
- Canonical Data와 Trusted Data를 구분.
- Trusted Data와 Training Eligible Data를 구분.

### 핵심 구분

`AI Result ≠ Canonical Data`

`Canonical Data ≠ Trusted Data`

`Trusted Data ≠ Training Eligible Data`

`Training Eligible Data ≠ Distribution Eligible Data`

### Invariant 후보

다수.

### ADR 후보

- PostgreSQL Source of Truth
- Binary Artifact external storage
- Artifact immutability/versioning

---

## 13. AI & Model Principles

### CONFIRMED

AI Engine과 Model은 시스템에서 교체 가능해야 한다.

주요 Engine category:

- ASR
- Diarization
- Speaker Embedding
- Alignment
- Enhancement
- Translation

Engine은 capability, language, resource requirement, model/version 정보를 가져야 한다.

### OPEN / BENCHMARK

- 최종 일본어 ASR
- 최종 한국어 ASR
- 최종 중국어 ASR
- 최종 영어 ASR
- alignment 조합
- speaker embedding model
- diarization model
- translation engine

---

## 14. Learning Principles

### CONFIRMED

Learning은 27단계 Core Processing Pipeline과 별도다.

Core Processing Server와 Learning Server는 물리적으로 분리한다.

Learning Server는:

- Dataset Builder
- Training
- Evaluation
- Model Build

를 담당한다.

Admin이 데이터 상태를 확인한 뒤 Training을 시작한다.

자동 Training 시작 및 자동 Production 배포를 기본 정책으로 하지 않는다.

### Invariant 후보

- Correction → Training 자동 승격 금지
- Training Model → Production 자동 승격 금지
- Learning Server가 Core Domain을 임의로 수정하지 않음

### ADR 후보

- Physical Learning Server separation
- Admin-controlled training
- Model Promotion workflow

---

## 15. Security & Privacy Principles

### 신규 반영 — PROPOSED → Foundation 반영 권장

- 최소 권한 원칙
- 인증과 권한 분리
- Project별 Data Access 통제
- Voice/Speaker 데이터의 민감성 고려
- Secret/API Key의 코드 및 일반 DB 데이터와 분리
- 민감 데이터의 저장/전송 보호
- 사용자 작업 및 주요 변경사항 추적
- Data Retention/Deletion 정책과 연동

### OPEN

- 구체적 Authentication 방식
- Encryption 구현
- Secret Manager
- 개인정보 분류 정책

### 후속 문서

`SECURITY-001`

---

## 16. Data Lifecycle Principles

### 신규 반영 — Foundation 포함 권장

Lifecycle:

`Create → Active → Archive → Retention → Delete/Purge`

Project 삭제와 다음 데이터의 삭제는 동일하지 않을 수 있음:

- Media
- Derived Artifact
- Speaker Sample
- Persistent Speaker Profile
- Dataset Item
- Model
- Audit Event

### OPEN

- Soft Delete 기준
- Physical Purge 기준
- Retention Period
- 이미 Dataset/Model에 사용된 원천 데이터 삭제 처리

### 후속 문서

`DATA-LIFECYCLE-001`

---

## 17. Legal / License / Data Usage Principles

### 신규 반영 — Foundation 포함 권장

Processing 가능 여부와 다른 사용 권한을 구분한다.

`Processing Permission ≠ Storage Permission ≠ Learning Permission ≠ Distribution Permission`

Processing 가능한 콘텐츠가 자동으로 Global Training Dataset에 들어가는 것은 아니다.

### OPEN

- License metadata schema
- 사용자 권한 선언 방식
- Training eligibility validation 방식
- 배포/공유 정책
- 국가별 서비스 정책

### Invariant 후보

- Processing permission만으로 Training eligibility를 추론하지 않음.

---

## 18. Reliability & Recoverability Principles

### CONFIRMED + 신규 확장

기존:

- Job status
- Cancel/Retry
- Partial Rerun
- Artifact invalidation
- Resume/Recovery

추가:

- PostgreSQL Backup
- Storage Backup
- Restore Procedure
- DB/Storage consistency recovery

### OPEN

구체적인 Backup 주기와 기술.

---

## 19. Observability Principles

### 신규 반영

Observability와 Audit/Provenance를 구분한다.

Observability:

- Logs
- Metrics
- Traces
- Health checks
- Job duration
- Stage duration
- Engine performance
- Error monitoring
- CPU/GPU/RAM/VRAM usage

Audit/Provenance:

- 누가
- 무엇을
- 언제
- 왜
- 어떤 상태에서 어떤 상태로

변경했는지 추적.

### 후속 문서

`OBSERVABILITY-001`

---

## 20. Resource Principles

### 신규 반영

CPU/GPU/RAM/VRAM/Storage/Concurrency/Cost를 first-class operational concern으로 취급.

Engine과 Job은 resource requirement 정보를 가질 수 있어야 함.

Core Processing과 Learning workload를 분리하여 관리.

### OPEN

- Scheduler 구현
- GPU allocation
- Cost accounting
- Quota
- Concurrency policy

---

# PART IV — CONSTRAINTS & QUALITY

## 21. Major Constraints

### CONFIRMED

- 초기 개발은 Windows 기반.
- 현재 개발 환경은 Python 3.12.
- 초기 로컬 장비에 NVIDIA CUDA GPU가 없음.
- 긴 Media Processing을 지원해야 함.
- 대용량 Artifact가 발생함.
- 장기적으로 여러 AI Engine/Model Version이 공존함.
- Core Processing과 Learning의 Runtime 요구사항이 다름.
- 다국어 입력/출력 확장성을 확보해야 함.

### 주의

현재 PC의 구체 사양은 영구 Architecture Constraint로 고정하지 않는다.

---

## 22. Success Criteria

### CONFIRMED 개념

PolyScript의 성공은 단순히 “텍스트가 출력됨”으로 정의하지 않는다.

평가 축:

- ASR Accuracy
- Speaker consistency
- Overlap handling
- Timestamp quality
- Translation quality
- Reviewability
- Traceability
- Recoverability
- Reproducibility
- Extensibility
- Usability
- Data integrity

### OPEN

각 항목의 정량 KPI와 합격 기준.

---

# PART V — EVOLUTION

## 23. Risks & Open Questions

### 주요 Risk

- 실제 미디어에서 ASR Accuracy 부족
- 여성 화자/저음량 음성 인식
- Overlap speech
- Diarization 오류
- Speaker Profile contamination
- 동일인/다른 인물 오인식
- 번역 품질
- Copyright / License
- GPU 비용
- Storage 증가
- Model/Framework compatibility
- 문서와 코드의 Semantic Drift

### 주요 OPEN

- 최종 ASR Engine
- Diarization Engine
- Alignment Engine
- Translation Engine
- Job Queue technology
- Scheduler technology
- Frontend framework
- Authentication
- Cloud Provider
- Object Storage
- Training framework
- Retention policy
- Deployment architecture 세부사항

---

# PART VI — GOVERNANCE

## 24. Documentation & Governance

### CONFIRMED

모든 정식 설계 문서는 네 가지 Variant를 기본으로 한다.

- `.spec.md` — Canonical Technical Specification
- `.ko.md` — 한국어 Owner/Human Version
- `.en.md` — English Version
- `.ja.md` — Japanese Version

Semantic Canonical Source는 `.spec.md`.

네 문서의 기술적 의미는 동일해야 함.

공통 상태:

- DRAFT
- REVIEW
- ACCEPTED
- BASELINED
- SUPERSEDED
- DEPRECATED

Architecture Decision은 ADR로 기록.

Architecture Invariant는 Engineering Contract에서 관리.

문서의 의미가 변경될 경우 네 Variant를 동기화.

### Invariant 후보

- Semantic changes MUST update all four variants.
- Coding Agent MUST respect BASELINED specifications and accepted ADRs.

---

## 25. Related Architecture Documents

### 예정 문서 구조

- `FOUNDATION-001` — Project Foundation
- `CONTEXT-001` — System Context
- `ARCH-001` — System Architecture
- `DOMAIN-001` — Domain Architecture
- `DATA-001` — Data Architecture
- `PROCESS-001` — Processing Architecture
- `LEARNING-001` — Learning Architecture
- `INFRA-001` — Infrastructure Architecture
- `SECURITY-001` — Security & Privacy Architecture
- `GOV-001` — PolyScript Engineering Contract
- 각 Domain 상세 문서
- 각 Pipeline Stage 상세 문서
- 각 ADR

---

# 누락 점검 결과

현재 Foundation 수준에서 추가적인 최상위 개념 영역이 반드시 필요하다고 판단되는 항목은 발견되지 않았다.

이번 Inventory에서 기존 설계에 명시적으로 편입한 영역:

1. Security / Privacy
2. Data Lifecycle
3. Backup / Recovery
4. Observability
5. Resource Governance
6. Legal / License / Data Usage
7. Documentation Governance

향후 새로운 요구가 나타날 수 있으나, 현 시점에서는 새로운 최상위 Architecture Domain 추가보다 기존 영역의 상세 설계가 우선이다.

---

# 주요 Invariant 후보 요약

1. `Project Speaker ≠ Persistent Speaker ≠ Character`
2. Character는 Project-level 독립 Entity다.
3. PostgreSQL은 관계형 Metadata와 Application State의 Source of Truth다.
4. 대용량 Binary Artifact는 PostgreSQL의 기본 저장 대상이 아니다.
5. Cache는 Source of Truth가 아니다.
6. AI Result는 자동으로 Canonical Data가 아니다.
7. Canonical Data는 자동으로 Trusted Data가 아니다.
8. Trusted Data는 자동으로 Training Eligible Data가 아니다.
9. Processing Permission은 Training Permission을 의미하지 않는다.
10. User Correction은 자동으로 Training Data가 되지 않는다.
11. 학습된 Model은 자동으로 Production으로 승격되지 않는다.
12. Core Processing과 Learning은 물리적으로 분리한다.
13. Artifact와 Dependency는 추적 가능해야 한다.
14. AI Engine은 교체 가능해야 한다.
15. 문서의 Semantic 변경은 4개 Variant에 동기화되어야 한다.

---

# 주요 ADR 후보 요약

1. Project Speaker / Persistent Speaker / Character 분리
2. Persistent Speaker의 Multi-Sample Embedding 구조
3. Character를 Project-level Entity로 유지
4. PostgreSQL을 Source of Truth로 사용
5. Binary Artifact의 외부 Storage 사용
6. Artifact/Dependency Graph 채택
7. Core Server와 Learning Server 물리적 분리
8. 단일 Shared PostgreSQL 사용
9. Admin-controlled Training
10. Model Promotion / Rollback 체계
11. Replaceable Engine Architecture
12. Markdown을 Documentation Source Format으로 채택
13. `.spec/.ko/.en/.ja` Four-Variant Documentation Policy
14. Trusted Data와 Training Eligible Data 분리
15. Processing/Storage/Learning/Distribution Permission 분리

---

# 0단계 판단

FOUNDATION-001 한국어 초안을 작성할 수 있을 만큼 기본 설계 정보는 충분하다.

다만 다음 항목은 Foundation에서 OPEN으로 유지해야 한다.

- 특정 AI Engine 선택
- Queue/Scheduler 기술
- Frontend 기술
- Authentication 기술
- Cloud Provider
- Object Storage 구현
- Training Framework
- 삭제/보존 세부정책
- 정량 Quality KPI

이 항목들은 임의로 확정하지 않는다.
