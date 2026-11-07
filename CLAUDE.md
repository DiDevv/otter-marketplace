# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

A Claude Code plugin marketplace (`otter-marketplace`) that currently ships a single plugin, `otter-skills`. There is no build, lint or test tooling; the content is JSON manifests and Markdown skills/agent prompts.

## Commands

```bash
claude plugin validate .                       # validate marketplace + root plugin manifests
claude plugin validate . --profile directory   # also fail on plugin-directory submission problems
```

Install from GitHub: `/plugin install otter-skills --marketplace DiDevv/otter-marketplace`

## Structure

- The repo root is **both** the marketplace and the plugin. `.claude-plugin/marketplace.json` lists `otter-skills` with `"source": "./"`, so `.claude-plugin/plugin.json` at the same root defines that plugin.
- Skills live under `skills/<category>/<skill-name>/SKILL.md`. Because of the extra category level, every skill directory must be listed explicitly in the `skills` array of `plugin.json`. Adding a skill folder without registering it there means it won't load.
- When adding a new plugin to the marketplace, give it its own folder and `plugin.json`, add an entry to `marketplace.json`'s `plugins` array, and validate that plugin's folder separately (`claude plugin validate <folder>`). Validating the root only covers the root plugin.

## The `code-best-practices` skill

`skills/development/code-best-practices/` is the only skill so far:

- `SKILL.md` is a clean-code workflow checklist: explore, code, test, adversarial review, document, then commit with Conventional Commits.
- `adversary-agent/AGENT.md` is a reviewer prompt (security, anti-patterns, complexity, hygiene; findings tagged `[CRITICAL]`/`[HIGH]`/`[MEDIUM]`/`[LOW]`). The skill tells Claude to launch it **without context** about the code's intent and to loop fix → re-review until it reports no new findings.
- The agent file sits inside the skill folder, so it's a supporting file the skill reads. Claude Code does **not** register it as a subagent. To make it a real plugin subagent, it would need to move to a top-level `agents/` dir as `<name>.md` with Claude Code subagent frontmatter (`name`, `description`, optional `tools`/`model`). Its current `tools: - type: agent_toolset_20260401` entry is Claude API / Managed Agents syntax, not Claude Code syntax.

## Conventions

- Commits use Conventional Commits (`feat`, `fix`, `chore(scope): ...`), as the skill itself prescribes.
- Skill frontmatter `name` should be kebab-case (e.g. `code-best-practices`), and `description` should say *when* to trigger the skill, since that's what Claude matches on.
- Line endings are LF (enforced by `.gitattributes`). Frontmatter values containing `: ` must be quoted or reworded, or the YAML won't parse.
- Bump `version` in `plugin.json` when shipping changes so installed copies update.

## Known gaps

- `README.md` is a placeholder. The plugin directory rejects READMEs under 40 words and warns when there's no icon (`.claude-plugin/icon.png`, square PNG of 512–2048 px).
