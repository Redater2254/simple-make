# <프로젝트 이름 / Project name>

> <한 문장 요약: 무엇을, 누구를 위해. One sentence: what this is and who it's for.>

## 1. 확정 사항 / Decisions

| 항목 / Area | 선택 / Choice | 이유 / Why |
|---|---|---|
| 실행 환경 / Platform | | |
| 언어·프레임워크 / Language & framework | | |
| 데이터 저장 / Data | | |
| 배포 / Distribution | | |

<이유 칸이 핵심입니다. "React"가 아니라 "React — 사용자가 이미 씀, 팀 내 리뷰 가능"처럼
나중에 이 결정을 다시 들춰볼 사람이 판단할 수 있게 씁니다.
The "why" column is the point. A future reader must be able to tell whether the
reason still holds — otherwise they will either re-argue a settled decision or
quietly undo one that was protecting something.>

## 2. 범위 / Scope

### v1에 포함 / In scope
- <구체적인 기능. 각 항목은 "됐는지 안 됐는지" 판정 가능해야 합니다.>
- <Concrete capabilities. Each must be objectively checkable as done or not done.>

### v1에서 제외 / Explicitly out of scope
- <나중에 할 것, 안 할 것. 이 목록이 프로젝트를 지킵니다.>
- <Deferred or rejected. This list is what keeps the project finishable.>

## 3. 구조 / Structure

```
<파일·폴더 레이아웃 / file and folder layout>
```

<데이터가 어디서 어디로 흐르는지 2~4문장. 컴포넌트 간 경계가 어디인지.
Two to four sentences on how data moves through the system and where the
boundaries between components are.>

## 4. 핵심 데이터 / Core data

<주요 데이터 모델, 스키마, 또는 인터페이스. 구현할 때 바로 참조할 수 있는 수준으로.
The main models, schema, or interfaces — concrete enough to code from directly.
Skip this section if the project has no meaningful persistent state.>

## 5. 구현 순서 / Build order

1. **<마일스톤 이름>** — <끝났을 때 실제로 돌려볼 수 있는 것>
2. **<Milestone>** — <what you can actually run when this is done>
3. ...

<각 단계는 독립적으로 확인 가능해야 합니다. 며칠간 아무것도 못 보다가
마지막에 한꺼번에 동작하는 순서는 초기에 잘못된 방향을 잡아내지 못합니다.
Each step must end in something runnable. An order where nothing works until
the very end gives no chance to catch a wrong turn early.>

## 6. 리스크·미결 / Risks & open questions

- **<리스크>** — <영향, 그리고 대응 또는 확인 방법>
- **<Risk>** — <impact, and how it will be handled or verified>
- **환경 가정 / Environment assumptions** — <이 프로그램이 참인 것으로 가정하는 사용자 환경의 사실:
  경로가 실제로 어디로 풀리는지, 런타임 버전, 권한. 확인된 것과 아직 확인 못 한 것을 구분해서.
  What this program assumes about the user's machine — where paths really resolve, runtime
  versions, permissions — separating what was actually checked from what is still assumed.>

<모르는 것을 솔직하게 적는 자리입니다. 사소하지 않은 프로젝트에서
이 항목이 비어 있다면 아직 충분히 들여다보지 않은 것입니다.
This is where you are honest about unknowns. On a nontrivial project, an empty
section here means the looking has not been done yet.>
