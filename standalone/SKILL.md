---
name: simple-make
description: Turn a rough "I want to build X" request into a chosen tech pipeline plus a concrete markdown design doc, then implement it. Use this whenever the user asks to build, create, make, or start any new program, app, tool, site, service, or script — especially when invoked as /simple-make — and even if they never name a stack or say the word "plan". Also use when the user is weighing frameworks against each other (Electron vs Tauri, React Native vs Flutter, FastAPI vs Express, Next.js vs plain React), asks "what should I use for this?", wants pros and cons before committing, or has an idea but no idea where to start. Covers stack recommendation, design document authoring, and the build that follows.
---

# simple-make (standalone)

*Single-file edition. The full version bundles a pipeline catalog and a document
template as separate files; this one carries the method alone, which is the part
that matters. https://github.com/Redater2254/simple-make*

A user arrives with an idea and no clear path: *"I want a tool that watches a folder and uploads new screenshots somewhere."* They know the outcome, not the route. The expensive mistake is not writing bad code — it is committing to the wrong shape of project and discovering it three weeks later, when switching costs are real.

Understand the request, pick the genuinely load-bearing unknowns, put two or three real routes on the table with an honest recommendation, then write a design document concrete enough to build from — and build it.

The document is what survives. It is what the user re-reads next week and what a future session picks up cold, so it must record *decisions and their reasons*, not a transcript of the conversation.

## The flow

1. Read the request and extract what is already decided
2. Ask only the questions that change the answer (2–4, often fewer)
3. Put 2–3 pipelines on the table with a clear recommendation
4. Write the design doc
5. Confirm, then build

Not rigid — if the user says "just use Next.js, I've decided", skip to step 4. Insisting on ceremony after the decision is made wastes their time.

## Step 1 — Read before you ask

Users encode far more than they realize, and asking about something they already told you reads as not listening.

From *"a desktop app for my team to tag video clips, we're all on Windows"* you already have: desktop, Windows-only, multi-user (so shared state matters), video (so file size and codecs matter), internal tool (so polish and app-store rules don't apply). That is most of a stack decision.

Also check the ground you're standing on. A `package.json`, `Cargo.toml`, or existing source means the user is likely extending something, not greenfielding. Recommending a fresh Vite scaffold inside someone's Django project destroys trust in everything else you say.

## Step 2 — Ask only load-bearing questions

The bar: **would a different answer lead to a different recommendation?** If not, decide it yourself and record the assumption in the doc.

Usually worth asking:

- **Distribution** — who runs this and how do they get it? "Just me" vs. "strangers download an installer" changes signing, updates, error reporting, packaging.
- **Persistence and sharing** — one machine, or shared? This is the local-file vs. real-backend fork, the most expensive one to reverse.
- **Existing skill** — a stack they can debug at 2am beats a better one they cannot. This is a legitimate engineering input, not a compromise.
- **The hard constraint** — offline, a specific OS, a deadline, no cloud spend, a system to integrate with. One of these can eliminate whole branches instantly.

Rarely worth asking: preferred language when the domain dictates it, "do you want it maintainable" (everyone says yes), anything answerable by looking at the directory, and detailed feature questions — features belong in the doc's scope section, not a stack interview.

Give each option a real consequence: "Local SQLite file — simplest, but two people can't use it at once" tells the user what they're choosing. "Use SQLite" does not.

## Step 3 — Put the routes on the table

Present **2–3** candidates. One isn't a choice; four or more is a menu that induces paralysis, and by the fourth you're usually padding with something you wouldn't recommend.

Lead with your pick and say it's your pick. The user came partly to borrow judgment — withholding it in the name of neutrality is a failure to do the job. They can still overrule you, and they'll do it better knowing what you would have done.

Per option:

```
### A. <stack, concretely named>  ← 추천 / recommended
<why this fits THIS project, referencing their actual requirements>

- 좋은 점 / Strengths: <what it makes easy, specific to their case>
- 감수할 점 / Costs: <the real price — bundle size, hosting cost, concepts to learn>
- 나중에 물리는 지점 / Where it bites later: <what they'd hit at week 6>
```

That last field is what separates this from a comparison table anyone could search for. Textbook pros and cons are free; what people need is *"this is fine until you want auto-update, and then you're setting up a release server."* Be willing to say something unflattering about your own recommendation — an option with no downside isn't credible, and it makes the whole comparison less trustworthy.

Ground tradeoffs in their project. "Electron ships a whole Chromium" is trivia. "Electron ships a whole Chromium, so your 8MB tool becomes a 120MB download — for an internal team on a shared drive that is genuinely irrelevant" is a decision.

**Choosing candidates.** Prefer boring, proven tools unless the user wants to explore; a decade of Stack Overflow answers is worth real points at midnight. Let these forks do most of the work: shared data or single machine; one platform or several; does it need a real backend at all. Consider including "don't build this — use an existing tool" as an honest third option when it's true, and recommending less is often the highest-value move available. Weigh each candidate on distribution friction, how much the user already knows, and what breaks first at scale.

## Step 4 — Write the design doc

Write to `DESIGN.md` at the project root, or `docs/<slug>-design.md` if the project already has a `docs/` directory.

**Match the user's language** — Korean question, Korean document. Keep technical terms in their original form; "IPC", "마이그레이션" reads better than a forced translation nobody uses. The doc is for the user, not for you.

Structure:

```markdown
# <이름 / Name>
> <한 문장 요약 / one-sentence summary>

## 1. 확정 사항 / Decisions
| 항목 | 선택 | 이유 |          <- the "why" column is the point

## 2. 범위 / Scope
### v1에 포함 / In scope
### v1에서 제외 / Explicitly out of scope

## 3. 구조 / Structure
<file layout + how data moves + where the boundaries are>

## 4. 핵심 데이터 / Core data
<models, schema, interfaces — concrete enough to code from; skip if no state>

## 5. 구현 순서 / Build order
1. **<milestone>** — <what you can actually run when this is done>

## 6. 리스크·미결 / Risks & open questions
- 환경 가정 / Environment assumptions — <what was checked vs. still assumed>
```

Why these sections:

- **Decisions** records what was chosen *and why*, so a future reader doesn't silently re-litigate a settled question or undo one that was protecting something.
- **Scope** with an explicit out-of-scope list is what saves the project. Creep lives in the gap between "we didn't discuss it" and "I assumed it was included". Push back gently on everything-in-v1 — a shipped small thing beats a stalled large one.
- **Build order** must be independently verifiable milestones, each ending in something runnable. "Set up the database, then all the models, then the UI" leaves the user with nothing to look at for days and no way to catch a wrong turn early.
- **Risks** is where you're honest about unknowns. An empty section on a nontrivial project means you weren't looking.

Keep it tight — a build plan, not a proposal. A small tool might need 40 lines; something substantial 150. Padding a simple project into a long document looks thorough while being less useful.

Show the doc and ask for corrections before building. Cheap now, expensive later.

## Step 5 — Build

Work through the build order milestone by milestone, reporting progress against the doc the user already agreed to.

If implementation reveals the plan was wrong, stop and say so rather than quietly routing around it — then update `DESIGN.md` to match what was built. A doc that has drifted from reality is worse than no doc, because it is trusted and wrong.

### Check the real environment, not a stand-in

Testing logic against fixture data proves the code does what you meant. It does not prove it works on the user's machine, because a sandbox is assembled from your assumptions — the very things most likely to be wrong.

Before calling a milestone done, verify the environmental facts the program depends on, on the real system: where its folders actually resolve, which runtime versions are really installed, whether the input files are shaped as you assumed, what permissions allow.

This matters most for the failure mode that produces no error. A tool pointed at a path that doesn't exist finds zero files, does zero work, and exits successfully — the user runs it, nothing happens, and there is nothing to search for. A Desktop redirected into OneDrive, a `python` that resolves to 3.9, a CSV whose real header differs from the sample: each quietly empties the program of effect rather than failing loudly. When such a fact turns out to be load-bearing, resolve it at runtime rather than hardcoding what you observed — the next machine will differ again.

Read-only checks are safe on your own initiative: listing a directory, resolving a path, printing a version. Anything that writes, moves, deletes, or installs on the user's real system needs their go-ahead, which is a strong reason to build a dry-run mode.

State plainly what you verified and what you couldn't. "Milestone 2 needs a live database and has not been run" is useful; describing untested code as working destroys the user's ability to trust any of your other claims.

## Judgment notes

**Match effort to stakes.** A weekend script doesn't need three candidates and a risk register — recommend the obvious thing, write a short doc, build it. Heavy process on a small ask is its own failure.

**Too vague to plan** (*"make me an app"*) — don't guess at a stack. Ask what problem it solves and who uses it; those two answers usually collapse the space enough to proceed.

**Asking for something that won't work** — a realtime multiplayer game in vanilla PHP, iOS background location without the entitlements — say it plainly, explain the blocker, offer the nearest route that does work. Cheerfully planning a doomed project isn't helpfulness.
