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
4. Treat this as a format conversion task, not a rewriting task. Convert the complete source text unless the user explicitly requests only part of it.
5. Preserve the source content verbatim except for Markdown/chat formatting. Do not add, remove, summarize, condense, translate, paraphrase, reorder, or change words.
6. Apply formatting only where it improves destination readability or preserves the original structure/emphasis.
7. Remove or convert unsupported Markdown constructs such as headings, tables, embedded links, HTML, and advanced layout without changing the underlying words. Keep task-list markers like `[ ]` as plain text.
8. Return only the converted text inside an external code block delimited by exactly four backticks, regardless of platform. The outer fence is for LLM output safety, not chat-platform syntax, and prevents conflicts with internal triple-backtick code blocks.

## Source Fidelity Check

Before producing the converted text:

1. Identify the exact source span: usually the assistant's immediately previous answer, including all sections and bullets.
2. Mentally count the major blocks such as headings, sections, numbered items, bullet groups, checklists, code blocks, and warnings.
3. After conversion, verify those same blocks are still present in the same order.
4. If the source is too long to safely convert in one answer, say so and ask whether to split it. Do not silently shorten it.

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
- Put a blank line after bold section titles and between major sections so WhatsApp remains readable on mobile.
- Keep list items as short visible lines, preserving every item and its words.
- Convert Markdown bold from `**text**` or `__text__` to `*text*`.
- Keep italic as `_text_` when useful.
- Convert links from `[label](url)` to `label: url`, or keep only the visible label when the URL adds no value.
- Convert tables into simple bullet lists or short labeled lines without dropping cells.
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
- Put a blank line after bold section titles and between major sections so Telegram remains readable on mobile.
- Keep list items as short visible lines, preserving every item and its words.
- Use `**text**` for bold. Do not use single asterisks for Telegram bold.
- Keep italic as `_text_` unless underline is intended with `__text__`.
- Convert links from `[label](url)` to `label: url`, or keep only the visible label when the URL adds no value.
- Convert tables into simple bullet lists or short labeled lines without dropping cells.
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

- Preserve all content from the source: every section, bullet, numbered item, checklist item, quote, warning, and example.
- Keep the original words. Formatting changes may add/remove Markdown markers and spacing only.
- Keep line breaks and list order close to the original, adding blank lines when the chat destination needs breathing room.
- Keep technical identifiers, file paths, commands, URLs, errors, and code exact.
- Do not introduce unsupported headings (`#`, `##`, `###`) in the converted result.
- Do not use Markdown tables.
- Do not use embedded links.
- Do not include explanations before or after the fenced result unless the user explicitly asks for commentary.

## Anti-Patterns

Avoid these mistakes:

- Turning the source into a shorter summary.
- Combining multiple bullets into one compressed bullet.
- Dropping examples, caveats, or final review questions.
- Rewriting sentences to sound better.
- Changing the amount of detail because the destination is WhatsApp or Telegram. Readability comes from spacing and supported formatting, not shortening.

## Trigger Examples

- `formate a ultima resposta para whatsapp`
- `transforma isso em markdown do Telegram`
- `manda a resposta anterior pronta pra colar no zap`
- `converter para markdown compatível com telegram`
- `/chat-markdown whatsapp`
- `/chat-markdown telegram`
