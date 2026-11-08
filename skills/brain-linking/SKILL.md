---
name: brain-linking
description: >-
  Link every new or updated note into an Obsidian or markdown knowledge
  vault graph so nothing is orphaned. Use whenever you write or edit a
  vault note, meeting summary, inbox item, or related page.
license: MIT
---

# Brain linking

A vault is a graph. A note with no links is lost. Every note you write must connect to at least two existing pages, and at least one existing page should point back to it.

This skill is for any Obsidian vault or markdown knowledge base that uses `[[wikilinks]]`. It is not tied to one person's files.

## Before you write

1. Read the vault schema at the root. Common names: `CLAUDE.md`, `AGENTS.md`, `README.md`. Follow that file if it exists.
2. Search for what already exists: `rg -il '<topic or name>' --glob '*.md'`.
3. If a page on the topic exists, add to it (or drop an inbox note pointing at it). Do not create a near-duplicate.

## Every note must have

- **An up link:** one `[[link]]` to its parent hub. Examples: a person or project `[[index]]` or `[[status]]`, a `[[concepts/...]]` page, a project hub, or an area hub.
- **Sideways links:** at least one `[[link]]` to a related person, project, product, concept, or meeting that already exists.
- **People and things as links, not plain text:** if a person, product, or project has a page, write `[[Name]]`. If it should have one and does not, link it anyway. Obsidian shows the dangling link, and the maintainer creates the page.
- **Frontmatter:** `about`, `area`, `source`, `suggested_home`, `date`, `agent`. Use `area` values the vault already uses (often work vs life, or the vault's own area list).
- **A Related section at the bottom:**

  ```markdown
  ## Related
  - [[parent-hub]]
  - [[sibling-or-person]]
  ```

## Link targets

- Use the exact existing file name (kebab-case). Check with `rg --files | rg <slug>` before linking.
- Prefer a few strong links over a pile of weak ones. Link what a reader would actually click next.
- Never link only to a date page, a daily note, or a tag. Those do not connect topics.

## Meeting and recording notes

A meeting or recording note links to:

- the person, team, or project it was about
- each attendee who has a page
- any concept or decision it produced

A transcript with zero links is the most common orphan.

## Raw sources

Files in `_raw/` (or the vault's equivalent capture folder) are never edited. Do not add links inside them. Instead, the summary or meeting page that cites them links to the raw file, so it joins the graph through that page.

## Writing to a shared inbox (bots)

- If the vault says to write only to `_agents/inbox/` (or another inbox path), do that.
- The link rules above still apply to inbox notes, and `suggested_home` must name a real page.

## Never put in the vault

- Credentials, tokens, or secrets
- Private contact details that do not belong in a shared graph
- Medical records or other sensitive personal data
- Third-party confidential material you are not allowed to store

## Quick check before you finish

- [ ] At least one up link and one sideways link, all to real files (or deliberate new pages)
- [ ] Names of people, products, and projects are linked
- [ ] Related section present
- [ ] No duplicate of an existing page
