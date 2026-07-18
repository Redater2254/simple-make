# simple-make

만들고 싶은 게 있는데 어디서 시작해야 할지 모를 때 쓰는 Claude Code 스킬입니다.

요청을 받으면 이미 정해진 것을 먼저 읽어내고, 결정에 실제로 영향을 주는 것만 2~4개 물은 뒤,
후보 기술 스택을 장단점과 함께 제시합니다. 선택이 끝나면 그대로 만들 수 있는 수준의
설계 문서(`DESIGN.md`)를 쓰고, 확인을 받아 구현까지 진행합니다.

A Claude Code skill for when you know what you want to build but not how to start.
It reads what your request already settles, asks only the questions that change the answer,
puts 2–3 real pipelines on the table with honest tradeoffs, then writes a design document
concrete enough to build from — and builds it.

## 설치 / Install

### Claude Code

```bash
claude plugin marketplace add Redater2254/simple-make
claude plugin install simple-make
```

세션 안에서 설치하려면:

```
/plugin marketplace add Redater2254/simple-make
/plugin
```

업데이트는 `claude plugin update simple-make`.

### Codex

```bash
codex plugin marketplace add Redater2254/simple-make
codex plugin install simple-make
```

세션 안에서는 `/plugins`를 실행하면 플러그인 브라우저가 열립니다. 거기서
`simple-make`를 찾아 설치하고, 설치된 항목 위에서 Space로 켜고 끌 수 있습니다.

> **검증 상태 안내.** Claude Code 설치는 실제로 등록·설치·로드까지 확인했습니다.
> Codex 쪽은 공식 문서의 플러그인 규격(`.codex-plugin/plugin.json` + `skills/`,
> 마켓플레이스 매니페스트는 `.agents/plugins/marketplace.json`)에 맞춰 구성했지만,
> 제작 환경에 Codex가 없어 실제 로드까지는 확인하지 못했습니다.
> 동작하지 않으면 이슈로 알려주시면 고치겠습니다.
>
> **Verification status.** The Claude Code path is tested end to end — added,
> installed, and loaded. The Codex manifests follow the documented plugin format
> but have not been loaded in a real Codex install; please open an issue if it
> fails.

### 파일 하나로 쓰기 / Single file, no plugin

플러그인 설치가 번거롭거나 다른 도구에서 쓰고 싶다면
[`standalone/SKILL.md`](standalone/SKILL.md) 하나만 받아서 스킬 폴더에 넣으면 됩니다.

```bash
mkdir -p ~/.claude/skills/simple-make
curl -o ~/.claude/skills/simple-make/SKILL.md \
  https://raw.githubusercontent.com/Redater2254/simple-make/main/standalone/SKILL.md
```

전체판과의 차이는 **파이프라인 카탈로그(`references/pipelines.md`)가 빠진 것** 하나입니다.
카탈로그는 후보 스택을 고를 때 참고하는 목록인데, 모델이 이미 아는 내용이고
전체판에서도 "출발점이지 제약이 아니다"라고 명시하고 있습니다. 대신 고르는 *관점*을
단일 파일 안에 압축해 넣었으므로, 판단의 질은 거의 그대로입니다.

오히려 컨텍스트는 더 가볍습니다 — 문서 템플릿을 안에 담고도 11KB로, 전체판의
`SKILL.md`(12.3KB)보다 작습니다. 전체판은 필요할 때 카탈로그 13KB를 추가로 읽습니다.

The only thing the standalone edition drops is the pipeline catalog — a reference
list the model largely knows anyway, and which the full edition itself calls "a
starting point, not a constraint". The *method* for choosing is compressed into
the single file, so the quality of the judgment is largely unchanged. It is in
fact lighter in context than the full edition's `SKILL.md` alone.

### 그 외 도구 / Other harnesses

`skills/simple-make/`는 프론트매터가 붙은 마크다운일 뿐이라 특정 도구에 묶여 있지
않습니다. 스킬을 읽는 도구라면 폴더를 해당 도구의 스킬 경로에 복사하는 것만으로도
동작합니다.

The skill body is just Markdown with frontmatter — nothing in it is tied to a
particular harness. Copying `skills/simple-make/` into any tool's skills
directory should work.

## 사용 / Usage

```
/simple-make 폴더에 새 스크린샷 생기면 자동으로 정리해주는 프로그램 만들고 싶어
/simple-make i want a shared flashcard app for my study group
```

명시적으로 `/simple-make`를 치지 않아도, 새 프로그램을 만들자는 요청이나
"Electron이랑 Tauri 중에 뭐가 나아?" 같은 스택 비교 질문에서도 동작합니다.

## 특징 / What it does differently

- **추천에 의견이 있습니다.** 후보만 나열하고 고르라고 하지 않고 1순위를 밝힙니다.
  판단을 빌리러 온 사람에게 중립을 지키는 건 일을 안 하는 것입니다.
- **장단점이 교과서가 아니라 이 프로젝트 기준입니다.** "Electron은 Chromium을 통째로 넣는다"는
  검색하면 나오는 사실이고, "그래서 8MB 도구가 120MB 설치 파일이 되는데, 사내에서 공유 드라이브로
  받는 팀에게는 사실상 무의미하다"가 판단입니다.
- **나중에 물리는 지점을 같이 씁니다.** 6주 뒤에 무엇이 발목을 잡는지가 실제로 필요한 정보입니다.
- **실제 환경을 확인합니다.** 가짜 데이터로 로직만 검증하면 내가 세운 가정을 다시 확인하는 것에
  불과합니다. 경로가 실제로 어디로 풀리는지, 런타임 버전이 무엇인지를 실제 시스템에서 봅니다.
- **범위를 먼저 닫습니다.** "v1에서 하지 않을 것" 목록이 프로젝트를 끝낼 수 있게 만듭니다.

## 산출물 / Output

프로젝트 루트에 `DESIGN.md`가 생깁니다 (`docs/` 폴더가 이미 있으면 그 안에).
사용자가 쓴 언어를 따라갑니다 — 한국어로 물으면 한국어 문서, 영어면 영어 문서.

문서 구성: 확정 사항(선택과 이유) / 범위(포함·제외) / 구조 / 핵심 데이터 /
구현 순서(단계마다 실행 가능한 마일스톤) / 리스크와 환경 가정.

## 라이선스 / License

MIT
