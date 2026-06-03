# Felinto Skills

Skills de agentes para uso com a CLI aberta `npx skills`.

## Skills Disponiveis

- `caprover-one-click-app` - Cria, converte, valida e melhora templates CapRover One-Click App.
- `dont-reinvent-the-wheel` - Pesquisa solucoes existentes antes de criar uma feature do zero.

## Instalar

```bash
npx skills add https://github.com/felinto-dev/felinto-skills
```

Depois escolha a skill que deseja instalar no menu interativo.

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
