# Working in this repo

Documentation for the **Omnia Voice** API — voice AI agents that answer the
phone, call your tools, and speak 50+ languages. Published with Mintlify at
`omnia-voice.com/docs`.

## The one rule

**The route handler is the source of truth.** Not the README, not this file, not
an older version of the docs, and not your recollection of how the API probably
works.

The API lives in [`omnia-v/omnia`](https://github.com/omnia-v/omnia) under
`app/api/v1/`. Each route's contract is defined by:

- its exported HTTP methods in `route.ts`
- its Zod schema in `lib/api/v1/validators/`
- its response formatter (`formatAgentResponse`, `formatToolResponse`, …)

Read those before writing. Every one of these was documented wrongly at some
point because someone trusted a document instead of the code:

| Was documented as | Actually |
| --- | --- |
| `PUT /agents/{id}` | `PATCH` — no `PUT` handler exists |
| `POST /agents/{id}/numbers` | Read-only. Assignment is `PATCH /numbers/{id}` |
| status `ARCHIVED` | `ACTIVE \| INACTIVE \| SUSPENDED` |
| `DELETE /agents/{id}` → 200 | 204, no body |
| response `joinUrl` | `websocketUrl` |
| voice webhooks | Do not exist — the only webhook model is chatbot-scoped |
| `GET /credits` with an API key | Session-only, returns 401 |

## Checks that must pass

```bash
npx mint@latest broken-links
npx @redocly/cli@latest lint openapi.yaml
```

Both must be clean. Every endpoint mentioned in prose must exist in
`openapi.yaml` **and** in a real handler.

## Deliberately absent

- **Actions and knowledge bases** — deprecated in favour of corpora and tools
- **Chatbots, conversations, service-agents** — a different product surface
- **Noise/VAD settings** — hardcoded in the platform, not user-configurable
- **Corpus management endpoints** — session-only, so documented as a dashboard
  workflow rather than API surface

Do not add them back without checking whether that is still true.

## Style

Explain *why*, not just *what*. A constraint without its reason gets removed by
the next person. Prefer a short warning that names the real failure over a long
description of correct usage.
