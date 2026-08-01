---
name: Retrieve relevant files from a codebase with Relace
description: Rank and retrieve only the files relevant to a query using the Relace Code Reranker and Repo retrieval.
api: openapi/relace-openapi-original.json
operations:
  - POST /v2/code/rank
  - POST /v1/repo/{repo_id}/retrieve
  - POST /v1/repo/{repo_id}/search
---

# Retrieve relevant files from a codebase with Relace

Narrow a large codebase down to the files an agent actually needs for a task,
using the reranker directly or two-stage retrieval over a Relace Repo.

## Auth
`Authorization: Bearer rlc-...`. Ranker host:
`https://ranker.endpoint.relace.run`. Repo host: `https://api.relace.run`.

## Steps
1. Stateless ranking: `POST /v2/code/rank` with the query and candidate
   `CodeFile`s; read the ranked relevance list from `v2CodeRerankerResponse`.
2. Against a stored Relace Repo: `POST /v1/repo/{repo_id}/retrieve` for
   two-stage semantic retrieval, or `POST /v1/repo/{repo_id}/search` for
   semantic search.

## Rules
- Prefer `/v2/code/rank` over the legacy `/v1/code/rank`.
- 404 on retrieve can mean the repo index does not yet exist — index first.
- Errors: `{ "error": "<message>" }`; see errors/relace-error-codes.yml.
