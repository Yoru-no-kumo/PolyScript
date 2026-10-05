# PolyScript AI

PolyScript AI는 영상과 음성 미디어를 분석하여 화자를 구분하고, 음성을 텍스트로 변환하며, 화자·캐릭터·번역 정보를 함께 관리하여 다국어 스크립트와 자막을 생성하는 AI 기반 미디어 처리 시스템입니다.

단순한 Speech-to-Text 또는 Subtitle Generator를 넘어, **AI Processing → Human Review → Canonical Data → Trusted Knowledge → Learning**으로 이어지는 장기적인 데이터·학습 구조를 목표로 합니다.

> 현재 프로젝트는 **설계 및 문서화 단계**에 있습니다.  
> 본격적인 Production 구현은 Design Baseline 수립 이후 시작할 예정입니다.

---

## Project Goals

PolyScript는 다음 기능을 하나의 일관된 시스템으로 통합하는 것을 목표로 합니다.

- Media / Audio 입력 및 전처리
- Speech Enhancement
- Speaker Diarization
- Overlap Speech 처리
- ASR
- Forced Alignment
- Speaker Identification
- Persistent Speaker Library
- Character Management
- Source Script 생성 및 수정
- Multilingual Translation
- Subtitle 생성
- Human Review
- Data Integrity / Quality Validation
- Artifact / Dependency Tracking
- Learning Dataset 관리
- Model Training / Evaluation
- Model Registry 및 Promotion / Rollback

초기 언어 우선순위는 일본어 음성 처리 및 일본어↔한국어이며, 장기적으로 일본어·한국어·영어·중국어 확장을 고려합니다.

---

## Core Principles

PolyScript는 다음 원칙을 기반으로 설계됩니다.

- **Local-first**
- **Accuracy-first**
- **Human-in-the-loop**
- AI 결과는 기본적으로 검증 가능한 Candidate로 취급
- 중요한 데이터와 결정은 추적 가능해야 함
- AI Engine과 Model은 교체 가능해야 함
- Processing과 Learning 환경 분리
- Canonical Data, Trusted Data, Training Data를 구분
- Versioning, Provenance, Data Lineage를 중요하게 취급
- Data Integrity와 장기적인 확장성을 단기 구현 편의보다 우선

---

## Core Domain Principle

PolyScript에서 다음 세 개념은 서로 다른 Entity입니다.

```text
Project Speaker ≠ Persistent Speaker ≠ Character
```

### Project Speaker

특정 Project 내부에서 Diarization을 통해 발견된 로컬 화자입니다.

### Persistent Speaker

여러 Project에서 재사용할 수 있는 장기적인 Speaker Profile입니다.

### Character

Project 내부에서 관리되는 독립적인 Character Entity입니다.

한 명의 Persistent Speaker가 여러 Character를 담당할 수 있으며, 하나의 Character를 여러 Speaker가 담당할 수도 있습니다.

---

## High-Level Architecture

```text
                    Browser
                       │
                       ▼
                API / Control Plane
                       │
               Scheduler / Dispatcher
                  │             │
                  ▼             ▼
            Core Server    Learning Server
            27 Pipeline    Dataset / Training
                  │         Evaluation / Model
                  └──────┬──────┘
                         ▼
                    PostgreSQL
                         +
                   Shared Storage
```

- **API / Control Plane**: 사용자 요청 및 시스템 제어
- **Core Server**: Media Processing 및 27-stage Pipeline
- **Learning Server**: Dataset, Training, Evaluation, Model Build
- **PostgreSQL**: 관계형 Metadata 및 Application State의 Source of Truth
- **File/Object Storage**: Media, Audio, Artifact, Embedding, Dataset, Model 등 대용량 데이터 저장

---

## Processing Pipeline

PolyScript의 Core Processing은 현재 27개의 Conceptual Stage로 정의되어 있습니다.

큰 흐름은 다음과 같습니다.

```text
Media
  ↓
Audio Preparation
  ↓
Speech Analysis
  ↓
Speaker Analysis
  ↓
ASR / Alignment
  ↓
Script
  ↓
Human Review
  ↓
Character / Translation
  ↓
Subtitle
  ↓
Output
```

Learning Pipeline은 Core Processing과 별도로 관리됩니다.

```text
Processing Result
  ↓
Validation
  ↓
Canonical Data
  ↓
Trust Evaluation
  ↓
Training Candidate
  ↓
Dataset
  ↓
Training
  ↓
Evaluation
  ↓
Approved Model
```

---

## Data Principles

PolyScript는 다음 데이터를 서로 다른 상태로 취급합니다.

```text
AI Result ≠ Canonical Data
Canonical Data ≠ Trusted Data
Trusted Data ≠ Training Eligible Data
Training Eligible Data ≠ Distribution Eligible Data
```

사용자가 수정한 데이터도 자동으로 Training Data가 되지 않습니다.

또한 학습된 Model은 Evaluation과 명시적 승인 없이 Production Model로 자동 승격되지 않습니다.

---

## Documentation

PolyScript는 설계를 코드보다 먼저 명확히 정의하는 것을 중요하게 생각합니다.

공식 설계 문서는 기본적으로 네 가지 Variant로 관리합니다.

```text
.spec.md  Canonical Technical Specification
.ko.md    Korean / Owner Version
.en.md    English Version
.ja.md    Japanese Version
```

기술적 의미의 Canonical Source는 `.spec.md`입니다.

주요 문서 구조는 다음과 같이 계획되어 있습니다.

```text
docs/
├─ 00_foundation/
├─ architecture/
├─ domain/
├─ database/
├─ pipeline/
├─ learning/
├─ infrastructure/
├─ security/
├─ governance/
└─ adr/
```

현재 첫 공식 Foundation 문서:

```text
FOUNDATION-001 — PolyScript Project Foundation
```

---

## Documentation Status

현재 상태:

```text
Project Phase: DESIGN / DOCUMENTATION
Implementation Status: NOT STARTED
Foundation Status: DRAFT
Design Baseline: NOT YET ESTABLISHED
```

본격적인 구현은 주요 Architecture, Domain, Data, Pipeline 및 Engineering Contract가 정리된 후 시작합니다.

---

## Planned Development Flow

```text
Project Foundation
        ↓
System Context
        ↓
System Architecture
        ↓
Domain Architecture
        ↓
Data Architecture
        ↓
Processing Architecture
        ↓
Learning / Infrastructure / Security
        ↓
Engineering Contract
        ↓
Detailed Design
        ↓
Design Baseline v1.0
        ↓
Implementation
        ↓
Test / Review / Refactor
```

---

## AI-Assisted Development

PolyScript는 AI-assisted development를 적극적으로 활용합니다.

역할은 개념적으로 다음과 같이 구분합니다.

```text
Project Owner
    ↓
Architecture / Design
    ↓
Documented Specification
    ↓
Coding Agent
    ↓
Implementation
    ↓
Review / Validation
```

Coding Agent는 확정된 Architecture, Engineering Contract, ADR 및 Baseline Specification을 임의로 변경해서는 안 됩니다.

설계상 모호한 부분은 구현 편의를 위해 임의로 결정하지 않고 `OPEN` 상태로 관리하거나 별도의 Architecture Decision을 거칩니다.

---

## Current Status

현재 PolyScript는 구현보다 **설계의 일관성, 추적성 및 장기 유지 가능성 확보**를 우선하고 있습니다.

다음 주요 작업은 다음과 같습니다.

1. `FOUNDATION-001` 검토 및 확정
2. Canonical `.spec.md` 작성
3. EN / JA Variant 생성
4. System Context 작성
5. System Architecture 작성
6. Engineering Contract 및 Architecture Invariant 정리
7. 상세 설계
8. Design Baseline v1.0 수립
9. Production Implementation 시작

---

## License

프로젝트 코드, 외부 AI Model, 학습 데이터 및 Media Content의 License는 서로 독립적으로 관리해야 합니다.

세부 License 및 Data Usage Policy는 향후 별도 문서에서 정의합니다.
