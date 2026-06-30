---
name: actionable-reading
description: Transform user-provided reading material into practical, day-by-day application guides. Use when the user wants to turn a book chapter, PDF, article, notes, transcript, essay, course material, or pasted text into actionable lessons, checklists, exercises, habits, reflection prompts, or a practical plan for applying the material in daily life. Especially useful for prompts like "transform this PDF into a practical guide", "turn chapter 1 into actions", "help me apply this book", "extract lessons I can practice", or "make an actionable reading guide".
---

# Actionable Reading

Turn supplied reading material into a practical guide the user can apply in real life.

## Core Principle

Prefer application over summary. Give enough context to understand the material, then convert the ideas into small concrete actions.

Do not invent content from a title, author, or chapter reference alone. If the material is not available in the conversation, a local file, or an accessible URL, ask the user to provide it.

## Input Handling

1. Identify the source type: pasted text, local file, PDF, EPUB/text export, notes, URL, transcript, or multiple materials.
2. If a local path is provided, inspect the file before answering.
3. If the user gives only a book title, chapter number, or vague reference, ask for the text, file, notes, or accessible source.
4. If the user asks for a specific chapter/section, focus only on that part when available.
5. If the material is long, process it in sections and keep a compact outline of key ideas before creating the guide.
6. If extraction fails or pages are unreadable, tell the user what failed and ask for a text export, OCR version, or clearer file.

## PDF Handling

PDFs are common for this skill. Treat PDF reading as a workflow, not a guess.

1. First inspect whether the environment has a direct PDF-reading tool or a document/PDF skill available. Use it when available.
2. If no dedicated tool is available, try local text extraction tools such as `pdftotext`, `python` libraries already installed, or platform tools. Do not install packages unless the user approves or the environment allows it.
3. Preserve page references when possible. Mention page numbers for important principles, exercises, or quotes.
4. Detect scanned PDFs. If extracted text is empty, garbled, or mostly whitespace, explain that OCR is needed.
5. Avoid over-reading irrelevant pages. For a chapter request, use table of contents, page ranges, headings, bookmarks, or user-provided page numbers to isolate the target section.
6. For copyrighted books, do not reproduce long passages. Summarize, paraphrase, and quote only short excerpts when useful.
7. For huge PDFs, create a staged approach: extract table of contents, confirm chapter/page range if unclear, then process the target section.

## Default Output

Unless the user specifies a different format, produce:

1. `Resumo rapido` - 5-10 bullets with the core message of the material.
2. `Ideias aplicaveis` - 3-7 principles that can change behavior.
3. `Guia pratico de 7 dias` - daily checklist with 1-3 small actions per day.
4. `Como usar` - brief instructions for executing the checklist.
5. `Revisao final` - reflection questions and a way to decide what to keep practicing.

Default plan length: 7 days.

Default daily workload: 1-3 actions, each doable in about 5-20 minutes.

Default tone: practical, clear, encouraging, grounded, and non-hype. Avoid generic motivational filler.

## Action Design

For every action, include:

- `Tarefa` - what to do.
- `Proposito` - why this action matters according to the material.
- `Como aplicar` - a concrete way to do it today.

Make actions behavioral and observable. Prefer "write down 3 current worries and classify each as controllable or not" over "be less worried".

Tie each action back to a specific idea from the material. If the connection is weak, remove the action.

## Adaptation Rules

Adapt the guide when the user provides constraints:

- Time horizon: 3, 7, 14, 21, or 30 days.
- Intensity: light, normal, deep.
- Context: work, study, relationships, health, leadership, creativity, spirituality, parenting, business.
- Output style: checklist, Notion-ready table, habit tracker, coaching plan, workshop, study guide, or journaling prompts.
- Language: match the user's language unless they ask otherwise.

If the user does not choose a tone, use the default tone. If tone matters for the material, briefly adapt it: reflective for philosophy, direct for business, gentle for self-help, precise for technical material.

## Quality Bar

A good actionable-reading result should:

- Help someone benefit from the material even before reading it fully.
- Preserve the author's core ideas without pretending to be a substitute for the original work.
- Convert abstract advice into concrete next steps.
- Be realistic enough that the user can start today.
- Avoid filler, vague lessons, and ungrounded claims.

## Example Shape

```markdown
## Resumo rapido
- ...

## Ideias aplicaveis
1. ...

## Guia pratico de 7 dias

### Dia 1 - Nome do foco
- [ ] Tarefa: ...
  Proposito: ...
  Como aplicar: ...

### Dia 2 - Nome do foco
- [ ] Tarefa: ...
  Proposito: ...
  Como aplicar: ...

## Como usar
- Reserve 10-20 minutos por dia.
- Marque apenas o que realmente fez.
- Anote uma evidencia concreta de aplicacao.

## Revisao final
- O que mudou no seu comportamento?
- Qual pratica vale manter por mais 7 dias?
```
