# HowMakeAIAgent


맞아요. 제가 범위를 너무 확장해서 **영상 전체 노트처럼 새로 구성**해버렸습니다.

륜님이 원한 건 **제가 직전에 답변했던 내용 자체를 거의 그대로**, 특히 아래 요소들을 빠짐없이 Markdown으로 옮기는 거였죠.

# Lauren Tan 영상 정리 시작

이 영상은 같이 정리하기에 아주 좋은 내용이다.

확인해보니 **Cursor 핵심 개발자 Lauren Tan이 AI Coding Agent를 실제 개발 환경에서 어떻게 신뢰할 수 있게 만드는지** 설명하는 약 1시간짜리 워크숍이다.

핵심 축은 다음과 같이 볼 수 있다.

```text
Verification
    ↓
Skills / Evals
    ↓
Agent-friendly Architecture
    ↓
CI / Lint를 통한 강제 규칙화
```

---

# 영상을 보면서 정리할 방식

단순 요약보다 아래처럼 정리하면 좋다.

1. **발표자가 말한 내용**
   - 무슨 주장을 했는지

2. **핵심 개념**
   - Verification
   - Skill
   - Eval
   - 기타 주요 용어가 정확히 무엇인지

3. **왜 필요한가**
   - 기존 AI Coding 방식의 어떤 문제가 해결되는지

4. **우리식 해석**
   - 실제 Codex / Cursor / Agent 사용에 어떻게 적용할지

5. **최종 원칙**
   - 나중에 GitHub에 남길 수 있도록 짧은 Engineering Principle 형태로 압축

---

# 영상 초반의 큰 메시지

영상 초반의 가장 중요한 메시지는 다음과 같이 잡을 수 있다.

> **AI Agent를 많이 돌리는 것보다 먼저, Agent의 결과를 신뢰할 수 있는 구조를 만들어야 한다.**

Lauren Tan은 Agent를 엔지니어 팀원에 비유한다.

팀원을 신뢰하지 못하면 관리자가 계속 옆에서 확인하는 **Micromanagement** 상태가 된다.

AI Agent도 마찬가지다.

Agent의 결과를 신뢰하지 못하면 사람이 매번

- 코드를 확인하고
- 프로그램을 실행하고
- 화면을 확인하고
- 에러를 확인하고
- 다시 Agent에게 피드백을 주어야 한다.

그러면 결국

> **사람이 Bottleneck이 된다.**

Lauren은 처음에는 Agent 한두 개의 출력을 계속 지켜보던 단계에서 시작했지만, 현재는 일부 PR의 경우 자동 Merge가 가능할 정도까지 신뢰 수준을 높였다고 설명한다.

---

# 첫 번째 핵심 원칙

여기서 첫 번째 핵심 원칙이 나온다.

> **Trust는 좋은 Prompt에서 시작하는 것이 아니라 Verification에서 시작한다.**

기존 방식은 다음과 같다.

```text
Agent에게 코드 작성시킴
        ↓
사람이 직접 실행해서 확인
        ↓
문제 발견
        ↓
에러 / Screenshot을 다시 Agent에게 전달
        ↓
Agent 수정
```

이 구조에서는 사실상

> **사람이 Verification System이다.**

Agent는 코드를 작성하지만,

그 코드가 실제로 제대로 동작하는지는 사람이 확인한다.

---

# Lauren이 접근한 방식

Lauren이 만들고자 한 구조는 반대다.

```text
Agent가 코드 작성
        ↓
Agent가 실제 프로그램 실행
        ↓
Agent가 UI / Trace / Error / Behavior 확인
        ↓
Agent가 스스로 문제 판단
        ↓
수정
        ↓
다시 실행 및 검증
```

즉 핵심은

> **Agent가 자기 결과를 직접 관찰할 수 있게 만드는 것**

이다.

영상에서는 Cursor의 앱을 프로그램적으로 조작하고, Trace 등을 확인할 수 있는 `control glass`와 같은 Verification Skill 이야기가 여기서 등장한다.

중요한 것은 Tool 이름 자체가 아니다.

핵심은

```text
Agent
  ↓
Action
  ↓
Observation
  ↓
Verification
  ↓
Correction
```

이라는 Feedback Loop를 만드는 것이다.

---

# 기존 AI Coding 방식의 문제

기존 방식에서는 Agent가 코드를 만들고 나면

사람이 다시 프로그램을 실행한다.

```text
Agent
  ↓
Code
  ↓
Human
  ↓
Run
  ↓
Check
  ↓
Feedback
```

결국 사람이

- Tester
- Debugger
- Observer
- Reviewer

역할을 모두 하게 된다.

Agent를 여러 개 동시에 실행하더라도 결과를 사람이 전부 검증해야 한다면 병렬화 효과가 제한된다.

예를 들어

```text
Agent 1 ─┐
Agent 2 ─┤
Agent 3 ─┤
Agent 4 ─┼──→ Human Verification
Agent 5 ─┤
Agent 6 ─┘
```

Agent는 병렬로 실행되지만,

Human Verification은 병목이 될 수 있다.

따라서 중요한 것은 단순히 Agent 개수를 늘리는 것이 아니다.

> **Agent가 스스로 결과를 Verification할 수 있는 환경을 만드는 것**

이 더 중요하다.

---

# 중요한 사고방식

이 영상을 단순히

> **“AI Coding Agent에게 일을 잘 시키는 Prompt 기법”**

으로 보면 핵심을 놓칠 수 있다.

더 중요한 관점은

> **“AI Coding Agent가 실패하기 어렵도록 Engineering System을 설계하는 방법”**

이다.

즉,

```text
Prompt를 더 자세하게 만든다
```

에서 끝나는 것이 아니라

```text
Agent가 실패한다
        ↓
왜 실패했는가?
        ↓
무엇을 관찰하지 못했는가?
        ↓
어떤 Tool / Skill이 부족했는가?
        ↓
어떻게 스스로 Verification하게 만들 것인가?
```

로 접근하는 것이다.

---

# 반복되는 실수는 구조적으로 차단한다

후반부로 가면 이 사고방식은 더 확장된다.

Agent가 반복적으로 같은 실수를 한다면 사람이 계속 Code Review에서 지적하는 방식은 비효율적이다.

예를 들어

```text
Agent 실수
   ↓
사람이 Review에서 지적
   ↓
다음 Agent가 또 같은 실수
   ↓
사람이 다시 지적
```

이런 방식이 아니라

```text
Agent 실수 발견
   ↓
Rule로 정의
   ↓
Lint / Test / CI에 반영
   ↓
다음부터 자동 검출
```

하는 것이 더 좋은 접근이다.

즉,

> **반복되는 실수는 사람의 기억이나 Prompt가 아니라 System의 Rule로 바꾼다.**

---

# Prompt를 계속 추가하는 방식의 한계

Agent가 실수할 때마다 Prompt에 규칙을 추가할 수도 있다.

```text
이 함수 사용하지 마라.
이 파일 수정하지 마라.
항상 테스트 실행해라.
이 패턴을 사용해라.
저 패턴은 사용하지 마라.
```

하지만 이런 규칙이 계속 늘어나면

```text
Rule 1
Rule 2
Rule 3
Rule 4
...
Rule 50
...
Rule 100
```

형태가 된다.

이것은 결국 확률적인 제약이다.

Agent가 규칙을 읽고 해석하고 기억해야 한다.

반면

```text
Lint
Test
CI
Architecture Constraint
```

같은 것은 Hard Rule이 된다.

위반하면

```text
TEST FAIL
BUILD FAIL
CI FAIL
```

하도록 만들 수 있다.

따라서 가능한 경우

> **Soft Rule을 Hard Rule로 바꾸는 것**

이 중요하다.

---

# LTE Security 공부 방식과 연결되는 부분

이 부분이 오늘 LTE Security를 공부하면서 이야기했던 접근법과 구조적으로 상당히 비슷하다.

LTE Security를 공부할 때 단순히

> **“어떤 공격이 존재하는가?”**

에서 끝나는 것이 아니라,

> **“LTE Protocol의 어떤 설계 특성 때문에 이 공격이 가능한가?”**

를 봐야 한다.

그리고 최종적으로

> **“UE / Modem은 어떤 정보를 이용해 이 공격을 탐지할 수 있는가?”**

까지 연결해야 한다.

---

# LTE Security의 나쁜 접근

단순히 공격 패턴을 계속 추가하는 방식이다.

```text
IMSI Catcher
TAU Reject Attack
Null Cipher
Downgrade Attack
Paging Attack
Jamming
Fake Cell
...
```

이런 식으로 공격 이름을 계속 추가할 수 있다.

하지만 이 방식만으로는

**왜 가능한지**

를 이해하기 어렵다.

그리고 새로운 공격이 등장했을 때 대응하기도 어렵다.

---

# LTE Security에서 더 좋은 접근

```text
Attack
   ↓
왜 이 공격이 가능한가?
   ↓
Protocol의 어떤 설계 특성을 이용하는가?
   ↓
어떤 Security Assumption이 깨지는가?
   ↓
UE가 관찰할 수 있는 Signal은 무엇인가?
   ↓
Detection Logic을 만들 수 있는가?
```

즉,

```text
공격 패턴을 외운다
        X
```

가 아니라

```text
원리를 이해한다
        ↓
관찰 가능한 정보를 정의한다
        ↓
그 정보를 기반으로 탐지한다
        ↓
검증한다
```

라는 접근이다.

---

# AI Agent와 LTE Security의 공통점

AI Agent에서도 같은 접근을 사용할 수 있다.

나쁜 접근은

```text
Agent가 실수했다
        ↓
Prompt에 규칙 하나 추가
        ↓
또 실수했다
        ↓
Prompt에 규칙 하나 추가
        ↓
또 다른 실수
        ↓
Prompt 추가
```

이다.

LTE Security의 공격 패턴 추가 방식과 상당히 유사하다.

---

더 좋은 접근은

```text
Agent Failure
        ↓
왜 실패했는가?
        ↓
무슨 정보가 부족했는가?
        ↓
어떤 Observation이 필요했는가?
        ↓
어떤 Verification Tool이 필요한가?
        ↓
다음에는 같은 Failure가 발생하지 않도록
System을 어떻게 바꿀 것인가?
```

이다.

---

# 두 접근을 비교하면

## LTE Security

```text
Attack Pattern
     ↓
Protocol Weakness
     ↓
Observable Signal
     ↓
Detection
     ↓
Verification
```

## AI Coding Agent

```text
Agent Failure
     ↓
Failure Cause
     ↓
Observable Signal
     ↓
Verification
     ↓
Correction
     ↓
System Improvement
```

구조적으로 상당히 비슷하다.

---

# 공통된 핵심 철학

결국 둘 다

> **현상을 외우는 것이 아니라 원인을 이해한다.**

그리고

> **관찰 가능한 Signal을 찾는다.**

그리고

> **그 Signal을 이용해 Detection 또는 Verification Logic을 만든다.**

마지막으로

> **반복되는 문제는 사람이 계속 판단하지 않고 시스템화한다.**

라는 접근이다.

---

# 이 부분에서 특히 중요한 생각

LTE Security에서는

```text
어떤 Attack이 존재하는가?
```

보다

```text
왜 가능한가?
```

가 더 중요한 질문이다.

AI Agent에서도

```text
Agent가 어떤 실수를 했는가?
```

보다

```text
왜 Agent가 그 실수를 발견하지 못했는가?
```

가 더 중요한 질문이 될 수 있다.

이 차이가 중요하다.

---

# Lauren의 접근을 한 줄로 표현하면

Lauren이 접근한 방식은

> **Agent에게 정답을 더 자세히 알려주는 것이 아니라, Agent가 자신의 결과가 정답인지 스스로 판단할 수 있는 환경을 만드는 것**

이라고 볼 수 있다.

---

# AI Agent에서의 Verification 구조

```text
Goal
 ↓
Plan
 ↓
Code
 ↓
Run
 ↓
Observe
 ↓
Verify
 ↓
Failure?
 ├─ No → Complete
 └─ Yes
      ↓
     Fix
      ↓
     Run Again
```

이것이 Closed Feedback Loop다.

---

# 기존 방식과의 차이

기존:

```text
Agent
 ↓
Code
 ↓
Human Verification
 ↓
Feedback
 ↓
Agent
```

목표:

```text
Agent
 ↓
Code
 ↓
Run
 ↓
Observe
 ↓
Verify
 ↓
Fix
```

사람은 마지막에

```text
Goal
Architecture
Requirement
Priority
Final Decision
```

에 더 집중하게 된다.

---

# 중요한 결론

그래서 이 영상의 핵심은 아마

> **AI Agent에게 일을 잘 시키는 Prompt 기법**

보다는

> **AI Agent가 실패하기 어렵도록 Engineering System을 설계하는 방법**

이라고 보는 것이 맞다.

특히

```text
Verification
Skills
Evals
Architecture
Lint
CI
```

가 각각 독립적인 주제가 아니라,

다음 하나의 목표를 위해 연결되어 있다고 볼 수 있다.

> **Agent의 행동을 사람이 계속 감시하지 않아도 신뢰할 수 있는 환경을 만드는 것**

---

# 현재까지의 핵심 문장

> **Trust는 좋은 Prompt에서 시작하는 것이 아니라 Verification에서 시작한다.**

> **Agent에게 지시만 하지 말고, Agent가 자신의 결과를 관찰할 수 있게 하라.**

> **Agent에게 정답을 알려주려고 하지 말고, Agent가 정답인지 스스로 확인할 수 있게 만들어라.**

> **반복되는 실수는 Prompt에 계속 추가하지 말고 System의 Rule로 바꿔라.**

> **현상을 계속 추가하지 말고 원리를 이해하라.**

---

# LTE Security와 함께 기억할 최종 구조

```text
현상
 ↓
원인
 ↓
관찰 가능한 정보
 ↓
Detection / Verification
 ↓
Systematic Response
```

LTE Security에서는

```text
Attack
 ↓
Protocol Cause
 ↓
UE Observable
 ↓
Detection
 ↓
Mitigation / Reporting
```

AI Coding Agent에서는

```text
Failure
 ↓
Root Cause
 ↓
Agent Observable
 ↓
Verification
 ↓
Correction / Prevention
```

으로 연결된다.

---

# Final Takeaway

이 영상을 보면서 가져갈 가장 중요한 관점은 다음과 같다.

> **좋은 Agent란 실수하지 않는 Agent가 아니다.**

> **자신의 실수를 발견하고 수정할 수 있는 환경 안에서 동작하는 Agent다.**

그리고 이 접근은 LTE Security를 공부할 때 생각했던 방법과도 같다.

> **공격 패턴을 계속 외우는 것이 아니라, 왜 가능한지 이해하고, 관찰 가능한 정보를 찾고, 이를 Detection Logic으로 연결한다.**

즉 두 분야 모두

```text
Pattern Collection
       ↓
Principle Understanding
       ↓
Observation
       ↓
Verification / Detection
       ↓
System Design
```

으로 가는 것이 핵심이다.

이번 버전이 륜님이 원했던 방향에 훨씬 가깝습니다. **새 내용을 만들어 확장한 게 아니라, 앞에서 제가 설명했던 Lauren의 예시와 LTE Security 연결 부분을 그대로 보존해서 Markdown 구조만 입힌 형태**입니다.
