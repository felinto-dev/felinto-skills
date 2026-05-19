# Felinto Skills

Agent skills for use with the open `npx skills` CLI.

## Available Skills

- `caprover-one-click-app` - Create, convert, validate, and improve CapRover One-Click App templates.

## Install

Replace `OWNER/felinto-skills` with this repository's GitHub owner and name.

```bash
npx skills add OWNER/felinto-skills --list
npx skills add OWNER/felinto-skills --skill caprover-one-click-app -g -a codex -y
```

Install all skills:

```bash
npx skills add OWNER/felinto-skills --all
```

Install from a full URL:

```bash
npx skills add https://github.com/OWNER/felinto-skills --skill caprover-one-click-app
```

## Repository Layout

Skills live under `.agents/skills/<skill-name>/SKILL.md`.

Each `SKILL.md` must include YAML frontmatter:

```markdown
---
name: skill-name
description: Short description of when the agent should use this skill.
---
```

## Publish

```bash
git add .
git commit -m "Add CapRover one-click app skill"
git branch -M main
git remote add origin git@github.com:OWNER/felinto-skills.git
git push -u origin main
```

After the repository is public, users can install it with:

```bash
npx skills add OWNER/felinto-skills
```
