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

```bash
claude plugin marketplace add <your-github-username>/simple-make-skill
claude plugin install simple-make
```

Claude Code 세션 안에서 설치하려면:

```
/plugin marketplace add <your-github-username>/simple-make-skill
/plugin
```

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
