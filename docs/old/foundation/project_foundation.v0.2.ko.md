# FOUNDATION-001 — PolyScript Project Foundation

## 0. 문서 정보

| 항목 | 내용 |
| --- | --- |
| Document ID | FOUNDATION-001 |
| Title | PolyScript Project Foundation |
| Variant | KO |
| Version | 0.2 |
| Status | ACCEPTED |
| Accepted Date | 2026-10-08 (Asia/Tokyo) |
| Approval | Project Owner의 정식 승격 지시 |
| Canonical Technical Source | 추후 `project_foundation.spec.md` 작성 및 의미 동기화 |
| Related Documents | CONTEXT-001, ARCH-001, DOMAIN-001, DATA-001, PROCESS-001, LEARNING-001, INFRA-001, GOV-001 |
| Purpose | PolyScript AI의 목적, 범위, 핵심 개념, 설계 철학과 이후 설계의 기준점을 정의한다. |

---

# PART I — PROJECT

## 1. 프로젝트 개요

PolyScript는 실제 영상·음성 미디어를 구조화된 학습 데이터로 변환하는 **Listening/Shadowing 중심 언어학습 제품**이다. AI가 발화와 화자, 시간축, 원문 및 번역을 분석하고 사용자가 검수·수정하여 실제 반복 청취와 쉐도잉에 사용할 수 있게 한다.

원문 Script, 번역 Script, Speaker/Narrator Script 및 SRT/VTT/ASS 등의 Subtitle Export도 주요 독립 출력이다. 사용자는 제품 내부 학습 기능을 거치지 않고 검수한 Script/Subtitle을 외부에서 활용할 수 있다. 실제 배포·공유 가능 여부는 Distribution Permission을 별도로 따른다.

PolyScript는 **Language-Agnostic / Multilingual-by-Design**으로 설계한다. KO/JA/ZH/EN은 초기 우선 언어이며 고정된 언어 한계가 아니다. 초기 일본어 음성 및 일본어↔한국어 검증을 집중할 수 있지만 Domain, 저장 구조, Engine 계약과 UI의 언어 표현을 네 언어 전용으로 고정하지 않는다. 모든 언어에 동등한 품질을 즉시 보장하는 것은 v1 조건이 아니다.

Core는 Deployment-Agnostic이다. 개발·개인 사용에서는 Local-first + Cost-first를 우선하고, 서비스 운영에서는 Cloud 또는 Distributed 실행을 허용한다. 배포 선택은 Core Domain의 의미와 데이터 계약을 바꾸지 않는다. 권리 불명 데이터는 Restricted를 기본으로 하며 이는 Processing/Storage 등 모든 목적에 대한 포괄 허가를 뜻하지 않는다.

---

## 2. 배경과 문제 정의

일반적인 미디어 음성 처리에서는 여러 종류의 도구를 별도로 사용해야 한다.

영상에서 음성을 추출하고, 음질을 개선하고, 화자를 분리하고, 음성을 인식하고, 자막을 만들고, 번역을 수행하는 과정이 각각 독립적인 프로그램이나 모델에 의해 처리되는 경우가 많다.

이러한 방식은 몇 가지 중요한 문제를 만든다.

첫째, 한 단계에서 사용자가 수정한 정보가 다른 단계에 제대로 전달되지 않는다.

둘째, 같은 사람이 여러 프로젝트에 등장해도 매번 새로운 익명 화자로 인식되는 경우가 많아 장기적인 화자 지식을 축적하기 어렵다.

셋째, 실제 사람으로서의 화자와 작품 안의 캐릭터가 서로 다른 개념임에도 단순한 자막 처리에서는 하나의 이름으로 취급되기 쉽다.

넷째, ASR이나 화자 식별 결과에 오류가 발생했을 때 해당 결과가 어떤 후속 데이터와 산출물에 영향을 주었는지 추적하기 어렵다.

다섯째, 사용자가 오랜 시간에 걸쳐 수정하고 확인한 결과가 다음 처리의 품질 향상이나 학습 데이터 구성으로 자연스럽게 연결되지 않는다.

PolyScript는 이러한 문제를 개별 AI 모델의 성능만으로 해결하려 하지 않는다.

대신 Processing, Human Review, Data Management, Knowledge, Learning을 하나의 일관된 시스템으로 연결하는 것을 목표로 한다.

---

## 3. 프로젝트 비전

사용자가 관심 있는 실제 외국어 미디어를 넣고, 분석·검수한 데이터를 Listening/Repeat/Shadowing에 지속적으로 사용하며, 필요한 Script/Subtitle을 Export할 수 있는 제품을 만든다.

`Media → AI Processing → Candidate Result → Validation → Human Review → Canonical Data → Learning Experience / Export`

검증된 근거는 별도의 신뢰·권한·가치 평가를 거쳐 장기 지식으로 축적할 수 있다.

`Canonical Data → Trust Evaluation + Purpose Permission + Value Evaluation → Reusable Knowledge → Improved Processing`

Model Training을 채택하는 경우에만 별도 조건부 흐름을 적용한다.

`Eligible Evidence/Knowledge → Training Candidate → Training Validation → Dataset → Training → Evaluation → Approval → Model Registry → Future Processing`

AI 연구나 자체 모델 보유가 제품 목적은 아니다. 검증된 지식, 통계, Engine 최적화와 모델 개선은 실제 미디어를 더 빠르고 정확하게 학습 자료로 만드는 수단이다.

---

## 4. 프로젝트 목표

PolyScript는 다음의 주요 능력을 제공하는 것을 목표로 한다.

### 4.1 Media Processing

영상 또는 음성 파일을 프로젝트에 입력하고 필요한 음성 데이터를 안정적으로 추출할 수 있어야 한다.

### 4.2 Speech Processing

음성 전처리, 향상, VAD, 화자 분리, 중첩 발화 분석 등의 과정을 통해 후속 AI 처리가 가능한 상태를 만들어야 한다.

### 4.3 Speech Recognition

원문의 언어를 인식하고 높은 품질의 ASR 결과를 생성할 수 있어야 한다.

### 4.4 Timing & Alignment

음성 구간, 발화, 단어 또는 문장이 실제 시간축과 일관되게 연결될 수 있어야 한다.

### 4.5 Speaker Understanding

하나의 프로젝트에서 발견된 화자를 관리하고, 기존 Speaker Library와 비교해 장기적인 화자 프로필과 연결할 수 있어야 한다.

### 4.6 Character Management

실제 화자와 작품 또는 프로젝트 안에서 사용되는 Character를 독립적인 개념으로 관리할 수 있어야 한다.

### 4.7 Script Management

화자와 시간 정보가 포함된 원문 스크립트를 구축하고 사용자가 이를 수정할 수 있어야 한다.

### 4.8 Translation

원문 스크립트를 한국어를 포함한 다른 언어로 번역하고, 번역 결과를 검토 및 수정할 수 있어야 한다.

### 4.9 Subtitle Generation

원문, 번역 및 통합 형태의 자막을 생성하고 SRT, VTT, ASS 등 다양한 출력 형식을 지원할 수 있어야 한다.

### 4.10 Human Review

AI가 생성한 결과 중 불확실하거나 중요한 항목을 사용자가 검토하고 수정할 수 있어야 한다.

### 4.11 Persistent Knowledge

프로젝트 간 재사용 가능한 Speaker, Terminology, Translation Memory, Name, Pronunciation 등의 지식을 축적할 수 있어야 한다.

### 4.12 Learning

실제 미디어 분석, 발화 검출, 화자 분리·식별, ASR 및 구조화된 Script 생성의 속도와 정확도를 개선한다. Learning은 Knowledge Accumulation, Statistics, Engine Optimization, Adaptation/Fine-tuning/Custom Model을 포괄한다. Dataset/Training은 권한과 검증을 통과한 경우의 수단이며 고급 Training 구현은 post-v1이다.

### 4.13 Traceability

AI 결과, 사용자 수정, Artifact, Dataset, Model이 어디에서 생성되고 어떻게 변경되었는지 추적 가능해야 한다.

### 4.14 Recoverability

긴 처리 작업이 실패하더라도 가능한 범위에서 완료된 결과를 재사용하고 필요한 부분만 다시 실행할 수 있어야 한다.

### 4.15 Listening / Repeat / Shadowing

원문·번역·화자·시간축이 연결된 데이터를 실제 미디어 재생과 함께 사용하고, 발화 구간 반복 청취와 Shadowing 학습을 수행할 수 있어야 한다. AI 발음 점수는 v1 완료조건이 아니다.

### 4.16 Collaboration

다중 사용자와 Project 공동 작업을 설계 전제로 삼고, Project 역할에 따른 접근·검수·편집을 지원한다. 실시간 공동 편집의 구체 방식은 OPEN이다.

---

## 5. 비목표

PolyScript는 다음과 같은 시스템을 목표로 하지 않는다.

첫째, AI가 생성한 화자 identity나 기타 추론 결과를 검토 없이 절대적 사실로 취급하지 않는다.

둘째, 사용자의 모든 수정 내용을 자동으로 Training Data로 사용하지 않는다.

셋째, 새롭게 학습된 Model을 평가와 승인 없이 자동으로 Production 환경에 배포하지 않는다.

넷째, Whisper, SenseVoice, pyannote 또는 특정 번역 모델 하나에 영구적으로 종속되는 시스템을 만들지 않는다.

다섯째, 대용량 영상, 음성, Embedding, Model 또는 기타 Binary Artifact를 PostgreSQL 자체에 집중 저장하는 구조를 목표로 하지 않는다.

실시간 Streaming 처리와 완전한 SaaS Multi-Tenant 운영을 초기 목표에서 제외할지는 기존 OPEN / PROPOSED 상태를 유지한다. Multi-user 설계 확정은 완전한 SaaS 운영 채택을 의미하지 않는다.

v1은 §22의 완성된 업로드 기반 학습·Export 흐름을 우선한다. 고급 Training, 완전 자동 Speaker Identification, AI 발음 채점, Enterprise/Billing/Mobile/대규모 인프라는 post-v1이며 영구적인 제품 비목표와 구별한다.

---

## 6. 대상 사용자

불특정 다수의 사용을 전제로 **Multi-user**를 설계한다. 개발자 개인의 Local-first 사용과 Dogfooding은 초기 검증 방법이며 제품을 단일 사용자 전용으로 제한하지 않는다. Project 공동 작업이 가능해야 한다.

- Global role: ADMIN, USER
- Project role: OWNER, EDITOR, REVIEWER, VIEWER

Global role과 Project role은 서로 다른 권한 축이다. 프로젝트 멤버십과 데이터 접근을 명시적으로 관리한다. 구체적인 역할별 행위 매트릭스, ADMIN의 프로젝트 데이터 접근 범위, 인증 기술과 동시 편집 충돌 처리는 SECURITY-001/DOMAIN-USER-001의 **OPEN** 사항이다.

Organization/Team/Workspace는 **Future / OPEN**이다. System Operator 또는 운영 전용 역할은 기존 Inventory의 **PROPOSED**를 유지하며 ADMIN/USER 외 역할을 새로 확정하지 않는다.

---

## 7. 핵심 사용 시나리오

### 7.1 v1의 제품 흐름

`Login → Project 생성 → Media Upload → Background Processing → Audio/Speaker Analysis → Diarization → ASR → Speaker-separated Script → Translation → Review/Correction → Subtitle → Listening/Repeat/Shadowing → Export`

이 흐름은 사용자 경험의 Vertical Slice이며 기존 27개 Conceptual Stage 목록을 대체하지 않는다. 중간 결과 검수, Alignment, Speaker 후보 검토는 필요한 위치에서 연결한다. 이 표시 순서를 모든 Stage의 고정 실행 순서로 해석하지 않는다.

사용자는 영상·음성을 입력하고, 브라우저를 닫은 뒤에도 지속되는 Job의 상태와 결과를 다시 확인한다. 실패 원인을 확인하고 Retry할 수 있어야 한다. 실제 수십 분~약 1시간급 미디어를 제품 UI 안에서 처리·검수·학습·Export한다.

원문, 번역, 화자, Character 및 Subtitle을 수정하면 Canonical Data에 반영하고 관련 downstream Artifact의 오래된 상태를 추적한다. 필요한 Artifact를 재생성하여 수정한 결과를 학습하거나 재 Export한다.

### 7.2 기본 Persistent Speaker reuse — v1 포함 / CONFIRMED

`Project Speaker → Persistent Speaker 후보 → 사용자 확인 → Speaker Sample/Provenance 저장 → 다음 Project에서 재사용`

Project Speaker를 기존 Persistent Speaker와 연결하거나 새 프로필을 만들 수 있다. 화자 이름 지정, Merge/Split, Character 생성·연결 및 잘못된 연결의 검수는 기존 사용 시나리오로 유지한다. 기본 reuse는 완전 자동 인물 식별이나 고급 Training을 필요로 하지 않는다.

재사용할 Sample과 Evidence의 출처·확인 상태·사용 범위를 보존한다. 자기 프로젝트 간 재사용 가능성과 사용자 간/Global Knowledge 공유 가능성은 별개이며 목적별 Permission과 Project 접근 권한을 확인한다.

### 7.3 독립 Export 및 향후 Learning

사용자는 원문 Script, 번역 Script, Speaker Script, Narrator Script와 SRT/VTT/ASS 등의 Subtitle을 Export할 수 있다. 제품 내부 학습 완료를 Export 전제조건으로 만들지 않는다.

고급 Learning 기능이 도입되는 경우 관리자는 적격 후보를 검토하여 Dataset Version을 구성하고 물리적으로 분리된 Learning Server에서 Training을 요청한다. Evaluation과 명시적 Approval을 통과한 Model만 Processing에 사용한다. 이 고급 시나리오의 완성은 v1 조건이 아니다.

---

## 8. 시스템 범위

PolyScript의 내부 책임에는 다음이 포함된다.

- Project 관리 및 공동 작업
- Listening / Segment Repeat / Shadowing
- Media 및 Audio Processing
- AI Processing orchestration
- Speaker 관리
- Character 관리
- Script 관리
- Translation 관리
- Subtitle 및 Output 관리
- Artifact와 Dependency 관리
- Job 관리
- Review Queue
- Data Integrity
- Quality Gate
- User Correction
- Audit 및 Provenance
- Learning Data 관리
- Dataset 관리
- Training 요청 및 결과 관리
- Model Evaluation
- Model Registry
- Global Knowledge 관리
- User와 Project Permission 관리

PolyScript는 FFmpeg, AI Model, Operating System, GPU Runtime, PostgreSQL 등의 외부 기술과 상호작용할 수 있지만 이러한 특정 구현 자체가 PolyScript의 Domain은 아니다.

위 책임은 전체 시스템의 장기 범위이며 모두 v1 구현 의무라는 뜻은 아니다. v1 경계는 §22를 따른다.

특정 Cloud Provider, Object Storage 제품 또는 외부 Translation Service 사용 여부는 아직 확정하지 않는다.

---

# PART II — CONCEPT

## 9. 핵심 개념

### Project

하나의 Media Processing 작업과 그와 관련된 데이터, 설정, 사용자 검토 및 Output을 묶는 기본 작업 단위이다.

### Project Speaker

특정 Project 안에서 Diarization을 통해 구분된 로컬 화자이다.

`SPEAKER_00`, `SPEAKER_01`과 같은 임시 Label로 시작할 수 있다.

Project Speaker 자체가 실제 사람의 영속적인 Identity를 의미하지 않는다.

### Persistent Speaker

여러 Project에서 재사용할 수 있는 장기적인 Speaker Profile이다.

Persistent Speaker는 실제 인물, 성우, 화자, 역할 또는 사용자가 관리하려는 특정 Voice Identity를 나타낼 수 있다.

### Speaker Sample

Persistent Speaker를 판단하거나 학습하는 근거가 되는 실제 음성 구간과 그 Provenance이다.

### Character

Project 안에서 사용되는 독립적인 개념이다.

Character는 Persistent Speaker의 속성이 아니다.

하나의 Persistent Speaker가 여러 Character를 연기할 수 있으며, 하나의 Character를 여러 Speaker가 담당할 수도 있다.

### Speaker Association

Project Speaker와 Persistent Speaker 사이의 관계이다.

AI가 제안할 수 있으나 신뢰도와 사용자 확인 여부를 별도로 관리한다.

### Artifact

Processing을 통해 생성되는 재사용 가능한 산출물이다.

Artifact는 자신이 어떤 입력으로부터 생성되었는지 추적할 수 있어야 한다.

### Script Segment

시간 정보와 Speaker/Character 관계를 가질 수 있는 원문 Script의 기본 단위이다.

### Translation

특정 Script Segment에 대해 생성된 대상 언어 텍스트이다.

### Canonical Data

현재 시스템이 공식적인 상태로 채택한 데이터이다.

### Trusted Data

Canonical Data 중에서도 별도의 신뢰 조건을 만족한 데이터이다.

### Training Candidate

향후 Dataset에 포함될 가능성이 있는 데이터이다.

Trusted Data라고 해서 자동으로 Training Candidate 또는 Training Data가 되는 것은 아니다.

### Evidence / Knowledge

Evidence는 화자 연결, 전사, 품질 평가 또는 Knowledge를 뒷받침하는 근거이며 출처와 생성·검수 이력을 가진다. Knowledge는 재사용 가능한 지식이다. 현재 사용량이 적다는 이유만으로 근거의 보존 가치를 낮게 단정하지 않는다.

### Usage Value / Retention Value

Usage Value는 현재 처리·학습·재사용에 쓰이는 가치이고 Retention Value는 향후 신뢰·재현·역사적 근거 등 보존할 가치이다. 두 축을 별도로 평가한다.

### Analysis Revision / Reconciliation

Analysis Revision은 특정 입력·Engine·Model·설정으로 생성된 분석 결과의 버전이다. Reconciliation은 새로운 Evidence와 현재 Canonical Data 및 사용자 수정 사이의 차이와 충돌을 검토하고 반영하는 절차이다.

### Restricted

권리 또는 사용 범위가 불명확한 데이터에 적용하는 기본 제한 상태이다. 단일 Boolean 허가나 Project 공개 여부를 대체하지 않는다. 목적별 Permission의 허용 범위를 확인한 작업만 수행한다.

### Dataset Version

특정 시점에 선택된 Training Data의 고정된 집합이다.

### Model Version

특정 Training 또는 외부 Model에 대해 추적 가능한 버전 단위이다.

---

## 10. 개념적 시스템 흐름

PolyScript의 Core Processing 흐름은 개념적으로 다음과 같다.

`Media → Audio Preparation → Speech Analysis → Speaker Analysis → ASR → Alignment → Script → Human Review → Character/Translation → Subtitle/Output`

기존 **27개 Conceptual Stage** 구분을 유지한다. 이번에 확인한 자료에는 전체 Stage 목록이 없으므로 번호·이름·순서를 임의로 복원하지 않는다. PROCESS-001에서 원래 목록과 dependency를 상세화한다. 개념 단계는 반드시 27개 Worker/Job 또는 모든 입력의 무조건 실행을 뜻하지 않는다.

학습 제품 흐름은 이 Core 결과를 Listening/Repeat/Shadowing과 독립 Export에 연결한다.

Learning 흐름은 Core Processing과 분리된다.

`Processing Result → Validation → Canonical Data → Trust Evaluation → Training Candidate → Training Validation → Dataset → Training → Evaluation → Model Candidate → Approval → Model Registry`

이 두 흐름은 연결되어 있지만 동일한 Pipeline은 아니다.

---

# PART III — PRINCIPLES

## 11. 설계 철학

### 11.1 Deployment-Agnostic Core / Local-first + Cost-first

Core의 Domain과 계약은 배포 방식에 독립적이다. 개발·개인 사용은 Local-first + Cost-first로 운영하고 서비스는 Cloud/Distributed 환경을 사용할 수 있다. Cost-first는 발화·화자 정보를 희생하는 무조건 저비용 정책이 아니다. §11.2의 정보 보존 우선순위 안에서 계산과 운영 비용을 통제한다.

Core/Learning의 물리적 분리 원칙은 유지한다. 구체적인 서버 배치, 네트워크 및 Runtime은 후속 설계에서 정한다.

### 11.2 Loss-Averse Adaptive Quality

**⑥ Quality & Performance Principle**은 원본 정보 손실을 최소화하면서 필요한 곳에 추가 계산을 투입해 실용적인 시간과 비용으로 높은 품질을 얻는 것이다.

- Information Preservation: 발화 전체 누락, 화자 누락·오할당, overlap 손실처럼 후속 단계에서 회복하기 어려운 정보 손실을 가장 비싸게 평가
- Diminishing Returns: 미미한 품질 개선을 위해 계산시간·비용을 과도하게 늘리지 않음
- Adaptive Compute: 불확실하고 어려운 구간에 계산량을 집중하는 방향
- Benchmark Driven: 실제 미디어에서 품질·속도·Resource·실패율을 함께 측정하고 결정

ASR 단어 오류, 분할, 번역, Subtitle 오류도 평가하지만 모든 오류를 같은 비용으로 취급하지 않는다. 세부 가중치·confidence threshold·RTF 목표는 **OPEN / BENCHMARK**이다. 쉬운 구간/어려운 구간의 판단과 선택적 재처리 방법은 후속 상세설계 대상이다.

### 11.2.1 Progressive Refinement — PROPOSED / BENCHMARK

실용적인 결과를 먼저 제공한 뒤 전체 또는 선택적 Background High-Quality Processing으로 개선하는 방식은 제안이며 **v1 필수가 아니다**. 모든 Job에 두 번째 Pass를 무조건 수행하지 않는다.

채택 여부는 Expected Quality Gain, Compute Cost, System Load, User Profile, Evidence/Knowledge/Benchmark Value와 목적별 Permission을 검증하여 결정한다. 대화에서 사용한 처리시간·품질 점수는 예시이며 실제 측정값이나 SLA가 아니다.

**PROCESS-REFINE-001 상위 규칙 / Engineering Contract 반영 대상**

새로운 고품질 AI 결과는 기존 Canonical Data와 사용자 수정사항을 무조건 덮어쓰지 않는다. 새로운 Analysis Evidence/Revision으로 기록하고 필요한 차이·충돌을 Reconciliation한다. 이 규칙은 일반 재처리에도 적용한다. 구체적인 자동 merge, 검수 UI, 승인 범위 및 충돌 해소 알고리즘은 **OPEN**이다. AI 결과의 높은 confidence는 ground truth나 Training Permission을 뜻하지 않는다.

### 11.3 Human-in-the-loop

AI가 불확실한 부분을 사용자가 확인하고 수정할 수 있어야 한다.

### 11.4 AI Result as Candidate

AI의 추론 결과는 기본적으로 검증 가능한 Candidate로 취급한다.

### 11.5 Traceability

중요한 데이터와 결과는 그 출처와 변경 과정을 추적할 수 있어야 한다.

### 11.6 Reproducibility

가능한 범위에서 어떤 설정, Engine, Model, Version으로 결과가 만들어졌는지 재현 가능해야 한다.

### 11.7 Replaceable AI Engines

PolyScript의 Domain과 Pipeline은 특정 Model에 직접 종속되지 않아야 한다.

### 11.8 Explicit Trust

신뢰도는 암묵적인 추정이 아니라 데이터로 표현되어야 한다.

### 11.9 Data Integrity

잘못된 화자 연결이나 오염된 Sample이 장기 데이터와 학습 데이터에 전파되는 것을 방지해야 한다.

### 11.10 Versioned Evolution

Script, Translation, Dataset, Model, Configuration 등의 중요한 상태 변화는 가능한 범위에서 버전과 History를 보존한다.

### 11.11 Processing과 Learning의 분리

일상적인 Media Processing과 Model Training은 서로 다른 Workload와 Runtime을 가지므로 별도의 실행 환경으로 관리한다.

### 11.12 Extensibility

현재 필요한 기능만을 과도하게 최적화하기보다 향후 언어, Engine, Model과 Infrastructure를 교체하거나 확장할 수 있는 구조를 우선한다. Language-Agnostic / Multilingual-by-Design을 유지하고 초기 KO/JA/ZH/EN을 Domain의 고정 열거 한계로 삼지 않는다. 언어별 기능·품질 차이는 Engine Capability와 Benchmark로 명시한다.

---

## 12. 데이터 기본 원칙

PolyScript는 데이터를 용도와 신뢰 수준에 따라 구분한다.

`AI Result ≠ Canonical Data`

AI가 생성한 결과가 항상 시스템의 공식 데이터가 되는 것은 아니다.

`Canonical Data ≠ Trusted Data`

시스템에서 현재 채택하고 있다는 사실과 장기적인 신뢰 데이터로 사용할 수 있다는 것은 다른 문제이다.

`Trusted Data ≠ Training Eligible Data`

높은 신뢰도를 가진 데이터라도 저작권, License, Privacy 또는 Data Usage Policy 때문에 Training에 사용할 수 없을 수 있다.

`Training Eligible Data ≠ Distribution Eligible Data`

Training에 사용 가능한 데이터나 Model이라고 해서 외부 배포까지 허용되는 것은 아니다.

관계형 Metadata와 Application State의 Source of Truth는 PostgreSQL을 기본으로 한다.

대용량 Media, Audio, Embedding, Model 및 기타 Binary Artifact는 File 또는 Object Storage에 보관한다.

Cache는 성능을 위한 보조 수단이며 Source of Truth로 사용하지 않는다.

Derived Artifact는 자신의 입력과 Dependency를 추적할 수 있어야 한다.

---

## 13. AI 및 Model 기본 원칙

PolyScript는 특정 AI Model 자체를 제품의 중심으로 정의하지 않는다.

ASR, Diarization, Speaker Embedding, Alignment, Enhancement, Translation 등은 교체 가능한 Engine으로 취급한다.

Engine은 가능한 범위에서 다음 정보를 표현할 수 있어야 한다.

- Capability
- Supported Language
- Model Name
- Model Version
- Resource Requirement
- Input/Output Capability
- Timestamp Capability
- 기타 Engine-specific Metadata

실제 사용할 Model은 실제 Target Media에 대한 Benchmark 결과를 바탕으로 선택한다.

특정 Engine 또는 Model은 Foundation 단계에서 고정하지 않는다.

---

## 14. Learning 기본 원칙

Learning의 목적은 실제 미디어의 빠르고 정확한 분석, 발화 검출, 화자 분리·식별, ASR 및 구조화된 Script 생성 개선이다. AI 연구 자체나 자체 모델 개발을 독립 성공 기준으로 두지 않는다.

Learning은 Knowledge Accumulation, Statistics, Engine Optimization, Adaptation, Fine-tuning, Custom Model을 포함한다. 기본 Persistent Speaker reuse 및 검증된 지식 재사용은 v1에 포함할 수 있는 개선 방식이다. 고급 Training과 대규모 자동화는 post-v1이다.

Learning Pipeline은 27단계 Core Processing Pipeline과 별도다. **Core Processing Server와 Learning Server는 물리적으로 분리**한다. 두 환경은 하나의 PostgreSQL과 Shared Storage를 사용할 수 있으나 실행 책임과 Runtime은 분리한다. 단일 Shared DB를 필수 구현으로 고정하지 않으며 Learning Server가 Core Domain을 임의로 수정하는 구조를 정당화하지 않는다.

이 물리 분리 원칙은 Model Training 등 Learning workload를 실행할 때 유지한다. v1 기본 화자 재사용을 위해 고급 Training Server 전체를 구현해야 한다는 뜻은 아니다. Knowledge 재사용과 Learning Server 업무의 상세 경계는 ARCH-001/LEARNING-001에서 정의한다.

사용자 Correction은 자동으로 Training Data가 되지 않는다. 품질·신뢰 검증과 **Training Permission**을 통과한 후보만 Dataset에 포함할 수 있다. Quality Improvement 또는 Global Knowledge Permission만으로 Training을 허가하지 않는다.

Training은 ADMIN이 데이터 축적량·품질·기간을 확인한 뒤 명시적으로 시작한다. Model은 Evaluation과 Approval을 통과한 후 Registry를 통해 Processing에 사용한다. 자동 Training 시작과 자동 Production 승격을 기본 정책으로 하지 않는다. Model Promotion/Rollback 상세는 후속 설계·ADR 대상이다.

---

## 15. Security 및 Privacy 기본 원칙

**상태 주의:** Inventory §15는 아래 일반 보호 원칙을 PROPOSED로 표시했다. v0.1의 단정적 서술만으로 이를 전체 CONFIRMED로 승격하지 않는다. Multi-user 역할·Project 접근 통제 및 목적별 permission 요구는 이번 결정에 따라 CONFIRMED이며, 나머지 보호 수단은 Foundation 검토 대상 PROPOSED, 구체 구현은 OPEN이다.

PolyScript는 Media, Voice, Speaker Profile, Script 및 사용자 Correction 등 민감할 수 있는 데이터를 처리한다.

따라서 Security와 Privacy는 부가 기능이 아니라 시스템 전체에 적용되는 Cross-Cutting Concern으로 취급한다.

기본 원칙은 다음과 같다.

- 최소 권한
- 명확한 Authentication과 Authorization
- Project Data Access 통제
- Secret 및 Credential 보호
- Voice/Speaker 관련 데이터의 적절한 보호
- 중요 변경행위의 추적
- Data Retention 및 Deletion 정책과의 연동

구체적인 인증 방식, 암호화 기술 및 Secret Management 제품은 후속 설계에서 결정한다.

---

## 16. Data Lifecycle 기본 원칙

데이터의 출처·소유/관리 주체·허용 목적과 적용 범위를 추적한다. 사용자 업로드나 Correction만으로 시스템이 Global Knowledge, Training 또는 Distribution 권리를 얻는다고 해석하지 않는다. 상세 권한 선언 방식과 철회 절차는 OPEN이다.

Lifecycle은 **Time + Value + Permission + Dependency**를 함께 판단한다. 시간 경과나 Project 종료만으로 모든 데이터에 일괄 TTL을 적용하지 않는다.

기본 운영 흐름은 `Create → Active → Archive / Retention`이며 삭제 요청 또는 정책상 삭제 판단 이후의 흐름은 다음과 같다.

`Logical Delete → Recovery/Retention → Policy/Dependency Check → Physical Purge / Anonymization`

Logical Delete는 물리 삭제 완료를 뜻하지 않는다. Recovery/Retention 단계에도 해당 목적의 Storage Permission과 정책을 확인한다. Project 삭제와 Media, Derived Artifact, Persistent Speaker/Sample, Dataset, Model, Audit의 삭제는 같지 않을 수 있다. 남길 데이터도 권한과 provenance를 유지해야 한다.

### 16.1 보존 분류와 가치

가치 있고 권한이 있는 Evidence/Knowledge는 **LONG_TERM / PERMANENT**로 보존할 수 있다. **PERMANENT는 자동 TTL이 없다는 뜻이며 삭제 불가를 뜻하지 않는다.** 권한 철회, 오염, 정책 변경, 명시적 삭제와 Dependency 검토를 면제하지 않는다.

**Usage Value**와 **Retention Value**를 분리한다. 현재 사용 빈도가 낮더라도 희소하거나 역사적 근거인 Evidence는 보존 가치가 높을 수 있다. 현재 사용량이 많아도 오염되었거나 권한이 없는 데이터는 사용/보존을 정당화할 수 없다.

Evidence는 다음 축으로 재평가한다.

- 품질: 음성·전사·정렬 등 근거 자체의 품질
- 신뢰도: 사용자 확인, 독립 검증, 오류·오염 여부
- 현재 활용성: 현재 처리·재사용·검수에 쓰이는 정도
- 희소성: 드문 화자·언어·발화 조건 등 얻기 어려운 근거
- 대체 가능성: 더 좋은 동일 목적 근거로 대체 가능한지
- provenance: 출처·생성·변경·사용 권한 이력의 완전성
- 역사적 가치: 과거 결정·모델·지식 검증 및 재현의 근거 가치

평가 결과로 **KEEP / REVIEW / DELETE_CANDIDATE** 등을 결정한다. 이는 개념적 판단 결과이며 완성된 상태 enum/schema는 아니다. DELETE_CANDIDATE는 즉시 Purge 명령이 아니고, KEEP도 Permission/Dependency 확인을 건너뛰는 허가가 아니다. 재평가 주기·가중치·임계치·자동화 범위는 **OPEN**이다.

### 16.2 Dependency와 삭제

삭제 전에 Artifact, Speaker Evidence, Global Knowledge, Dataset Item/Version, Model, Audit와의 의존관계를 확인한다. 의존관계가 있다는 사실만으로 불허 데이터를 영구 보존하지 않으며, 필요한 영향 평가·참조 무효화·대체·재생성·삭제 처리 방법을 정책으로 정한다.

Physical Purge와 Anonymization 중 무엇을 적용할지, Backup/복제본에서 언제 반영할지, 이미 Dataset/Model에 사용된 데이터의 삭제·재학습·unlearning 처리와 보장은 **OPEN**이다. 익명화가 자동으로 Training/Distribution 권한을 부여한다고 가정하지 않는다.

---

## 17. Legal / License / Data Usage 기본 원칙

**목적별 Permission 분리**

하나의 “사용 가능” 상태 대신 다음 여섯 목적을 독립적으로 관리한다.

- Processing: 미디어 분석 및 요청 결과 생성
- Storage: 원본·파생 데이터의 저장/보존
- Quality Improvement: 처리 품질 분석·통계·최적화 등의 개선 목적 사용
- Global Knowledge: Project 범위를 넘어 공유되는 지식 축적/활용
- Training: Dataset 구성 및 모델 학습 목적 사용
- Distribution: 콘텐츠·파생물 등의 외부 배포/공유

위 설명은 목적의 구분이며 완성된 권한 schema가 아니다. Project/user/global scope, 기간, 적용 데이터 종류와 실제 허용 행위는 후속 정책으로 상세화한다. Library 재사용과 Export에서도 관련 권한 및 scope를 확인한다.

`Processing Permission ≠ Storage Permission ≠ Quality Improvement Permission ≠ Global Knowledge Permission ≠ Training Permission ≠ Distribution Permission`

어느 하나를 허용해도 다른 목적을 자동 허용하지 않는다. v0.1의 포괄적 Learning Permission을 Training 전용 허가 또는 개선 목적 전체의 단일 허가로 사용하지 않는다. Background Processing도 목적을 명시하고 해당 Permission을 확인한다.

**권리 불명 데이터는 Restricted 기본**이다. Local-first, 개인용 사용 또는 높은 Evidence Value만으로 Restricted를 해제하거나 Global Knowledge/Training/Distribution을 허용하지 않는다. Processing 가능 사실로 Storage나 다른 목적의 권한을 추론하지 않는다.

Data Usage는 License, Permission, Privacy와 적용 Policy를 고려한다. License Metadata, 사용자 권한 선언·검증·철회 방식, 서비스 국가별 정책, Model/파생물의 배포 조건은 **OPEN**이며 법률적 결론을 Foundation에서 확정하지 않는다.

---

## 18. Reliability 및 Recoverability 기본 원칙

PolyScript는 긴 Media Processing 작업을 다룬다.

따라서 하나의 오류 때문에 모든 Processing을 처음부터 다시 실행해야 하는 구조를 피한다.

v1의 Job은 브라우저 세션과 독립적으로 계속 실행되고 진행 상태와 실패 이유를 다시 확인하며 Retry할 수 있어야 한다. Job/Artifact/Revision 연결을 보존하여 다른 프로젝트나 실패·재시도 결과와 뒤섞이지 않아야 한다.

Job과 Artifact는 가능한 범위에서:

- 진행상태 추적
- Retry
- Cancel
- Resume
- Partial Rerun
- Downstream Invalidation

을 지원한다.

또한 장기적으로 PostgreSQL과 File/Object Storage에 대한 Backup 및 Restore 체계를 가져야 한다.

DB와 Storage 사이의 불일치 역시 복구 가능한 문제로 취급한다. Canonical 변경과 재처리 시 dependency를 확인해 downstream 결과를 outdated/invalidate하고 필요한 산출물을 다시 생성한다. 삭제·실패·취소된 입력으로 완료 결과가 잘못 게시되는 문제를 검토한다.

체크포인트, idempotency, retry 정책과 Job 상태 머신은 OPEN이며 위 행동 요구를 만족하도록 상세화한다. Backup 주기·제품·복구 목표는 OPEN이다.

---

## 19. Observability 기본 원칙

PolyScript는 시스템이 무엇을 하고 있는지 관찰할 수 있어야 한다.

Observability는 다음 요소를 포함할 수 있다.

- Logs
- Metrics
- Traces
- Health Checks
- Job Duration
- Stage Duration
- Engine Performance
- Error Monitoring
- Resource Usage

Observability는 Audit 및 Provenance와 동일하지 않다.

Observability는 시스템의 운영 상태를 이해하기 위한 것이고, Audit과 Provenance는 데이터와 의사결정의 역사 및 출처를 설명하기 위한 것이다.

---

## 20. Resource 기본 원칙

PolyScript의 AI Processing과 Learning은 CPU, RAM, GPU, VRAM, Storage와 같은 제한된 Resource를 사용한다.

따라서 Resource는 단순한 배포 세부사항이 아니라 시스템 운영에 영향을 주는 주요 정보로 취급한다.

Engine과 Job은 필요한 Resource 특성을 표현할 수 있는 구조를 가져야 한다.

특히 Core Processing과 Learning Workload의 Resource 경쟁을 방지할 수 있도록 실행 환경을 분리한다.

개발·개인 환경에서는 비용을 먼저 고려하되 정보 손실을 가장 비싸게 평가하는 품질 원칙과 함께 판단한다. 서비스 환경의 Resource 경쟁과 Cloud 비용도 가시화한다.

구체적인 Scheduler, GPU Allocation, Concurrency, Quota 및 Cost Policy는 OPEN이며 후속 Infrastructure 설계에서 결정한다.

---

# PART IV — CONSTRAINTS & QUALITY

## 21. 주요 제약조건

기존 Inventory에 기록된 초기 개발 환경은 Windows와 Python 3.12 기반이다. 이는 설계 당시 환경 정보이며 이번 작업에서 장비나 Runtime의 현재 상태를 새로 검증한 결과는 아니다.

기존 문서에 기록된 개발 장비에는 NVIDIA CUDA GPU가 없으므로 초기 로컬 Processing은 CPU 사용 가능성을 고려한다. 이 기록으로 CUDA/GPU 또는 Cloud 사용을 금지하지 않는다.

그러나 현재 개인 개발 장비의 구체적인 Hardware 사양을 PolyScript의 영구적인 Architecture Constraint로 고정하지 않는다.

PolyScript는 기존의 한 시간 이상 긴 Media 지원 방향을 유지한다. v1 수용 검증에는 수십 분~약 1시간급 실제 장시간 자료를 포함한다. 지원 시간·파일 크기 상한과 하드웨어별 성능 보장은 OPEN이다.

Audio, Intermediate Artifact, Model 및 Dataset 등으로 인해 Storage 사용량이 증가할 수 있다.

여러 종류와 Version의 AI Engine이 장기간 공존할 수 있어야 한다.

Core Processing과 Learning 환경은 서로 다른 Dependency 및 Hardware 요구사항을 가질 수 있다.

---

## 22. 성공 기준

v1은 Minimum Demo가 아니라 **First Complete Product**이다. 실제 외국어 미디어를 안정적으로 분석해 화자별 원문·번역·자막 데이터를 생성하고, 사용자가 검수·수정한 뒤 Listening/Repeat/Shadowing에 사용하고 Export하는 전체 흐름이 완주되어야 한다.

`Login → Project → Media Upload → Background Processing → Audio/Speaker Analysis → Diarization → ASR → Speaker-separated Script → Translation → Review/Correction → Subtitle → Listening/Repeat/Shadowing → Export`

### 22.1 필수 완료 기준

- Functional Completeness: 제품 UI 안에서 전체 흐름 완주; 수작업 DB 수정이나 별도 처리 스크립트가 일상 완료에 필요하지 않음
- Real-Media Usability: 수십 분~약 1시간급 실제 미디어를 반복 처리하고 학습·Export에 사용 가능
- Human-Correctable AI: Speaker, Script, Translation, Character, Subtitle 오류를 검수·수정하고 Canonical 반영 및 재 Export 가능
- Operational Reliability: 브라우저 종료 후 Job 지속·상태 재확인, 실패 확인·Retry, Job/Revision/Artifact 무결성
- Multi-user / Collaboration: 로그인·프로젝트 접근과 역할을 적용하고 허용된 공동 작업 가능; 타 사용자 데이터 노출을 방지
- Persistent Speaker Reuse: 후보 제안/연결 → 사용자 확인 → Sample/Provenance 저장 → 다음 Project에서 기본 재사용
- Independent Outputs: Script와 Subtitle을 독립 결과로 내보낼 수 있고 수정한 revision과 결과가 일치
- Benchmark Suite: 실제 corpus와 측정·비교·재현 가능한 평가 기록을 갖춤
- SUCCESS-001 — Dogfooding: 개발자가 실제 외국어 학습에서 지속적으로 Listening/Repeat/Shadowing과 필요 시 Export를 사용

AI가 100% 정확하거나 모든 화자를 완전 자동 식별해야 한다는 조건을 두지 않는다. 다만 잘못된 결과를 제품 안에서 발견·수정·반영할 수 있어야 한다.

### 22.2 Benchmark Suite — v1 필수 / CONFIRMED

다음 조건을 포함하는 실제 미디어 검증 세트를 갖춘다.

- 깨끗한 1인 발화와 다중 화자
- 남/여 혼합 화자와 저음량 발화
- 배경음악·노이즈와 Overlap
- 장시간 Media
- 여러 초기 우선 언어

Speech Recall, Diarization Quality, Speaker Coverage, ASR Quality, Overlap Recovery, Timestamp/Translation Quality, Runtime/RTF, Resource Usage 및 Failure Rate를 함께 측정한다. RTF는 `Processing Time / Media Duration`으로 비교하고 어떤 단계·대기 시간을 포함했는지 기록한다.

언어·음질·화자 수·Overlap·Engine/Model/Config·Hardware에 따라 결과를 구분한다. Dataset/corpus 권한과 provenance를 확인하며 고품질 AI 출력만으로 ground truth를 확정하지 않는다.

구체 corpus 구성·크기, annotation/정답 검증 절차, metric 정의, 반복 횟수, regression 기준과 정량 목표는 **OPEN / BENCHMARK**이다. 지금 “ASR 98%”, “Diarization 95%”, “RTF 0.5” 등을 v1 승인 수치로 고정하지 않는다. Suite와 평가 기록 자체는 필수이며 높은 품질 점수 하나로 전체 완료조건을 대체하지 않는다.

### 22.3 수용 검증 Evidence — PROPOSED

아래 기록 형식은 완료조건을 실제로 검토하기 위한 **PROPOSED** 절차이며 신규 수치 기준을 확정한 것이 아니다.

- 장시간 E2E를 반복 수행: 입력 조건/시간, Job 이력, 학습 사용 및 Export 결과
- 브라우저 종료·재접속 및 실패 Retry: 지속된 Job 상태, 실패 원인, 재시도 결과와 Artifact 연결
- Canonical 수정 후 재생성: 수정 revision, 영향받은 dependency, 새 출력의 일치 여부
- Persistent Speaker 다음 Project 재사용: 확인된 association, Sample 출처, scope/permission 및 재사용 기록
- 공동 작업·역할 적용: 허용/거부 동작과 변경 이력
- 실제 Dogfooding: 실제 미디어를 반복 학습한 사용 기록과 발견·수정한 문제

Dogfooding 기간·빈도와 검증 반복 횟수는 **OPEN**이다. 구현 속도나 생성된 코드량만으로 완료를 선언하지 않는다.

### 22.4 post-v1 / Future 경계

자체 Foundation Model, 대규모 Fine-tuning, 고급 Learning Server 자동화, 완전 자동 Speaker Identification, AI 발음 채점, 고급 개인화 학습 AI, Organization/Enterprise, Billing/Subscription, Mobile App, 대규모 Kubernetes/Auto Scaling 및 모든 언어의 동등한 품질 보장은 v1 필수에서 제외한다. 고급 Translation Memory도 후속 후보로 둔다.

기본 Persistent Speaker reuse를 이 목록에 넣어 제외하지 않는다. Progressive Refinement/Background High-Quality Processing은 **PROPOSED / BENCHMARK**, post-v1 candidate로 유지한다. 장기 아키텍처는 확장을 허용하되 위 기능의 구현 완료를 v1 gate로 삼지 않는다.

---

# PART V — EVOLUTION

## 23. 주요 위험과 OPEN 사항

주요 위험은 실제 미디어의 발화/화자/Overlap 손실, 저음량·배경음 발화, ASR·Diarization·Translation 오류, 잘못된 identity 연결 및 Speaker Profile Contamination이다. 장시간 Job 실패, 재처리의 사용자 수정 손실, 공동 작업의 충돌·접근 통제 오류, Storage 증가, GPU 비용, Model/Framework 호환성, License/Privacy 및 문서·구현 Semantic Drift도 검토한다.

- ① 제품 정체성/언어 설계, ② 배포 철학, ③ Multi-user/roles, ④ lifecycle/목적별 permission, ⑤ Learning 목적, ⑥ 품질 원칙, ⑦ v1 경계: CONFIRMED (후속 책임 문서: 관련 전체 설계)
- Progressive Refinement / Background High-Quality Processing: PROPOSED / BENCHMARK; v1 필수 아님 (후속 책임 문서: PROCESS-001 / INFRA-001)
- 사용자 수정의 무조건 overwrite 금지: CONFIRMED 상위 규칙; reconciliation 방법 OPEN (후속 책임 문서: DATA-001 / PROCESS-001)
- ASR, Diarization, Alignment, Speaker Embedding, Translation Engine/Model 조합: OPEN / BENCHMARK (후속 책임 문서: PROCESS-001)
- Job Queue, Scheduler, Frontend, Authentication 기술: OPEN (후속 책임 문서: ARCH-001 / INFRA-001 / SECURITY-001)
- Cloud Provider, Object Storage 제품, 배포 topology: OPEN (후속 책임 문서: INFRA-001)
- Training Framework, Dataset 검증, Model Promotion/Rollback 상세: OPEN (후속 책임 문서: LEARNING-001)
- Evidence 점수/가중치/재평가 주기, Retention 기간, Recovery 기간: OPEN (후속 책임 문서: DATA-LIFECYCLE-001)
- Purge/Anonymization 기준 및 이미 Dataset/Model에 반영된 원천 삭제: OPEN (후속 책임 문서: DATA-001 / LEARNING-001)
- Permission schema/scope, 권한 선언·검증·철회, License metadata, 국가별 정책: OPEN (후속 책임 문서: DATA-001 / SECURITY-001 / Legal)
- 정량 Quality/Runtime KPI, corpus 크기, annotation/metric 정의, 수용 반복 횟수: OPEN / BENCHMARK (후속 책임 문서: Quality / Benchmark 상세)
- Dogfooding 기간·빈도: OPEN (후속 책임 문서: v1 수용 검토)
- Organization/Team/Workspace: Future / OPEN (후속 책임 문서: DOMAIN-USER-001)
- System Operator 역할: PROPOSED (후속 책임 문서: SECURITY-001)
- 실시간 Streaming, 완전한 SaaS multi-tenant 초기 제외 여부: 기존 OPEN / PROPOSED 유지 (후속 책임 문서: CONTEXT-001 / ARCH-001)
- Security 보호 수단, encryption·secret management: 기존 PROPOSED / 구현 OPEN; §15 참고 (후속 책임 문서: SECURITY-001)
- Backup 주기/기술/복구 목표, Quota/Concurrency/Cost 정책: OPEN (후속 책임 문서: INFRA-001)
- Multi-Sample Embedding, Artifact immutability 상세, 단일 Shared DB, Model Rollback: 기존 ADR 후보; 상세 OPEN (후속 책임 문서: DOMAIN-001 / DATA-001 / LEARNING-001)
- 27개 Stage 전체 목록 및 dependency의 원본 대조: 원본 상세 자료 미확보; 27개 구분 유지 (후속 책임 문서: PROCESS-001)
- SPEC/EN/JA 의미 동기화와 기준선 승인: 미완료; KO v0.2는 ACCEPTED (후속 책임 문서: GOV-001)

Core/Learning 물리 분리와 Deployment-Agnostic은 충돌하지 않는다. Multi-user 전제와 SaaS multi-tenant 운영 여부도 별개다. LONG_TERM/PERMANENT는 permission을 초과한 보존 권한이 아니며 dependency 역시 삭제 면제권이 아니다.

Inventory §15는 Security 보호 원칙을 PROPOSED로 표시했으나 v0.1은 이를 단정형으로 서술했다. v0.2에서는 §15와 본 절의 상태 등록부에 따라 제안 상태를 유지한다. 후보 목록의 내용을 승인된 ADR/Engineering Contract로 승격하지 않는다.

OPEN/PROPOSED/BENCHMARK 항목은 구현 편의 때문에 Coding Agent가 임의로 확정하지 않는다. 필요한 결정은 Architecture 검토, 실험과 ADR을 거쳐 확정한다.

---

# PART VI — GOVERNANCE

## 24. Documentation 및 Governance

PolyScript의 정식 설계 문서는 Markdown을 기본 Source Format으로 한다.

각 공식 문서는 기본적으로 다음 네 가지 Variant를 가진다.

### SPEC

`.spec.md`

기술적 의미의 Canonical Specification이다.

구현자 및 Coding Agent가 오해 없이 해석할 수 있도록 규범적이고 명확한 표현을 사용한다.

### KO

`.ko.md`

Project Owner와 한국어 독자를 위한 설명 중심 문서이다.

### EN

`.en.md`

영어권 개발자와 협업을 위한 정식 영어 문서이다.

### JA

`.ja.md`

일본어 사용자 및 개발자를 위한 정식 일본어 문서이다.

네 Variant의 표현 방식은 다를 수 있지만 기술적 의미는 동일해야 한다.

문서의 Semantic Meaning이 변경되면 네 Variant 모두 같은 Version으로 갱신해야 한다.

FOUNDATION-001 KO v0.2는 Project Owner의 2026-10-08 정식 승격 지시에 따라 ACCEPTED로 승인한다. 승인된 문서에 기록된 OPEN, PROPOSED, BENCHMARK, Future 및 ADR/INVARIANT 후보의 개별 상태는 유지한다. 문서 승인은 미결 설계의 확정 또는 구현 완료를 의미하지 않는다.

문서는 v0.1의 PART I~VI 및 0~25절 구조, 설명 중심 문단과 목록 형식을 따른다. 원본 v0.1은 `project_foundation.v0.1.ko.md`에 보존하며 현재 문서 파일명은 `project_foundation.v0.2.ko.md`이다. 확정된 설계와 미결사항은 본문 및 §23의 상태 등록부에서 관리한다.

이번 승인은 KO 문서에 적용한다. SPEC/EN/JA 의미 동기화 및 후속 기준선 승인은 아직 완료되지 않았으며, 미작성 SPEC를 대체하는 Canonical Technical Source나 BASELINED 문서로 선언하지 않는다. 정식 Design Baseline 반영 전 SPEC 작성과 EN/JA 동기화·검토를 수행한다. Inventory와 README의 후속 갱신도 필요하다.

결정 상태는 다음과 같이 해석한다.

- CONFIRMED: 확인된 설계 합의.
- OPEN: 아직 결정하지 않은 사항.
- PROPOSED: 검토 중인 제안이며 채택·구현 의무가 없음.
- BENCHMARK: 실험과 비교 평가를 거쳐 판단할 사항.
- Future / post-v1: 향후 범위이며 채택 확정과 구별.
- ADR / INVARIANT 후보: 후속 문서화 대상이며 승인된 ADR 또는 Engineering Contract와 구별.

문서 상태는 기본적으로 다음을 사용한다.

- DRAFT
- REVIEW
- ACCEPTED
- BASELINED
- SUPERSEDED
- DEPRECATED

중요한 Architecture Decision은 ADR로 기록한다.

구현이 변경되더라도 반드시 유지되어야 하는 핵심 규칙은 PolyScript Engineering Contract의 Architecture Invariant로 관리한다.

Design Baseline은 영구적으로 변경 불가능한 문서가 아니라, 변경 시 명시적인 검토와 기록을 요구하는 기준선이다.

---

## 25. 후속 Architecture 문서

FOUNDATION-001은 세부 구현을 정의하지 않는다.

세부 설계는 다음 문서로 분리한다.

`CONTEXT-001 — System Context`

PolyScript와 외부 시스템 및 사용자의 관계를 정의한다.

`ARCH-001 — System Architecture`

Control Plane, Core Processing, Learning, Data Plane 등 전체 물리·논리 Architecture를 정의한다.

`DOMAIN-001 — Domain Architecture`

Project, Speaker, Character, Script, Translation, Artifact, Learning 등 주요 Domain을 정의한다.

`DATA-001 — Data Architecture`

PostgreSQL, Artifact, Storage, Versioning, Lineage, Lifecycle을 정의한다.

`PROCESS-001 — Processing Architecture`

27개 Conceptual Pipeline Stage와 Stage 간 관계를 정의한다.

`LEARNING-001 — Learning Architecture`

Knowledge/Statistics/Engine Optimization과 Training/Adaptation의 경계, Dataset, Evaluation, Model Registry, Promotion 및 Rollback을 정의한다.

`INFRA-001 — Infrastructure Architecture`

Job, Scheduler, Worker, Engine Registry, Storage, Resource 및 Observability를 정의한다.

`SECURITY-001 — Security & Privacy Architecture`

Authentication, Authorization, Secret, Privacy, Data Access 및 Protection을 정의한다.

`GOV-001 — PolyScript Engineering Contract`

Architecture Invariant, Coding Agent Rules, Change Management 및 Documentation Rules를 정의한다.

추가 후속 영역:

- `DATA-LIFECYCLE-001`: Time/Value/Permission/Dependency, Evidence 재평가, Retention/Deletion/Purge/Anonymization 상세
- `OBSERVABILITY-001`: Job/Stage/Engine/Resource 운영 관측; Audit/Provenance와 구분
- Quality/Benchmark 상세 문서: corpus, metric, 재현 절차 및 모델·설정 비교 (정식 ID OPEN)
- 학습 제품 UI/Domain 상세: Listening/Repeat/Shadowing과 Script/Subtitle 독립 Export (정식 ID OPEN)

이 문서들은 Inventory 또는 본 Foundation에서 참조하는 예정 상세 영역이며 이번 검색에서 실제 본문은 확보하지 못했다. 이후 각 Domain, Pipeline Stage, API, UI, Database Schema 및 Deployment 상세 문서를 추가한다.

---

# FOUNDATION-001의 역할

FOUNDATION-001은 PolyScript의 모든 상세 기술을 설명하는 문서가 아니다.

이 문서의 목적은 다음 질문에 일관된 답을 제공하는 것이다.

- PolyScript는 무엇인가?
- 왜 만드는가?
- 어디까지를 책임지는가?
- 어떤 철학으로 설계하는가?
- 어떤 데이터를 중요하게 구분하는가?
- AI를 어떤 방식으로 취급하는가?
- Learning은 어떤 원칙으로 이루어지는가?
- 향후 상세 설계는 어떤 기준을 따라야 하는가?

세부 기술 선택은 후속 문서와 ADR에서 결정한다.

---

## 현재 상태

`FOUNDATION-001 KO v0.2 — ACCEPTED`

Project Owner의 2026-10-08 정식 승격 지시에 따라 승인된 한국어 Foundation 문서이다. 기존 v0.1의 25개 본문 절과 문서 형식을 유지하며, 7개 상위 결정 및 충돌하지 않는 기존 합의를 반영한다.

개별 OPEN/PROPOSED/BENCHMARK/Future 항목은 §23에 기록한 상태를 유지한다. 후속 작업은 다음과 같다.

1. 미결 제품·정책 사항의 논의와 상세 Architecture 설계.
2. 원래 27개 Pipeline Stage 목록과 Dependency 자료 확보 및 대조.
3. Quality/Benchmark와 v1 수용 검증의 세부 절차 정의.
4. Canonical SPEC 작성 및 EN/JA 의미 동기화.
5. Inventory와 README의 후속 갱신.
6. ADR/Engineering Contract 작성 및 명시적 Design Baseline 승인.

미결사항과 후속 결정은 §23의 상태 등록부를 기준으로 관리한다.
