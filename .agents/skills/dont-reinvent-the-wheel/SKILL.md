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

1. Clarify the job-to-be-done: user, workflow, must-have behavior, success criteria, timeline, data ownership, auth, hosting preference, budget sensitivity, compliance needs, stakeholders, and integration surface.
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
7. Validate candidates in two layers: first identify plausible alternatives, then verify fit against the user's actual workflow, codebase, constraints, and non-negotiable requirements.

Ask at most 3 concise questions only when missing constraints would materially change the recommendation. If the user asks for speed, proceed with explicit assumptions.

## Build Versus Buy Gate

Recommend building only when at least one strong reason applies:

- The workflow is core product differentiation.
- Existing tools fail must-have requirements or create unacceptable lock-in, security, privacy, latency, or cost risk.
- The economics work better at the user's expected scale.
- The team can own maintenance, support, security updates, and migrations.
- A hybrid approach would leave too much fragile glue code.

Recommend buying, integrating, or reusing when the need is table stakes, existing tools cover most requirements, specialized vendors or OSS projects maintain the hard parts better, or the team should focus on product-specific work.

## Confidence Gates

Classify each serious candidate before recommending it:

- `HIGH`: verified primary sources, clear feature fit, usable integration path, active maintenance, acceptable license/cost/security posture.
- `MEDIUM`: likely fit, but one or two material unknowns remain such as pricing edge cases, migration effort, missing trial evidence, or unclear API depth.
- `LOW`: weak source evidence, stale project, unclear feature parity, risky lock-in, unsupported auth/export, or major unknowns.

Do not recommend a `LOW` confidence option as the primary path. Keep it in risks or rejected alternatives if relevant.

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

When a weighted scorecard helps, use this default weighting and adjust it only when the user's context demands it:

| Criterion | Weight |
| --- | ---: |
| Functional fit | 35% |
| Integration and technical fit | 20% |
| Operations and maintainability | 10% |
| Security, privacy, and compliance | 10% |
| Maturity and vendor/project health | 10% |
| Cost and pricing model | 10% |
| Lock-in and exit path | 5% |

Score finalists from 1-5 per criterion. Treat the score as decision support, not the decision itself; call out any criterion that should override the total.

Do not invent support for APIs, webhooks, OAuth, pricing, or licenses. If source evidence is weak, label it as unverified.

## Replacement Safety

When replacing custom code, a current dependency, or a planned custom feature with an existing solution:

- Map the current or proposed behavior to the candidate's API, workflow, or configuration surface.
- Preserve public interfaces unless the user explicitly accepts a migration.
- Prefer native platform APIs and existing project dependencies before adding new packages or SaaS.
- Skip replacement when feature parity is below roughly 80%, when the code is domain-specific business logic, or when the remaining glue code is more complex than the original.
- Check maintenance activity, license compatibility, security advisories, export paths, and deprecation history before recommending OSS or SaaS.
- Estimate effort realistically: `S` under 1 hour, `M` 1-4 hours, `L` over 4 hours.

For implementation-oriented requests, propose an atomic proof of concept:

1. Integrate or trial one candidate only.
2. Rewrite the smallest representative workflow.
3. Verify with tests, demo flow, or real sample data.
4. Keep the candidate only if verification passes; otherwise discard and restore the prior path.
5. Repeat for the next candidate only when needed.

## Recommendation Shape

Put the decision first:

- `Use <tool>` when one option is clearly best.
- `Prototype <tool A> and <tool B>` when uncertainty is mostly practical fit.
- `Use native <stack feature>` when the user's stack already solves it simply.
- `Build custom` only after explaining why existing options are not suitable.

Then include:

1. Short rationale in plain language.
2. Comparison table with the strongest options and a custom-build baseline, including confidence, effort, cost model, lock-in, and source quality.
3. Integration sketch for the recommended path: auth, data flow, API/webhook/embed points, hosting, migration/export, and code the app still needs.
4. Rejected alternatives and why they failed must-have, confidence, cost, lock-in, or operational requirements.
5. Risks and tradeoffs that could change the decision.
6. Minimal proof-of-concept plan with 2-5 concrete steps and explicit keep/discard criteria.
7. Source links used for current facts.

Keep recommendations pragmatic. The goal is to save build time without outsourcing the product's core differentiator.

## Failure Patterns To Avoid

- Starting with vendor demos or GitHub popularity before defining the need.
- Comparing tools without a custom-build baseline.
- Trusting search snippets, marketplace claims, or old blog posts without primary-source verification.
- Recommending OSS only because it exists, without checking maintenance, license, security, and API fit.
- Recommending SaaS without checking data export, renewal/usage pricing, auth model, audit logs, and exit path.
- Replacing domain-specific product logic with a generic package that forces awkward workflows.
- Ignoring the cost of glue code, migrations, operational ownership, and support burden.
- Choosing the highest score when one low-scoring must-have should be disqualifying.

## Useful Patterns

For bookmark/directories/content curation, look for self-hosted knowledge-base, bookmarking, read-it-later, and AI-organized archive tools before proposing a custom directory.

For AI observability/tracing, check native SDK/platform tracing first, then compare specialized tools only if the user needs collaboration, long retention, evals, prompt management, cost analytics, or multi-provider traces.

For AI chat/admin/user-facing agent panels, look for mature chat UIs with API, user provisioning, OAuth/OIDC, model/provider routing, iframe/subdomain deployment, and admin controls before building chat UX from scratch.
