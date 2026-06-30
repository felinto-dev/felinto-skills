---
name: actionable-reading
description: Transform user-provided reading material into practical application guides. Use when the user wants to turn a book chapter, PDF, article, notes, transcript, essay, course material, or pasted text into actionable lessons, checklists, exercises, habits, reflection prompts, or a practical plan for applying the material in daily life. Distinguish practical material from theoretical material, extract rules, principles, and concepts, then help the user apply them without inventing content. Especially useful for prompts like "transform this PDF into a practical guide", "turn chapter 1 into actions", "help me apply this book", "extract lessons I can practice", or "make an actionable reading guide".
---

# Actionable Reading

Turn supplied reading material into a practical guide the user can apply in real life.

## Core Principle

Prefer application over summary. Give enough context to understand the material, then convert the ideas into small concrete actions or concept-integration exercises.

Do not invent content from a title, author, or chapter reference alone. If the material is not available in the conversation, a local file, or an accessible URL, ask the user to provide it.

Do not force a fixed plan length. Choose the smallest useful guide structure for the material. Maximum plan length is 5 days unless the user explicitly asks for a non-day format. Never pad the guide just to reach the maximum.

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

## Reading Type Classification

Before designing the guide, classify the material:

- `Practical` - gives rules, methods, steps, habits, principles, frameworks, or advice meant to be tested in life.
- `Theoretical` - builds concepts, history, interpretation, philosophy, literature, or analysis without an immediate step-by-step method.
- `Mixed` - contains both conceptual explanation and concrete practices.

This classification changes the output. Practical material should become tests and actions. Theoretical material should become concept clarification, connections, observation prompts, and decision lenses. Mixed material should do both.

## Practical Material Workflow

For practical material, extract and translate what the author gives:

1. Identify each `Regra` or `Principio`.
2. If the material gives a rule, infer the underlying principle because rules are often tied to the author's context.
3. For each item, answer:
   - `O que e` - the rule or principle in plain language.
   - `Por que existe` - the purpose or problem it solves.
   - `Como aplicar` - the concrete behavior.
   - `Quando aplicar` - the trigger, moment, or situation.
   - `Onde aplicar` - work, home, study, relationships, health, etc.
4. Turn the strongest items into a short experiment the user can run.
5. Include a result check: what evidence would show that the idea helped?

Example pattern:

```text
Regra especifica -> Principio transferivel -> Acao no contexto do usuario -> Evidencia de resultado
```

## Theoretical Material Workflow

For theoretical material, do not fake immediate productivity actions. The goal is to enrich the user's concepts so future decisions improve.

1. Extract the main concepts, distinctions, tensions, and examples.
2. Explain how each concept changes the user's way of seeing reality.
3. Connect new concepts to nearby concepts the user may already know.
4. Create reflection or observation exercises instead of forced tasks.
5. Add prompts for applying the concept to real decisions, conversations, or interpretations.

Useful exercise types:

- `Mapa conceitual` - concept, definition, contrast, example, non-example.
- `Observacao guiada` - where to notice the concept in daily life.
- `Lente de decisao` - how this idea changes a choice.
- `Pergunta de integracao` - how this connects to what the user already knows.

## Default Output

Unless the user specifies a different format, produce:

1. `Tipo de leitura` - practical, theoretical, or mixed, with one sentence explaining why.
2. `Resumo rapido` - 5-10 bullets with the core message of the material.
3. `Ideias aplicaveis` - 3-7 rules, principles, or concepts worth using.
4. `Guia pratico` - checklist, experiment, or concept-integration plan. Use days only when a day-by-day plan fits the material.
5. `Como usar` - brief instructions for executing the guide.
6. `Revisao final` - reflection questions and a way to decide what to keep practicing or studying.

Plan length rule: if the user does not specify a time horizon, infer the shortest useful structure from the material. Use a single session, 2 days, 3 days, 4 days, or 5 days as appropriate. The hard maximum is 5 days. If the user asks for more than 5 days, provide the first 5-day cycle and suggest repeating or requesting the next cycle later.

Default daily workload: 1-3 actions, each doable in about 5-20 minutes.

Default tone: practical, clear, encouraging, grounded, and non-hype. Avoid generic motivational filler.

## Action Design

For every action, include:

- `Tarefa` - what to do.
- `Proposito` - why this action matters according to the material.
- `Como aplicar` - a concrete way to do it today.

When useful, also include:

- `Quando` - trigger or schedule.
- `Onde` - context where the action belongs.
- `Evidencia` - what result or observation to look for.

Make actions behavioral and observable. Prefer "write down 3 current worries and classify each as controllable or not" over "be less worried".

Tie each action back to a specific idea from the material. If the connection is weak, remove the action.

## Adaptation Rules

Adapt the guide when the user provides constraints:

- Time horizon: respect the requested horizon up to the 5-day maximum; otherwise compress into a 5-day cycle.
- Intensity: light, normal, deep.
- Context: work, study, relationships, health, leadership, creativity, spirituality, parenting, business.
- Output style: checklist, Notion-ready table, habit tracker, coaching plan, workshop, study guide, or journaling prompts.
- Language: match the user's language unless they ask otherwise.

If the user does not choose a tone, use the default tone. If tone matters for the material, briefly adapt it: reflective for philosophy, direct for business, gentle for self-help, precise for technical material.

## Quality Bar

A good actionable-reading result should:

- Help someone benefit from the material even before reading it fully.
- Preserve the author's core ideas without pretending to be a substitute for the original work.
- Convert practical advice into concrete tests.
- Convert theoretical material into clearer concepts and better questions.
- Be realistic enough that the user can start today.
- Avoid filler, vague lessons, and ungrounded claims.

## Example Shape

```markdown
## Tipo de leitura
Practical/Mixed/Theoretical - ...

## Resumo rapido
- ...

## Ideias aplicaveis
1. Regra/principio/conceito: ...
   Por que existe: ...

## Guia pratico

### Sessao ou Dia 1 - Nome do foco
- [ ] Tarefa: ...
  Proposito: ...
  Como aplicar: ...
  Evidencia: ...

### Dia 2 - Nome do foco
- [ ] Tarefa: ...
  Proposito: ...
  Como aplicar: ...

## Como usar
- Reserve 10-20 minutos quando houver tarefa pratica.
- Marque apenas o que realmente fez ou observou.
- Anote uma evidencia concreta de aplicacao ou compreensao.

## Revisao final
- O que mudou no seu comportamento ou entendimento?
- Qual principio, regra, ou conceito vale manter ativo?
```
