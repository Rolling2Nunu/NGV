---
name: integration-tester
description: A-SPICE SWE.5(Software Component Verification and Integration Verification/Testing)와 ISO 26262 Part 6 소프트웨어 통합 테스트 원칙을 준수하는 SW 통합 테스트를 계획·실행할 때 사용한다. 아키텍처 설계서의 인터페이스·통합순서를 테스트 베이시스로 사용, 함수 커버리지/Call 커버리지 100% 달성, ISO 26262 Part 6 기반 테스트 기법 선정이 필요할 때 로드한다.
---

# 소프트웨어 통합 테스터 (Integration Tester)

이 스킬은 A-SPICE PAM 4.1 SWE.5(Software Component Verification and
Integration Verification)와 ISO 26262 Part 6 소프트웨어 통합 테스트
원칙을 준수하는 통합 테스트 절차를 정의한다. `architecture-designer`가
정의한 인터페이스·통합순서를 테스트 베이시스로 사용한다.

## 문서 작성 스타일

단답형(개조식: 명사형 종결, 불필요한 서술어 생략). 테스트 함수
docstring 등 `tdd` 스킬이 요구하는 형식은 그대로 유지.

## 0. 템플릿 — 확정, 실제 파일 사용

정식 템플릿 확정됨(사용자 승인으로 교육용 저작권 표시 무시하고 사용).
경로(저장소 루트 `D:\NGV\NGV` 기준):

- `.claude/0_WP_Templates/Engineering/SoftwareComponentVerificationAndIntegrationVerification/TPL-SWE5-001_SW 통합전략 및 통합시험 명세서 템플릿.docx`
- `.claude/0_WP_Templates/Engineering/SoftwareComponentVerificationAndIntegrationVerification/TPL-SWE5-002_SW 통합시험 케이스 템플릿.xlsx`
- `.claude/0_WP_Templates/Engineering/SoftwareComponentVerificationAndIntegrationVerification/TPL-SWE5-003_SW 통합시험 결과서 템플릿.xlsx`
- `.claude/0_WP_Templates/Engineering/Traceability/TPL-TRC-001_양방향 요구사항 추적 매트릭스 템플릿.xlsx`
- `.claude/0_WP_Templates/PRC-TPL-001_표준 산출물 양식 등록부.xlsx` — 산출물 ID(예: `ENG-SWE5-001/002/003`)

**산출물 생성 절차**: 마크다운 신규 작성 대신 위 docx/xlsx 템플릿을
산출물 경로로 복사 후 `docx`/`xlsx` 스킬로 채운다. 파일명은 등록부 ID
규칙 따름(예: `ENG-SWE5-001_SW 통합전략 및 통합시험 명세서.docx`,
`ENG-SWE5-002_SW 통합시험 케이스.xlsx`, `ENG-SWE5-003_SW 통합시험
결과서.xlsx`).

## 1. 저작권 유의

A-SPICE PAM 4.1과 ISO 26262 **표준 원문**(조항 번호, 정확한 문구, 표
전문, 기법별 ASIL 권고표 전체)은 VDA/intacs와 ISO 저작물 — 통째 인용
금지. 공개적으로 널리 알려진 기법 명칭(인터페이스 테스트, 결함 주입,
경계값 분석 등)만 사용. 정확한 ASIL별 권고 수준이 필요하면 사용자가
보유한 ISO 26262 Part 6 원본을 참조하도록 안내한다. (0_WP_Templates
구조 자체는 사용자 지시로 무시하고 사용.)

## 2. 문서 구조 — TPL-SWE5-001 목차(확정, 14장)

1. 목적 및 범위
2. 통합 원칙
3. 통합 항목과 순서(ID/선행조건/의존성/순서/담당/계획 베이스라인) —
   **`architecture-designer` 11장(통합 전략)에서 넘겨받은 상세 통합
   순서를 이 장에서 확정·상세화**한다(중복 유지보수 방지를 위해 이
   문서가 상세 통합 순서표의 단일 진실 공급원이 된다).
4. 환경 및 형상
5. 진입/종료 기준
6. 통합시험 케이스 요약
7. 시험 설계기법(ISO 26262 Part 6 기반, 아래 4장 참조)
8. 실행/결과 기록 규칙
9. 회귀 전략
10. 실패/편차 처리
11. 추적성과 보고
12. 적용 한계
13. 추적성
14. 참고자료

공통 골격(모든 TPL-*.docx 동일): 표지 → 문서통제(변경이력표, 작성검토
승인상태표) → 목차 → 본문.

## 3. 테스트 베이시스 (필수) — 아키텍처 인터페이스 + 통합순서

- **인터페이스**: `architecture-designer` 산출물 6장(내부 API/이벤트/
  데이터저장소/동기화, 외부 HW/통신/사용자/외부서비스)의 오퍼레이션·
  입력·출력·사전/사후조건·오류계약을 테스트 케이스의 근거로 삼는다.
  아키텍처 산출물이 없으면 사용자에게 알리고 진행하지 않는다(임의로
  인터페이스를 지어내지 않는다).
- **통합순서**: 이 스킬 2장(문서 3장)에서 확정한 통합 항목·순서를
  따라 테스트를 진행한다. 순서를 벗어난(아직 통합되지 않은 상위
  컴포넌트에 의존하는) 테스트를 먼저 실행하지 않는다.
- 각 통합 단계마다: 해당 단계에서 새로 노출되는 인터페이스 식별 →
  그 인터페이스의 오퍼레이션별 테스트 케이스 도출 → 실행 → 회귀(이전
  단계 인터페이스 재검증) 순으로 진행한다.

## 4. ISO 26262 Part 6 기반 테스트 기법 (7장)

공개적으로 알려진 소프트웨어 통합 테스트 기법 중 해당 컴포넌트/
인터페이스 특성에 맞는 것을 선정하고, 케이스마다 사용 기법을 명시한다.

- **요구사항 기반 테스트(Requirements-based test)**: 인터페이스에
  할당된 요구사항(수용기준)을 근거로 케이스 도출.
- **인터페이스 테스트(Interface test)**: 오퍼레이션별 입력/출력 타입,
  범위, 프로토콜 준수 여부 검증.
- **경계값 분석(Boundary value analysis)**: 입력 파라미터의 경계·
  한계값 검증.
- **동등 분할(Equivalence classes)**: 입력 값의 클래스별 대표값 검증.
- **오류 처리/결함 주입(Fault injection / error handling test)**:
  인터페이스 계약의 오류 코드·타임아웃·재시도 경로를 의도적 오류
  입력·지연·연결 끊김 등으로 검증.
- **자원 사용 평가(Resource usage test)**: 실시간 제약·메모리·CPU
  등 아키텍처가 명시한 자원 제약을 통합 시점에 확인(해당 시).
- ASIL별 권고 강도(권장/강력 권장)는 프로젝트가 보유한 ISO 26262 Part 6
  원본 표를 따른다. 이 스킬은 임의로 ASIL-기법 매핑을 확정하지 않으며,
  불명확하면 "ASIL별 권고 확인 필요"로 표시한다.

## 5. 커버리지 요구사항 — 함수 커버리지·Call 커버리지 100% (필수)

- **함수 커버리지(Function Coverage) 100%**: 통합 대상 범위의 모든
  함수/메서드가 통합 테스트 중 최소 1회 이상 실제로 호출되어야 한다.
  측정: `coverage.py`(`coverage run --branch -m unittest discover` 후
  `coverage report`/`coverage html`)의 함수 단위 실행 여부로 확인.
- **Call 커버리지(Call/Call-pair Coverage) 100%**: `architecture-designer`/
  `detailed-designer`의 호출관계 다이어그램(.drawio, 컴포넌트 간·모듈
  간 호출 엣지)에 정의된 모든 호출 경로가 통합 테스트 중 최소 1회 이상
  실제로 실행되어야 한다.
  - **측정 방법(제약 명시)**: 일반적인 오픈소스 커버리지 도구는
    "호출 엣지" 단위를 직접 리포트하지 않는다. 따라서 다음을 조합해
    실측한다: (1) `sys.settrace`/`trace` 모듈 또는 프로파일러(예:
    Python 표준 `trace` 모듈의 `--trace`/콜 그래프 로그)로 실행 중
    실제 호출 쌍(caller→callee)을 기록, (2) 이 로그를 호출관계
    다이어그램의 엣지 목록과 대조해 모든 엣지가 최소 1회 실행됐는지
    수작업/스크립트로 대조. 이 방법이 이 환경에서 실행 불가능하면
    "Call 커버리지 실측 불가 — 대체 방법 확인 필요"로 명시하고 100%를
    임의로 단정하지 않는다.
  - 100% 미달 시: 누락된 함수/호출 경로를 식별해 테스트 케이스를
    추가하거나, 도달 불가능한 코드(dead code)로 판명되면 그 근거를
    남기고 사용자에게 코드 제거 여부를 확인한다(임의 삭제 금지).

## 6. 테스트 케이스 작성 (TPL-SWE5-002 컬럼 기준)

컬럼: `Test ID | Trace | Integration Item | Stimulus | Expected Result | Technique | Automation`

- **Trace**: 근거 요구사항 ID + 아키텍처 인터페이스 ID(둘 다 기재).
- **Technique**: "4. ISO 26262 Part 6 기반 테스트 기법" 중 실제 사용.
- 각 테스트는 `tdd` 스킬의 문서화 규칙을 따른다 — 자동화 테스트
  함수 docstring에 `@brief`(목적), `@technique`(위 4장 기법명),
  `@case`(positive/negative) 기재. 통합 단계 정보도 `@integrationStep`
  으로 추가 기재(3장 통합 순서의 단계 번호).

## 7. 테스트 실행 및 결과 (TPL-SWE5-003 컬럼 기준)

컬럼: `Test ID | Trace | Result | Actual Result | Evidence Locator | Defect ID | Disposition`

- Evidence Locator에는 실제 커버리지 리포트 파일 경로, 로그 경로를
  남긴다(재현 가능하도록).
- 실패 시 Defect ID를 부여하고(형식은 프로젝트 결함관리 규칙 따름,
  없으면 "결함관리 규칙 미정 — 확인 필요"), 회귀 테스트 범위에 포함.

## 8. 추적성 — TPL-TRC-001 `SWE.5` 컬럼(확정)

- `Upper Req | SW Req | Architecture | Detailed Design | Code | SWE.4 | SWE.5 | SWE.6 | Coverage`
  매트릭스의 `SWE.5` 컬럼에 통합 테스트 케이스 ID를 기재한다.
- `Coverage` 컬럼에는 이 스킬의 함수/Call 커버리지 실측 결과(달성률,
  100% 미달 시 사유)를 요약 기재한다.
- 통합 테스트 추가/변경 시 같은 작업 안에서 매트릭스를 함께 갱신한다.

## 9. 하지 말아야 할 것

- 아키텍처 인터페이스·통합순서 없이 임의로 테스트 케이스를 작성하는 것.
- 정의된 통합 순서를 벗어나(선행 컴포넌트 미통합 상태로) 테스트를
  진행하는 것.
- 함수/Call 커버리지를 실제로 측정하지 않고 100% 달성을 단정하는 것.
- ISO 26262/A-SPICE **표준 원문**이나 ASIL-기법 권고표 전문을 통째로
  인용하는 것.
- 통합 테스트 변경 후 추적 매트릭스(`SWE.5`/`Coverage` 컬럼) 갱신을
  누락하는 것.
- 실제 템플릿 파일(docx/xlsx) 대신 임의의 마크다운 구조로 산출물을
  대체하는 것.
