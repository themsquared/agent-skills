# agent-skills

Public agent skills by [Mike Moore](https://webofmike.com). Installable with the [skills](https://skills.sh) CLI.

Each skill is a folder with a `SKILL.md` that follows the [Agent Skills spec](https://agentskills.io/specification): YAML frontmatter with `name` and `description`, then instructions.

## Skills

### brain-linking

Teaches an agent to treat an Obsidian or markdown knowledge vault as a graph. Every new or updated note gets an up link, a sideways link, frontmatter, and a Related section, so nothing is orphaned.

Use when the agent is writing or editing vault notes, meeting summaries, or inbox items.

### property-assessor-fight

Ops recipe for helping a US homeowner contest a county property assessment. Builds a private fact packet, public-records request, appeal-board ladder, oversight checklist, refund map, and optional media tip. Assistants draft. Humans send.

Not legal advice. The free public guide with copy-paste prompts is at [webofmike.com/wa-property-tax-appeal](https://webofmike.com/wa-property-tax-appeal/).

Use when someone pastes an assessment notice or asks to fight an assessor valuation.

### government-accountability

Citizen case manager for making a county assessor, or any local agency, show its work. Intake of a notice and record card, error finding, specific public-records requests with the right state statute and response clock, a deadline tracker, comps, appeal drafts, and escalation letters drafted the moment a clock is missed. Lawful by design: one real person, one property, specific requests, and the human sends everything.

Not legal advice. The free public playbook with templates, a printable checklist, and a neighbor one-pager is at [webofmike.com/make-the-assessor-show-its-work](https://webofmike.com/make-the-assessor-show-its-work/).

Use when someone pastes a property tax notice, record card, or records-request reply, or asks how to escalate a stalled agency.

## Install

All skills in this repo:

```bash
npx skills add themsquared/agent-skills
```

List what the CLI would install, without installing:

```bash
npx skills add themsquared/agent-skills --list
```

One skill:

```bash
npx skills add themsquared/agent-skills --skill brain-linking
npx skills add themsquared/agent-skills --skill property-assessor-fight
npx skills add themsquared/agent-skills --skill government-accountability
```

Global install:

```bash
npx skills add -g themsquared/agent-skills
```

Directory listing (once install telemetry has indexed the repo): [skills.sh/themsquared/agent-skills](https://skills.sh/themsquared/agent-skills)

## Related

- Site: [https://webofmike.com](https://webofmike.com)
- Spec: [agentskills.io/specification](https://agentskills.io/specification)
- Listing notes (skills.sh and Hermes): [LISTINGS.md](LISTINGS.md)
- MCP server for live posts: [themsquared/webofmike-mcp](https://github.com/themsquared/webofmike-mcp)

## License

[MIT](LICENSE)
