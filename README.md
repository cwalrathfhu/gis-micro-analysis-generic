## 1) Starter README (Draft)

```markdown
# Generic GIS Micro-Analysis Tool

A lightweight, configurable GIS screening tool that runs entirely in the browser. This repo is intended to host a **generic core framework** that can be extended into bespoke, project-specific tools (e.g., Small Starts land use screening).

## Goals
- Provide a reusable, front-end-only analysis framework.
- Keep geometry workflows consistent (points → buffers → union polygon).
- Support modular analysis (population, employment, equity, services, etc.).
- Allow configuration via simple profile files (no code changes required for most customizations).

## What this repo is (and isn’t)
- ✅ A generic core engine that can be reused across projects.
- ✅ A place to add new analysis modules and profiles.
- ❌ Not the production Small Starts tool.
- ❌ Not a backend system or database.

## Quick start (placeholder)
1. Open `index.html` in your browser (or use a lightweight local server).
2. Add station points by clicking on the map.
3. Upload datasets or configure API inputs in your selected profile.

## Configuration
- Profiles live in `/profiles/` (JSON files).
- Each profile turns modules on/off and defines rating rules.

## Contributing
If you’re new to GitHub:
1. Make changes in a new branch.
2. Commit and push.
3. Open a pull request.

## License
TBD
