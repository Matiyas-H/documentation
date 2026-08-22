# Omnia Voice — documentation

Public documentation for the Omnia Voice API, published with
[Mintlify](https://mintlify.com) at **https://omnia-voice.com/docs**.

## Layout

| Path | What |
| --- | --- |
| `docs.json` | Site config — theme, colours, navigation |
| `openapi.yaml` | API specification. The **API Reference** tab is generated from this |
| `*.mdx` | Pages, grouped by section |

## Local preview

```bash
npx mint@latest dev
```

## Before you commit

```bash
npx mint@latest broken-links                  # navigation and internal links
npx @redocly/cli@latest lint openapi.yaml     # spec must be error-free
```

## House rule: the handler is the source of truth

Everything here is derived from the route handlers in
[`omnia-v/omnia`](https://github.com/omnia-v/omnia) — their Zod validators and
response formatters. Nothing is written from memory.

That is not a style preference. Before this discipline, `PUT /agents/{id}` was
documented in three separate places and does not exist in the code; the real verb
is `PATCH`. Assigning a phone number was documented as
`POST /agents/{id}/numbers`, which is read-only — assignment is
`PATCH /numbers/{id}`.

When you change a page:

1. Read the handler and its validator before writing
2. Every documented endpoint must exist and accept the verb you claim
3. Every constraint (min, max, enum, pattern) must match the validator
4. Run both checks above

## Not this repo

`docs.omnia-voice.com` is a **different product** and lives in
`Matiyas-H/omnia-docs`. Do not confuse the two.
