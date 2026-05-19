# Felinto Skills

Skills de agentes para uso com a CLI aberta `npx skills`.

## Skills Disponiveis

- `caprover-one-click-app` - Cria, converte, valida e melhora templates CapRover One-Click App.

## Instalar

```bash
npx skills add felinto-dev/felinto-skills --list
npx skills add felinto-dev/felinto-skills --skill caprover-one-click-app -g -a codex -y
```

Instalar todas as skills:

```bash
npx skills add felinto-dev/felinto-skills --all
```

Instalar usando a URL completa:

```bash
npx skills add https://github.com/felinto-dev/felinto-skills --skill caprover-one-click-app
```

## Estrutura Do Repositorio

As skills ficam em `.agents/skills/<nome-da-skill>/SKILL.md`.

Cada `SKILL.md` deve incluir frontmatter YAML:

```markdown
---
name: skill-name
description: Descricao curta de quando o agente deve usar esta skill.
---
```

## Repositorio

https://github.com/felinto-dev/felinto-skills
