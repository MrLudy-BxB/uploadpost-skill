# Upload-Post Skill

A source-grounded skill for working with the Upload-Post API documentation. It covers REST/cURL usage, Python SDK examples, JavaScript/Node.js SDK examples, upload endpoints, scheduling, queueing, platform data, profiles, JWT flows, FFmpeg jobs, webhooks, Instagram interactions, limits, and troubleshooting.

## Install

From a standalone GitHub repository:

```bash
codex skills install https://github.com/<owner>/upload-post-skill
```

From a local clone:

```bash
codex skills install ./upload-post
```

From a repo-local published skills collection, if one is created later:

```bash
codex skills install ./published-skills/skills/upload-post
```

## Example Questions

- How do I upload a video to TikTok and YouTube with Upload-Post?
- Show the Python SDK, JavaScript SDK, and REST examples for uploading media.
- Which fields are required for scheduled uploads?
- How should I poll async upload status without hitting rate limits?
- What endpoints manage user profiles and JWT URLs?
- How do AutoDM monitors work for Instagram comments?

## Package Structure

```text
README.md
SKILL.md
metadata.json
references/
  INDEX.md
  by-topic/
  llm-sections/
  data/
  reports/
```

## Source And Safety Notes

Sources are official Upload-Post docs, the official OpenAPI spec, the official `llm.txt` export, and docs pages from the official sitemap. The package excludes raw scrape files, workflow logs, private dashboards, account data, and credentials.

Start with `SKILL.md` for agent instructions, `references/INDEX.md` for routing, `references/llm-sections/INDEX.md` for the full official `llm.txt` split, and `references/reports/validation-report.md` for validation status.
