# webdesign-2026

Brand website for Funsho Peters: CRM & AI automation. Built with [Astro](https://astro.build).

## Run it locally

You need Node.js 22.12 or newer.

```bash
npm install
npm run dev      # opens a local preview at http://localhost:4321
npm run build    # production build into dist/
```

## CodeGraph (optional, saves Claude Code tokens as the site grows)

This repo is set up to use [CodeGraph](https://github.com/colbymchenry/codegraph) with Claude Code. It was security-scanned with NVIDIA SkillSpector and is approved only in this hardened form:

```bash
# one time per computer: pinned version from npm (not the curl installer)
npm install -g @colbymchenry/codegraph@1.6.2
codegraph telemetry off

# one time in this folder: build the code graph
codegraph init
```

That's it. Claude Code picks it up through `.mcp.json` (approve the "codegraph" server when it asks the first time), and the graph updates itself as files change. Don't run `codegraph upgrade` or `codegraph install`; a new version should be re-scanned first. The scan report is in `docs/security/`.
