---
name: dont-reinvent-the-wheel
description: Research existing products, open-source projects, SaaS tools, SDK features, APIs, webhooks, embeds, OAuth/SSO options, and integration paths before building a new app feature or stack from scratch. Use when the user asks to avoid reinventing the wheel, validate a feature idea, choose between building or integrating, find alternatives for a product workflow, analyze whether an app feature can be replaced by an existing solution, compare open-source/self-hosted versus paid tools, or discover simpler native options in an existing stack.
---

# Dont Reinvent The Wheel

Find existing solutions that can replace, simplify, or accelerate a requested feature before recommending custom implementation.

## Default Bias

Prefer solutions in this order unless the user gives different constraints:

1. Native capability already available in the user's current stack or SDK.
2. Mature open-source or self-hostable tool with API/webhook/embed support.
3. Paid SaaS with clear cost/benefit and low integration risk.
4. Hybrid approach: integrate an existing core and build only the missing differentiator.
5. Custom build only when existing options fail key requirements or create worse risk.

## Discovery Workflow

1. Clarify the job-to-be-done: user, workflow, must-have behavior, data ownership, auth, hosting preference, budget sensitivity, and integration surface.
2. If the user points to an existing repo or feature, inspect local code first to understand the real requirement before researching alternatives.
3. Browse the web for current options. This skill depends on fresh information because projects, pricing, APIs, and maintenance status change.
4. Search across categories, not only exact wording. Use queries such as:
   - `<feature> open source self hosted`
   - `<feature> API webhook OAuth embed iframe`
   - `<feature> GitHub docker compose`
   - `<feature> alternatives`
   - `<current stack/tool> built in <feature>`
5. Prioritize primary sources: official docs, GitHub repos, Docker/package registries, changelogs, pricing pages, API docs, and integration docs. Use lists/blogs only for discovery, then verify from primary sources.
6. Build a candidate set of 5-10 options, then shortlist 3-5 serious choices plus a custom-build baseline.

Ask at most 3 concise questions only when missing constraints would materially change the recommendation. If the user asks for speed, proceed with explicit assumptions.

## Evaluation Criteria

Evaluate each serious candidate on:

- Functional fit: how directly it solves the user's workflow.
- Integration: API, webhooks, SDKs, iframe/embed, OAuth/OIDC/SAML, SCIM, export/import, admin automation.
- Ownership: open source, source-available, paid SaaS, license, data portability, lock-in.
- Operations: Docker/Compose availability, deployment complexity, backups, upgrades, scaling, observability.
- Product maturity: recent releases, active maintainers, docs quality, community, issue velocity, ecosystem.
- Cost/benefit: free tier, self-host cost, paid plans, per-seat/per-usage traps, enterprise-only features.
- Security/privacy: auth model, secret handling, tenant isolation, audit logs, compliance needs when relevant.
- Build avoidance: what custom code remains after integration, and whether that code is worth building.

Do not invent support for APIs, webhooks, OAuth, pricing, or licenses. If source evidence is weak, label it as unverified.

## Recommendation Shape

Put the decision first:

- `Use <tool>` when one option is clearly best.
- `Prototype <tool A> and <tool B>` when uncertainty is mostly practical fit.
- `Use native <stack feature>` when the user's stack already solves it simply.
- `Build custom` only after explaining why existing options are not suitable.

Then include:

1. Short rationale in plain language.
2. Comparison table with the strongest options and a custom-build baseline.
3. Integration sketch for the recommended path: auth, data flow, API/webhook/embed points, hosting, and code the app still needs.
4. Risks and tradeoffs that could change the decision.
5. Minimal proof-of-concept plan with 2-5 concrete steps.
6. Source links used for current facts.

Keep recommendations pragmatic. The goal is to save build time without outsourcing the product's core differentiator.

## Useful Patterns

For bookmark/directories/content curation, look for self-hosted knowledge-base, bookmarking, read-it-later, and AI-organized archive tools before proposing a custom directory.

For AI observability/tracing, check native SDK/platform tracing first, then compare specialized tools only if the user needs collaboration, long retention, evals, prompt management, cost analytics, or multi-provider traces.

For AI chat/admin/user-facing agent panels, look for mature chat UIs with API, user provisioning, OAuth/OIDC, model/provider routing, iframe/subdomain deployment, and admin controls before building chat UX from scratch.
