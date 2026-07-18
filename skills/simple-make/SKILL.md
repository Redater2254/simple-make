---
name: simple-make
description: Turn a rough "I want to build X" request into a chosen tech pipeline plus a concrete markdown design doc, then implement it. Use this whenever the user asks to build, create, make, or start any new program, app, tool, site, service, or script — especially when invoked as /simple-make — and even if they never name a stack or say the word "plan". Also use when the user is weighing frameworks against each other (Electron vs Tauri, React Native vs Flutter, FastAPI vs Express, Next.js vs plain React), asks "what should I use for this?", wants pros and cons before committing, or has an idea but no idea where to start. Covers stack recommendation, design document authoring, and the build that follows.
---

# simple-make

A user arrives with an idea and no clear path: *"I want to make a tool that watches a folder and uploads new screenshots somewhere."* They know the outcome, not the route. The expensive mistake is not writing bad code — it is committing to the wrong shape of project and discovering it three weeks later, when switching costs are real.

This skill front-loads that decision. Understand the request, pick the genuinely load-bearing unknowns, put two or three real routes on the table with an honest recommendation, then write a design document concrete enough to build straight from — and build it.

The document is the artifact that survives. It is what the user re-reads next week and what a future session picks up cold, so it must record *decisions and their reasons*, not a summary of the conversation.

## The flow

1. Read the request and extract what is already decided
2. Ask only the questions that change the answer (2–4, often fewer)
3. Put 2–3 pipelines on the table with a clear recommendation
4. Write the design doc
5. Confirm, then build

Steps run in order, but not rigidly — if the user says "just use Next.js, I've decided", skip to step 4. Insisting on the ceremony after the user has already made the decision wastes their time and makes the skill feel like a form to fill out.

## Step 1 — Read before you ask

Mine the request first. Users encode far more than they realize, and asking about something they already told you reads as not listening.

From *"a desktop app for my team to tag video clips, we're all on Windows"* you already have: desktop, Windows-only, multi-user (so shared state matters), video handling (so file size and codecs matter), and an internal tool (so polish and app-store constraints do not apply). That is most of a stack decision already.

Also check the ground you are standing on. If the working directory has a `package.json`, `Cargo.toml`, `pyproject.toml`, or existing source, the user is likely extending something rather than greenfielding — read enough to know which. Recommending a fresh Vite scaffold inside someone's existing Django project is the kind of error that destroys trust in everything else you say.

Write down (internally) what you now know and what genuinely remains open. Only the second list can justify a question.

## Step 2 — Ask only load-bearing questions

Use the `AskUserQuestion` tool. Cap at 4, aim for 2. The bar for asking: **would a different answer lead to a different recommendation?** If not, do not ask — decide it yourself and state the assumption in the doc.

Questions that usually earn their place:

- **Distribution** — who runs this and how do they get it? Deciding between "just me, on my machine" and "strangers download an installer" changes almost everything downstream: signing, updates, error reporting, packaging.
- **Persistence and sharing** — does data live on one machine or does it need to be shared? This is the local-file vs. real-backend fork, and it is the most expensive one to reverse.
- **Existing skill** — what does the user already know? A stack they can debug at 2am beats a theoretically better one they cannot. This is a legitimate engineering input, not a compromise.
- **The hard constraint** — offline operation, a specific OS, a deadline, no cloud spend, an existing system to integrate with. One of these can eliminate whole branches instantly.

Questions that rarely earn their place: preferred language when the domain already dictates it, "do you want it to be fast/maintainable" (everyone says yes), anything answerable by looking at the directory, and detailed feature questions — features belong in the doc's scope section, not in a stack interview.

Give each option a real consequence in its description, not a restatement of the label. "Local SQLite file — simplest, but two people can't use it at once" tells the user what they are choosing. "Use SQLite" does not.

## Step 3 — Put the routes on the table

Present **2–3** candidates. One is not a choice; four or more is a menu that induces paralysis, and by the fourth option you are usually padding with something you would not actually recommend.

Lead with your pick and say it is your pick. The user came here partly to borrow judgment — withholding it in the name of neutrality is a failure to do the job. They can still overrule you, and they will do so better when they know what you would have done.

Use this shape per option:

```
### A. <stack, concretely named>  ← 추천 / recommended
<one or two sentences: why this fits THIS project, referencing their actual requirements>

- 좋은 점 / Strengths: <what it makes easy, specific to their case>
- 감수할 점 / Costs: <the real, quantified price — bundle size, hosting cost, extra concepts to learn>
- 나중에 물리는 지점 / Where it bites later: <the thing they'd hit at week 6>
```

That last field is what separates this from a comparison table anyone could search for. Textbook pros and cons are free; what people actually need is *"this is fine until you want auto-update, and then you're setting up a release server."* Be concrete and be willing to say something unflattering about your own recommendation — an option presented with no downside is not credible, and the user will trust the whole comparison less.

Ground the tradeoffs in their project. "Electron ships a whole Chromium" is trivia. "Electron ships a whole Chromium, so your 8MB tagging tool becomes a 120MB download — for an internal team on a shared drive, that is genuinely irrelevant" is a decision.

Consult `references/pipelines.md` for candidate stacks by project type and the tradeoffs that actually bite in each. It is a starting point, not a constraint — if the project needs something not in there, recommend that instead, and prefer boring proven tools over novel ones unless the user signals they want to explore.

Then let the user choose, via `AskUserQuestion` with your recommendation first.

## Step 4 — Write the design doc

Write to `DESIGN.md` at the project root. If the project already has a `docs/` directory, use `docs/<slug>-design.md` instead so you are following the convention already in place.

**Match the user's language.** If they wrote in Korean, write the document in Korean; English, English. Keep technical terms in their original form — "IPC", "Electron", "마이그레이션" reads better than a forced translation nobody uses. This matters because the doc is for the user, not for you.

Use `assets/design-doc-template.md` as the structure. The sections exist for specific reasons:

- **Decisions** records what was chosen *and why*, so a future reader — including a future session — does not silently re-litigate a settled question or, worse, undo it without knowing what it was protecting.
- **Scope** with an explicit out-of-scope list is the section that saves the project. Feature creep happens in the gap between "we didn't discuss it" and "I assumed it was included". Writing down what v1 will *not* do closes that gap. Push back gently if the user wants everything in v1 — a shipped small thing beats a stalled large one.
- **Build order** must be a sequence of independently verifiable milestones, each ending in something runnable. "Set up the database, then write all the models, then the UI" leaves the user with nothing to look at for days and no way to catch a wrong turn early.
- **Risks and open questions** is where you are honest about what you do not know yet. An empty section here on a nontrivial project means you were not looking.

Keep it tight. This is a build plan, not a proposal document — no filler sections, no restating the request three times. A small tool might need 40 lines; something substantial might need 150. Length should track real complexity, and padding a simple project into a long document is a way of looking thorough while being less useful.

Show the user the doc and ask for corrections before building. Cheap to fix now, expensive after implementation.

## Step 5 — Build

Once the doc is approved, implement it, working through the build order milestone by milestone. Report progress against the doc's milestones so the user can follow along against something they already read and agreed to.

If implementation reveals the plan was wrong — a library does not do what its README implied, an approach hits a wall — stop and say so rather than quietly routing around it. Then update `DESIGN.md` to match what was actually built. A design doc that has drifted from reality is worse than no doc, because it is trusted and wrong.

If the user only wanted the plan, they will say so. Take that at face value and stop there.

### Check the real environment, not a stand-in

Testing logic against fixture data proves the code does what you meant. It does not prove the code works on the user's machine, because a sandbox is assembled from your assumptions — the very things most likely to be wrong.

So before calling a milestone done, verify the environmental facts the program actually depends on, on the real system: where the folders it reads and writes actually resolve, which runtime and library versions are really installed, whether the input files exist and are shaped the way you assumed, what the account's permissions allow.

This matters most for the failure mode that produces no error at all. A tool pointed at a path that does not exist finds zero files, does zero work, and exits successfully. The user runs it, nothing happens, and there is no error message to search for — which is far harder to diagnose than a crash. A Windows Desktop or Documents folder redirected into OneDrive or a network share, a `python` that resolves to 3.9 instead of 3.12, a CSV whose real header row differs from the sample: each quietly empties the program of effect rather than failing loudly. When such a fact turns out to be load-bearing, resolve it at runtime rather than hardcoding what you observed, since the next machine will differ again.

Read-only checks are safe to run on your own initiative — listing a directory, resolving a path, printing a version, reading a config. Anything that writes, moves, deletes, or installs on the user's real system needs their go-ahead first, and this is a strong reason to build a dry-run mode: it lets you confirm the program finds the right things before it is trusted to change any of them.

State plainly what you verified and what you could not. "Milestone 2 needs a live database and has not been run" is useful; describing untested code as working is the thing that destroys the user's ability to trust any of your other claims.

## Judgment notes

**Match effort to stakes.** A weekend script does not need three candidate pipelines and a risk register — recommend the obvious thing, write a short doc, build it. Save the full treatment for projects where a wrong turn actually costs something. Applying heavy process to a small ask is its own kind of failure.

**When the request is too vague to plan** — *"make me an app"* — do not guess at a stack. Ask what problem it solves and who uses it. Those two answers usually collapse the space enough to proceed.

**When the user asks for something that will not work** — a real-time multiplayer game in vanilla PHP, a mobile app that needs background location on iOS without the entitlements — say it plainly, explain the blocker, and offer the nearest route that does work. Cheerfully planning a doomed project is not helpfulness.
