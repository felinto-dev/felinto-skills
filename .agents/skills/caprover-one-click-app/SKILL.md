---
name: caprover-one-click-app
description: Create, convert, validate, and improve CapRover One-Click App templates and custom one-click app repositories. Use when the user mentions CapRover, one-click apps, one-click deploy, CapRover templates, app YAML files, converting docker-compose to CapRover, adding apps to a one-click repository, or preparing official-style CapRover app submissions with logos and validation.
---

# CapRover One-Click App Template Creator

Create CapRover One-Click App YAML files from scratch, from Docker Compose, or for a custom one-click app repository. Prefer the official repository patterns from `https://github.com/caprover/one-click-apps`.

## Format

Start every template with `captainVersion: 4`, then add `services:` and `caproverOneClickApp:`.

CapRover parses only these Docker Compose service fields:

- `image`
- `environment`
- `ports`
- `volumes`
- `depends_on`
- `hostname`
- `command`
- `cap_add`
- `restart` (used throughout the official repo; keep it when useful)

It ignores unsupported Compose fields. Remove or translate unsupported keys such as `networks`, `build`, `healthcheck`, `container_name`, `labels`, `expose`, `profiles`, `deploy`, and `extra_hosts`.

Each service may also include `caproverExtra`:

- `containerHttpPort`: HTTP port inside the container; default is `80`.
- `notExposeAsWebApp`: set to `'true'` for DBs, caches, workers, cron, exporters, and other non-public services.
- `websocketSupport`: set to `'true'` for realtime apps when known needed.
- `dockerfileLines`: list of Dockerfile lines. Use instead of `image`, not together with it. Use only when `command` cannot express the required startup.

## Variables

Variables use the `$$cap_` prefix and are replaced during install.

Built-ins:

- `$$cap_appname`
- `$$cap_root_domain`
- `$$cap_gen_random_hex(length)`

Custom variables in `caproverOneClickApp.variables` require `id` and `label`. Add `defaultValue`, `description`, and `validRegex` when useful. Without `validRegex`, fields are optional and may be empty.

Use these default patterns:

```yaml
- id: $$cap_app_version
  label: App Version
  defaultValue: '1.2.3'
  description: Check valid tags at https://hub.docker.com/r/org/app/tags
  validRegex: /^([^\s^\/])+$/
- id: $$cap_db_password
  label: Database Password
  defaultValue: $$cap_gen_random_hex(16)
  validRegex: /.{12,}/
- id: $$cap_enable_feature
  label: Enable Feature
  defaultValue: 'false'
  validRegex: /^(true|false)$/
```

For latest tags or versions, browse current official docs/package registries. Do not guess stale versions.

## Output Target

Decide the output target before creating or editing files. There are two common targets:

- Standalone template: a single YAML the user can paste into CapRover's **>> TEMPLATE <<** flow. This is the default when the user says only "create a template", "make a one-click template", or similar. Do not create repository folders for this target unless the user explicitly asks for a file path.
- One-click app repository entry: YAML under `public/v4/apps/<app-name>.yml`, usually with a logo under `public/v4/logos/`. Use this only when the user clearly says they are building or updating a CapRover one-click app repository, asks to add an app to this repo, references `public/v4/apps`, or the current repo is already unmistakably a one-click app repository and the request targets it.

If the target is ambiguous and writing files would require creating repository structure such as `public/v4/apps`, ask a short clarifying question before making directories. If the user wants speed and does not specify a repository target, provide a standalone YAML file or inline YAML instead of creating repository structure.

## Discovery And Setup Choice

Many apps have multiple valid deployment shapes. Do not assume the most complex setup is best. First identify likely options, then pick with the user.

When the request is underspecified, run a short interview before writing YAML. Ask only the questions that change the template:

- Goal: testing, personal use, small production, or serious production.
- Data: persistent storage required, expected size, backup needs.
- Dependencies: embedded SQLite/file DB, external managed DB, or bundled Postgres/MySQL/Redis.
- Scale: single web service only, or separate worker/queue/scheduler services.
- Networking: one public domain, extra public API/domain, HTTPS-only config, WebSocket support.
- Auth/secrets: admin account, SMTP, OAuth, object storage, license keys.
- Maintenance preference: simplest template, official recommended Compose, or production-like stack.
- Operations: whether the app requires post-install CLI commands, migrations, seed/admin creation, cron setup, or manual config in the container.

If there are clear alternatives, present 2-3 concise options before implementation:

- Simple: few services, easiest to operate, acceptable for tests/small deployments.
- Recommended: follows upstream docs, persistent data, common production defaults.
- Advanced: workers, queues, cache, object storage, SMTP/OAuth, extra public endpoints.

State the tradeoffs and ask the user to choose. If the user asks for speed or gives no preference after reasonable context, default to the Recommended option.

## Research Workflow

Research current upstream docs before finalizing a template unless the user explicitly provides complete current docs/Compose. Verify:

- Official Docker image and current stable tag.
- Official Compose or deployment docs.
- Required env vars and generated secret requirements.
- Ports, volumes, migrations, workers, queues, caches, and DB support.
- Post-deploy requirements such as HTTPS, WebSocket support, domain variables, SMTP, or first-user setup.
- Any required `docker exec`/container CLI commands, one-time initialization, upgrade commands, or recurring maintenance jobs.

If web/search tools and subagents are available and the user has allowed or requested delegation, delegate research to a subagent to preserve the main context. Keep the subagent task narrow, for example:

```text
Research the current official Docker/Compose deployment docs for <app>. Return: images/tags, services, ports, volumes, required env vars, optional production services, post-deploy steps, and source URLs. Do not write the CapRover template.
```

Use the subagent's summary as input, then implement the template locally. If subagents are unavailable, do the web research locally and cite source URLs in the final answer when relevant.


## Local Docker Testing

When Docker CLI is available, use it to test the template locally before finalizing. The goal is to catch bad images, wrong commands, missing required env vars, wrong ports, and startup failures before the user pastes the YAML into CapRover.

First check availability without assuming Docker works:

```bash
docker version
```

If Docker is unavailable, the daemon is not running, permissions are missing, or network pulls are blocked, do not block the template. State the exact limitation in the final answer and continue with static validation.

For each public service or critical worker, when practical:

- Pull or run the exact image tag selected for the template. Avoid validating only `latest` if the template defaults to a pinned version.
- Start a temporary container with the same command, required env vars, and a temporary host port mapped to `caproverExtra.containerHttpPort`.
- Use dummy generated secrets for local tests. Never print or hard-code real user secrets.
- Mount a temporary volume or directory when the template uses persistent data.
- Wait briefly, inspect logs, and confirm the process remains running or exits only for an expected documented reason.
- Probe the mapped HTTP port with `curl` or equivalent when the service should expose HTTP.
- For multi-service templates, prefer a temporary `docker compose` file that mirrors the CapRover services closely enough to test dependencies, internal hostnames, ports, env vars, and startup order. Use local Compose service names for the test file, but keep CapRover `srv-captain--...` hostnames in the final YAML.
- Clean up temporary containers, networks, volumes, and files created for testing.

Do not run destructive Docker cleanup such as broad `docker system prune`. Only remove resources created for the current test.

Acceptable local test commands include focused checks such as:

```bash
docker run --rm IMAGE:TAG command --help
docker run --rm -e KEY=value IMAGE:TAG command version
docker run --name TEMP_NAME -d -p 127.0.0.1:HOST_PORT:CONTAINER_PORT -e KEY=value IMAGE:TAG command
curl -fsS http://127.0.0.1:HOST_PORT/
docker logs TEMP_NAME
docker rm -f TEMP_NAME
```

If local Docker testing reveals that upstream docs are wrong, an image tag is missing, a command fails, or the app requires setup not expressible in CapRover YAML, fix the template or ask the user to choose a different deployment shape before presenting the final result.

In the final answer, include a compact validation note: Docker tested, partially tested, or not tested, plus the important commands or checks performed.

## Naming And Networking

Use official naming conventions:

- Main public service: `$$cap_appname`.
- Supporting services: `$$cap_appname-postgres`, `$$cap_appname-db`, `$$cap_appname-redis`, `$$cap_appname-worker`, etc.
- Volumes: prefix with `$$cap_appname-`, e.g. `$$cap_appname-postgres-data`.
- Internal service hostnames: use CapRover's runtime DNS form `srv-captain--$$cap_appname-postgres`, not the raw service key, inside env vars and connection strings.
- Public URL: use `http://$$cap_appname.$$cap_root_domain` by default. Use `https://...` only when the app's own config requires final HTTPS and instructions tell the user to enable HTTPS.

Example DB URL:

```yaml
DATABASE_URL: postgres://app:$$cap_db_password@srv-captain--$$cap_appname-postgres:5432/app
```

## Creation Workflow

1. Identify app image(s), official docs, required env vars, ports, volumes, workers, queues, DB/cache dependencies, and whether WebSocket support or HTTPS must be enabled after deploy.
2. Convert Compose carefully:
   - Keep only supported service fields plus `restart`.
   - Convert `build` to an image or `caproverExtra.dockerfileLines`.
   - Remove networks; CapRover handles internal service networking.
   - Replace hard-coded container names/hosts with `srv-captain--$$cap_appname-*`.
   - Move secrets, versions, domains, credentials, SMTP settings, admin users, and ports into `variables`.
3. Write YAML with 4-space indentation, unquoted service keys unless quoting is needed, and string env values for booleans/numbers where the app expects strings.
4. Set `caproverExtra.containerHttpPort` on the public HTTP service if it is not `80`.
5. Set `caproverExtra.notExposeAsWebApp: 'true'` on internal services and workers.
6. Add `caproverOneClickApp` metadata:
   - `displayName`
   - `isOfficial`: `true` only if all runtime images are official/trusted. Use `false` when unsure.
   - `description`: under 200 chars.
   - `documentation`: source docs or Compose URL.
   - `instructions.start`: concise purpose and important prerequisites.
   - `instructions.end`: final URL, required post-deploy actions, credentials, readiness delay notes, and any minimum CLI/admin commands needed before the app is usable.

## Post-Deploy Instructions

Always include the minimum post-deploy steps the user needs to make the app work reliably. Keep them practical and app-specific; avoid dumping every upstream best practice.

Check whether the app requires:

- Enabling HTTPS in CapRover HTTP Settings.
- Enabling WebSocket support.
- Setting or updating the canonical/public URL after adding a custom domain.
- Running migrations, creating the first admin user, seeding data, generating keys, or clearing cache.
- Executing commands inside the container, e.g. via CapRover's app terminal or SSH:

```bash
docker exec -it srv-captain--APPNAME command args
```

- Configuring SMTP/email before signup, password reset, or invites work.
- Adding object storage, uploads directory permissions, or persistent volume checks.
- Waiting for DB migrations/startup before opening the app.
- Checking logs when first boot returns 502.

Put these steps in `instructions.end` and repeat the essential subset in the final answer after delivering the YAML. Include commands only when researched from official docs or clearly required by the image. Use `APPNAME` or `$$cap_appname` placeholders consistently and tell the user which value to replace.

## Common Patterns

### Web app with Postgres

```yaml
captainVersion: 4
services:
    $$cap_appname-postgres:
        image: postgres:$$cap_postgres_version
        volumes:
            - $$cap_appname-postgres-data:/var/lib/postgresql/data
        restart: always
        environment:
            POSTGRES_DB: app
            POSTGRES_USER: app
            POSTGRES_PASSWORD: $$cap_db_password
        caproverExtra:
            notExposeAsWebApp: 'true'

    $$cap_appname:
        image: org/app:$$cap_app_version
        restart: always
        depends_on:
            - $$cap_appname-postgres
        environment:
            DATABASE_URL: postgres://app:$$cap_db_password@srv-captain--$$cap_appname-postgres:5432/app
            APP_URL: http://$$cap_appname.$$cap_root_domain
        caproverExtra:
            containerHttpPort: '3000'
caproverOneClickApp:
    variables:
        - id: $$cap_app_version
          label: App Version
          defaultValue: '1.0.0'
          description: Check valid tags at https://hub.docker.com/r/org/app/tags
          validRegex: /^([^\s^\/])+$/
        - id: $$cap_postgres_version
          label: PostgreSQL Version
          defaultValue: '16-alpine'
          description: Check valid tags at https://hub.docker.com/_/postgres/tags
          validRegex: /^([^\s^\/])+$/
        - id: $$cap_db_password
          label: Database Password
          defaultValue: $$cap_gen_random_hex(16)
          validRegex: /.{12,}/
    instructions:
        start: App description and source docs URL.
        end: >-
            App is deployed at http://$$cap_appname.$$cap_root_domain.
            It may take a few minutes before the first request succeeds.
    displayName: App
    isOfficial: false
    description: Short app description.
    documentation: https://example.com/docs
```

### App with worker or queue

Use a separate non-exposed service for background work. Reuse the same image/version and env vars as the web service when required.

```yaml
    $$cap_appname-worker:
        image: org/app:$$cap_app_version
        restart: always
        command: worker
        depends_on:
            - $$cap_appname-postgres
            - $$cap_appname-redis
        environment:
            DATABASE_URL: postgres://app:$$cap_db_password@srv-captain--$$cap_appname-postgres:5432/app
            REDIS_URL: redis://srv-captain--$$cap_appname-redis:6379
        caproverExtra:
            notExposeAsWebApp: 'true'
```

### Redis with password

Prefer `command` before `dockerfileLines` if the official image supports it.

```yaml
    $$cap_appname-redis:
        image: redis:$$cap_redis_version
        volumes:
            - $$cap_appname-redis-data:/data
        restart: always
        command: ['sh', '-c', 'redis-server --requirepass "$$cap_redis_password"']
        caproverExtra:
            notExposeAsWebApp: 'true'
```

### Secondary public endpoint

For apps that need a second public domain, such as MinIO console plus S3 API, create a second web service backed by `caprover/nginx-reverse-proxy`.

```yaml
    $$cap_appname-api:
        image: caprover/nginx-reverse-proxy:1-ef5ffcb
        restart: always
        depends_on:
            - $$cap_appname
        environment:
            UPSTREAM_HTTP_ADDRESS: http://srv-captain--$$cap_appname:9000
            CLIENT_MAX_BODY_SIZE: '0'
```

Mention the secondary URL in `instructions.end`, e.g. `https://$$cap_appname-api.$$cap_root_domain`.

### Database-only app

Set `notExposeAsWebApp: 'true'` on the only service. In `instructions.end`, show the internal host and connection example:

```text
Postgres is deployed and available as srv-captain--$$cap_appname:5432 to other apps.
```

## Official Repository Work

Only perform repository work after the output target is confirmed as a CapRover one-click app repository entry. Do not infer this from a generic request to create a template. Do not create `public/`, `public/v4/`, `public/v4/apps/`, or `public/v4/logos/` unless the repository target is explicit or confirmed by the user.

When adding an app to a CapRover one-click repository:

- Put YAML in `public/v4/apps/<app-name>.yml`.
- Add a matching icon in `public/v4/logos/<app-name>.png`.
- Use lowercase hyphenated filenames unless the target repo already uses another name for the app.
- Run repository checks:

```bash
npm i
npm run formatter-write
npm run validate_apps
npm run build
```

For private repositories, build static output from `dist/` and host it anywhere. Configure the repository URL in CapRover's One-Click Apps settings.

## Validation Checklist

- `captainVersion: 4` is first.
- YAML parses.
- No unsupported Compose keys remain except `restart`.
- `dockerfileLines` is not combined with `image`.
- Public services have correct `containerHttpPort`.
- Internal services/workers use `notExposeAsWebApp: 'true'`.
- Internal hostnames use `srv-captain--$$cap_appname-*`.
- Persistent data uses named volumes prefixed with `$$cap_appname-`.
- Variables include `id` and `label`; required fields have `validRegex`.
- Version variables link to valid tag pages/docs and avoid `latest` unless upstream explicitly recommends it.
- Secrets default to `$$cap_gen_random_hex(length)` when possible.
- `instructions.end` includes required post-deploy actions: enable HTTPS, enable WebSocket support, set domains, wait for migrations, or initial credentials.
- `description` is under 200 chars.
- `isOfficial` is conservative.
- Docker CLI was used for local startup/port/command checks when available, or the reason it could not be used is reported.
- Temporary Docker resources created during testing are cleaned up.
- If targeting an app repository, matching logo exists and `npm run validate_apps` passes.

## Manual Template Testing

Tell the user to test the YAML in CapRover:

1. Open CapRover dashboard.
2. Go to **Apps** -> **One-Click Apps/Databases**.
3. Select **>> TEMPLATE <<**.
4. Paste YAML.
5. Click **NEXT**, fill variables, deploy, then verify logs and public URL.
