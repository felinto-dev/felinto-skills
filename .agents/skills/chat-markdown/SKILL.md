---
name: chat-markdown
description: Convert the assistant's previous response, or user-provided text, into copy-ready Markdown supported by WhatsApp or Telegram. Use when the user asks to format the last answer for WhatsApp, Telegram, chat, messenger, copy/paste, or platform-compatible markdown. Preserve the original content while replacing unsupported Markdown with the chosen chat app syntax.
---

# Chat Markdown

Convert an existing answer into Markdown that can be pasted into WhatsApp or Telegram.

## Core Behavior

1. Determine the target platform from the user's request: `whatsapp` or `telegram`.
2. Use the assistant's immediately previous answer as the source text unless the user provides different text explicitly.
3. If the target platform is missing or ambiguous, ask the user to choose WhatsApp or Telegram.
4. Preserve the source content as faithfully as possible. Do not add, remove, summarize, translate, or change meaning.
5. Apply formatting only where it improves readability or preserves the original emphasis.
6. Remove or convert unsupported Markdown constructs such as headings, tables, embedded links, task-list checkboxes, HTML, and advanced layout.
7. Return only the converted text inside an external code block delimited by exactly four backticks, regardless of platform. The outer fence is for LLM output safety, not chat-platform syntax, and prevents conflicts with internal triple-backtick code blocks.

## WhatsApp Output

Use only WhatsApp-supported Markdown:

- Bold: `*text*`
- Italic: `_text_`
- Inline code: `` `text` ``
- Code block: triple backticks
- Unordered lists: `- item`
- Ordered lists: `1. item`
- Quote blocks: `> text`

Formatting rules:

- Convert Markdown headings into bold plain lines, for example `## Title` becomes `*Title*`.
- Convert Markdown bold from `**text**` or `__text__` to `*text*`.
- Keep italic as `_text_` when useful.
- Convert links from `[label](url)` to `label: url`, or keep only the visible label when the URL adds no value.
- Convert tables into simple bullet lists or short labeled lines.
- Preserve internal code blocks with exactly three backticks.

Wrap the final WhatsApp result in an external code block delimited by exactly four backticks:

`````markdown
````
*Title*
- item
```
internal code block, if present
```
````
`````

## Telegram Output

Use only Telegram-supported Markdown:

- Bold: `**text**`
- Italic: `_text_` or `__text__`
- Inline code: `` `text` ``
- Internal code block: exactly three backticks
- Strikethrough: `~text~` when needed
- Underline: `__text__` when it fits the context
- Spoiler: `||text||` only when content should be hidden
- Unordered lists: `- item`
- Ordered lists: `1. item`
- Quote blocks: `> text`

Formatting rules:

- Convert Markdown headings into bold plain lines, for example `## Title` becomes `**Title**`.
- Use `**text**` for bold. Do not use single asterisks for Telegram bold.
- Keep italic as `_text_` unless underline is intended with `__text__`.
- Convert links from `[label](url)` to `label: url`, or keep only the visible label when the URL adds no value.
- Convert tables into simple bullet lists or short labeled lines.
- Preserve internal code blocks with exactly three backticks.

Wrap the final Telegram result in an external code block delimited by exactly four backticks. This follows the skill-wide output rule and avoids conflicting with internal triple-backtick code blocks:

`````markdown
````
**Title**
- item
```
code block inside the content
```
````
`````

## Output Fence Rule

Always wrap the final result in an external code block delimited by exactly four backticks, for every supported chat platform now or added later. Keep any code blocks inside the converted content delimited by exactly three backticks. Never use the same delimiter for the outer wrapper and inner code blocks.

## Fidelity Rules

- Keep line breaks and list order close to the original.
- Keep technical identifiers, file paths, commands, URLs, errors, and code exact.
- Do not introduce unsupported headings (`#`, `##`, `###`) in the converted result.
- Do not use Markdown tables.
- Do not use embedded links.
- Do not include explanations before or after the fenced result unless the user explicitly asks for commentary.

## Trigger Examples

- `formate a ultima resposta para whatsapp`
- `transforma isso em markdown do Telegram`
- `manda a resposta anterior pronta pra colar no zap`
- `converter para markdown compatível com telegram`
- `/chat-markdown whatsapp`
- `/chat-markdown telegram`
