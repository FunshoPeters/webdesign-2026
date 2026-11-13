# webdesign-2026

Brand website for **Funsho Peters**, a freelancer offering CRM and AI automation services on Upwork. The site's job is to establish him as an authority in AI automation. Detailed content and design briefs are still to come, so ask before inventing copy, prices, testimonials or client names.

## This is a redesign of funshopeters.io

The live site at https://funshopeters.io (as of 13 Nov 2026) is a single pre-launch page, with no other pages and no navigation menu:

- Brand: "FP Funsho Peters", tagline "AI-powered business systems"
- Hero: "Turn attention into followed-up opportunities." Practical frameworks, tools and real-world proof for founders and operators.
- Three pillars:
  - **Build**: AI-powered lead systems that capture enquiries, follow up over long sales cycles, and measure results.
  - **Prove**: the Lead-Machine Framework, installed publicly in a real estate business, with a baseline first and results published along with their limitations.
  - **Teach**: Capture, Nurture, Convert, Optimize, taught free in the 3-Day Lead-Machine Quickstart.
- A launch countdown to 30 August 2026 (now in the past, and the countdown shows "--")
- Email signup ("Join free"): the 3-Day Quickstart plus a weekly newsletter, Lead-Machine Weekly
- Contact: hello@funshopeters.io
- Tone: direct and plain-spoken, "No hype, no tool-chasing". No testimonials and no performance numbers.

Before replacing copy, the email signup or the Lead-Machine branding, ask what to keep. Also confirm which email tool the signup form posts to.

## Hosting: Hostinger

- The domain and hosting are on Hostinger. This site is static Astro (the default `output: 'static'`), so no adapter is needed.
- To deploy: in hPanel, go to Websites → Add Website → Node.js Apps and import this GitHub repo. Hostinger builds with `npm run build`, and each push to the linked branch redeploys. This needs a Business Web Hosting or Cloud plan. On other plans, run `npm run build` and upload the contents of `dist/` to `public_html` with File Manager.
- Don't switch the live domain over until the user approves the new site, and back up the current site first.

## Stack

- [Astro](https://docs.astro.build) 7, static output, TypeScript strict
- Pages live in `src/pages/`, the shared page shell in `src/layouts/BaseLayout.astro`, and static files in `public/`
- Commands: `npm install`, `npm run dev` (local preview), `npm run build` (output in `dist/`)

## Rules

- Never put fake testimonials, reviews, client logos or invented results on the site.
- Keep pages fast: no heavy client-side JavaScript unless a feature needs it.

## CodeGraph (security rules)

CodeGraph was security-scanned with NVIDIA SkillSpector on 13 Nov 2026. It is approved for this repo **only** in the hardened form below:

- Only version **1.6.2**, installed from npm: `npm install -g @colbymchenry/codegraph@1.6.2`
- Never use the `curl … | sh` / `irm … | iex` installers, and never run `codegraph upgrade` or `codegraph install`. A new version must be re-scanned before use.
- Telemetry stays off (`codegraph telemetry off`; `.mcp.json` also passes `--no-telemetry`).
- The project connection lives in `.mcp.json` only; the user approves the server when Claude Code first asks. Don't add CodeGraph hooks, permission auto-allows or global config.
- If `codegraph` isn't installed in a session, say so and carry on with normal file tools. Install it (pinned, as above) only when the user asks, then run `codegraph init` to build the index.

<!-- CODEGRAPH_START -->
## CodeGraph

In repositories indexed by CodeGraph (a `.codegraph/` directory exists at the repo root), reach for it BEFORE grep/find or reading files when you need to understand or locate code:

- **MCP tool** (when available): `codegraph_explore` answers most code questions in one call — the relevant symbols' verbatim source plus the call paths between them, including dynamic-dispatch hops grep can't follow. Name a file or symbol in the query to read its current line-numbered source. If it's listed but deferred, load it by name via tool search.
- **Shell** (always works): `codegraph explore "<symbol names or question>"` prints the same output.

If there is no `.codegraph/` directory, skip CodeGraph entirely — indexing is the user's decision.
<!-- CODEGRAPH_END -->
