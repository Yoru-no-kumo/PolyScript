# PolyScript AI 문서 안내

현재 제품 방향은 **Windows 전용 개인용 데스크톱**이다. 현재 문서 세트는 **한국어 v0.3 검토본**이며 세부 기술 선택과 정식 설계 승인은 미결이다.

## 현재 문서와 읽는 순서

| 순서 | 문서 | 목적 |
| --- | --- | --- |
| 1 | [프로젝트 기반 v0.3](foundation/project_foundation.v0.3.ko.md) | 제품 목적·범위·설계 원칙 |
| 2 | [의사결정 v0.3](decisions/architecture_decisions.v0.3.ko.md) | 결정 상태·근거·영향 |
| 3 | [아키텍처 v0.3](architecture/system_architecture.v0.3.ko.md) | 논리 책임·처리 흐름·미결 구조 |
| 4 | [데이터 정책 v0.3](data/data_policy.v0.3.ko.md) | 출처·목적·보존·삭제·전송·복구 |
| 5 | [설계 전환 기록 v0.3](history/migration_changelog.v0.3.ko.md) | 과거 대비 변경·문서 정리 이력 |

## 이전 기준과 참고 자료

| 문서 | 상태와 용도 |
| --- | --- |
| [Foundation v0.2](old/foundation/project_foundation.v0.2.ko.md) | 당시 ACCEPTED. Windows 개인용 전환 전 기준 |
| [Foundation v0.1](old/foundation/project_foundation.v0.1.ko.md) | 과거 DRAFT 백업의 대표본 |
| [Inventory v0.1](old/inventory/foundation_inventory.v0.1.ko.md) | 과거 설계의 범위·누락·후속 문서 목록. 현재 기준으로 미갱신 |
| [변경·충돌 검토서 v0.2](old/reviews/foundation_review_notes.v0.2.ko.md) | 고유한 근거·검토 판단을 보존하는 참고 자료 |

old의 문서 상태는 당시 기록이다. 현재 기준과 충돌할 때 자동 적용하지 않는다. 검토서가 남아 있다는 사실은 검토본 Foundation을 별도로 유지한다는 뜻이 아니다.

## 폴더 구조

```text
docs/
├─ README.md
├─ AGENTS.md
├─ foundation/     제품 기반
├─ decisions/      ADR
├─ architecture/   시스템 구조
├─ data/           데이터 정책
├─ history/        전환·변경 기록
└─ old/
   ├─ foundation/  이전 버전 대표본
   ├─ inventory/   과거 설계 목록
   └─ reviews/     근거·검토 기록
```

작업 지침은 [AGENTS.md](AGENTS.md)를 따른다. 동일 목적·버전·언어는 대표본 하나로 관리하고, 기존 미결사항을 정리 과정에서 확정으로 바꾸지 않는다.
