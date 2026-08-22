# The Reveal — course roadmap and state

Doc version: v1.0 · Aug 22 2026
This file is the source of truth for the course. Read it first when picking the work back up. Update it (new doc version at top) whenever something ships or a decision changes. Most recent is authoritative; if the code and this doc disagree, trust the code and fix the doc, flagging the conflict.

---

## What this is

An interactive course that teaches the fundamentals under Mot's own builds, for his learning first and as a public shelf piece second. Not a read-it course. A do-reps-until-it-sticks course: every part has real working sandboxes and a practice Gym with drills checked live against a real engine.

Primary goal: help Mot learn what he needs to keep building.
Second goal: a shareable teaching piece on the website (motbuchanan.com shelf).

### What it demonstrates (the framing worth keeping)

This is, in effect, a working demonstration of a training-and-development method: take a goal, break it into what a person needs to know at each step, and build the interactive tool that gets them there, with practice tuned to how they will actually use it. The "Your work" gym level is the proof: same engine, drills reshaped to the real job.

Framed to an organization, that is custom training plans for any path through the company (onboard a fabrication tech, ramp a clinician on a new scanner, get a comms hire up to speed), built as live practice tools rather than passive slides.

Two honesty rails so the pitch stays defensible:
- This is a demonstration of the method, not a finished L&D platform. No admin dashboard, no cross-user completion tracking, no content-authoring UI. A real engagement gets scoped separately.
- The appeal for Mot is the version where he still builds. A pure training/enablement role that removes hands-on building is not the target. This piece is training-through-building.

---

## Decisions on record

- Aug 22: This course SUPERSEDES The Reveal (the old foundations-course / fieldguide). The overlap is resolved by absorbing it, not running two courses.
- Aug 22: Public shelf identity pulls from The Reveal. See "Pull from The Reveal" below.
- Aug 22: Tier 2 chapters get built next, in order, THEN the three Tier 3 branches. Confirmed.
- Aug 22: Aesthetic direction: each section keeps its own theme (colors/fonts fit the concept); a cohesive layer holds it together, especially the hub as branches grow. A dedicated design pass happens later, done as pick-from-options, not decided unilaterally. DEFERRED, do not design-pass early.
- Nothing goes public yet. Keep developing.

### Open decisions (settle before the work they gate)

- PUBLIC TITLE: make "The Reveal" the shelf title and demote "The Whole Machine" to the Unit 1 name? This sets the hub wordmark. Not yet locked. (Recommended: yes.)
- Design pass: deferred by choice until the branches start growing. Not blocking Tier 2.

---

## Current state — what is built (all shipped, all validated)

Seven single-file HTML apps, same origin, deploy together in one folder. The hub reads each app's progress key same-origin. Progress is per-device / per-browser.

| File | Title | Version | Storage key | State shape |
|---|---|---|---|---|
| computer-basics.html | The Fast Idiot: What a Computer Actually Is | v3.0 | computer101_v1 | state.m{c1..c5} + quizBest + gym |
| internet-basics.html | The Second After You Tap | v3.0 | internet101_v1 | state.m{n1..n6} + quizBest + gym |
| browser-basics.html | The Painter | v3.0 | browser101_v1 | state.m{b1..b6} + quizBest + gym |
| linux-basics.html | Under the Hood | v3.0 | linux101_v1 | state.missions{m1..m6} + quizBest + gym |
| python-basics.html | Second Language | v3.0 | python101_v1 | state.lessons{p1..p4} + quizBest + gym |
| git-basics.html | Save Points | v3.0 | git101_v1 | state.m{m1..m7} + state.boss + quizBest + gym |
| course.html | The Whole Machine (hub) | v1.2 | reads all six | reads .m / .missions / .lessons + .gym |

Course order: computer, internet, browser (Unit 1: how anything reaches your screen), then linux, python, git (Unit 2: your toolbox).

NOTE the three different state shapes (.m vs .missions vs .lessons). The hub already handles all three. Any new app should pick one and the hub must be taught it.

### The Gym pattern (locked, in all six)

Each app has a practice Gym section: randomized-name drill templates, five skill tabs (four concept levels + a fifth "Your work" level, lv:4) plus "Surprise me," a plain-words goal, a live check against the REAL engine, two-stage hints (concept, then exact answer), a "type it for me" fill, Skip (reveals answer, resets streak), and reps/streak/best persisted. Level filter: `pool = GYM_TPLS.filter(t => gymLevel === -1 || t.lv === gymLevel)`.

Each app exposes `window._gym` test hooks (start/state, plus app-specific solve/pick/apply).

Design rule to preserve: computer + internet field-call drills are the ONLY ones where a wrong answer ends the drill and resets the streak (they test the mental model). Everywhere else you keep working the problem. Prediction and diagnosis are tests; building is practice.

### "Your work" drill inventory (the his-stack level, lv:4)

- git w01-w06: ship a game to the shelf, selective-stage index.html while leaving sw.js unsaved, branch a redesign off the live hub and merge back, recover from forgetting to branch, survive a conflict on a version badge, init a new project.
- linux w01-w06: start a puff-kart project folder + index.html, echo/cat a VERSION.txt, move a misfiled asset to the flat repo root, back up index.html before an AI rewrite, touch .nojekyll, clean junk before upload.
- python z01-z05: build a version string (concat, no str()), make a storage key gameshelf_v1, USB cost math, len on an app name, a "Saved for..." toast. (z01/z04 were rewritten to stay inside the mini-interpreter, which has no str() and no list literals. Do not reintroduce those.)
- computer w01-w04: game-loop and score-bonus predictions, tune-a-score and lives-countdown builds.
- internet w01: five rotating deploy-desk diagnosis scenarios (404 after upload, content:// persistence trap, stale cache, blank page for one tester, custom-domain DNS).
- browser w01-w05: app header (h1 + p), brand accent color, version-badge rule (italic + color), grow the game list, wire a full interaction.

### Engines (what each gym checks against, honestly)

- computer: real toy CPU. Instrs SET/ADD/SUB/ADDR/SHOW/JUMPIF(reg>0 -> 1-based line)/HALT, regs A and B. Exports freshMachine/stepMachine/runMachine/bytesOf.
- internet: real DNS book + BFS-routable 8-node network (kill routers, it reroutes) + mini HTTP server (200/404/301). Exports makeDNS/dnsLookup/dnsAdd/bfsPath/serveRequest/NET_EDGES.
- browser: live DOMParser tree + scoped-CSS engine + real JS pokes. Exports scopeCSS/cssRules.
- linux: real in-memory filesystem + terminal. Commands pwd/ls/cd/mkdir/touch/echo>/cat/rm/cp/mv/clear/help. Exports freshFS/runCommand/nodeAt/pathString.
- python: mini interpreter. Supports print, variables, + (concat), * (repeat), len() on a string, math. NO str(), NO list literals, NO if/for. Exports runPython/pyTokenize/pyValToString.
- git: full mini-git. init/status/add/commit -m/log/branch/switch/checkout/merge (LCA 3-way + conflict flow). Live SVG commit graph. Exports freshRepo/runGit/statusOf/isAncestor/lca/headId.

---

## Roadmap

### Tier 2 — build next, in this order

- Ch 7 — Hosting & GitHub Pages. Repos, the upload-from-phone flow, flat vs folder, the content:// persistence trap as a first-class lesson (open from Downloads = quarantined origin = storage dies). Gym: fix a broken deploy, spot why a file 404s, choose flat vs folder.
- Ch 8 — Where data lives. localStorage -> files -> a real database, ending pointed at Firebase. Gym: pick the right storage for a scenario, debug a lost-save.
- Ch 9 — What an API call is. Mot already makes them. Request/response, status, shape. Gym: read a request/response, predict the result.
- Ch 10 — The Shelf Dissected. THE KEYSTONE. His actual Game Shelf taken apart and rebuilt: hub, game registrations, sw.js precache, Firebase sync, GitHub Pages deploy, the version timeline. This is the chapter that makes the course unmistakably his and carries the training-tool proof.

### Tier 3 — the three branches, after Tier 2

- Firebase / backend. Turn the buzzer learning-project into the lesson. Mot's named next skill. Auth, Firestore, live listeners, cross-device state.
- How AI models work. Prompts, context, what the model is actually doing. Includes a practical prompting/context gym (a Mot strength, so it teaches well).
- How digital media works. Images, video, audio as data. This is the on-ramp to the down-the-line media track.

### Growth areas the course is being aimed at (Mot named these)

- Website development -> Tier 2 (hosting, data) + a future "how a real website is structured" chapter (multi-file, components, why frameworks exist). This is the bridge past single-file, his stated wall.
- Finding new tools to build -> a meta-skill piece: "how I turn a passing question into a built tool." His curiosity-tools practice made teachable. Doubles as portfolio.
- Growing with AI -> the How-AI-Models-Work branch + the prompting gym.
- Game development -> its own multi-chapter Unit (game loops, state, input, collision, canvas, juice/game-feel). Where his energy actually is. The computer gym already seeds the game-loop idea.

### Down-the-line / future (explicitly "down the line," after fundamentals)

- AI-generated images and video.
- Using AI for editing and post-production.
Placed after the fundamentals on purpose. This is Mot's mastery lane, so it is where the course eventually meets what he is already best at. The "how digital media works" branch is the natural lead-in.

### Proposed, not yet ruled on by Mot

- Debugging as a taught skill (read an error, form a hypothesis, test it).
- Working-with-AI-to-code specifically (how to hand off, verify what came back, avoid black-box dependence). Aimed at his worry that his code is vulnerable / full of junk.
- Security and cleanup basics (from The Reveal's Unit 5). Same worry, addressed directly.

---

## Pull from The Reveal (absorbed project)

The Reveal was reveal.html (was fieldguide.html), key reveal_v1, an 18-chapter read-only course. Its mission survives here. Pull, in priority order:

1. Name + premise. "The Reveal" and the magician framing ("every trick looks like magic until someone shows you how it's done") as the public shelf title and series story. Personally resonant: Mot was a professional magician / balloon twister who put himself through college with it. HIGHEST-VALUE pull.
2. Shelf-dissected chapters -> becomes Tier 2 Ch 10.
3. The self-source peek mechanic (a page shows its own outerHTML + char count). The literal "reveal." Browser explainer has a light version; the full one is a signature move.
4. Specimen-frame visual identity (crop-mark corners, syntax palette). A candidate for the cohesive layer in the design pass. Optional.
5. The cross-chapter growing glossary -> a hub reference tab.

DO NOT pull: the 18-chapter read-only structure, the passive quiz-only progression. That is the part this course replaces.

---

## Standing build rules (apply to every app here)

- Single self-contained HTML. No base64. Literal hex in SVG. Escape `</script` inside JS strings. Guarded localStorage with a versioned key. Version badge, tappable -> toast.
- No em dashes anywhere, including app copy. No cream/warm-light backgrounds (the "default AI" tell). No uniform fade-up-on-scroll (also an AI tell).
- Honest-interactive: never fake an effect. If the engine cannot show the real thing, change the metric to what is true. (This is why the two python drills got rewritten.)
- Deploy: all seven files in ONE folder, same origin. Mot uploads from his phone via GitHub "Upload files" (never paste large file contents into the mobile editor, it truncates). Subfolders are OK for a self-contained app with relative paths and its own scoped sw.js; flat is the default.
- Publishable copy: no personal or employer names in the public version. The "Your work" drills currently name real projects (puff-kart, gameshelf, etc.). Fine for Mot's private build; sanitize for any public release.

## Validation gate (run before every ship)

Extract the single script block, then: node --check, acorn parse (ecmaVersion 2020), grep for 0 Jekyll tokens and 0 unescaped `</script` and 0 em/en dashes, then a headless jsdom smoke with url:'https://example.test/' (localStorage needs a real origin), wait ~100-120ms for init, assert. Strongest gate: every drill template must be machine-proven solvable by its own hint solution. When an app's version or tab count changes, update the older suites' stale assertions.

Test suites (in the working dir, not shipped): tier0-engine-test.js, git-engine-test.js, linux-v2-engine-test.js, linux-v2-smoke.js, gym-git-py-smoke.js, gym-rest-smoke.js, work-drills-smoke.js.

---

## Changelog

- v1.0 (Aug 22 2026): Doc created. Captures state after all six apps reached v3.0 (Gym pattern + "Your work" level in all six) and hub reached v1.2. Records the supersede-The-Reveal decision, the Tier 2 -> Tier 3 roadmap, the growth areas, the down-the-line media track, the training-tool framing, and the deferred design pass.
