---
name: upload-post
description: Use this skill when answering questions about Upload-Post API integration, social media upload endpoints, scheduling, queueing, user profiles, platform data, FFmpeg processing, webhooks, Instagram interactions, SDKs, limits, and troubleshooting.
metadata:
  short-description: Source-grounded Upload-Post API docs.
---

# Upload-Post Skill

## Lookup Workflow

1. Start with `references/INDEX.md` to choose the right topic reference.
2. For endpoint or API operation questions, inspect `references/data/endpoint-index.json` first, then load the `topicFile` listed on the endpoint record.
3. For Python SDK, JavaScript/Node.js SDK, or REST/cURL examples, load `references/by-topic/integrations-sdks.md`.
4. For guide, schema, object, troubleshooting, or concept questions, inspect `references/data/concept-index.json` or `references/data/topic-index.json`, then load the matching `references/by-topic/` file.
5. When exact docs wording, examples, limits, platform-specific parameters, or unabridged details matter, inspect `references/data/llm-section-index.json` and load the matching full file under `references/llm-sections/`.
6. Use `references/data/source-index.json` when source traceability or citation details matter.

## Answering Rules

- Cite public source URLs from the bundled references when source-grounded answers are needed.
- Use `https://api.upload-post.com/api` as the OpenAPI production base URL unless a docs-page endpoint already includes `/api` in its full path.
- For cURL answers, include robust flags such as `--fail-with-body --show-error --location`, preserve response bodies on failures, and use idempotency keys for upload/create retries when documented.
- Never include real API keys. Use placeholders such as `<YOUR_API_KEY>` or environment variables.
- Treat the official `llm.txt` split sections as the primary documentation source. Use OpenAPI for structured endpoint lookup and field metadata.
- Do not invent behavior absent from bundled references. If OpenAPI and `llm.txt` evidence differ, call out the mismatch and cite the closest source.
- Treat `workflow/` files as build artifacts only; final answers should use bundled `references/` paths and public source URLs.
