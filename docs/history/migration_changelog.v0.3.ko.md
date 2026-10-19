# PolyScript AI — 설계 전환 및 변경 기록

버전 0.3 · 한국어 · 검토본 · 2026-10-19 (Asia/Tokyo)

## 1. 기준과 기록의 의미

변경 전 기준은 FOUNDATION-001 KO v0.2이며 문서 상태는 ACCEPTED, 승인일은 2026-10-08이다. 새 세트는 Windows 전용 개인용 데스크톱을 기준으로 정리한 검토본이다.

여기서 Migration은 설계·문서 전환이다. 앱 코드·DB·미디어·모델·사용자 데이터의 실제 이전·변환을 수행하지 않았다.

기존 문서는 다중 사용자·공동 작업을 확정했지만 완전한 SaaS 멀티테넌트 운영은 미결이었다. 과거 설계 전체를 확정 SaaS로 부르지 않는다.

## 2. 직접 근거와 확인 한계

현재 사용자가 Windows 개인용 전환, 한국어 5개 문서, 기존 문서의 old 이동, 새 문서의 docs 직접 배치를 지시했다. 세부 기술은 아직 미결이라고 명시했다.

참조 대화의 최근 5개 turn을 조회했으나 assistant 본문 대신 참조 표시가 반환되었고 이전 페이지는 제공되지 않았다. 현재 지시와 로컬 문서에서 확인 가능한 범위만 반영했다. 확인하지 못한 합의는 추정하지 않았다.

## 3. 변경표

| 항목 | 과거 기준 | 현재 처리 |
| --- | --- | --- |
| 실행 환경 | 개인 Local-first, 서비스·분산 실행 허용 | Windows 개인용 데스크톱으로 전환 — 확정 |
| 사용자 | 다중 사용자·공동 작업 전제 | 개인 중심. 서비스 역할·멤버십은 현재 필수 범위에서 제외 |
| SaaS | 완전 멀티테넌트 채택 미결 | 현재 범위 밖. 과거 확정 SaaS를 폐기했다는 식으로 기록하지 않음 |
| 기본 흐름 | 로그인·업로드·브라우저·서버 Job | 미디어 입력·데스크톱 검수·학습·출력. 세부 UX 미결 |
| 작업 지속 | 브라우저 종료 후 서버 작업 지속 | 앱 종료·절전·재부팅 동작 재설계 — 미결 |
| DB | PostgreSQL 기본 | 새 저장 엔진 재선정 — 미결. SQLite로 확정하지 않음 |
| 산출물·캐시 | 메타데이터와 분리, 캐시 비권위 | 원칙 유지, 경로·제품 미결 |
| Core/Learning | 서버 물리 분리 확정 | 과거 결정 보존, 새 실행 배치 재검토 — 미결 |
| 모델 적용 | ADMIN 시작·평가·승인 | 통제 원칙 유지, 개인용 승인 UX 미결 |
| 제품 목적 | Listening/Shadowing·독립 출력 | 유지 |
| 화자·캐릭터 | 프로젝트 화자·영속 화자·캐릭터 독립 | 유지 |
| 수정·재처리 | 무조건 overwrite 금지·의존성 추적 | 유지, 충돌 해결 구현 미결 |
| 기본 화자 재사용 | v1에 포함 | 개인 프로젝트 간 재사용 유지 |
| Global Knowledge | 프로젝트·사용자 범위 구분 | 개인 라이브러리 매핑 미결, 외부 공유는 현재 범위 밖 |
| 목적·신뢰 | 여섯 목적 분리·Restricted 기본 | 유지, 개인용 UX·스키마 미결 |
| 수명주기 | 시간·가치·허용 목적·의존성 | 유지, 삭제·백업 운용 미결 |
| 품질 | 정보 손실 회피·계산 효과·Benchmark | 유지. 고정 2회 처리 의무 없음 |
| 27개 개념 단계 | 존재 언급·전체 목록 미확보 | 임의 복원하지 않음. 실제 작업 수와 구분 |
| 문서 언어 | SPEC/KO/EN/JA 정책 | 이번엔 한국어만. 장기 정책 폐기는 미결 |
| 문서 상태 | v0.2 KO ACCEPTED, 개별 미결 존재 | 새 v0.3 세트는 검토본. 작성 승인과 설계 승인 구분 |

관련 근거·영향·상태는 [의사결정 v0.3](../decisions/architecture_decisions.v0.3.ko.md)를 따른다.

## 4. 개발 저장소 반영과 문서 정리

2026-10-19에 한국어 문서 5개를 개발 저장소에 반영한 뒤, 사용자의 추가 지시에 따라 docs 전체를 목적별로 재구성했다. 처음 docs 루트에 배치했던 파일은 다음과 같이 이동·개명했다.

| 정리 전 | 현재 위치 |
| --- | --- |
| FOUNDATION.md | [foundation/project_foundation.v0.3.ko.md](../foundation/project_foundation.v0.3.ko.md) |
| DECISIONS.md | [decisions/architecture_decisions.v0.3.ko.md](../decisions/architecture_decisions.v0.3.ko.md) |
| ARCHITECTURE.md | [architecture/system_architecture.v0.3.ko.md](../architecture/system_architecture.v0.3.ko.md) |
| DATA_POLICY.md | [data/data_policy.v0.3.ko.md](../data/data_policy.v0.3.ko.md) |
| MIGRATION_CHANGELOG.md | [history/migration_changelog.v0.3.ko.md](migration_changelog.v0.3.ko.md) |
| docs/00_foundation/AGENTS.md | [docs/AGENTS.md](../AGENTS.md) |

새 문서의 내용 버전은 v0.3 그대로다. 이번 정리는 새 제품 설계의 승인이나 v0.4 개정이 아니다. README와 문서 간 링크를 현재 경로에 맞게 갱신했다.

## 5. 대표본 선정과 중복 정리

| 목적·버전 | 남긴 대표본 | 정리 판단 |
| --- | --- | --- |
| Foundation v0.1 | [old/foundation/project_foundation.v0.1.ko.md](../old/foundation/project_foundation.v0.1.ko.md) | 날짜가 붙은 백업을 버전명으로 통일. 본문은 유지 |
| Foundation v0.2 | [old/foundation/project_foundation.v0.2.ko.md](../old/foundation/project_foundation.v0.2.ko.md) | 개발 저장소의 ACCEPTED 본문을 대표본으로 선정 |
| Foundation v0.3 | [foundation/project_foundation.v0.3.ko.md](../foundation/project_foundation.v0.3.ko.md) | 현재 한국어 검토본 하나 유지 |
| Inventory v0.1 | [old/inventory/foundation_inventory.v0.1.ko.md](../old/inventory/foundation_inventory.v0.1.ko.md) | Foundation과 목적이 다른 자료. 전환 전 상태임을 표시하여 보관 |
| 변경 검토서 v0.2 | [old/reviews/foundation_review_notes.v0.2.ko.md](../old/reviews/foundation_review_notes.v0.2.ko.md) | 출처·충돌·미결 판단이 있는 별도 목적의 참고 자료 |

v0.2 ACCEPTED 사본 두 개를 대조한 결과 차이는 세 문단의 검토서 참조·백업 위치·상태 안내였다. 한 사본의 검토서 참조를 제거한 개발 저장소본을 대표본으로 선택했으며 제품·도메인 결정 차이는 없었다. 참조 사본과 REVIEW 본문은 같은 v0.2 Foundation의 중복으로 제거했다. 정식 승인 이력은 대표본에, 변경 근거·검토 중 상태는 별도 검토서에 남긴다.

기존에는 사본 간 해시가 달라 둘 다 보관했으나, 이번에는 실제 내용 차이를 대조한 뒤 하나로 통합했다. 바이트가 같아서 삭제한 것이 아니다. v0.3에서 기존 합의를 유지한 판단은 바뀌지 않는다.

## 6. 지침과 정리 범위

AGENTS.md는 docs 루트로 이동하고 적용 범위를 docs 전체로 명시했다. 목적별 배치·버전명·이전 버전 보관·상태 구분·경로 검증 규칙을 현재 구조에 맞게 갱신했다. 이전의 루트 배치 지시는 이번 목적별 배치 지시로 대체한다.

개발 저장소에 이미 있던 작업은 되돌리지 않았다. sources와 앱 코드·데이터는 변경하지 않았다. 과거 승격 스크립트는 이번 문서 정리 대상에 포함하지 않으며 과거 경로를 새 승인 절차로 재사용하지 않는다. 이 정리는 개발 저장소에 적용했고 채팅의 로컬 미러는 이전 산출물 상태로 남긴다. 커밋·푸시는 수행하지 않았다.

## 7. 푸시 전 리뷰 권장사항 반영

2026-10-19 사용자 지시에 따라 루트 README를 현재 Windows 개인용 v0.3 검토본의 소개로 다시 작성했다. 기존 본문에 남아 있던 브라우저·서버·PostgreSQL 배포도, Accuracy-first 표현, 이전 폴더 구조, DRAFT 상태와 중복 제목을 정리했다. 과거 설계의 확인 경로는 old의 버전별 대표본으로 연결한다.

현재 제품 목표와 구현 완료를 구분하고, UI·DB·실행 배치·AI 외부 연결 등의 미결 상태를 명시했다. 기존 프로젝트 목적 및 이용 안내 문구는 보존했다. ADR 목록에서도 ADR-006의 배치 방식이 ADR-008로 대체되었음을 표시했다. 문서 버전·승인 상태와 기술 선택은 변경하지 않았다.

전체 문서의 내부 파일 링크, 보관 파일 중복, 버전·상태와 변경 형식을 다시 확인한다. 커밋 메시지는 별도로 제안하며 이 작업에서 커밋·푸시를 실행하지 않는다.

## 8. 향후 변경 기록 — 제안

새 합의가 생기면 ADR 상태와 영향받는 문서를 함께 갱신하고 변경일·내용·근거를 기록한다. 이전 결정을 대체하면 관계를 명시한다. 검토본 승인·실제 구현·데이터 이전은 별도 판단으로 남긴다.