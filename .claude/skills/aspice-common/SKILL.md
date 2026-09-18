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
  - docx: **HTML 초안 → 검토 → DOCX 확정** 절차를 따른다(3.3 참조).
  - xlsx: **LibreOffice(soffice, UNO API/매크로)를 정식 도구로
    사용한다.** 임시 python-docx/openpyxl 스크립트나 OOXML을 직접
    unzip해 XML을 조작하는 방식은 LibreOffice가 없던 시점의 임시방편이었고,
    LibreOffice가 설치된 이후로는 사용하지 않는다. LibreOffice가 이
    환경에 아직 없으면 그 작업을 시작하는 시점에 설치한다(사전 설치
    불필요 — 필요할 때 설치).
  - drawio: XML을 직접 작성(실제 UML/SysML 표기 셰이프로, 템플릿의
    범용 박스 스캐폴드를 그대로 두지 않는다).
- 파일명은 `.claude/0_WP_Templates/PRC-TPL-001_표준 산출물 양식
  등록부.xlsx`의 산출물 ID 규칙을 따른다(예: `ENG-SWE1-001_SW
  요구사항 명세서.docx`). 정확한 템플릿 경로·산출물 ID는 각 개별
  스킬의 "템플릿" 절에 명시되어 있다.
- 아직 정식 템플릿/양식이 제공되지 않은 영역이 있다면, 있는 것처럼
  임의로 지어내지 않고 사용자에게 확인한다.

### 3.1 표 우선 원칙 (줄글 대신 표)

- 템플릿이 표 형식을 명시하지 않은 장(예: "작성 안내" 서술문만 있는
  목적/범위/가정/원칙 등)이라도, **나열 가능한 개별 항목이 여럿이면
  줄글로 풀어쓰지 않고 적절한 컬럼의 표를 만들어 그 안에 작성한다.**
  사용자가 훑어보기 쉬운 형태를 우선한다.
- 표 컬럼은 내용 성격에 맞게 정의한다(예시, 고정 규칙 아님):
  - 후보/대안 비교: 후보 | 개요 | 장점 | 단점 | 적합성(선정 사유)
  - 가정·미확정 사항: 항목 | 가정/미확정 내용 | 근거 | 영향 범위 | 확인 방법
  - 리스크/이슈: ID | 내용 | 영향 | 대응 | 상태
  - 결정 이력: 일자 | 결정 사항 | 사유 | 근거 문서
- 이미 템플릿이 표를 지정한 장(예: 요구사항 표, 인터페이스 표, 테스트
  케이스 표)은 그 표 형식을 그대로 따른다 — 이 원칙으로 컬럼을 임의
  변경하지 않는다.
- 표로 옮기기 부적절한 진짜 서술(배경 설명, 목적 문장 한두 줄 등)까지
  억지로 표로 쪼개지 않는다 — 나열 가능한 개별 항목이 있을 때만 적용.

### 3.2 다이어그램: 원본 보존 + 렌더링 이미지 삽입 (필수)

- 다이어그램 원본(.drawio 등)은 지금까지와 동일하게 산출물 하위
  `diagrams/` 폴더에 별도 파일로 보존한다(문서 안에 원본 XML을 그대로
  붙여넣지 않는다 — 기존 관행 유지).
- **문서(docx)에는 원본 대신, 그 다이어그램을 이미지로 렌더링한 결과를
  삽입한다.** 렌더링 없이 파일 경로 텍스트만 언급하고 끝내지 않는다.
- 렌더링 도구: draw.io 데스크톱 CLI(winget `JGraph.Draw`로 설치됨).
  경로: `%LOCALAPPDATA%\Programs\draw.io\draw.io.exe`.
  ```
  draw.io.exe -x -f png -o <출력.png> -p <페이지번호(1부터)> <입력.drawio>
  ```
  페이지가 여러 개면 페이지별로 반복 실행해 각각 이미지를 만든다
  (`-p 1`, `-p 2`, ...). 렌더링 이미지는 원본과 같은 `diagrams/` 폴더에
  `<원본파일명>_p<페이지번호>.png` 형식으로 저장한다.
- 렌더링이 이 환경에서 실행 불가능하면(도구 없음 등) "다이어그램 렌더링
  불가 — 원본(.drawio) 경로만 첨부"로 명시하고, 임의로 이미지가 있는
  것처럼 위장하지 않는다.
- `docx`/`xlsx` 스킬로 문서를 채울 때 이 렌더링 PNG를 이미지로 삽입한다
  (원본 XML 텍스트를 표/문단에 붙여넣지 않는다).

### 3.3 DOCX 산출물: HTML 초안 → 검토 → DOCX 확정 (필수)

대상 템플릿: TPL-SWE1-001(요구사항), TPL-SWE1-002(Use Case),
TPL-SWE2-001(아키텍처), TPL-SWE3-001(상세설계), TPL-SWE5-001(통합전략).
(xlsx 산출물 — 추적매트릭스/테스트케이스/SBOM/리뷰기록 등 — 은 이 절차
대상이 아니다. 표 형태 그대로 xlsx로 직접 작성한다.)

1. **HTML 초안 작성**: 실제 TPL-*.docx의 장/절 구조(표지·문서통제·목차·
   본문 순서)를 그대로 반영한 HTML 파일을 작성한다. 3.1(표 우선)과
   3.2(다이어그램 렌더링 이미지 `<img>` 삽입)를 이 초안 단계에서 이미
   적용한다. 저장 위치: 산출물 폴더 하위 `drafts/`, 파일명은 산출물 ID
   기준(예: `Engineering/SoftwareRequirementsAnalysis/drafts/
   ENG-SWE1-001_SW 요구사항 명세서.html`).
2. **검토**: 별도 체크포인트를 새로 만들지 않는다 — 이 프로젝트의 기존
   phase 단위 리뷰(phase 종료 시 1회, 아키텍처 후보안만 예외)에 초안
   HTML을 포함해 함께 검토받는다. 사용자는 브라우저로 초안을 열어
   확인할 수 있다.
3. **DOCX 확정**: 사용자 승인 후, 실제 TPL-*.docx 템플릿을 산출물
   경로로 복사하고 LibreOffice(soffice, UNO)로 승인된 초안 내용을 그
   템플릿에 옮겨 채운다. 템플릿의 표지·문서통제·목차 구조는 그대로
   유지하고 본문만 초안 내용으로 채운다.
4. 확정 후에도 초안 HTML은 `drafts/`에 그대로 남긴다(삭제하지 않음 —
   검토·승인 근거로 보존).
5. 승인 없이 곧바로 DOCX를 확정하지 않는다. 초안 단계에서 이미 문제가
   있으면 DOCX로 옮기기 전에 고친다.

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

## 8. 서브에이전트 간 인수인계(Handoff) 로그 (필수)

여러 서브에이전트(요구사항→아키텍처→상세설계→구현→통합테스트→시스템
테스트)가 순차적으로 작업을 이어받을 때, 서로의 맥락(확정 사항, 가정,
다음 단계가 주의할 점)이 오케스트레이터(사용자와 대화하는 세션)의
프롬프트에만 존재하면 세션이 끊기거나 다른 사람이 이어받을 때 유실된다.
이를 막기 위해 **저장소 안에 append-only 텍스트 로그를 남긴다.**

- **경로**: `Engineering/_handoff/<phase-slug>.md` (예:
  `Engineering/_handoff/phase-1-input-validation.md`). phase-slug는
  해당 작업의 Git 브랜치명(예: `feature/phase-1-input-validation`)에서
  `feature/` 접두사를 뺀 값과 일치시킨다. phase 개념이 없는 1회성
  작업(예: aspice-auditor 감사)은 이 로그를 생략해도 된다.
- **시작 시**: 이 phase의 handoff 로그 파일이 이미 있으면 **작업을
  시작하기 전에 반드시 전체를 읽는다** — 이전 스테이지가 남긴 확정
  사항·가정·주의사항을 그대로 재사용하고, 이미 답한 질문을 다시 묻지
  않는다.
- **종료 시**: 파일 끝에 **자신의 섹션을 추가한다(append-only —
  다른 스테이지가 이미 쓴 섹션은 절대 수정·삭제하지 않는다)**. 형식:

  ```markdown
  ## <스테이지>(<A-SPICE 프로세스>) — <담당 에이전트> — <완료 시각/커밋 근거>

  - 완료 산출물: <파일 경로 목록>
  - 확정 사항(다음 단계가 재사용할 것): <ID/이름 목록과 한 줄 설명>
  - 가정/미확정 사항(다음 단계가 검토할 것): <항목과 현재 처리 방식>
  - 다음 단계 주의사항: <구체적으로>
  ```

- 파일이 아직 없으면(그 phase의 첫 스테이지) 새로 생성한다.
- 이 로그는 대화 요약이 아니라 **다음 에이전트가 파일 하나만 읽고도
  이어서 작업할 수 있게 하는 실행용 문서**다 — 장황한 서술 대신 3장(표
  우선 원칙)에 따라 표로 정리해도 된다.
- 오케스트레이터(에이전트를 호출하는 쪽)도 각 에이전트 실행 프롬프트에
  "먼저 `Engineering/_handoff/<phase-slug>.md`를 읽어라"를 포함해
  자연어 프롬프트와 handoff 로그가 이중으로 맥락을 보강하게 한다 —
  handoff 로그가 프롬프트 재구성의 부담을 줄이고, 프롬프트가 handoff
  로그 누락/오독을 보완한다.
