# README UI Overhaul Plan

## Summary
Rebuild [README.md](file:///Users/akshyatmanandhar/Desktop/AkshyatMan/README.md) into a polished, animated GitHub profile README with a consistent Neon Cyberpunk theme, add all requested extra sections, and add a GitHub Action so the contribution snake actually renders.

Two files touched:
1. `README.md` — full rewrite (structure + visuals)
2. `.github/workflows/snake.yml` — new, generates snake SVG

## Current State Analysis

Repo contents (verified via LS):
- `README.md` — 82 lines, single file, no `.github/` dir, no assets
- `.trae/documents/enhanced-github-readme-plan.md` — prior plan, leave untouched
- Git repo initialized (`git rev-parse --is-inside-work-tree` → `true`), no remote configured yet

Problems in current [README.md](file:///Users/akshyatmanandhar/Desktop/AkshyatMan/README.md):

| Line | Problem |
|---|---|
| [L5](file:///Users/akshyatmanandhar/Desktop/AkshyatMan/README.md#L5) | `readme-typing-svg.herokuapp.com` — Heroku free tier is dead, this domain is unreliable. Must move to `readme-typing-svg.demolab.com` |
| [L37](file:///Users/akshyatmanandhar/Desktop/AkshyatMan/README.md#L37) | `github-readme-streak-stats.herokuapp.com` — same dead-Heroku problem. Must move to `streak-stats.demolab.com` |
| [L53](file:///Users/akshyatmanandhar/Desktop/AkshyatMan/README.md#L53) | Snake SVG points at `AkshyatMan/AkshyatMan` branch `output` which does not exist → broken image |
| [L8-L13](file:///Users/akshyatmanandhar/Desktop/AkshyatMan/README.md#L8-L13) | Flat left-aligned bullet list — visually plain, breaks the centered rhythm of the rest of the page |
| [L20-L24](file:///Users/akshyatmanandhar/Desktop/AkshyatMan/README.md#L20-L24) | Skill icons ungrouped and unlabeled; claims `rust` and `kubernetes` which are not in the stated stack |
| [L32-L33](file:///Users/akshyatmanandhar/Desktop/AkshyatMan/README.md#L32-L33) | `width="48%"` on two imgs — GitHub's sanitizer strips this inconsistently, cards wrap awkwardly on mobile |
| [L61](file:///Users/akshyatmanandhar/Desktop/AkshyatMan/README.md#L61) | LinkedIn URL `linkedin.com/in/akshyatmanandhar` was assumed, never confirmed by user |
| — | No section dividers, no visual hierarchy, no projects, no About Me |

Confirmed-good data to preserve: username `AkshyatMan`, email `manandharakshyat@gmail.com`, fun fact "I write Go by day, React by night, and dream in TypeScript interfaces".

## Proposed Changes

### File 1: `README.md` (full rewrite)

Section order and exact approach for each:

**1. Header banner**
- `capsule-render` wave-type banner, animated gradient, height 220, with name + tagline baked in
- URL: `capsule-render.vercel.app/api?type=waving&color=0:8A2BE2,100:FF1493&height=220&section=header&text=Akshyat%20Manandhar&fontColor=ffffff&fontSize=42&animation=fadeIn&desc=Fullstack%20Developer&descSize=18`
- Why: gives instant visual impact; replaces plain `<h1>` at [L1-L2](file:///Users/akshyatmanandhar/Desktop/AkshyatMan/README.md#L1-L2)

**2. Typing animation**
- Migrate to `readme-typing-svg.demolab.com` (fixes dead Heroku)
- Keep the 5 existing lines, add color `A78BFA`, `size=24`

**3. Social / contact badge row**
- `for-the-badge` style badges (taller, bolder than current `flat-square`)
- Gmail → `mailto:manandharakshyat@gmail.com`
- GitHub → `github.com/AkshyatMan`
- LinkedIn → `linkedin.com/in/akshyatmanandhar` (kept; flagged as unverified in Assumptions)
- Profile views counter moved here from [L73](file:///Users/akshyatmanandhar/Desktop/AkshyatMan/README.md#L73)

**4. About Me — collapsible `<details>`**
- Native HTML `<details>`/`<summary>`, no dependency
- Contains the 6 bullets from [L8-L13](file:///Users/akshyatmanandhar/Desktop/AkshyatMan/README.md#L8-L13), reworded, plus a fenced `json` code block "quick facts" object (name, role, stack, currently_learning, fun_fact)
- Why collapsible: keeps page scannable while still holding detail

**5. Tech Stack — grouped with labels**
- Three labeled `skillicons.dev` rows: **Languages**, **Frontend & Mobile**, **Backend, Data & DevOps**
- Drop `rust`, `kubernetes`, `figma` (not in stated stack — no fabrication)
- Final icon sets:
  - Languages: `js,ts,go,python,html,css`
  - Frontend & Mobile: `react,nextjs,redux,tailwind,vite,figma` → figma removed, use `react,nextjs,redux,tailwind,vite`
  - Backend/Data/DevOps: `nodejs,express,graphql,postgres,mongodb,redis,docker,git,linux,aws,vercel`
- Add `&theme=dark` for consistency

**6. Featured Projects**
- Two-column table of `github-readme-stats` **pinned repo cards** (`/api/pin/?username=AkshyatMan&repo=...`)
- Repo names are unknown → use 4 placeholder slots with an HTML comment instructing which string to replace. Rationale in Assumptions.

**7. GitHub Stats**
- Wrap the two stats cards in a `<table>` with `<td>` cells instead of `width="48%"` — fixes the mobile wrap issue at [L32-L33](file:///Users/akshyatmanandhar/Desktop/AkshyatMan/README.md#L32-L33)
- Streak card → `streak-stats.demolab.com` (fixes dead Heroku)
- All three cards on `theme=radical`, `hide_border=true`

**8. Activity Graph**
- `github-readme-activity-graph.vercel.app/graph?username=AkshyatMan&theme=react-dark` — animated line chart of commit activity
- Why: renders immediately with zero setup, so the page still looks complete before the snake workflow's first run

**9. Trophies**
- Keep `github-profile-trophy` from [L45](file:///Users/akshyatmanandhar/Desktop/AkshyatMan/README.md#L45), add `column=8` for a single tidy row

**10. Contribution Snake**
- Same URL as [L53](file:///Users/akshyatmanandhar/Desktop/AkshyatMan/README.md#L53), now backed by the real workflow in File 2
- Wrap in `<picture>` with `prefers-color-scheme` dark/light sources so it looks right in both GitHub themes

**11. Metrics / extras**
- Repos-per-language and most-commit-language compact cards from `github-readme-stats`

**12. Dev joke**
- `readme-jokes.vercel.app/api?hideBorder&theme=radical` — refreshes on each page load

**13. Footer**
- `capsule-render` footer wave mirroring the header
- Typing-SVG "Thanks for visiting" + the Cory House quote from [L82](file:///Users/akshyatmanandhar/Desktop/AkshyatMan/README.md#L82)

Cross-cutting: `<img src="https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png">` divider between major sections for visual separation.

### File 2: `.github/workflows/snake.yml` (new)

- Trigger: `schedule` cron `0 */12 * * *`, plus `workflow_dispatch`, plus `push` on default branch
- `permissions: contents: write` — scoped to the minimum needed to push the output branch
- Steps: `Platane/snk@v3` pinned to major tag, generating both `github-contribution-grid-snake.svg` and `-dark.svg`, then `crazy-max/ghaction-github-pages@v4` publishing to branch `output`
- Why branch `output`: it is what the README URL already expects, so no README churn

## Assumptions & Decisions

1. **Theme = Neon Cyberpunk.** The theme question came back unanswered; defaulting to it because the existing README already uses `theme=radical` at [L32](file:///Users/akshyatmanandhar/Desktop/AkshyatMan/README.md#L32), which is that palette. Purple→pink `8A2BE2`/`FF1493` gradients everywhere.
2. **Heroku → demolab migration is not optional.** Heroku killed free dynos; both `*.herokuapp.com` services in the current README are broken or will break. This is a correctness fix, not a style change.
3. **Project repo names are placeholders.** I have no remote configured and no repo list, so inventing project names would be fabrication. Four clearly-marked placeholder slots with an inline HTML comment instead.
4. **LinkedIn URL carried over as-is.** It was assumed in the prior session and never confirmed. Kept so the section isn't empty, but call it out — verify or tell me the real handle.
5. **Snake needs a first run.** The image stays broken until the workflow runs once on GitHub. That's why the activity graph is included — the page is never visually empty.
6. **Workflow only runs on GitHub.** Cannot be executed or verified locally.
7. **No local commit or push.** No remote is configured. Repo is `AkshyatMan/AkshyatMan` per the README URLs; pushing is left to you.

## Verification Steps

Local (what I will actually do):
1. Re-read `README.md` after writing to confirm structure
2. `GetDiagnostics` on both files for YAML/markdown errors
3. `curl -o /dev/null -s -w "%{http_code}"` every external image URL — confirm 200, catch typos and dead services. Snake URL is expected to 404 until the workflow runs; that is the one known-acceptable failure.
4. Confirm no `herokuapp.com` remains: `Grep` for it, expect zero hits
5. `python3 -c "import yaml,sys;yaml.safe_load(open('.github/workflows/snake.yml'))"` to validate workflow YAML parses

Yours (requires GitHub):
6. Push to `AkshyatMan/AkshyatMan` on the default branch
7. Actions tab → run **Generate Snake** manually → confirm branch `output` is created
8. Reload profile, confirm snake renders in both light and dark mode
9. Replace the 4 project placeholders with real repo names
10. Confirm or correct the LinkedIn URL
