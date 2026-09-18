---
name: sw-system-test
description: SW 요구사항 명세서 기반으로 SW 시스템 테스트 케이스를 생성하기 위한 방법론 스킬입니다. ISO 26262 Part 6, A-SPICE SWE.4/5/6, ISO 29119/ISTQB, ISO 25000을 기반지식으로 하고, 기법 선택 기준·작성 원칙·PICT 도구 사용법을 정의합니다.
---

# SW 시스템 테스트 케이스 생성 (A-SPICE SWE.6)

SW 요구사항 명세서 기반 시스템 테스트 케이스 설계 방법론. "무엇을
근거로, 어떤 기법으로, 어떤 원칙으로" 설계할지만 정의. 요구사항 분석,
사용자 확인, 실제 산출물 작성은 이 스킬을 쓰는 에이전트(`sw-system-tester`)
가 수행.

**전제**: `aspice-common` 스킬(문서 스타일, 저작권 유의, 템플릿 사용
절차, TPL-TRC-001 스키마, 공통 금지사항)을 전제로 한다.

## 0. 템플릿 경로 (이 스킬 고유, 저장소 루트 `D:\NGV\NGV` 기준)

- `.claude/0_WP_Templates/Engineering/SoftwareVerification/TPL-SWE6-001_SW 검증 명세서 템플릿.xlsx`
  — 시트 "Verification Specification"(컬럼: `Test ID | SW Req |
  Level/Environment | Stimulus | Expected Result | Technique |
  Execution`), "Environment", "Change History".
- `.claude/0_WP_Templates/Engineering/SoftwareVerification/TPL-SWE6-002_SW 검증 결과서 템플릿.xlsx`
  — 시트 "Verification Results"(컬럼: `Test ID | SW Req | Result |
  Actual Result | Evidence Locator | Execution Time | Scope Note`),
  "Summary", "Change History". 실행/결과 기록은 이 스킬 범위 밖(별도
  실행 담당자/에이전트 몫).
- 산출물 ID: `ENG-SWE6-001`(명세서), `ENG-SWE6-002`(결과서). 템플릿
  사용/파일명 절차는 `aspice-common` 3장 참조.

## 1. 기반지식 (개요만, 원문 인용 아님)

- **ISO 26262 Part 6**: SW 단위/통합/시스템 테스트 요건. ASIL 할당
  요구사항은 판정기준의 전 분기(경계값, 조건 조합)를 빠짐없이 검증.
- **A-SPICE SWE.4/5/6**: 단위검증/통합시험/시스템시험 Base Practice.
  양방향 추적성(요구사항↔테스트케이스)과 표준화된 시험 절차 요구.
- **ISO 29119 및 ISTQB**: 테스트 시나리오·케이스 설계 프로세스와 용어
  체계(긍정/부정 분류 등) 근거.
- **ISO 25000(SQuaRE)**: 비기능 요구사항 분류 기준. 비기능 테스트
  케이스는 이 표준의 품질특성(기능적합성/성능효율성/호환성/사용성/
  신뢰성/보안성/유지보수성/이식성)별로 그룹화.

## 2. 적용 대상 테스트 기법

요구사항 성격에 맞춰 선택, 각 케이스의 "사용 기법"(TPL-SWE6-001
`Technique` 컬럼)에 실제 적용 기법 명시.

- **경계값 분석**: 수치·범위·임계값 있는 판정기준 → 경계값 미만/경계값/
  경계값 초과 각각 별도 케이스.
- **동치 클래스(동등분할)**: 입력 도메인을 유효/무효 클래스로 나눠
  대표값 도출.
- **조합 테스트(전 조건 조합)**: 조건 수 적어 전수 조합 실행 가능할 때
  우선 적용.
- **페어와이즈**: 조합 수 많아 전수 조합이 비현실적일 때 대체 적용.
- **엣지 케이스**: 정상 범위 극단, 동시성/타이밍 등 일반 기법으로
  못 잡는 특이 조건.
- **경험 기반 테스트**: 유사 시스템 결함 이력·QA 경험 기반 오류 추정
  (오류 추정, 탐색적 테스트 등).

## 3. 작성 원칙

- 커버리지 수치 달성만을 위한 무의미한 케이스 생성 금지.
- 예상 결과(Expected Result)를 임의로 가정 금지 — 명세서에 근거 없으면
  에이전트가 사용자 확인 후 반영.
- 프로젝트 커버리지 목표(CLAUDE.md 지정) 준수.
- 실행자가 그대로 재현 가능한, 실행 가능한 케이스만 작성.
- 기법 선택·커버리지 범위가 애매하면 임의 결정 금지, 사용자 검토 요청.
- 명세서에 직접 근거 없으나 QA 관점 제안 케이스는 케이스명 끝에
  "(제안)" 표기.
- 사용 기법을 각 케이스에 명시.

## 4. 도구 사용 — PICT

- 페어와이즈 조합 생성: 오픈소스 PICT 사용(설치 경로 예:
  `%LOCALAPPDATA%\Microsoft\WinGet\Packages\Microsoft.PICT_*\pict.exe`,
  PATH 등록 후 새 세션에서 `pict` 명령으로 호출 가능).
- 전수 조합(모든 조건 조합) 생성: PICT `/o:max` 옵션 사용.
- 모델 파일(입력 파라미터·값) 작성 후 `pict model.txt > cases.txt`로
  조합 생성, 생성된 조합을 실제 테스트 케이스 행으로 변환.

## 5. 산출물 구조

- **기능 요구사항 테스트 케이스**: TPL-SWE6-001 "Verification
  Specification" 시트에 작성(템플릿 컬럼 그대로: Test ID/SW Req/
  Level-Environment/Stimulus/Expected Result/Technique/Execution).
  프로젝트 확장 컬럼 `확정 근거` 추가(요구사항 원문에 없거나 원문보다
  엄격한 판정기준을 사용자 확인 거쳐 확정한 경우, 확정 일자·승인 주체
  기록).
- **비기능 요구사항 테스트 케이스**: 템플릿에 전용 시트 없음 — 동일
  파일에 확장 시트(예: "Non-Functional Verification")를 추가해 같은
  컬럼 구조 + `ISO25000 품질특성` 컬럼으로 그룹화 작성. 등록부에 정식
  반영이 필요하면 사용자에게 제안.
- **결과서**: 실행 후 TPL-SWE6-002 "Verification Results" 시트(Test
  ID/SW Req/Result/Actual Result/Evidence Locator/Execution Time/
  Scope Note)에 기록 — 이 스킬은 케이스 설계까지만, 실행 담당은 별도.

## 6. 양방향 추적성 — TPL-TRC-001 컬럼 소유

- 이 스킬은 `SWE.6` 컬럼을 채운다(테스트 케이스 ID 기재. 스키마·경로·
  공통 절차는 `aspice-common` 4장 참조).
- `Coverage` 컬럼에는 요구사항 커버리지(모든 SW 요구사항이 최소 1개
  이상 시스템 테스트 케이스로 커버됐는지)와, ASIL 할당 요구사항의 경우
  판정기준 전 분기 커버 여부를 공유 기재한다(다른 스킬 기재분은 지우지
  않는다).

## 7. 하지 말아야 할 것 (이 스킬 고유 — 공통 금지사항은 `aspice-common` 7장 참조)

- 요구사항 명세서에 없는 내용을 임의로 추측해 케이스 작성.
- 예상 결과를 임의로 가정(명세서에 근거 없으면 사용자 확인 필수).
- ISTQB 용어 정의를 통째로 인용.
- 커버리지 수치만을 위한 무의미한 케이스 생성.
