---
name: dont-reinvent-the-wheel
description: Research existing products, open-source projects, commercial script marketplaces such as CodeCanyon/Envato Market, SaaS tools, SDK features, APIs, webhooks, embeds, OAuth/SSO options, and integration paths before building a new app feature or stack from scratch. Use when the user asks to avoid reinventing the wheel, audit an existing app for custom code that could be replaced by native stack features or maintained tools, validate a feature idea, choose between building or integrating, find alternatives for a product workflow, run an interactive discovery interview or plan stress-test before deciding, analyze whether an app feature can be replaced by an existing solution, compare open-source/self-hosted/paid marketplace/SaaS options, or discover simpler native options in an existing stack.
---

# Dont Reinvent The Wheel

Find existing solutions that can replace, simplify, or accelerate a requested feature before recommending custom implementation.

## Existing App Audit Mode

When invoked in an existing application without a specific feature request, audit the current codebase before researching external options:

1. Identify the stack from manifests, lockfiles, Docker/Compose files, framework config, routes, DB/schema files, auth config, job/queue config, CI files, and package imports.
2. Build a short inventory of custom-built capabilities, especially auth, billing, admin panels, CMS/content, search, file uploads, email/notifications, analytics, feature flags, chat, queues, scheduling, observability, permissions, CRUD scaffolding, webhooks, integrations, import/export, and AI tooling.
3. Classify each capability:
   - `Keep custom`: domain-specific, core differentiator, or already simpler than an integration.
   - `Check native`: likely covered by the current framework, SDK, platform, DB, hosting provider, or existing dependency.
   - `Research replacement`: table-stakes feature with meaningful maintenance/security/ops cost.
   - `Defer`: too little evidence or too risky to recommend without product constraints.
4. Research only the highest-leverage `Check native` and `Research replacement` items first. Prefer current-stack/native solutions before adding new services or dependencies.
5. Produce an organized opportunity report, not a rewrite plan by default. Include evidence from the repo, candidate replacements, confidence, migration effort, risk, and a smallest useful proof of concept.

Do not suggest replacing every custom component. Prioritize places where existing solutions reduce ongoing maintenance, security exposure, operational burden, or non-differentiating product work.

## Default Bias

Prefer solutions in this order unless the user gives different constraints:

1. Native capability already available in the user's current stack or SDK.
2. Mature open-source or self-hostable tool with API/webhook/embed support.
3. Commercial source-available or self-hosted script from a reputable marketplace, when docs/demo/license/support fit the project and integration surfaces are verifiable.
4. Paid SaaS with clear cost/benefit and low integration risk.
5. Hybrid approach: integrate an existing core and build only the missing differentiator.
6. Custom build only when existing options fail key requirements or create worse risk.

## Interaction Mode Selection

Choose the lightest mode that can produce a defensible recommendation.

Use `Quick Check Mode` when the request is narrow, reversible, low-cost, or asks for a quick comparison. Ask at most 1 clarifying question; otherwise proceed with explicit assumptions.

Use `Standard Research Mode` when the request affects architecture, dependencies, paid tools, auth, data ownership, migration, or user-facing workflows. Ask up to 3 concise questions only when the answers materially change the recommendation.

Use `Interactive Discovery Interview Mode` when:

- The user explicitly asks for an interview, grilling, stress-test, requirements discovery, or decision-tree walkthrough.
- The plan is vague but high-impact.
- Multiple decisions depend on each other.
- Choosing wrong would create significant migration, cost, lock-in, security, compliance, or product risk.
- A defensible recommendation requires understanding user/workflow/data/auth/integration constraints first.

Do not force an interview for simple requests. If a quick answer is enough, answer quickly and mention assumptions.

## Interactive Discovery Interview Mode

When selected, interview the user before producing a recommendation.

Interview goal: reach shared understanding of the job-to-be-done, constraints, decision dependencies, and option space before recommending build/buy/reuse paths.

Use this loop:

1. Build a concise decision tree from the current plan or feature idea.
2. Ask one high-leverage question at a time.
3. For each question, include why the decision matters, 2-4 concrete answer options when useful, a clearly labeled recommended answer with short rationale, and downstream decisions that depend on it.
4. After the user answers, restate the locked-in decision and any assumptions.
5. Browse the web before the next question when current options, pricing, APIs, marketplace scripts, native stack features, or implementation tradeoffs could change the next decision.
6. Use the research to choose the next dependent question.
7. Walk down the decision tree in dependency order: user/workflow, current stack, data ownership, auth/SSO, integration surface, hosting/deploy, budget, compliance/security, migration/export, operational ownership, and success criteria.
8. Stop interviewing when remaining uncertainty no longer changes the recommendation materially.
9. Finish with the normal Recommendation Shape and source links.

Press on vague answers and hidden tradeoffs, but avoid philosophical questions. If the user asks to move faster, batch the next 3 most important questions and state what uncertainty remains.

## Discovery Workflow

1. Clarify the job-to-be-done: user, workflow, must-have behavior, success criteria, timeline, data ownership, auth, hosting preference, budget sensitivity, compliance needs, stakeholders, and integration surface.
2. If the user points to an existing repo or feature, inspect local code first to understand the real requirement before researching alternatives.
3. If the skill is invoked in an existing app with no specific command, run Existing App Audit Mode and choose 3-7 high-leverage opportunities before deep research.
4. Browse the web for current options. This skill depends on fresh information because projects, pricing, APIs, and maintenance status change.
5. Search across categories, not only exact wording. Use queries such as:
   - `<feature> open source self hosted`
   - `<feature> API webhook OAuth embed iframe`
   - `<feature> GitHub docker compose`
   - `<feature> CodeCanyon script REST API OAuth`
   - `<feature> Envato Market <stack> script API`
   - `<feature> alternatives`
   - `<current stack/tool> built in <feature>`
6. Run the Marketplace Pass Gate before final recommendations.
7. Prioritize primary sources: official docs, GitHub repos, Docker/package registries, changelogs, pricing pages, API docs, integration docs, marketplace item pages, live demos, author docs, comments/support signals, and license/support terms. Use lists/blogs only for discovery, then verify from primary sources.
8. Build a candidate set of 5-10 options, then shortlist 3-5 serious choices plus a custom-build baseline.
9. Validate candidates in two layers: first identify plausible alternatives, then verify fit against the user's actual workflow, codebase, constraints, and non-negotiable requirements.

In `Quick Check Mode`, ask at most 1 clarifying question.

In `Standard Research Mode`, ask at most 3 concise questions only when missing constraints would materially change the recommendation. If the user asks for speed, proceed with explicit assumptions.

In `Interactive Discovery Interview Mode`, ask one question at a time until the decision tree is resolved enough to make a defensible recommendation.

## Research Depth And Tool Preference

Before broad research, check the available tool and skill surface from the current session: installed skills, MCP tools/servers, web/deep-research tools, and subagent/task tools. If a suitable deep-research skill, research MCP, package/docs MCP, vendor-discovery tool, or subagent is available, prefer it for candidate discovery and source verification over manual shallow search.

Keep research depth reasonable by default:

- Use local repo inspection first when a codebase is involved.
- Use 2-4 targeted searches per major category, then stop expanding once 5-10 plausible candidates are found.
- Add a marketplace-script pass only when buying/self-hosting source would plausibly beat OSS/SaaS/custom build for speed, ownership, or cost.
- Verify only the 3-5 strongest candidates deeply from primary sources.
- Go deeper only for high-cost, high-risk, regulated, security-sensitive, or explicitly requested decisions.
- For simple native-stack checks, avoid heavyweight deep research.

## Marketplace Pass Gate

Before a final recommendation, explicitly record one of these outcomes:

- `Marketplace checked`: search CodeCanyon/Envato Market at least once, include 2-5 plausible marketplace candidates when any exist, and explain why each is accepted, rejected, or left unverified.
- `Marketplace skipped`: state the concrete reason marketplace scripts do not fit this task.

Do not skip the marketplace pass for self-hosted apps, admin panels, AI chat/workflow tools, SaaS starters, dashboards, CRM, LMS, booking, ecommerce, chatbot widgets, automation modules, payment/subscription integrations, or any reusable app module that could plausibly be bought with source.

When the marketplace pass is required but expected to be low-fit, still do a quick pass. A likely rejection is not a reason to skip verification.

When subagents are available, delegate bounded research tasks to preserve main-session context. Ask for a compact structured result only: candidates, primary source links, evidence for APIs/webhooks/auth/export, maintenance/maturity signals, pricing/license notes, confidence, and unresolved gaps. Do not pass unnecessary conversation history, and do not import long raw outputs into the main context; summarize and cite the facts needed for the decision.

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

For marketplace scripts such as CodeCanyon/Envato Market items, also evaluate:

- Integration proof: documented REST/GraphQL API, webhooks, OAuth/OIDC/social login/SSO, embed/admin surfaces, or clear extension points; do not infer these from screenshots.
- Source and customization: source access, framework/version, dependency age, install path, update workflow, ability to fork/patch, and whether critical code is encrypted or vendor-locked.
- Marketplace health: last update date, changelog quality, author history, sales/reviews/comments trend, support status, live demo quality, docs depth, and responsiveness to security/compatibility issues.
- License and usage fit: regular/extended license fit, SaaS/client/multi-tenant redistribution limits, update/support duration, renewal cost, and whether the user's intended deployment is allowed.
- Security posture: auth implementation, password/session handling, tenant isolation, file upload safety, dependency vulnerabilities, payment handling, and data export/backup path.

Treat marketplace scripts as serious candidates, not automatic winners. They are strongest when the user wants a common self-hosted workflow quickly and accepts source-level ownership. Downgrade or reject them when APIs/auth/export are unverified, the framework is stale, customization would be extensive, the license is unclear, or support/updates look weak.

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

When the solution would run as a separate hosted system, or when the user needs to integrate it into an existing app, give extra attention to standard integration surfaces. Prefer candidates with documented REST or GraphQL APIs, outbound webhooks, standards-based auth such as OAuth 2.0 or OpenID Connect, embeddable UI when useful, admin/provisioning automation, and reliable export paths. Highlight these strengths or gaps in the rationale, risks, or integration sketch where they materially affect the decision; do not force them into every comparison table when they are irrelevant.

## Replacement Safety

When replacing custom code, a current dependency, or a planned custom feature with an existing solution:

- Map the current or proposed behavior to the candidate's API, workflow, or configuration surface.
- Preserve public interfaces unless the user explicitly accepts a migration.
- Prefer native platform APIs and existing project dependencies before adding new packages or SaaS.
- Skip replacement when feature parity is below roughly 80%, when the code is domain-specific business logic, or when the remaining glue code is more complex than the original.
- Check maintenance activity, license compatibility, security advisories, export paths, and deprecation history before recommending OSS, marketplace scripts, or SaaS.
- For marketplace scripts, inspect docs/demo/changelog/license/support before recommending purchase or integration; if purchase is required to verify a critical claim, label the option `MEDIUM` or lower until verified.
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
2. Comparison table with the strongest options and a custom-build baseline, including confidence, effort, cost model, lock-in, source/license type, and source quality.
3. Integration sketch for the recommended path: auth, data flow, API/webhook/embed points when relevant, hosting, migration/export, and code the app still needs.
4. Rejected alternatives and why they failed must-have, confidence, cost, lock-in, or operational requirements.
5. Marketplace pass: `checked` or `skipped`, with accepted/rejected marketplace candidates or the concrete skip reason.
6. Risks and tradeoffs that could change the decision.
7. Minimal proof-of-concept plan with 2-5 concrete steps and explicit keep/discard criteria.
8. Source links used for current facts.

Keep recommendations pragmatic. The goal is to save build time without outsourcing the product's core differentiator.

For Existing App Audit Mode, use this shape:

1. Stack detected: framework, runtime, DB, auth, hosting/deploy, key packages, and unknowns.
2. Opportunity table: current custom area, repo evidence, suggested native/tool alternative, confidence, effort (`S/M/L`), expected code removed or maintenance reduced, and risk.
3. Top recommendations: 1-3 items with the best effort-to-impact ratio.
4. Keep custom: areas that look domain-specific or cheaper to maintain as-is.
5. Research notes and source links for current external facts.
6. Minimal proof-of-concept plan for the first recommendation only.

## Failure Patterns To Avoid

- Starting with vendor demos or GitHub popularity before defining the need.
- Comparing tools without a custom-build baseline.
- Trusting search snippets, marketplace claims, live demos, reviews, or old blog posts without primary-source verification.
- Recommending OSS only because it exists, without checking maintenance, license, security, and API fit.
- Ignoring commercial script marketplaces when a table-stakes self-hosted app module could be bought with source and integrated faster.
- Recommending marketplace scripts without checking license fit, update/support posture, API/auth/export proof, framework age, and security implications.
- Recommending SaaS without checking data export, renewal/usage pricing, auth model, audit logs, and exit path.
- Replacing domain-specific product logic with a generic package that forces awkward workflows.
- Ignoring the cost of glue code, migrations, operational ownership, and support burden.
- Choosing the highest score when one low-scoring must-have should be disqualifying.

## Useful Patterns

For bookmark/directories/content curation, look for self-hosted knowledge-base, bookmarking, read-it-later, and AI-organized archive tools before proposing a custom directory.

For AI observability/tracing, check native SDK/platform tracing first, then compare specialized tools only if the user needs collaboration, long retention, evals, prompt management, cost analytics, or multi-provider traces.

For AI chat/admin/user-facing agent panels, look for mature chat UIs with API, user provisioning, OAuth/OIDC, model/provider routing, iframe/subdomain deployment, and admin controls before building chat UX from scratch.

For common business workflows such as CRM, LMS, bookings, classifieds, marketplaces, helpdesk, invoicing, directories, subscriptions, ecommerce add-ons, and admin dashboards, search CodeCanyon/Envato Market alongside GitHub and SaaS options, especially for Laravel, WordPress, Node, React, Flutter, and mobile app starters.
