# FOUNDATION-001 v0.1 → v0.2 변경·충돌 검토서

- 문서 ID: FOUNDATION-001-REVIEW-NOTES
- Version: 0.2
- Status: REVIEW
- 검토일: 2026-10-06 (Asia/Tokyo)
- 목적: 기존 문서와 최신 7개 상위 결정을 대조하고 FOUNDATION-001 KO v0.2의 반영 근거와 미결 사항을 기록한다.
- 이 문서는 내부 검토 자료이며 Canonical Technical Specification 또는 승인된 ADR이 아니다.

## 1. 실제 확인한 자료와 우선순위

프로젝트 미러의 `sources/`는 검색 당시 비어 있었다. 같은 프로젝트의 연결된 로컬 저장소 `C:/workspace/programming/PolyScript`에서 아래 자료를 찾아 전체 본문을 확인했다. 원본 및 synced 파일은 수정하지 않았다.

| 근거 | 확인한 경로/대화 | 용도 |
| --- | --- | --- |
| S1 | C:/workspace/programming/PolyScript/docs/00_foundation/project_foundation.ko.md | FOUNDATION-001 KO v0.1 DRAFT; 개정의 본문 기준 |
| S2 | C:/workspace/programming/PolyScript/docs/00_foundation/foundation_001_inventory.ko.md | Inventory v0.1 DRAFT; 기존 결정 상태와 후속 문서/Invariant/ADR 후보 확인 |
| S3 | C:/workspace/programming/PolyScript/README.md | 관련 설계 요약, 27-stage 및 물리적 Core/Learning 분리 확인 |
| S4 | [PolyScript 설계 요약](chatgpt-conversation://6ac12924-8018-83e8-8ff5-33bc3c5d5628) | read_thread로 반환된 최근 5개 turn; ⑥·⑦, refinement 및 전달 대화 확인 |
| S5 | 이 작업의 사용자 요청 | ①~⑦ 전체의 명시적 확정 및 기존 확정사항 유지 지시 |

적용 우선순위는 S5 → 충돌 없는 S1/S2의 기존 합의 → S3/S4의 보조 설명이다. 대화의 assistant 설명만으로 새 결정을 확정하지 않는다. S4는 `nextCursor=null`, `hasMore=false`로 반환되어 ①~⑤의 앞선 원문까지 확인하지 못했으며 해당 부분은 S5를 직접 근거로 삼았다.

별도 handoff 파일, CONTEXT/ARCH/DOMAIN/DATA/PROCESS/LEARNING/INFRA/SECURITY/GOV 본문, 27개 Stage의 전체 목록은 로컬 저장소 검색에서 발견되지 않았다. 문서명이 참조되거나 계획되어 있다는 사실을 파일 존재 또는 완료로 해석하지 않는다. 27단계 수와 개념적 구분은 유지하되 Stage 이름·번호·실행 순서는 만들지 않는다.

### 원본 식별용 SHA-256

- S1: `B26E5B902F276DA1557CC646853D33E878D47396413DF832ED4743B06C6B0E6E`
- S2: `0F54FDF23E82F5AA1123A08C881E67FF9B14F32C960057938C53481196A9F727`
- S3: `FCFDD7B3A44FE0120CC58FD4D9F44FA82E010F1F28ECDBB3F8AA16635BCB0FCB`

## 2. 대조 결과와 개정 방향

| 결정 | 기존 문서 위치와 표현 | 변경/충돌 판단 | v0.2 반영 위치 |
| --- | --- | --- | --- |
| ① Product Identity | S1 §1·3·4·7·8: 미디어 처리/지식·학습 순환 중심. §1 및 S2 §1·4: 일본어 중심, KO/JA/EN/ZH 장기 범위 | Listening/Shadowing을 제품 중심으로 명시. Script/Subtitle Export도 독립 주요 출력. 네 언어를 고정 지원 한계로 읽을 수 있는 표현 수정 | §1·3·4·7·8·11.12·22 |
| ② Deployment | S1 §1·11.1 및 S2 §11: Local-first. S3도 동일 | 개발/개인 운영 우선순위와 Core의 배포 독립성을 구분. Local-only 또는 CPU-only 제약으로 확대하지 않음 | §1·11.1·20·21 |
| ③ User Scope | S1 §6: 개인에서 시작하여 향후 다중 사용자 고려. S2 §6 역할은 CONFIRMED | Multi-user를 현재 설계 전제로 상향. 개인 dogfooding과 다중 사용자 설계는 양립. SaaS multi-tenant 운영 여부와 Organization 범위는 별개 | §5·6·7·22·23 |
| ④ Permissions/Lifecycle | S1 §16: Create→Active→Archive→Retention→Delete/Purge. §17: Processing/Storage/Learning/Distribution. S2 §16·17: 세부 삭제 미결 | 6개 목적별 권한으로 분해. 시간만으로 삭제하지 않음. 삭제 절차·장기 가치·PERMANENT 의미 확정. 구체 기간/점수/모델 삭제는 OPEN 유지 | §9·12·16·17·23 |
| ⑤ AI Learning Goal | S1 §4.12·14: Dataset/Training 중심. S2 §14: Core/Learning 물리 분리 CONFIRMED | 실제 분석 개선 목적을 명시하고 knowledge/statistics/optimization/adaptation/custom model로 Learning 범위 확장. Training은 조건부 수단 | §3·4.12·14·22 |
| ⑥ Quality/Performance | S1 §11.2 및 S2 §11, S3: Accuracy-first | Loss-Averse Adaptive Quality로 개정. 정보 손실의 큰 비용, diminishing returns, adaptive compute, benchmark-driven 확정. 무조건 2-pass 처리 또는 최고 품질 추구로 해석하지 않음 | §11.2·13·18·20·22 |
| ⑦ v1 Success | S1 §22 및 S2 §22: 품질 축; KPI OPEN. S1 §7: export 이후 Training 시나리오 포함 | First Complete Product의 장시간 E2E 및 human-correctable/job/benchmark/dogfooding 기준 구체화. 기본 Persistent Speaker reuse 포함. 고급 Learning은 post-v1 | §5·7·14·22·23 |

S1 §21의 “한 시간 이상의 긴 Media 지원”은 삭제하지 않았다. v1 수용 검증은 수십 분~약 1시간급 실제 자료를 포함하며, 1시간 초과 처리의 구체 상한과 성능 보장은 OPEN이다.

## 3. 유지한 기존 설계

| 내용 | 기존 근거 | 처리 |
| --- | --- | --- |
| 27 conceptual pipeline stages | S1 §10·25, S2 §10·14, S3 Processing Pipeline | 단계 수 및 별도 PROCESS-001 상세화 유지; 제품 E2E 흐름과 단계 목록을 혼동하지 않음 |
| Project Speaker ≠ Persistent Speaker ≠ Character | S1 §9, S2 §9 및 Invariant 요약 | Project-level Character 및 다대다 관계 유지 |
| Candidate/Canonical/Trusted/Training/Distribution 분리 | S1 §12·14, S2 §12·14 | 목적별 permission 분리를 추가하고 trust와 permission을 독립 유지 |
| Provenance/Trust/Versioning | S1 §9·11·12, S2 §12 | refinement에도 적용; 신규 AI 결과의 무조건 overwrite 금지 |
| Artifact dependency/partial rerun | S1 §7·12·18, S2 §18 | canonical 변경 시 downstream outdated/invalidation 및 재생성 원칙 유지 |
| PostgreSQL + File/Object Storage; Cache 비권위 | S1 §12, S2 §12 | 유지; 특정 storage 제품은 OPEN |
| Core/Learning physical separation | S1 §14, S2 §14, S3 architecture | 유지; Deployment-Agnostic과 양립. v1에 고급 Training Server 구축을 강제하지 않음 |
| Admin-controlled Training/Evaluation/Approval | S1 §14, S2 §14 | 유지; 자체 모델 자동 생성/배포를 확정하지 않음 |
| 4개 문서 Variant와 .spec canonical | S1 §24, S2 §24, S3 Documentation | 정책 유지. 이번 결과는 KO REVIEW이며 동기화 완료나 baseline으로 선언하지 않음 |

## 4. 판단이 필요한 항목과 상태 보존

| ID | 사항 | 상태/처리 | 후속 검토 |
| --- | --- | --- | --- |
| R-01 | Progressive Refinement 및 Background High-Quality Processing | PROPOSED / BENCHMARK, post-v1 candidate. 구현 여부 미확정 | PROCESS-001/INFRA-001; gain·cost·permission·reconciliation 실험 |
| R-02 | Refinement가 Canonical/사용자 수정을 덮어쓰는 문제 | 무조건 overwrite 금지는 S5에 따라 CONFIRMED. diff/merge UI·충돌 규칙은 OPEN | DATA-001/PROCESS-001 |
| R-03 | Core/Learning 물리 분리와 Local-first | 충돌 없음: 배포 독립성은 실행 환경 병합 허가가 아님. local-only 장비에서 Training 실행은 정당화되지 않음 | ARCH-001/INFRA-001; 배치·연결·장애 영향 상세 OPEN |
| R-04 | Multi-user와 SaaS multi-tenant | Multi-user 설계·공동 작업 가능은 CONFIRMED. SaaS 운영/tenant 모델은 기존 OPEN 유지 | CONTEXT-001/SECURITY-001 |
| R-05 | Organization/Team/Workspace | Future / OPEN | DOMAIN-USER-001 |
| R-06 | 실시간 Streaming 초기 제외 여부 | 기존 OPEN / PROPOSED 유지. v1 필수 E2E는 업로드 처리이며 Streaming 배제를 새로 확정하지 않음 | CONTEXT-001/PROCESS-001 |
| R-07 | Security & Privacy 기본 원칙 | S1은 단정적이지만 S2 §15는 PROPOSED. 역할·접근 통제 요구는 S5로 확정; 나머지 보호 수단은 PROPOSED/OPEN으로 명시 | SECURITY-001 |
| R-08 | System Operator 역할 | S2 §6의 PROPOSED 유지. ADMIN/USER 외 역할을 확정하지 않음 | SECURITY-001 |
| R-09 | Retention 기간·Evidence 평가 가중치·삭제 예외 | 상위 lifecycle/평가 축만 CONFIRMED. 기간·수치·자동화 기준은 OPEN | DATA-LIFECYCLE-001 |
| R-10 | Dataset/Model에 반영된 원천 데이터 삭제 | dependency 확인 의무 유지. 모델 unlearning·재학습·회수 방법/보장 OPEN | DATA-001/LEARNING-001/Legal |
| R-11 | 권리 불명 데이터의 Restricted | default CONFIRMED. local processing 여부도 실제 목적의 permission 확인 필요; legal entitlement로 오해하지 않음 | Legal/License/Data Usage |
| R-12 | 기본 Persistent Speaker reuse와 Global Knowledge | 기본 reuse v1 CONFIRMED. 다른 사용자/프로젝트로 공유 가능 여부는 별도 permission/scope 판단 | DOMAIN-001/SECURITY-001 |
| R-13 | Multi-Sample Embedding, 단일 Shared PostgreSQL, Model Rollback | S2는 ADR 후보로 언급. 허용되는 Shared DB/Storage를 필수 단일 DB로 확정하지 않음. 구조/정책 상세 OPEN | ARCH-001/DATA-001/LEARNING-001 |
| R-14 | Benchmark와 수용 검증 세부 | suite 자체 v1 CONFIRMED. engine·threshold·RTF 목표·반복 횟수·dogfooding 기간은 OPEN/BENCHMARK | Quality/Benchmark 상세 |
| R-15 | 문서 동기화 | SPEC/EN/JA 없음. KO REVIEW 완성 후 의미 동기화 및 승인 필요 | GOV-001 |
| R-16 | 장시간 지원 | 기존 1시간 이상 지원 방향 유지. 파일 크기·시간 상한·하드웨어별 SLA는 OPEN | PROCESS-001/INFRA-001 |

기존 후보 Invariant/ADR가 목록에 있다는 이유로 승인된 Engineering Contract/ADR로 승격하지 않는다. 이번 사용자 요청으로 확인된 상위 규칙과, 기존 Inventory에서 확인된 합의, 아직 제안인 상세 구현을 구분한다.

## 5. 후속 문서 반영 작업

1. Owner가 KO v0.2 REVIEW를 검토한다. 문서 REVIEW와 개별 결정 CONFIRMED는 서로 다른 상태다.
2. PROJECT README의 제품/언어/품질 표현과 Inventory §1·4·6·11·14·16·17·22를 후속 갱신한다. 이번 작업에서는 원본을 덮어쓰지 않는다.
3. SPEC를 작성하고 EN/JA와 같은 의미로 동기화한 뒤 ACCEPTED/BASELINED 여부를 결정한다.
4. PROCESS-001에서 원래 27단계 목록을 확보하여 reconciliation, dependency invalidation과 v1 흐름을 매핑한다.
5. DATA-LIFECYCLE-001에서 permission scope, retention value, delete cascade, anonymization 및 dataset/model dependency 처리를 상세화한다.
6. Benchmark 상세에서 corpus·annotation·hardware/config version·metric 정의 및 재현 절차를 확정한다.

작성 산출물: `project_foundation.v0.2.review.ko.md`. 이 검토서는 원본 문서에 대한 완전한 승인이나 누락된 상세 문서의 복원을 의미하지 않는다.

