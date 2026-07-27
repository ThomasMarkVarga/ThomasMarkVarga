# GitHub Profile README — Design

**Date:** 2026-07-27
**Owner:** Thomas Mark Varga (github.com/ThomasMarkVarga)
**Goal:** A profile README that is professionally structured (skills, projects, contact) with playful animated elements — audience is both recruiters and casual visitors.

## How it works

GitHub renders the `README.md` of a public repository named exactly `ThomasMarkVarga` at the top of the user's profile page. All dynamic elements are embedded images served by free widget services, plus one GitHub Action for the snake animation.

## Approach (chosen: classic widget-powered README)

Hosted widget services + one GitHub Action. Alternatives considered and rejected: fully self-hosted SVG generation via `lowlighter/metrics` (more reliable but more setup and a token to manage), and minimal static markdown (not funky enough for the stated goal).

## Repository structure

```
README.md                       — the profile page
.github/workflows/snake.yml     — daily workflow rendering the contribution snake
docs/superpowers/specs/         — this design doc
```

The snake workflow (`Platane/snk`) runs on a daily cron schedule plus `workflow_dispatch`, and pushes the generated SVGs to an `output` branch so `main` stays clean. The README references the SVG via `raw.githubusercontent.com/ThomasMarkVarga/ThomasMarkVarga/output/...`.

## Page layout (top to bottom)

1. **Greeting + typing animation** — "Hi, I'm Thomas Mark Varga 👋" heading, then a `readme-typing-svg` image cycling phrases: "Software Developer", "C++ · Java · C#", "Always building something". Centered.
2. **About** — 3-4 lines describing a general software developer who builds across domains (games, desktop apps, automation, hardware-adjacent projects). Followed by contact badges:
   - Email badge → `mailto:thomasvarga64@gmail.com` (user approved making the address public)
   - LinkedIn badge → https://www.linkedin.com/in/thomas-mark-varga-225644258/
3. **Tech stack** — shields.io `for-the-badge` style icons, grouped:
   - Languages: C++, Java, C#, JavaScript, MATLAB, Python
   - Tools: Git, Arduino, Visual Studio
4. **GitHub stats** — `github-readme-stats` stats card and top-languages card side by side; `github-readme-streak-stats` streak card below. `show_icons=true`, `include_all_commits=true`.
5. **Snake animation** — full-width contribution snake, with `<picture>` element serving the dark variant in dark mode.

## Theme

All widgets use the **tokyonight** theme (dark navy, blue/purple accents) for a coherent look; the typing SVG uses a matching accent color (`#70a5fd`-family). Theme is a URL parameter on each widget, trivially swappable later.

## Error handling / known gotchas

- Repo must be **public** and named exactly `ThomasMarkVarga`, or GitHub shows nothing.
- The snake image is broken until the workflow's first run — trigger it manually (`gh workflow run`) right after pushing.
- The snake workflow needs `permissions: contents: write` to push to the `output` branch.
- Hosted stats cards occasionally rate-limit and render blank for a few minutes; self-recovering, no mitigation needed.

## Testing / acceptance

After pushing and running the workflow once:

- https://github.com/ThomasMarkVarga shows the README on the profile.
- Typing animation, both stats cards, streak card, all badges, and the snake SVG render (no broken images).
- Email badge opens a mail draft; LinkedIn badge opens the profile.
- The `Generate snake` workflow run is green and the `output` branch exists.
