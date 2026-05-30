# SDKs and Workflow Integrations

Collected: 2026-05-30

## Source Traceability

| Source | Public URL |
| --- | --- |
| `docs/sdk-examples.md` | https://docs.upload-post.com/sdk-examples |
| `docs/guides/ai-shorts-uploader.md` | https://docs.upload-post.com/guides/ai-shorts-uploader |
| `docs/guides/airtable-integration.md` | https://docs.upload-post.com/guides/airtable-integration |
| `docs/guides/make-integration.md` | https://docs.upload-post.com/guides/make-integration |
| `docs/guides/mcp-server-integration.md` | https://docs.upload-post.com/guides/mcp-server-integration |
| `docs/guides/n8n-integration.md` | https://docs.upload-post.com/guides/n8n-integration |

## Endpoint Routing

No OpenAPI endpoint group is assigned to this topic; use the source excerpts and concept index.

## Practical Notes

- This topic covers the official SDK examples page, including REST/cURL, Python SDK, and JavaScript/Node.js SDK examples.
- Use the endpoint index for exact method/path lookup and this reference for client-style examples.
- Use placeholders such as `<YOUR_API_KEY>` in generated examples; never include real API keys.

## REST, Python, and JavaScript SDK Coverage

Source: https://docs.upload-post.com/sdk-examples

### REST / cURL

The SDK examples page includes REST-style cURL examples for video and photo uploads against `https://api.upload-post.com/api/upload`. The OpenAPI spec provides 43 endpoint records that can be called directly with cURL. Use `references/data/endpoint-index.json` for each method, path, required fields, and topic route.

Robust cURL defaults:

- Base URL: `https://api.upload-post.com/api`.
- Authentication: `Authorization: Apikey <YOUR_API_KEY>`.
- Use `--fail-with-body --show-error --location` for scripts so non-2xx responses fail while preserving the response body.
- Use `--connect-timeout 10 --max-time 120` for ordinary metadata requests; uploads may need a larger `--max-time`.
- For upload/create retries, send `Idempotency-Key: <unique-request-id>` when the docs list it, especially on multipart upload endpoints.
- For async uploads, prefer `async_upload=true`, poll status at documented intervals, and stop after terminal states such as completed or failed.

Video upload:

```bash
curl --fail-with-body --show-error --location \
  -H 'Authorization: Apikey <YOUR_API_KEY>' \
  -H 'Idempotency-Key: upload-video-2026-05-30-001' \
  -F 'video=@/path/to/your/video.mp4' \
  -F 'title="Your Video Title"' \
  -F 'user="test"' \
  -F 'platform[]=tiktok' \
  -F 'async_upload=true' \
  -X POST https://api.upload-post.com/api/upload
```

Photo upload:

```bash
curl --fail-with-body --show-error --location \
  -H 'Authorization: Apikey <YOUR_API_KEY>' \
  -H 'Idempotency-Key: upload-photos-2026-05-30-001' \
  -F 'photos[]=@/path/to/your/image1.jpg' \
  -F 'user="test"' \
  -F 'platform[]=instagram' \
  -F 'title="My Photo Title"' \
  -F 'description="My photo description"' \
  -X POST https://api.upload-post.com/api/upload
```

JSON request pattern:

```bash
curl --fail-with-body --show-error --location \
  -H 'Authorization: Apikey <YOUR_API_KEY>' \
  -H 'Content-Type: application/json' \
  -d '{"monitor_id":"<MONITOR_ID>"}' \
  -X POST https://api.upload-post.com/api/uploadposts/autodms/pause
```

GET with query parameters:

```bash
curl --fail-with-body --show-error --location \
  -H 'Authorization: Apikey <YOUR_API_KEY>' \
  'https://api.upload-post.com/api/uploadposts/status?job_id=<JOB_ID>'
```

PATCH JSON pattern:

```bash
curl --fail-with-body --show-error --location \
  -H 'Authorization: Apikey <YOUR_API_KEY>' \
  -H 'Content-Type: application/json' \
  -d '{"scheduled_date":"2026-06-15T10:00:00Z"}' \
  -X PATCH https://api.upload-post.com/api/uploadposts/schedule/<JOB_ID>
```

DELETE pattern:

```bash
curl --fail-with-body --show-error --location \
  -H 'Authorization: Apikey <YOUR_API_KEY>' \
  -X DELETE https://api.upload-post.com/api/uploadposts/schedule/<JOB_ID>
```

Shell script pattern with status capture:

```bash
response_file="$(mktemp)"
status="$(
  curl --silent --show-error --location \
    --output "$response_file" \
    --write-out "%{http_code}" \
    -H "Authorization: Apikey ${UPLOAD_POST_API_KEY}" \
    "https://api.upload-post.com/api/uploadposts/status?job_id=${JOB_ID}"
)"

if [ "$status" -lt 200 ] || [ "$status" -ge 300 ]; then
  printf 'Upload-Post API error %s:\n' "$status" >&2
  cat "$response_file" >&2
  exit 1
fi

cat "$response_file"
```

When answering cURL questions for a specific operation, combine the exact endpoint metadata from `data/endpoint-index.json` with the relevant topic reference. Do not guess fields that are absent from the endpoint record or bundled source excerpt.

### Python SDK

The docs show a Python SDK package named `upload-post` on PyPI and import from `upload_post`.

```python
from upload_post import UploadPostClient

client = UploadPostClient(api_key="<YOUR_API_KEY>")

response = client.upload_video(
    video_path="/path/to/video.mp4",
    title="My Awesome Video",
    user="testuser",
    platforms=["tiktok", "instagram"],
)

print("Upload successful:", response)
```

### JavaScript / Node.js SDK

The docs show an npm package named `upload-post` and an `UploadPost` client export.

```javascript
import { UploadPost } from "upload-post";

const uploader = new UploadPost("<YOUR_API_KEY>");

const result = await uploader.upload("/path/to/video.mp4", {
  title: "My Awesome Video",
  user: "test-user",
  platforms: ["tiktok"],
});

console.log("Upload successful:", result);
```

## Source Excerpts

### docs/sdk-examples.md

SDK Examples

Explore real-world examples using the Upload Post SDK in Python and JavaScript.

PyPI version
npm version

cURL

Upload Video

Upload Photos

Python

Basic Upload

JavaScript/Node.js

Basic Upload

### docs/guides/ai-shorts-uploader.md

AI Shorts Uploader

The AI Shorts Uploader is a built-in assistant in the Upload-Post dashboard that watches your short-form video and writes the title, description and hashtags for each platform you publish to — YouTube, Instagram and TikTok — in seconds.

It is available from the Shorts Uploader screen in the app via the Generate with AI button.

What it does

When you click Generate with AI, the platform:

Sends your video to a multimodal AI model (Gemini).

Analyses the visual content, pacing and on-screen text.

Returns optimized copy per platform:

YouTube — title (≤100 chars) + description (≤5,000 chars) with relevant hashtags.

Instagram — caption (≤2,200 chars) tuned for Reels.

TikTok — caption (≤150 chars) tuned for the For You feed.

You can also rewrite an already-generated set of captions into another language (Spanish, English, Japanese, …) without re-uploading the video. Each rewrite counts as one analysis.

Monthly quotas per plan

Each successful analysis (and each language rewrite) consumes one unit from your monthly quota. The counter resets every 30 days from your last reset.

| Plan          | Analyses per month |
| :------------ | -----------------: |
| Free          |                 10 |
| Basic         |                100 |
| Professional  |                300 |
| Advanced      |                600 |
| Business      |              1,000 |

When you reach your quota, the dashboard shows an in-app message and the API responds with HTTP 429. Quotas reset automatically at the start of your next billing cycle.

Video requirements

[Excerpt truncated in this topic reference; see the source URL for full page context.]

### docs/guides/airtable-integration.md

Airtable Integration

Manage your video uploads to social media platforms directly from Airtable.

Set Up

Running Airtable Automation Scripts requires a paid Airtable plan that includes automations with scripts.

This guide shows you how to automatically upload videos to social media platforms from Airtable via Upload-Post.

Gather Your API Key

Start by getting your API Key from your Upload-Post account in the "API Keys" section. This key will be used in the script.

Be sure you have configured your social media accounts in Upload-Post before proceeding.

:::info
URL Support for Media Files: You can now pass URLs for both photo and video uploads instead of binary files. Simply provide the direct URL to your media file in the video or photos\[] parameter.
:::

Create an Airtable Workspace

In Airtable, create a new workspace with these fields:

Title as Single Line Text

Platforms as Multi Select with types: tiktok, instagram

Video as Attachment

User as Single Line Text

Status as Single Line Text

Enter Test Video Data

Add sample data to test the integration:

Title: Enter a title for your video

Platforms: Select one or more platforms (tiktok, instagram)

Video: Attach a video file (MP4 format recommended)

User: Enter your username

Status: Enter pending (lowercase)

Build an Automation Script

Let's create an Airtable automation script that uploads videos via Upload-Post:

Add Trigger

In your workspace, click on Automation then +New automation

Name the automation

Click Choose a Trigger

Select When a Record is Created

Select your table

Click Done

Add Action

[Excerpt truncated in this topic reference; see the source URL for full page context.]

### docs/guides/make-integration.md

Make Integration

Upload-Post provides seamless integration with Make (formerly Integromat) for automated video publishing workflows. This guide walks you through connecting your Upload-Post account with Make in 3 simple steps.

Getting Started with Upload-Post

Create an account or log in to your existing Upload-Post account

Navigate to the "API Keys" section

Generate an API key for your Make integration

API Configuration

For Make integration, you'll need to configure an HTTP module with the following parameters:

Note: Find your API key in your Upload-Post Manage API Keys section.

:::info
URL Support for Media Files: You can now pass URLs for both photo and video uploads instead of binary files. Simply provide the direct URL to your media file in the video or photos\[] parameter.
:::

Form Data Configuration & Make.com Setup

Configure your Make HTTP module with these parameters:

| Field | Value | Required |
| ----- | ----- | -------- |
| title | Your video title | Optional |
| user | Your username | Required |
| platform\[] | tiktok | Required |
| video | Binary file | Required |

Make.com Configuration Steps:

Add an HTTP Module: In your Make.com scenario, add an HTTP module and choose the "Make a Request" action.

Configure the Request Settings:

Method: Set to POST.

URL: Enter https://api.upload-post.com/api/upload.

Headers: Add a header with:

Key: Authorization

Value: Apikey \[YOUR\_API\_KEY]

Set the Request Body: Change the body type to multipart/form-data and add the following form fields:

[Excerpt truncated in this topic reference; see the source URL for full page context.]

### docs/guides/mcp-server-integration.md

MCP Server (Claude / Cursor)

Upload-Post ships an official Model Context Protocol (MCP) server. Connect it to Claude Desktop, Claude Code, Cursor, or any other MCP-compatible AI agent and your assistant can publish, schedule, analyze and manage social media on your behalf — without writing any code.

The server exposes 40 tools that map 1:1 to the public Upload-Post REST API.

Hosted endpoint: https://mcp.upload-post.com/mcp

Source: github.com/Upload-Post/upload-post-mcp (MIT)

How authentication works

The server is multi-tenant and stateless. Each MCP client sends its own Upload-Post API key in the Authorization header of every request. The server uses that key for the duration of the session and stores nothing.

Authorization: Bearer YOUR\_UPLOAD\_POST\_API\_KEY is also accepted, for clients that only allow Bearer tokens.

Get your API key

Sign in to app.upload-post.com.

Open the API Keys section.

Generate a new key and copy it.

Connect Claude Desktop / Claude Code / Cursor

Add the following entry to your MCP config (~/.claude/mcp.json, ~/.cursor/mcp.json, or your IDE's equivalent):

Restart your client. You should see 40 upload-post tools become available.

What the agent can do

| Group         | Tools |
|---------------|-------|
| Upload        | upload\_video, upload\_photos, upload\_text, upload\_document |
| Status        | get\_status, get\_job\_status, get\_history, get\_media |
| Schedule      | list\_scheduled, cancel\_scheduled, edit\_scheduled |
| Analytics     | get\_analytics, get\_total\_impressions, get\_post\_analytics, get\_platform\_metrics |

[Excerpt truncated in this topic reference; see the source URL for full page context.]

### docs/guides/n8n-integration.md

n8n Integration

Upload-Post provides seamless integration with n8n for automated video publishing workflows. This guide walks you through connecting your Upload-Post account with n8n.

Getting Started with Upload-Post

Create an account or log in to your existing Upload-Post account

Navigate to the "API Keys" section

Generate an API key for your n8n integration

API Configuration

For n8n integration, you'll need to configure an HTTP Request node with the following parameters:

:::info
URL Support for Media Files: You can now pass URLs for both photo and video uploads instead of binary files. Simply provide the direct URL to your media file in the video or photos\[] parameter.
:::

n8n Workflow Configuration

Configure your n8n HTTP Request node with these parameters:

| Field | Value | Required |
| ----- | ----- | -------- |
| title | Your video title | Optional |
| user | Your username | Required |
| platform\[] | tiktok | Required |
| video | Binary file | Required |

Node Configuration Steps:

Add an HTTP Request Node to your workflow

Configure the node settings:

Method: POST

URL: https://api.upload-post.com/api/upload

Headers: Authorization: Apikey \[YOUR\_API\_KEY]

Body: Set to multipart/form-data and add the required fields

Complete JSON Node Configuration

Below is the complete JSON configuration for the HTTP Request node in n8n:

For Instagram Uploads

To upload to Instagram instead, change the platform value:

Uploading to Multiple Platforms

To upload to both TikTok and Instagram simultaneously:

Security Best Practices

[Excerpt truncated in this topic reference; see the source URL for full page context.]
