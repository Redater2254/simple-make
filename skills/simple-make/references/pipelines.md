# Pipeline catalog

Candidate stacks by project type, with the tradeoffs that actually surface during a build rather than the ones that show up in framework marketing.

Use this to generate options quickly — but it is a starting point, not a menu to read aloud. If a project needs something not listed here, recommend that. Prefer boring, proven tools unless the user signals they want to explore; a stack with a decade of Stack Overflow answers behind it is worth real points when someone is debugging alone at midnight.

**Contents**
- [Desktop applications](#desktop-applications)
- [Mobile applications](#mobile-applications)
- [Web frontend](#web-frontend)
- [Backend and APIs](#backend-and-apis)
- [CLI tools](#cli-tools)
- [Browser extensions](#browser-extensions)
- [Automation and scripts](#automation-and-scripts)
- [Data, analysis, and ML](#data-analysis-and-ml)
- [Games](#games)
- [Cross-cutting decisions](#cross-cutting-decisions)

---

## Desktop applications

**Electron + web frontend** — the default when the app is UI-heavy, needs deep OS integration (tray, global shortcuts, file watching, native menus), or the user already knows web tech. Mature ecosystem; almost any problem has been solved publicly. *Bites later:* ~100–150MB installers and ~150–250MB baseline memory; auto-update needs a release server (electron-updater plus somewhere to host); code signing costs money and time on both Windows and macOS, and unsigned apps trigger scary OS warnings that make an internal tool look broken.

**Tauri** — same web-frontend model but a Rust core and the OS webview instead of bundled Chromium. Installers land in single-digit MB. *Bites later:* the webview differs per platform, so a layout correct on Windows can break on macOS; anything the JS side cannot do requires writing Rust; the ecosystem is thinner, so unusual needs mean building rather than importing. A poor fit if the user has no appetite for Rust.

**Python + Qt (PySide6) or Tkinter** — strong when the app is a GUI wrapper around Python work already being done: data processing, scripting, scientific libraries. Tkinter ships with Python and is ugly but zero-friction; Qt looks professional and is far more capable. *Bites later:* packaging to a distributable binary (PyInstaller) is the reliable pain point, especially with native dependencies; Qt's licensing needs a check for commercial closed-source use.

**Native (SwiftUI / WinUI / WPF)** — right when the app must feel native, hook deep into one OS, or ship on an app store, and only one platform matters. Best performance and platform fidelity. *Bites later:* the second platform means a second codebase, not a port.

**Rule of thumb:** internal tool, one platform, team already knows web → Electron. Public download where size and polish matter → Tauri or native. Wrapping existing Python → Qt.

---

## Mobile applications

**React Native (Expo)** — the default for cross-platform when the team knows JS/React. Expo removes most native build pain and enables over-the-air updates. *Bites later:* any native module outside the Expo ecosystem forces ejection to bare workflow, which is a meaningfully harder project; heavy animation and list performance need deliberate work; iOS builds still need a Mac or a paid cloud build service.

**Flutter** — one codebase, consistently high performance, excellent animation. Widgets are drawn rather than mapped to native controls, so the app looks identical everywhere. *Bites later:* Dart is a new language for most teams; identical-everywhere means it can feel subtly foreign on both platforms; larger baseline app size.

**Native (Swift / Kotlin)** — required for demanding platform work: background location, HealthKit, widgets, complex camera pipelines. *Bites later:* two codebases, two skill sets, two release cycles.

**PWA / responsive web** — often the honest answer. If it is fundamentally content or forms and does not need push, offline, or hardware, a website avoids app-store review entirely. *Bites later:* iOS restricts PWA capabilities; no store presence, which matters if discoverability is the point.

**Watch for:** app stores add weeks of latency (review, developer accounts, provisioning). If the user says "for our team", ask whether it must be in a store at all — internal distribution or a web app can skip the entire apparatus.

---

## Web frontend

**Next.js** — the default for anything with pages, routing, SEO, or a server component. Batteries included; the deployment story on Vercel is nearly frictionless. *Bites later:* server/client component boundaries confuse newcomers and produce baffling errors; self-hosting is meaningfully more work than the docs suggest; the framework churns fast enough that tutorials go stale.

**Vite + React (or Vue/Svelte)** — right for SPAs, dashboards, and internal tools with no SEO need. Fast, simple, easy to reason about, deploys as static files anywhere. *Bites later:* routing, data fetching, and auth are all decisions you now own; that freedom becomes assembly work as the app grows.

**Astro** — excellent for content-first sites: docs, blogs, marketing. Ships near-zero JS by default. *Bites later:* fights you once the site becomes app-like and heavily interactive.

**Plain HTML/CSS/JS** — genuinely correct more often than people admit. A single-page tool, a form, a calculator, a visualization: no build step, no dependency upgrades, works in ten years. *Bites later:* past roughly a few hundred lines of interactive logic, hand-rolled state management gets worse than a framework would have been.

**Svelte / SvelteKit** — less boilerplate, smaller bundles, genuinely pleasant. *Bites later:* smaller hiring pool, fewer libraries, fewer answers when stuck.

---

## Backend and APIs

**FastAPI (Python)** — the pick when the backend touches data work, ML, or scripting, or when the user is Python-first. Auto-generated OpenAPI docs are a real productivity gain. *Bites later:* async is easy to get subtly wrong; a blocking call inside an async route silently destroys throughput.

**Express / Fastify (Node)** — natural when the frontend is already JS: one language, shared types, shared validation. *Bites later:* Express leaves structure entirely to you, and unmanaged projects drift into a tangle; error handling in async middleware is a classic footgun.

**Django** — best when the app is CRUD plus an admin interface plus auth. The built-in admin alone can replace weeks of work. *Bites later:* opinionated in ways that resist unusual architectures; heavier than needed for a pure JSON API.

**Go (net/http, chi)** — right for services where throughput, low memory, and single-binary deployment matter. Deployment is genuinely trivial. *Bites later:* more verbose; less convenient for rapid iteration on data models.

**Serverless (Lambda, Cloudflare Workers)** — good for spiky, low-baseline traffic; scales to zero, so idle costs nothing. *Bites later:* cold starts; local development is awkward; long-running work and persistent connections fit badly; per-request pricing can surprise at scale.

**Backend-as-a-service (Supabase, Firebase)** — frequently the right recommendation for small projects. Auth, database, storage, and realtime without writing a backend, which can compress weeks into an afternoon. *Bites later:* vendor lock-in — Supabase mitigates this by being Postgres underneath, Firebase does not; complex queries and reporting get awkward; costs scale in ways that are hard to predict early.

---

## CLI tools

**Python + Typer/Click** — fastest to write, ideal when the tool wraps data or file work. *Bites later:* distribution — the user needs Python and the right dependencies; `pipx` or PyInstaller help but neither is seamless.

**Go or Rust** — a single static binary with no runtime, which makes distribution trivial: hand over one file. Right for tools other people will install. *Bites later:* slower to write; cross-compiling for three platforms needs CI setup.

**Node + Commander** — sensible if the tool belongs to a JS project and users already have npm. *Bites later:* startup latency is noticeable for a frequently-run command; the node_modules footprint is large relative to the tool.

**Shell script** — legitimate for under ~100 lines of orchestration. *Bites later:* portability (bash vs. zsh vs. Windows), and past a certain size a rewrite becomes inevitable.

---

## Browser extensions

**Manifest V3 + vanilla JS or a small framework** — the only real option now; MV2 is being retired. Chrome, Edge, and Firefox share most of the API surface. *Bites later:* MV3 service workers terminate aggressively, so persistent state must live in `chrome.storage` rather than memory — a common source of "it works then stops working"; store review adds days; content-script CSS leaks into and out of the host page without shadow DOM.

Use a bundler (Vite with a CRX plugin) as soon as the extension has more than one script; hand-managing the manifest and file paths gets old fast.

---

## Automation and scripts

**Python** — the default. Rich libraries for files, HTTP, parsing, spreadsheets, images. *Bites later:* dependency and environment drift on the user's machine over time; pin versions and use a venv.

**Playwright** — for browser automation and scraping where the site needs real rendering or a login session. More reliable than requests-plus-parsing for modern sites. *Bites later:* target sites change their markup and break selectors; expect ongoing maintenance, not a one-time build.

**PowerShell** — natural on Windows for OS-level work: registry, services, scheduled tasks, AD. *Bites later:* execution policy blocks scripts on locked-down machines.

**Scheduling** — cron on Linux/macOS, Task Scheduler on Windows, GitHub Actions when the work can run in the cloud. Ask early where this will actually run; a script that only works while the user's laptop is open is a different design from one that runs nightly regardless.

---

## Data, analysis, and ML

**Jupyter + pandas** — right for exploration and one-off analysis. *Bites later:* notebooks resist version control and reuse; when the same notebook gets run monthly, it should have become a script.

**Python script + pandas/Polars** — the move once the analysis is repeated. Polars is markedly faster on large data. *Bites later:* pandas silently handles multi-gigabyte data badly; if the file does not fit comfortably in memory, plan for chunking, Polars, or DuckDB up front.

**DuckDB** — excellent middle ground: SQL over local CSV/Parquet, no server, very fast. Often the best answer for "analyze this large file". *Bites later:* single-writer; not a multi-user database.

**Streamlit / Gradio** — turns an analysis into a shareable UI in remarkably little code. *Bites later:* the whole script reruns on every interaction, which surprises people and constrains how state can work; not suited to complex UI.

**Model training** — ask what "ML" actually means here. Frequently the honest recommendation is an API call to an existing model rather than training anything, and saying so saves the user weeks. Reserve training for cases with real labeled data and a genuine reason not to use a hosted model.

---

## Games

**Godot** — the strong default for 2D and increasingly 3D. Free, no royalties, small install, good editor. *Bites later:* fewer tutorials and assets than Unity; GDScript is another language, though an easy one.

**Unity** — deepest asset store and largest community; the standard for 3D and mobile. *Bites later:* licensing terms have changed in ways that alienated developers; heavier install and longer iteration cycles.

**Web (Phaser, PixiJS, plain canvas)** — best when the game must run instantly in a browser with no install, or is a small experiment. *Bites later:* mobile performance and input handling take real work; monetization and distribution are on you.

---

## Cross-cutting decisions

**Database.** SQLite for single-machine or single-writer (genuinely underrated — it handles far more than people assume). Postgres when multiple users write concurrently or the data model is relational and real. A hosted Postgres (Supabase, Neon, Railway) beats self-managed for small projects. Reach for a document store only with a specific reason; "flexible schema" usually means the schema moved into application code where it is harder to enforce.

**Auth.** Do not hand-roll it. Clerk, Auth0, Supabase Auth, or the framework's built-in system. The failure mode of custom auth is a breach, not a bug.

**Hosting.** Vercel/Netlify for frontend and serverless; Railway/Render/Fly for containers and always-on services; a plain VPS when cost predictability at scale matters more than convenience. Cloud-provider consoles (AWS/GCP) are usually over-scoped for a first project.

**When the answer is "you don't need a backend."** A surprising share of "app" ideas are a static frontend plus a BaaS, or even a spreadsheet plus a script. Check for this before designing a service; recommending less is often the highest-value thing this skill can do.
