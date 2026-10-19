# FOUNDATION-001 — PolyScript Project Foundation

## 0. 문서 정보

| 항목 | 내용 |
| --- | --- |
| Document ID | FOUNDATION-001 |
| Title | PolyScript Project Foundation |
| Variant | KO |
| Version | 0.1 |
| Status | DRAFT |
| Canonical Technical Source | 추후 `project_foundation.spec.md` 작성 |
| Related Documents | CONTEXT-001, ARCH-001, DOMAIN-001, DATA-001, PROCESS-001, LEARNING-001, INFRA-001, GOV-001 |
| Purpose | PolyScript AI의 목적, 범위, 핵심 개념, 설계 철학과 이후 설계의 기준점을 정의한다. |

---

# PART I — PROJECT

## 1. 프로젝트 개요

PolyScript AI는 영상과 음성 미디어를 분석하여 화자를 구분하고, 음성을 텍스트로 변환하며, 화자·캐릭터·번역 정보를 함께 관리하여 다국어 스크립트와 자막을 생성하는 AI 기반 미디어 처리 시스템이다.

PolyScript의 목적은 단순한 음성 인식이나 자막 생성에 그치지 않는다.

미디어 처리 과정에서 생성되는 음성, 화자, 스크립트, 번역, 수정 기록, 사용자 검토 결과와 품질 정보를 서로 연결하고 추적함으로써, 하나의 프로젝트에서 얻은 검증된 지식이 향후 프로젝트와 AI 시스템 개선에 안전하게 활용될 수 있는 구조를 목표로 한다.

초기 개발은 일본어 음성 처리를 중심으로 진행하며 일본어와 한국어 간 번역을 우선한다. 장기적으로 일본어, 한국어, 영어, 중국어를 주요 입출력 언어로 지원할 수 있는 구조를 지향한다.

초기 운영 방향은 Local-first이며, 클라우드 또는 분산 환경은 필요에 따라 확장 가능하도록 설계한다.

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

PolyScript가 지향하는 장기적인 흐름은 다음과 같다.

`Media → AI Processing → Candidate Result → Validation → Human Review → Canonical Data → Trust Evaluation → Trusted Knowledge`

그리고 검증된 데이터 중 별도의 조건을 통과한 데이터만 다음 흐름으로 이동한다.

`Trusted Knowledge → Training Candidate → Training Validation → Dataset → Training → Evaluation → Approved Model → Future Processing`

따라서 PolyScript는 AI가 자동으로 모든 것을 결정하는 시스템이 아니라, AI와 사용자가 반복적으로 협력하면서 품질과 지식을 축적하는 시스템을 지향한다.

궁극적으로는 프로젝트를 반복할수록 화자, 용어, 이름, 번역, 발음과 같은 지식이 축적되고, 이를 통해 새로운 프로젝트의 처리 효율과 정확도가 향상되는 구조를 목표로 한다.

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

검증된 데이터를 별도의 Learning Pipeline을 통해 Dataset으로 구성하고 AI 모델 개선에 활용할 수 있어야 한다.

### 4.13 Traceability

AI 결과, 사용자 수정, Artifact, Dataset, Model이 어디에서 생성되고 어떻게 변경되었는지 추적 가능해야 한다.

### 4.14 Recoverability

긴 처리 작업이 실패하더라도 가능한 범위에서 완료된 결과를 재사용하고 필요한 부분만 다시 실행할 수 있어야 한다.

---

## 5. 비목표

PolyScript는 다음과 같은 시스템을 목표로 하지 않는다.

첫째, AI가 생성한 화자 identity나 기타 추론 결과를 검토 없이 절대적 사실로 취급하지 않는다.

둘째, 사용자의 모든 수정 내용을 자동으로 Training Data로 사용하지 않는다.

셋째, 새롭게 학습된 Model을 평가와 승인 없이 자동으로 Production 환경에 배포하지 않는다.

넷째, Whisper, SenseVoice, pyannote 또는 특정 번역 모델 하나에 영구적으로 종속되는 시스템을 만들지 않는다.

다섯째, 대용량 영상, 음성, Embedding, Model 또는 기타 Binary Artifact를 PostgreSQL 자체에 집중 저장하는 구조를 목표로 하지 않는다.

실시간 Streaming 처리와 완전한 SaaS Multi-Tenant 운영을 초기 목표에서 제외할지는 아직 확정하지 않았으며 후속 설계에서 결정한다.

---

## 6. 대상 사용자

PolyScript는 개인 사용자에서 시작하여 향후 여러 사용자가 프로젝트를 함께 다룰 수 있는 구조를 고려한다.

시스템 전체 수준에서는 기본적으로 다음 역할을 고려한다.

- `ADMIN`
- `USER`

각 Project 안에서는 다음과 같은 역할을 구분한다.

- `OWNER`
- `EDITOR`
- `REVIEWER`
- `VIEWER`

권한 체계의 구체적인 Authentication 및 Authorization 방식은 Security 상세 설계에서 결정한다.

---

## 7. 핵심 사용 시나리오

PolyScript의 대표적인 사용 흐름은 다음과 같다.

사용자는 새로운 Project를 생성하고 영상 또는 음성 파일을 입력한다.

시스템은 Media에서 Audio를 추출하고 필요한 전처리를 수행한다.

이후 화자를 분리하고 음성을 인식하며 시간 정보를 정렬한다.

각 프로젝트에서 발견된 화자는 `Project Speaker`로 관리된다.

시스템은 Project Speaker의 음성 특징을 기존 `Persistent Speaker` Library와 비교하고 후보를 제안할 수 있다.

사용자는 AI가 제안한 화자를 확인하거나 수정할 수 있다.

필요한 경우 새로운 Persistent Speaker를 생성하거나 기존 Speaker와 연결할 수 있다.

Project Speaker는 별도로 Project 내 Character와 연결할 수 있다.

AI가 생성한 원문 Script를 사용자가 검토하고 수정하면, 그 결과를 바탕으로 번역과 자막을 생성할 수 있다.

수정된 데이터로 인해 기존 번역이나 자막이 오래된 상태가 되면 관련 Artifact만 선택적으로 다시 생성할 수 있다.

프로젝트가 완료된 후 사용자는 원문 Script, 번역 Script, Speaker Script, Narrator Script 및 다양한 Subtitle Format을 Export할 수 있다.

관리자는 충분한 학습 후보 데이터가 축적된 경우 이를 검토하여 Dataset Version을 만들고 별도의 Learning Server에서 Training을 요청할 수 있다.

학습된 Model은 Evaluation과 명시적 승인을 거친 후에만 실제 Processing에 사용될 수 있다.

---

## 8. 시스템 범위

PolyScript의 내부 책임에는 다음이 포함된다.

- Project 관리
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

### Dataset Version

특정 시점에 선택된 Training Data의 고정된 집합이다.

### Model Version

특정 Training 또는 외부 Model에 대해 추적 가능한 버전 단위이다.

---

## 10. 개념적 시스템 흐름

PolyScript의 Core Processing 흐름은 개념적으로 다음과 같다.

`Media → Audio Preparation → Speech Analysis → Speaker Analysis → ASR → Alignment → Script → Human Review → Character/Translation → Subtitle/Output`

실제 Core Processing은 별도의 27개 Conceptual Stage로 상세 정의된다.

Learning 흐름은 Core Processing과 분리된다.

`Processing Result → Validation → Canonical Data → Trust Evaluation → Training Candidate → Training Validation → Dataset → Training → Evaluation → Model Candidate → Approval → Model Registry`

이 두 흐름은 연결되어 있지만 동일한 Pipeline은 아니다.

---

# PART III — PRINCIPLES

## 11. 설계 철학

### 11.1 Local-first

초기 PolyScript는 사용자의 로컬 환경에서 독립적으로 동작할 수 있는 구조를 우선한다.

Cloud는 선택 가능한 확장 수단으로 고려한다.

### 11.2 Accuracy-first

특히 음성 인식, 화자 구분, 중첩 발화와 같은 핵심 기능에서는 단순 처리 속도보다 결과 품질을 우선한다.

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

현재 필요한 기능만을 과도하게 최적화하기보다 향후 언어, Engine, Model과 Infrastructure를 교체하거나 확장할 수 있는 구조를 우선한다.

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

Learning은 Core Processing Pipeline과 별도의 Pipeline이다.

Core Processing Server와 Learning Server는 물리적으로 분리한다.

두 환경은 하나의 PostgreSQL과 Shared Storage를 사용할 수 있으나 실행 책임과 Runtime은 분리한다.

사용자의 Correction이 자동으로 Training Data가 되지 않는다.

검증된 Data는 먼저 Training Candidate가 되며 별도의 Validation을 통과해야 Dataset에 포함될 수 있다.

Training은 Admin이 데이터 축적량, 품질, 기간 등의 상태를 확인한 후 명시적으로 시작한다.

Training된 Model은 자동으로 Production Model이 되지 않는다.

Evaluation과 Approval을 통과한 Model만 Model Registry를 통해 실제 Processing에 사용한다.

---

## 15. Security 및 Privacy 기본 원칙

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

PolyScript의 Data는 생성 이후 영구히 무조건 보존되는 것으로 가정하지 않는다.

기본 Lifecycle은 다음과 같이 본다.

`Create → Active → Archive → Retention → Delete/Purge`

Project 삭제와 모든 관련 Data의 물리적 삭제가 반드시 동일한 의미를 가지지는 않는다.

특히 Persistent Speaker Sample, Training Dataset, Model, Audit 기록 등은 별도의 Lifecycle Policy가 필요할 수 있다.

구체적인 보존 기간과 삭제 방식은 Data Lifecycle 상세 설계에서 결정한다.

---

## 17. Legal / License / Data Usage 기본 원칙

PolyScript에서는 데이터에 대해 하나의 “사용 가능” 상태만 두지 않는다.

다음 권한은 서로 다른 의미를 가진다.

`Processing Permission`

`Storage Permission`

`Learning Permission`

`Distribution Permission`

어떤 Media를 PolyScript에서 개인적으로 Processing할 수 있다는 사실만으로 그 Media에서 생성된 음성이나 Script를 Global Training Dataset에 사용할 수 있다고 판단하지 않는다.

Data Usage 가능 여부는 License, Permission, Privacy 및 적용되는 Policy를 고려하여 별도로 판단할 수 있어야 한다.

구체적인 법률 판단과 서비스 정책은 별도 문서에서 다룬다.

---

## 18. Reliability 및 Recoverability 기본 원칙

PolyScript는 긴 Media Processing 작업을 다룬다.

따라서 하나의 오류 때문에 모든 Processing을 처음부터 다시 실행해야 하는 구조를 피한다.

Job과 Artifact는 가능한 범위에서:

- 진행상태 추적
- Retry
- Cancel
- Resume
- Partial Rerun
- Downstream Invalidation

을 지원한다.

또한 장기적으로 PostgreSQL과 File/Object Storage에 대한 Backup 및 Restore 체계를 가져야 한다.

DB와 Storage 사이의 불일치 역시 복구 가능한 문제로 취급한다.

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

구체적인 Scheduler, GPU Allocation, Concurrency 및 Cost Policy는 후속 Infrastructure 설계에서 결정한다.

---

# PART IV — CONSTRAINTS & QUALITY

## 21. 주요 제약조건

초기 개발 환경은 Windows와 Python 기반이다.

현재 개발 장비에는 NVIDIA CUDA GPU가 없으므로 초기 로컬 Processing은 CPU 사용 가능성을 고려해야 한다.

그러나 현재 개인 개발 장비의 구체적인 Hardware 사양을 PolyScript의 영구적인 Architecture Constraint로 고정하지 않는다.

PolyScript는 한 시간 이상의 긴 Media를 처리할 수 있어야 한다.

Audio, Intermediate Artifact, Model 및 Dataset 등으로 인해 Storage 사용량이 증가할 수 있다.

여러 종류와 Version의 AI Engine이 장기간 공존할 수 있어야 한다.

Core Processing과 Learning 환경은 서로 다른 Dependency 및 Hardware 요구사항을 가질 수 있다.

---

## 22. 성공 기준

PolyScript의 성공은 단순히 Media를 입력했을 때 Text가 출력되는 것으로 정의하지 않는다.

다음 품질 축을 함께 고려한다.

- Speech Recognition Accuracy
- Speaker Separation Consistency
- Speaker Identification Quality
- Overlap Speech Handling
- Timestamp Accuracy
- Translation Quality
- Reviewability
- Traceability
- Recoverability
- Reproducibility
- Data Integrity
- Extensibility
- Usability

각 지표의 정량적인 목표값은 Benchmark 및 Quality 설계에서 정의한다.

---

# PART V — EVOLUTION

## 23. 주요 위험과 OPEN 사항

현재 예상되는 주요 기술적 위험은 다음과 같다.

- 실제 미디어에서의 ASR Accuracy
- 저음량 또는 배경음이 강한 음성
- 여성 화자 인식 품질
- Overlap Speech
- Diarization 오류
- Speaker Identity 오판
- Speaker Profile Contamination
- Translation 오류
- GPU Resource와 비용
- Storage 증가
- Model 및 Framework Version Compatibility
- 저작권 및 License
- 문서와 구현의 불일치

다음 사항은 아직 결정하지 않는다.

- 최종 ASR Engine
- 최종 Diarization Engine
- 최종 Alignment Engine
- 최종 Translation Engine
- Job Queue Technology
- Scheduler Technology
- Frontend Framework
- Authentication Technology
- Cloud Provider
- Object Storage 구현
- Training Framework
- Data Retention 기간
- 삭제 세부정책
- 각 Quality Metric의 정량 기준

OPEN 항목은 구현 편의를 위해 Coding Agent가 임의로 확정하지 않는다.

필요한 결정은 Architecture 검토 및 ADR을 통해 확정한다.

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

Dataset, Training, Evaluation, Model Registry, Promotion 및 Rollback을 정의한다.

`INFRA-001 — Infrastructure Architecture`

Job, Scheduler, Worker, Engine Registry, Storage, Resource 및 Observability를 정의한다.

`SECURITY-001 — Security & Privacy Architecture`

Authentication, Authorization, Secret, Privacy, Data Access 및 Protection을 정의한다.

`GOV-001 — PolyScript Engineering Contract`

Architecture Invariant, Coding Agent Rules, Change Management 및 Documentation Rules를 정의한다.

이후 필요에 따라 각 Domain, Pipeline Stage, API, UI, Database Schema 및 Deployment에 대한 상세 문서를 추가한다.

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

`FOUNDATION-001 v0.1 — DRAFT`

다음 단계에서 확인할 주요 항목:

1. Project Vision이 실제 목표와 일치하는가.
2. Goals에서 빠진 핵심 기능이 있는가.
3. Non-Goals가 지나치게 제한적이거나 잘못된 부분이 있는가.
4. Core Concepts의 용어가 적절한가.
5. Design Philosophy에 추가하거나 제거할 원칙이 있는가.
6. Data 및 Learning Principle이 의도와 일치하는가.
7. 아직 CONFIRMED가 아닌 내용을 실수로 확정한 부분이 없는가.
