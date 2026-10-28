---
name: openapi-to-docs
description: Turns an OpenAPI (Swagger) 3.x specification into human-friendly API documentation - a five-minute quickstart, an authentication guide, per-endpoint pages with realistic request and response examples, an error catalogue, pagination and rate-limit sections, and changelog and versioning conventions - while flagging gaps in the spec itself. Use when publishing developer docs from a spec, onboarding partners to an API, or auditing whether a spec is complete enough to document.
---

# OpenAPI to Docs

A spec tells a machine what is valid; docs tell a developer how to succeed. Use the spec as the source of truth and add the narrative it lacks.

## When to use
- You have an `openapi.yaml`/`.json` and need a docs site, README or partner guide.
- Generated reference pages exist but developers still ask basic questions.
- You want to know what the spec is missing before docs go public.

## Step 1 - Read and inventory the spec
- Servers (base URLs per environment), security schemes, tags, paths and operations, shared components, error schemas.
- Note gaps: operations without `summary`, parameters without `description`, schemas without `example`, undocumented error responses, missing `operationId`. List them as a **Spec gaps** section; do not paper over them with invented behaviour.

## Step 2 - Structure the docs
1. **Overview** - what the API lets you do, in three bullets; base URLs; versioning scheme.
2. **Quickstart (5 minutes)** - get credentials, make one authenticated read call, make one write call, see the result. Show the exact request with a placeholder like `Authorization: Bearer <your-key>`, and the trimmed response.
3. **Authentication** - per scheme in `securitySchemes`: how to obtain credentials, where they go (header, query, cookie), scopes and what each allows, expiry and refresh, and what a failed auth looks like (status and body). Recommend keeping keys server-side and rotating them.
4. **Core concepts** - the main resources and how they relate, IDs format, timestamps (format and timezone), money (minor units or decimals, currency field), idempotency keys if supported.
5. **Endpoint reference** - grouped by tag, one section per operation.
6. **Pagination** - the model actually used (cursor, page/size, offset), parameter names, max page size, how to detect the last page, and a loop described in prose.
7. **Errors** - catalogue table, plus retry guidance.
8. **Rate limits** - limits, the headers that report them, behaviour on 429, recommended back-off. If the spec does not define them, mark as "to confirm" rather than guessing numbers.
9. **Webhooks** (if `webhooks` or callbacks exist) - events, payloads, signature verification steps, retries.
10. **Changelog and versioning** - see Step 4.

## Step 3 - Per-endpoint template
```
### <Summary> 
`METHOD /path`  - required scope: ...
<One paragraph: when to use it, side effects, idempotency.>

Path / query parameters
| Name | Type | Required | Description | Example |

Request body (fields table, then a realistic example)

Responses
- 200: what it means + example
- 4xx/5xx: link to error catalogue entries that apply

Example request (one HTTP block; optional language tabs)
Notes: limits, ordering, eventual consistency
```
Examples must validate against the schema. Use realistic but fake data (names like "Sara Ahmed", amounts with currency, ISO 8601 timestamps with offset). Never paste real keys or customer data.

## Error catalogue format
```
| HTTP | code | Meaning | Common cause | How to fix | Retry? |
```
Collect codes from response schemas and examples; if the API returns a structured error body, document its fields once.

## Step 4 - Changelog conventions
- Date-stamped entries, newest first, labelled Added / Changed / Deprecated / Removed / Fixed / Security.
- Every breaking change states the affected endpoints, the migration step and the removal date.
- Deprecations surface in docs pages with a banner and, if the API sends them, `Deprecation`/`Sunset` headers.

## Arabic or bilingual docs
If docs are needed in Arabic, keep code, field names, paths and HTTP verbs in Latin script and left-to-right blocks; translate the narrative and table descriptions natively.

## Output
Return the docs as Markdown pages (overview, quickstart, auth, concepts, one file per tag, errors, pagination, rate limits, changelog), then the Spec gaps list ordered by how much it would confuse a developer.

## Worked example (fragment)
Spec: `GET /v1/invoices` with `cursor` and `limit` (max 100), `security: bearerAuth`, 200 returns `{data: [...], next_cursor}`.
Docs: "List invoices, newest first. Pass `limit` (1-100, default 20). When `next_cursor` is null you have reached the last page; otherwise send it back as `cursor`." Spec gap: no 401 response documented; no example for `status` enum values.

## Pitfalls
- Copying schema descriptions verbatim when they are empty or say "the id".
- Examples that fail the schema (wrong enum, missing required field).
- Documenting behaviour the spec does not define as if it were guaranteed.
