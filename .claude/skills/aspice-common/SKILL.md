---
name: aspice-common
description: 이 프로젝트의 모든 A-SPICE/ISO 26262 관련 스킬(aspice-auditor, requirements-analyst, architecture-designer, detailed-designer, tdd, coding-standards, integration-tester, sw-system-test)이 공통으로 전제하는 문서 스타일, 저작권 원칙, 실제 템플릿 사용 절차, TPL-TRC-001 추적 매트릭스 스키마, 공통 금지사항, 에이전트-스킬 역할 경계를 정의한다. 다른 A-SPICE 스킬을 로드하기 전(또는 함께) 참고하며, 각 스킬은 이 내용을 반복 정의하지 않는다.
---

# A-SPICE 공통 규칙 (aspice-common)

이 스킬은 다른 A-SPICE/ISO 26262 스킬 7개(아래 "6. 스킬 목록")가 공유하는
규칙의 단일 진실 공급원이다. 개별 스킬은 자신만의 고유 내용(문서 구조,
기법, 판정 기준)만 담고, 여기 정의된 내용은 참조만 한다.

## 1. 문서 작성 스타일

CLAUDE.md "문서 작성 스타일" 정책 그대로 적용: 단답형(개조식, 명사형
종결, 불필요한 서술어/수식어 생략). 필수 정보(ID/근거/수치/절차)는
생략하지 않는다.

## 2. 저작권 유의 (공통 원칙)

- ISO 26262, ISO/IEC 25010, ISO/IEC 29119, ISO/IEC 25000, A-SPICE
  PAM 4.1, ISTQB 등 **표준 원문**(조항 번호, 정확한 문구, 표 전문)은
  각 표준 기구/VDA·intacs의 저작물이다. 공개적으로 널리 알려진
  프레임워크 구조·기법 명칭·분류명만 사용하고, 원문을 통째로 인용하지
  않는다. 세부 조문이 필요하면 사용자가 보유한 원본 표준 문서를
  참조하도록 안내한다.
- `.claude/0_WP_Templates/`, `.claude/0_OEM_Sample/`의 템플릿 파일
  자체에는 Synetics의 SW 품질교육용 저작권 표시가 있으나, **사용자
  승인에 따라 이 프로젝트에서는 그 제약을 무시하고 자유롭게 사용한다**
  (재확인 불필요, 매번 다시 묻지 않는다).

## 3. 실제 템플릿 사용 절차 (공통)

- 산출물은 마크다운을 새로 작성하지 않고, `.claude/0_WP_Templates/`
  (또는 `.claude/0_OEM_Sample/`)의 실제 docx/xlsx/drawio 템플릿 파일을
  프로젝트 산출물 경로로 복사한 뒤 채운다.
  - docx/xlsx: `docx`/`xlsx` 스킬로 내용 작성.
  - drawio: XML을 직접 작성(실제 UML/SysML 표기 셰이프로, 템플릿의
    범용 박스 스캐폴드를 그대로 두지 않는다).
- 파일명은 `.claude/0_WP_Templates/PRC-TPL-001_표준 산출물 양식
  등록부.xlsx`의 산출물 ID 규칙을 따른다(예: `ENG-SWE1-001_SW
  요구사항 명세서.docx`). 정확한 템플릿 경로·산출물 ID는 각 개별
  스킬의 "템플릿" 절에 명시되어 있다.
- 아직 정식 템플릿/양식이 제공되지 않은 영역이 있다면, 있는 것처럼
  임의로 지어내지 않고 사용자에게 확인한다.

## 4. 추적 매트릭스 — TPL-TRC-001 스키마 (단일 정의)

- 경로: `.claude/0_WP_Templates/Engineering/Traceability/TPL-TRC-001_양방향 요구사항 추적 매트릭스 템플릿.xlsx`
  (산출물 사본: `ENG-TRC-001_양방향 요구사항 추적 매트릭스.xlsx`)
- Sheet "Bidirectional Trace" 컬럼(고정, 이 순서, 9개):
  `Upper Req | SW Req | Architecture | Detailed Design | Code | SWE.4 | SWE.5 | SWE.6 | Coverage`

**컬럼 소유자(이 컬럼은 이 스킬/에이전트만 갱신한다 — 다른 스킬은
참조만 하고 값을 덮어쓰지 않는다):**

| 컬럼 | 소유 스킬 | 소유 에이전트 |
|---|---|---|
| Upper Req, SW Req | requirements-analyst | requirements-analyzer |
| Architecture | architecture-designer | architecture-design-agent |
| Detailed Design | detailed-designer | detailed-design-agent |
| Code | coding-standards | coding-agent |
| SWE.4 | tdd | coding-agent |
| SWE.5 | integration-tester | integration-test-agent |
| SWE.6 | sw-system-test | sw-system-tester |
| Coverage | 공유 컬럼(전용 소유자 없음) | (해당 없음) |

`Coverage` 컬럼은 각 담당 스킬이 자신의 커버리지 지표를 이어 붙여
기재하는 공유 컬럼이다(단위: SWE.4=함수/branch, SWE.5=함수+Call,
SWE.6=요구사항+ASIL 분기). 다른 스킬이 이미 적은 내용을 지우지 않고
추가만 한다.

**공통 절차**: 산출물을 추가/변경/삭제할 때마다 **같은 작업 안에서**
자신이 소유한 컬럼을 함께 갱신한다(별도 작업으로 미루지 않는다). 하위
산출물이 아직 없으면 "미정"으로 표기한다.

## 5. 에이전트-스킬 역할 경계 (필수 원칙)

- **스킬(SKILL.md)**: 판정 기준, 문서/템플릿 구조, 기법 목록, 품질
  지표, 금지사항의 **단일 진실 공급원**. 상세 내용은 스킬에만 존재.
- **서브에이전트(.md)**: 실행 절차만 담당 — "어떤 스킬을 언제 로드하고,
  어떤 순서로 선행 산출물을 확인하고, 언제 사용자에게 확인받는지"의
  흐름 제어. **스킬에 있는 기준표·기법 목록·템플릿 컬럼 정의·긴
  금지사항 목록을 절대 재기술하지 않고, "스킬 N장 참조/적용"으로
  위임한다.** 에이전트 파일이 스킬 파일의 상세 규칙을 복붙하듯 다시
  나열하고 있다면 경계 위반 — 발견 시 삭제하고 참조로 교체한다.
- 스킬 간에도 동일 원칙: 다른 스킬이 이미 정의한 내용(예: TPL-TRC-001
  스키마, 저작권 유의, 템플릿 사용 절차)은 이 `aspice-common`만 정의하고,
  개별 스킬은 참조한다.

## 6. 스킬 목록과 담당 A-SPICE 프로세스

| 스킬 | 담당 에이전트 | A-SPICE 프로세스 |
|---|---|---|
| aspice-auditor | aspice-cl2-auditor | (전체 프로세스 감사) |
| requirements-analyst | requirements-analyzer | SWE.1 |
| architecture-designer | architecture-design-agent | SYS.3/SWE.2 |
| detailed-designer | detailed-design-agent | SWE.3 |
| tdd + coding-standards | coding-agent | SWE.4 |
| integration-tester | integration-test-agent | SWE.5 |
| sw-system-test | sw-system-tester | SWE.6 |

## 7. 공통 금지사항 (모든 A-SPICE 스킬/에이전트에 적용)

- ISO 26262/ISO 25010/ISO 29119/ISO 25000/A-SPICE/ISTQB **표준 원문**을
  통째로 인용하는 것.
- 산출물을 직접 확인하지 않고 등급·판정·수치를 임의로 부여하는 것.
- 아직 제공되지 않은 템플릿/양식/기준을 있는 것처럼 임의로 지어내는 것.
- 산출물 변경 후 TPL-TRC-001 추적 매트릭스의 자기 담당 컬럼 갱신을
  누락하는 것.
- 실제 템플릿 파일(docx/xlsx/drawio) 대신 임의의 마크다운 구조로
  산출물을 대체하는 것.
- 다른 스킬/에이전트가 소유한 추적 매트릭스 컬럼을 임의로 덮어쓰는 것.
