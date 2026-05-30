# Validation Report

Generated: 2026-05-30T10:14:32.699385+00:00

## Checks

- PASS: Required files exist: `SKILL.md`, `README.md`, `metadata.json`, `references/INDEX.md`.
- PASS: Data indexes exist: endpoint, concept, source, topic, taxonomy, and skill-reference indexes.
- PASS: All 47 official `llm.txt` sections are available in full under `references/llm-sections/`.
- PASS: Topic files listed in `references/INDEX.md` exist.
- PASS: Skill name `upload-post` is product/API based and consistent across package metadata.
- PASS: Final package contains no raw source files.
- PASS: Private dashboard and account-specific data are excluded.

## Smoke Tests

- Endpoint lookup: `POST /upload` routes to `by-topic/content-upload.md`.
- Google Business lookup: `GET /api/uploadposts/google-business/locations` routes to `by-topic/platform-data.md`.
- Concept lookup: authentication routes to `by-topic/overview-auth.md`.
- Topic lookup: AutoDM routes to `by-topic/instagram-interactions-autodm.md`.
- Source lookup: OpenAPI routes to https://docs.upload-post.com/openapi.json.

## Residual Risk

Docs-page-only endpoint records may not be represented in OpenAPI; those records are explicitly marked and should be answered with source-disagreement caveats when relevant.
