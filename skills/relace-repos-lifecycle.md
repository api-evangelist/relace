---
name: Manage a Relace Repo lifecycle
description: Create, list, update, and delete a Relace Repo (source control for AI agents) via the API.
api: openapi/relace-openapi-original.json
operations:
  - POST /v1/repo
  - GET /v1/repo
  - POST /v1/repo/{repo_id}/update
  - DELETE /v1/repo/{repo_id}
---

# Manage a Relace Repo lifecycle

Relace Repos are "GitHub for AI agents" with built-in two-stage retrieval.

## Auth
`Authorization: Bearer rlc-...`. Host: `https://api.relace.run`. For narrower
access, mint a scoped Repo Token.

## Steps
1. Create: `POST /v1/repo` with a `FileSource` (files) or `GitSource` (GitHub
   repo). Capture `repo_id` from `CreateRepoResponse` (201).
2. List: `GET /v1/repo` for a paginated list of repos you own.
3. Update: `POST /v1/repo/{repo_id}/update` with `FilesOverwriteSource`,
   `FilesDiffSource`, or a Git sync source.
4. Delete: `DELETE /v1/repo/{repo_id}` (204 No Content).

## Rules
- Max file upload is 50MB (`payload_too_large`, 413).
- 423 `resource_locked` means another operation holds the repo — retry.
- Errors: `{ "error": "<message>" }`; see errors/relace-error-codes.yml.
