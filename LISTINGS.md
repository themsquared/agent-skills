# Listing notes

Prep only. Nothing in this file has been submitted. Do not file the Hermes PR, comment on vercel-labs/skills, or deploy the MCP from this repo.

## skills.sh (Vercel skills directory)

There is no submission form and no registry PR.

Official guide: [Agent Skills: Creating, Installing, and Sharing Reusable Agent Context](https://vercel.com/kb/guide/agent-skills-creating-installing-and-sharing-reusable-agent-context)

Maintainer confirmation: [vercel-labs/skills#880](https://github.com/vercel-labs/skills/issues/880) (antfu, 2026-11-05): skills.sh indexes a repo when someone runs `npx skills add owner/repo` with telemetry on. You do not file a request.

### Requirements

- Public GitHub repo (this one: `themsquared/agent-skills`)
- `SKILL.md` files the CLI can find. This repo uses `skills/<name>/SKILL.md`, which is a standard discovery path.
- Frontmatter `name` (lowercase, digits, hyphens; matches the parent directory) and `description` (what it does and when to use it). Spec: [agentskills.io/specification](https://agentskills.io/specification)

### Exact steps (after this PR is merged to `main`)

1. Confirm discovery without installing:

   ```bash
   npx skills add themsquared/agent-skills --list
   ```

   Expect `brain-linking` and `property-assessor-fight`.

2. Install with telemetry left on (the CLI default). This is what creates the directory row:

   ```bash
   npx skills add themsquared/agent-skills
   ```

   Or globally: `npx skills add -g themsquared/agent-skills`

3. Wait. Indexing is from aggregated install events, not a GitHub crawl. Topics on the repo do not trigger listing. There is no documented minimum install count. If the page is still empty a few days after a real install, comment on [vercel-labs/skills#880](https://github.com/vercel-labs/skills/issues/880) with the command you ran. Do not open a new issue unless that thread is locked.

4. Check [https://skills.sh/themsquared/agent-skills](https://skills.sh/themsquared/agent-skills)

5. Optional README badge after it exists:

   ```markdown
   [![skills.sh](https://skills.sh/b/themsquared/agent-skills)](https://skills.sh/themsquared/agent-skills)
   ```

### Submission text

Not used. There is no form. If you need a one-liner for a social post or README:

> Two public agent skills: vault graph linking, and a US property-assessment fight playbook. `npx skills add themsquared/agent-skills` Guide: https://webofmike.com/wa-property-tax-appeal/

## Hermes Agent marketplace (webofmike MCP)

Two catalogs exist. Pick the MCP catalog unless a maintainer tells you it is closed to new entries.

### Status of the server (do not claim a public URL)

Repo: [themsquared/webofmike-mcp](https://github.com/themsquared/webofmike-mcp)

Tools: `search_posts` (query plus optional limit) and `get_post` (slug or canonical URL). Read-only. Indexes live public pages on webofmike.com (RSS, `llms.txt`, sitemap, per-post markdown). No auth.

Transports in that repo:

- Stdio: `npx tsx src/index.ts` (what Cursor and Claude Desktop spawn)
- Streamable HTTP for local or future hosting: `npm run start:http` binds `http://127.0.0.1:43147/mcp` by default (`GET /`, `GET /health`, `POST /mcp`)

Remote HTTP: **not deployed**. `mcp.webofmike.com` does not resolve. `https://webofmike.com/mcp/` is a blog series, not an MCP endpoint. The README's example `https://mcp.webofmike.com/mcp` is a planned host, not a live one.

Do not submit an HTTP catalog row until a public HTTPS Streamable HTTP URL answers `initialize` anonymously. Pattern to copy then: [optional-mcps/deepwiki/manifest.yaml](https://github.com/NousResearch/hermes-agent/blob/main/optional-mcps/deepwiki/manifest.yaml) (`auth: none`, `transport.type: http`).

Until that URL exists, submit stdio from the public git repo.

### Path A: MCP catalog (picker: `hermes mcp`)

Docs: [MCP (Model Context Protocol)](https://hermes-agent.nousresearch.com/docs/user-guide/features/mcp)

Quote from that page: catalog entries live under `optional-mcps/` in `NousResearch/hermes-agent`. Presence in that directory is Nous approval. There is no community self-serve tier. To add an MCP, open a PR against `optional-mcps/`.

- Repo to PR: https://github.com/NousResearch/hermes-agent
- Open compare: https://github.com/NousResearch/hermes-agent/compare
- New file: `optional-mcps/webofmike/manifest.yaml`
- No web form

#### Draft `optional-mcps/webofmike/manifest.yaml` (stdio, until HTTPS is live)

Pin `install.ref` to a full 40-character SHA from `themsquared/webofmike-mcp` at submit time. Replace `REPLACE_WITH_FULL_SHA`.

```yaml
# Nous-approved MCP catalog entry.
# Presence in this directory = approval. Merged via PR review.
manifest_version: 1

name: webofmike
description: Search and fetch live public posts from webofmike.com (read-only).
source: https://github.com/themsquared/webofmike-mcp

install:
  source: https://github.com/themsquared/webofmike-mcp.git
  ref: REPLACE_WITH_FULL_SHA
  bootstrap:
    - npm install --omit=dev

transport:
  type: stdio
  command: npx
  args: ["tsx", "${INSTALL_DIR}/src/index.ts"]

auth:
  type: none

suggest:
  keywords:
    - webofmike
    - mike moore
  hosts:
    - webofmike.com

post_install: |
  No account or credentials. Tools search_posts and get_post read live
  public pages on webofmike.com. Restart the session so tools load.
  Needs Node.js 20+ and network access to webofmike.com.
```

`${INSTALL_DIR}` is substituted at install time with the clone path (Hermes MCP docs). Confirm the live manifest schema in a current catalog stdio entry before sending. If review wants HTTP-only, deploy HTTPS first and switch to the deepwiki shape.

#### Draft PR title

`feat(mcp): add webofmike read-only blog MCP`

#### Draft PR body

```
## Summary

Add a Nous catalog entry for the public webofmike MCP server.

- Upstream: https://github.com/themsquared/webofmike-mcp
- Site: https://webofmike.com
- Tools: search_posts, get_post (read-only, no auth)
- Transport in this PR: stdio (clone + npx tsx). No public HTTPS MCP URL yet.
- Network: outbound HTTPS to webofmike.com only (RSS, llms.txt, sitemap, post markdown)

## Disclosure

- Network calls to https://webofmike.com (public content)
- No credentials, no OAuth, no telemetry
- Runs a local Node process (tsx) after npm install
- Does not write to the user's files

## Test plan

1. hermes mcp install webofmike
2. hermes mcp test webofmike
3. Call search_posts with query "property assessment"
4. Call get_post with slug wa-property-tax-appeal
5. Confirm no auth prompt
```

### Path B: Plugin catalog (marketplace shelf)

Use this if a maintainer says new MCP picker entries must go through plugins, or if you want a card on [hermes-agent.nousresearch.com/marketplace](https://hermes-agent.nousresearch.com/marketplace).

Guide: [Submitting to the Plugin Catalog](https://hermes-agent.nousresearch.com/docs/developer-guide/plugins/catalog-submission)

Canonical rules: [plugin-catalog/README.md](https://github.com/NousResearch/hermes-agent/blob/main/plugin-catalog/README.md)

This path needs a **plugin package**, not the MCP repo as-is. The MCP repo has no `plugin.yaml` or `plugin.json`. Do not PR `plugin-catalog/webofmike.yaml` pointing at webofmike-mcp until you add a portable Agent Plugin (for example `plugin.json` plus `mcp.json`) in a public repo, pin a 40-character SHA, and pass:

```bash
hermes plugins validate /path/to/the-plugin --install-deps
```

Then add one file `plugin-catalog/webofmike.yaml` on a PR to `NousResearch/hermes-agent`.

#### Draft `plugin-catalog/webofmike.yaml` (only after a plugin package exists)

```yaml
name: webofmike
repo: https://github.com/themsquared/REPLACE_WITH_PLUGIN_REPO
sha: REPLACE_WITH_FULL_40_CHAR_SHA
description: Search and fetch live public posts from webofmike.com (read-only MCP).
maintainer: themsquared
tier: community
category: tools
version: "1.0.0"
capabilities:
  provides_tools: []
  provides_hooks: []
  provides_middleware: []
  requires_env: []
```

`tier: community` only. Official and verified are set by Nous.

#### Draft PR title

`catalog: add webofmike community plugin`

#### Draft PR body

Same disclosure as Path A, plus: which Hermes surfaces it uses, that validation passed at the pinned SHA, and that the MCP is read-only against public webofmike.com content.

### What not to do

- Do not open either Hermes PR from this skills repo.
- Do not list a `url: https://mcp.webofmike.com/mcp` row until DNS and HTTPS work.
- Do not wrap this skills repo as the plugin. Skills.sh is the skills listing; Hermes MCP catalog is for the server in themsquared/webofmike-mcp.
