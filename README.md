# Felinto Skills

Skills de agentes para uso com a CLI aberta `npx skills`.

## Skills Disponiveis

- `actionable-reading` - Transforma materiais de leitura fornecidos pelo usuario em guias praticos de aplicacao.
- `caprover-one-click-app` - Cria, converte, valida e melhora templates CapRover One-Click App.
- `chat-markdown` - Converte a ultima resposta da IA para Markdown compativel com WhatsApp ou Telegram.
- `dont-reinvent-the-wheel` - Pesquisa solucoes existentes antes de criar uma feature do zero.

## Instalar

```bash
npx skills add https://github.com/felinto-dev/felinto-skills
```

Depois escolha a skill que deseja instalar no menu interativo.

## Estrutura Do Repositorio

As skills ficam em `<nome-da-skill>/SKILL.md`, no diretorio principal do repositorio.

Arquivos auxiliares especificos de uma skill devem ficar dentro do proprio diretorio da skill. Templates CapRover ficam em `caprover-one-click-app/templates/`.

Cada `SKILL.md` deve incluir frontmatter YAML:

```markdown
---
name: skill-name
description: Descricao curta de quando o agente deve usar esta skill.
---
```

## Repositorio

https://github.com/felinto-dev/felinto-skills
