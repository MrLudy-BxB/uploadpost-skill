# Upload-Post Skill References

Collected: 2026-05-30

## What Is Included

- Official Upload-Post OpenAPI spec (`openapi.json`).
- Official Upload-Post `llm.txt` documentation export split into 47 full, traceable source-equivalent files under `llm-sections/`.
- Official docs pages from the sitemap for page-only endpoint details.

## Lookup Order

1. For endpoint questions, open `data/endpoint-index.json` and route to the listed topic file.
2. For concept, schema, guide, or troubleshooting questions, open `data/concept-index.json` or `data/topic-index.json`.
3. Load the relevant `by-topic/*.md` file for practical guidance and source excerpts.
4. If the topic reference is not detailed enough, open `data/llm-section-index.json` and load the matching full `llm-sections/*.md` file.
5. Cite public source URLs from the topic's Source Traceability table, `data/llm-section-index.json`, or `data/source-index.json`.

## Topics

| Topic | File | Endpoint records |
| --- | --- | --- |
| Overview, Authentication, and Quickstart | `by-topic/overview-auth.md` | 0 |
| Content Upload Endpoints | `by-topic/content-upload.md` | 4 |
| Upload Status, History, and Scheduled Posts | `by-topic/upload-management.md` | 5 |
| Async Uploads, Scheduling, and Queue System | `by-topic/scheduling-queue-async.md` | 4 |
| Platform Data and Analytics | `by-topic/platform-data.md` | 7 |
| User Profiles, Current User, and JWT | `by-topic/user-management-jwt.md` | 7 |
| FFmpeg Editor and Webhooks | `by-topic/ffmpeg-webhooks.md` | 5 |
| Instagram Media, Comments, DMs, and AutoDM | `by-topic/instagram-interactions-autodm.md` | 11 |
| Limits, Errors, Requirements, and Troubleshooting | `by-topic/limits-errors.md` | 0 |
| SDKs and Workflow Integrations | `by-topic/integrations-sdks.md` | 0 |

## Data Files

- `data/endpoint-index.json` - methods, paths, operation IDs, request fields, response codes, and topic routes.
- `data/concept-index.json` - schemas and high-level concepts.
- `data/source-index.json` - source traceability back to official URLs and raw workflow evidence.
- `data/topic-index.json` - topic routing table.
- `data/skill-reference-index.json` - package-level routing index.
- `data/llm-section-index.json` - routes every full section from official `llm.txt`.
- `data/taxonomy.json` - endpoint tag to topic mapping.

## Full LLM Export

- `llm-sections/INDEX.md` lists all 47 full sections from the official `llm.txt` export.
- Use `llm-sections/*.md` for exact examples, platform-specific fields, limits, requirements, and troubleshooting details.

## Known Limitations

- Some docs pages contain endpoints not present in the collected OpenAPI spec; those are marked as docs-page endpoint records.
- The final skill does not include raw source files, workflow logs, private dashboard pages, or account-specific data.
