---
name: Apply LLM code edits with Relace Instant Apply
description: Merge an LLM-generated code snippet into an existing file at >10k tok/s using Relace Instant Apply.
api: openapi/relace-openapi-original.json
operations:
  - POST /v1/code/apply
---

# Apply LLM code edits with Relace Instant Apply

Use Relace's `relace-apply-3` merge model to apply a partial, LLM-generated
edit into a full source file without regenerating the whole file.

## Auth
Send `Authorization: Bearer rlc-...`. Get a key at
https://app.relace.ai/settings/api-keys. Host: `https://instantapply.endpoint.relace.run`.

## Steps
1. Gather the original file contents and the LLM edit snippet.
2. `POST /v1/code/apply` with the `InstantApplyRequest` body (initial code +
   edit snippet).
3. Read the merged file from the `InstantApplyResponse`.

## Rules
- Errors return `{ "error": "<message>" }`; branch on HTTP status
  (400 invalid, 401 auth, 429 rate limited, 500 retry). See
  errors/relace-error-codes.yml.
- Free tier is limited to 3 requests/minute — back off on 429.
- No idempotency key is supported; apply is a pure function of its inputs.
