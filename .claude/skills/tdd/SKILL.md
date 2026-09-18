---
name: tdd
description: 테스트 주도 개발(TDD) 방식으로 코드를 구현할 때 사용한다. 실패하는 테스트를 먼저 작성하고, 최소 구현으로 통과시킨 뒤, 리팩터링하는 Red-Green-Refactor 절차, 좋은 테스트 작성 규칙(가짜/실제 검증 구분), 테스트 기법·긍정/부정 케이스·Doxygen 목적 설명 기재 규칙이 필요할 때 로드한다.
---

# TDD (Test-Driven Development)

출처: https://github.com/obra/superpowers 의 `test-driven-development`
스킬을 이 프로젝트(Python 3.14, unittest)에 맞게 번역·각색. 기존 NGV
tdd 스킬에서 유효한 부분(unittest 표준, 상세설계 함수 계약과의 커버리지
매핑, `test_*.py` 파일명 규칙)은 그대로 유지.

## 문서 작성 스타일

단답형(개조식: 명사형 종결, 불필요한 서술어 생략). 단, 예시 코드·표는
원문 구조 유지.

## 0. 핵심 원칙 — Iron Law

```
실패하는 테스트 없이 구현 코드 작성 금지
```

- 테스트보다 구현을 먼저 썼다면: 그 코드는 삭제하고 처음부터 다시.
  "참고용으로 남긴다", "테스트 쓰면서 살짝 고친다", "일단 보기만 한다"
  전부 금지 — 삭제는 삭제.
- 테스트를 먼저 보지 않고 실패를 확인하지 않았다면, 그 테스트가 맞는
  것을 검증하는지 알 수 없다.
- **규칙의 문구만 지키고 취지를 어기는 것도 규칙 위반이다.**

## 1. 언제 적용하나

- **항상**: 신규 기능, 버그 수정, 리팩터링, 동작 변경.
- **예외(사용자 확인 필요)**: 일회성 프로토타입, 생성된 코드, 설정 파일.
- "이번만 TDD 생략" 생각이 들면 그 자체가 합리화 신호 — 멈추고 원칙대로.

## 2. Red-Green-Refactor 절차

### RED — 실패하는 테스트 작성

하나의 최소 단위 동작을 검증하는 테스트를 작성한다.

좋은 예:
```python
def test_retryOperation_failsTwiceThenSucceeds_returnsResult(self):
    """
    @brief 2회 실패 후 3회째 성공 시 최종 결과 반환 검증
    @technique 상태 기반 시나리오 테스트(재시도 횟수 경계)
    @case positive
    """
    attempts = {"count": 0}

    def operation():
        attempts["count"] += 1
        if attempts["count"] < 3:
            raise RuntimeError("fail")
        return "success"

    result = retryOperation(operation)

    self.assertEqual(result, "success")
    self.assertEqual(attempts["count"], 3)
```
명확한 이름, 실제 코드 검증, 한 가지 동작만 검증.

나쁜 예:
```python
def test_retry_works(self):
    mock = Mock(side_effect=[RuntimeError(), RuntimeError(), "success"])
    retryOperation(mock)
    self.assertEqual(mock.call_count, 3)
```
모호한 이름, 실제 코드가 아니라 모의객체(mock) 자체를 검증.

**요구사항**: 한 가지 동작만 / 명확한 이름 / 실제 코드(불가피할 때만
모의객체) / **테스트 기법·긍정·부정 케이스·목적을 Doxygen 형식으로 기재**
(아래 "6. 테스트 문서화 규칙" 참조).

### Verify RED — 실패 확인 (필수, 생략 금지)

```bash
python -m unittest tests.test_module.TestClass.test_method -v
```

확인 사항:
- 테스트가 실패(에러 아님)하는가
- 실패 메시지가 예상과 같은가
- 기능이 없어서 실패하는가(오타 때문이 아니라)

테스트가 통과해버리면 → 이미 있는 동작을 테스트한 것, 테스트를 수정.
테스트가 에러(오류)를 내면 → 오류를 고치고 "실패"가 나올 때까지 재실행.

### GREEN — 최소 구현

테스트를 통과시키는 가장 단순한 코드만 작성한다.

좋은 예:
```python
def retryOperation(fn, maxAttempts=3):
    for attemptIdx in range(maxAttempts):
        try:
            return fn()
        except Exception:
            if attemptIdx == maxAttempts - 1:
                raise
```
딱 통과할 만큼만.

나쁜 예:
```python
def retryOperation(fn, options=None):
    options = options or {}
    maxRetries = options.get("maxRetries", 3)
    backoff = options.get("backoff", "linear")
    onRetry = options.get("onRetry")
    # YAGNI: 아직 필요하지 않은 기능 선반영
    ...
```
과잉설계(YAGNI 위반).

테스트가 요구하지 않은 기능 추가, 다른 코드 리팩터링, "개선" 금지.

### Verify GREEN — 통과 확인 (필수)

```bash
python -m unittest discover -s tests -v
```

확인 사항: 대상 테스트 통과 / 기존 테스트 모두 통과 / 출력에 오류·경고
없음(pristine).

테스트 실패 → 코드를 고친다(테스트를 고치지 않는다).
다른 테스트 깨짐 → 즉시 고친다.

### REFACTOR — 정리

Green 상태에서만 진행: 중복 제거, 이름 개선, 헬퍼 추출. 테스트는 계속
통과 상태 유지, 동작(behavior) 추가 금지.

### 반복

다음 동작에 대해 RED부터 반복.

## 3. 좋은 테스트 vs 나쁜 테스트

| 품질 | 좋음 | 나쁨 |
|---|---|---|
| 최소성 | 한 가지만 검증. 이름에 "그리고"가 들어가면 분리 | `test_validates_email_and_domain_and_whitespace` |
| 명확성 | 이름이 동작을 설명 | `test1` |
| 의도 표현 | 원하는 API를 그대로 보여줌 | 코드가 뭘 해야 하는지 감춤 |

테스트 작성/변경 시 "4. 진짜 테스트 작성 규칙"(원문 writing-good-tests.md
각색분, `writing-good-tests.md` 참고)을 함께 확인한다:
- 테스트가 실패하게 만드는 실제 변경을 먼저 특정
- 모의객체가 아니라 실제 동작을 검증
- 테스트 전용 코드는 테스트 유틸리티에만, 운영 코드에 넣지 않음
- 모킹 전 의존성의 부작용을 먼저 파악

## 4. 흔한 합리화와 반박

| 핑계 | 실제 |
|---|---|
| "너무 단순해서 테스트 불필요" | 단순한 코드도 깨진다. 테스트는 30초면 작성. |
| "나중에 테스트 작성" | 나중에 쓴 테스트는 바로 통과 — 아무것도 증명 못함. 놓친 엣지케이스도 놓친 채로 남음. |
| "정신은 같으니 순서는 상관없다" | 선행 테스트는 "무엇을 해야 하나"를, 후행 테스트는 "지금 뭘 하나"를 답함. 이미 짠 코드에 편향됨. |
| "수동으로 이미 확인함" | 수동 테스트는 기록도, 재실행도 안 됨. 압박 상황에서 케이스 누락. |
| "지운 시간이 아깝다" | 매몰비용 오류 — 이미 쓴 시간은 어차피 못 돌림. 신뢰 못 할 코드를 유지하는 게 더 큰 낭비. |
| "일단 참고용으로 남기고 테스트부터" | 결국 그 코드를 손봐서 쓰게 됨 — 그건 후행 테스트. |
| "탐색이 먼저 필요" | 좋다. 탐색 코드는 버리고 TDD로 다시 시작. |
| "테스트 짜기 어려움 = 설계가 나쁨" | 테스트가 어렵다는 신호를 들어라. 쓰기 어려운 API는 쓰기도 어렵다. |
| "TDD가 느리게 함" | TDD가 실용적 경로다 — 커밋 전 버그 차단, 회귀 방지, 안심하고 리팩터링. |
| "기존 코드에 테스트가 없었음" | 지금 손대는 김에 테스트를 추가한다. |

## 5. 경고 신호 — 발견 시 중단하고 재시작

코드 먼저 작성 / 구현 후 테스트 작성 / 테스트가 바로 통과 / 왜
실패했는지 설명 못함 / "나중에" 테스트 추가 / "이번만" 합리화 / "이미
수동 테스트함" / "정신은 같다" / "참고용으로 남긴다" / "이미 X시간
썼으니 아깝다" / "TDD는 교조적, 나는 실용적" / "이건 경우가 다르다".

**모두 동일한 조치: 코드 삭제, TDD로 재시작.**

## 6. 테스트 문서화 규칙 (이 프로젝트 고유 요구사항)

모든 테스트 함수는 `coding-standards` 스킬의 Doxygen 규칙을 테스트
코드에도 동일 적용한다. 테스트 함수 docstring에 최소 다음을 기재한다.

```python
def test_<대상>_<조건>_<기대결과>(self):
    """
    @brief <이 테스트가 검증하는 목적을 한 줄로>
    @technique <사용한 테스트 기법>
    @case <positive | negative>
    """
```

- **@technique**: 동등분할(Equivalence Partitioning), 경계값분석
  (Boundary Value Analysis), 결정테이블(Decision Table), 상태전이
  테스트(State Transition), 오류추정(Error Guessing) 등 실제 사용한
  기법을 명시한다. 기법을 특정할 수 없으면 "단순 시나리오 검증"으로
  표기하되 임의로 그럴듯한 기법명을 지어내지 않는다.
- **@case**: 정상 동작을 검증하면 `positive`, 오류/예외/거부/경계 위반
  등 비정상 입력·상태를 검증하면 `negative`. 하나의 테스트가 둘 다
  검증하면 안 되므로(최소성 원칙) 분리한다.
- 사전조건 위반, 예외 발생 등 상세설계 함수 계약(`detailed-designer`
  산출물의 `@pre`/`@throws`)을 검증하는 테스트는 `@case negative`로
  표기하고, 어떤 계약 항목을 검증하는지 `@brief`에 명시한다.

## 7. 뮤테이션 점검 (완료 전 필수)

구현 코드를 마음속으로 다음처럼 바꿔보고, 각 변형마다 실패하는 테스트가
최소 하나는 있는지 확인한다: 상수/인자 오류, 분기 오류, 상태변경/부작용
누락, 빈 값·기본값 반환, 0/빈값/None/미인가/잘못된 형식 입력에 대한
검증 누락. 아무 테스트도 못 잡는 변형이 있으면 그 동작은 무방비 상태
이거나 테스트가 동어반복(tautology)이라는 뜻.

## 8. 완료 전 체크리스트

- [ ] 모든 신규 함수/메서드에 테스트 존재
- [ ] 각 테스트를 구현 전에 실패시켜 확인
- [ ] 각 테스트가 예상된 이유로 실패(오타 아님, 기능 부재)
- [ ] 각 테스트를 통과시키는 최소 구현 작성
- [ ] 전체 테스트 통과
- [ ] 출력 pristine(오류·경고 없음)
- [ ] 실제 코드로 테스트(모의객체는 불가피할 때만)
- [ ] 엣지 케이스·오류 경로 포함
- [ ] 모든 테스트 함수에 `@brief`/`@technique`/`@case` 기재
- [ ] `coding-standards`의 품질 지표(라인수/순환복잡도/중복/네이밍)를
      테스트 코드에도 적용

체크 못하는 항목이 있으면 TDD를 건너뛴 것 — 처음부터 다시.

## 9. 막혔을 때

| 문제 | 해결 |
|---|---|
| 테스트 방법을 모르겠다 | 원하는 API를 먼저 쓰고, 원하는 검증(assert)부터 작성. 막히면 사용자에게 확인. |
| 테스트가 너무 복잡함 | 설계가 복잡한 것 — 인터페이스를 단순화. |
| 모든 걸 모킹해야 함 | 코드 결합도가 너무 높음 — 의존성 주입 사용. |
| 테스트 준비 코드가 거대함 | 헬퍼로 추출. 그래도 복잡하면 설계를 단순화. |

## 10. 디버깅 연계

버그 발견 시: 그 버그를 재현하는 실패 테스트부터 작성 → TDD 사이클
그대로 수행. 테스트 없이 버그를 고치지 않는다.

## 11. 이 프로젝트 적용 사항 (기존 NGV tdd 스킬 반영)

- 프레임워크: Python `unittest`(CLAUDE.md 지정). 테스트 파일명
  `test_*.py`, `unittest.TestCase` 상속.
- 커버리지 매핑: 상세설계(`detailed-designer`) 함수 계약(사전조건,
  사후조건, 예외, 경계값)을 빠짐없이 테스트 케이스로 변환, 각 케이스의
  `@case`(positive/negative)로 구분.
- 요구사항 수용기준(`requirements-analyst` 산출물)과 테스트 결과 대응
  확인.
- Verify RED/GREEN 명령은 `python -m unittest` 계열 사용, 실제 실행
  결과를 근거로 보고(임의로 "통과했다" 단정 금지 — `coding-standards`의
  실측 원칙과 동일).

## 12. 최종 규칙

```
운영 코드 → 그 코드를 위해 존재했고 먼저 실패했던 테스트가 있어야 함
아니면 → TDD 아님
```

예외는 사용자 승인 없이는 없다.
