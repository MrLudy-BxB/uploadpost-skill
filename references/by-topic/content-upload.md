# Content Upload Endpoints

Collected: 2026-05-30

## Source Traceability

| Source | Public URL |
| --- | --- |
| `docs/api/upload-video.md` | https://docs.upload-post.com/api/upload-video |
| `docs/api/upload-photo.md` | https://docs.upload-post.com/api/upload-photo |
| `docs/api/upload-text.md` | https://docs.upload-post.com/api/upload-text |
| `docs/api/upload-document.md` | https://docs.upload-post.com/api/upload-document |
| `docs/api/photo-requirements.md` | https://docs.upload-post.com/api/photo-requirements |
| `docs/api/video-requirements.md` | https://docs.upload-post.com/api/video-requirements |
| `openapi.json` | https://docs.upload-post.com/openapi.json |

## Endpoint Routing

| Method | Path | Operation | Summary | Required fields |
| --- | --- | --- | --- | --- |
| `POST` | `/upload` | `uploadVideo` | Upload video | platform[], user, video |
| `POST` | `/upload_photos` | `uploadPhotos` | Upload photos | photos[], platform[], user |
| `POST` | `/upload_text` | `uploadText` | Upload text post | platform[], title, user |
| `POST` | `/upload_document` | `uploadDocument` | Upload document | document, platform[], title, user |

## Practical Notes

- Upload endpoints accept multipart form data.
- Use `async_upload=true` for background processing; if synchronous processing exceeds the documented timeout behavior, the API may switch to async.
- `scheduled_date` and `add_to_queue` are mutually exclusive where documented.

## Source Excerpts

### docs/api/upload-video.md

Upload Video

Upload video to various social media platforms using this endpoint.

Endpoint

Headers

| Name | Value | Description |
|------|-------|-------------|
| Authorization | Apikey <YOUR_API_KEY> | Your API key for authentication |
| Idempotency-Key | unique-string | Optional. Prevents duplicate uploads if the same request is retried (e.g., after a timeout). Can also be sent as X-Idempotency-Key or X-Request-Id. When provided, if a matching upload job already exists, the API returns the existing job instead of creating a duplicate. |

Common Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| user | String | Yes | User identifier |
| platform\[] | Array | Yes | Platform(s) to upload to (e.g., "tiktok", "instagram", "linkedin", "youtube", "facebook", "twitter", "threads", "pinterest", "bluesky", "reddit", "google\_business") |
| video | File | Yes | The video file to upload (can be a file upload or a video URL) |
| title | String | Conditional | Default title of the video. Required for YouTube and Reddit. Optional for all other platforms (TikTok, Instagram, Facebook, LinkedIn, X, Threads, Bluesky, Pinterest). |
| description | String | No | Optional extended text used only on LinkedIn commentary, Facebook descriptions, YouTube descriptions, and Pinterest notes. Ignored elsewhere. |
| scheduled\_date | String (ISO-8601) | No | Optional date/time (ISO-8601) to schedule publishing, e.g., "2024-12-31T23:45:00Z". Must be in the future (≤ 365 days). Omit for immediate upload. |

[Excerpt truncated in this topic reference; see the source URL for full page context.]

### docs/api/upload-photo.md

Upload Photos

Upload photos (and mixed media for supported platforms) to various social media platforms using this endpoint.

Endpoint

Headers

| Name          | Value                      | Description                              |
|---------------|----------------------------|------------------------------------------|
| Authorization | Apikey <YOUR_API_KEY>   | Your API key for authentication          |
| Idempotency-Key | unique-string | Optional. Prevents duplicate uploads if the same request is retried (e.g., after a timeout). Can also be sent as X-Idempotency-Key or X-Request-Id. When provided, if a matching upload job already exists, the API returns the existing job instead of creating a duplicate. |

Common Parameters

| Name       | Type   | Required | Description                                                                                   |
|------------|--------|----------|-----------------------------------------------------------------------------------------------|
| user       | String | Yes      | User identifier                                                                               |
| platform\[] | Array  | Yes      | Platform(s) to upload to. Supported values: tiktok, instagram, linkedin, facebook, x, threads, pinterest, bluesky, reddit, google\_business |
| photos\[]   | Array  | Yes      | Array of files to upload. Accepts photos (jpg, png, etc.).  Note: You can also include videos (mp4, mov, etc.) ONLY for Instagram and Threads mixed carousels. |

[Excerpt truncated in this topic reference; see the source URL for full page context.]

### docs/api/upload-text.md

Upload Text

Upload text posts to various social media platforms using this endpoint.

Note: Currently, this endpoint supports X (Twitter), LinkedIn, Facebook, Threads, Reddit, Bluesky, and Google Business Profile. More platforms will be added in future updates.

Endpoint

Headers

| Name          | Value                      | Description                              |
|---------------|----------------------------|------------------------------------------|
| Authorization | Apikey <YOUR_API_KEY>   | Your API key for authentication          |
| Idempotency-Key | unique-string | Optional. Prevents duplicate uploads if the same request is retried (e.g., after a timeout). Can also be sent as X-Idempotency-Key or X-Request-Id. When provided, if a matching upload job already exists, the API returns the existing job instead of creating a duplicate. |

Common Parameters

| Name          | Type   | Required | Description                                                                |
|---------------|--------|----------|----------------------------------------------------------------------------|
| user          | String | Yes      | User identifier                                                            |
| platform\[]    | Array  | Yes      | Platform(s) to upload to. Supported values: linkedin, x, facebook, threads, reddit, bluesky, google\_business |
| title         | String | Yes      | Default text content for the post.                                         |

[Excerpt truncated in this topic reference; see the source URL for full page context.]

### docs/api/upload-document.md

Upload Document

Upload documents (PDF, PPT, PPTX, DOC, DOCX) to LinkedIn as native document posts. Documents are displayed as carousels/viewers on LinkedIn.

Endpoint

Headers

| Name | Value | Description |
|------|-------|-------------|
| Authorization | Apikey <YOUR_API_KEY> | Your API key for authentication |

Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| user | String | Yes | User identifier (profile name) |
| platform\[] | Array | Yes | Must be \["linkedin"] - only LinkedIn supports document uploads |
| document | File/URL | Yes | The document file to upload (file upload or URL). Supported formats: PDF, PPT, PPTX, DOC, DOCX |
| title | String | Yes | Document title (displayed on the post) |
| description | String | No | Post commentary/text that appears above the document |
| visibility | String | No | Visibility setting: "PUBLIC", "CONNECTIONS", "LOGGED\_IN", or "CONTAINER". Default: "PUBLIC" |
| target\_linkedin\_page\_id | String | No | LinkedIn organization/page ID to post to a company page instead of personal profile |
| first\_comment | String | No | Automatically post a first comment after publishing the document. |
| linkedin\_first\_comment | String | No | Platform-specific first comment override. Takes priority over first\_comment. |

Document Requirements

| Requirement | Value |
|-------------|-------|
| Supported Formats | PDF, PPT, PPTX, DOC, DOCX |
| Maximum File Size | 100 MB |
| Maximum Pages | 300 pages |

Example Request (File Upload)

Example Request (URL)

Example Request (Company Page)

[Excerpt truncated in this topic reference; see the source URL for full page context.]

### docs/api/photo-requirements.md

Photo Format Requirements

This document outlines the photo format requirements for uploading to various social media platforms via the API. For platforms where specific requirements are not listed, standard image formats like JPEG and PNG are generally accepted. However, for the most accurate and up-to-date information, please consult the official documentation of each respective platform.

Threads Photo Requirements

Format: JPEG, PNG

File Size: 8 MB maximum

Aspect Ratio: Limit: 10:1 (e.g., can be from 1:10 to 10:1)

Width:

Minimum: 320px (images narrower than 320px will be scaled up to 320px)

Maximum: 1440px (images wider than 1440px will be scaled down to 1440px)

Color Space: sRGB (images with other color spaces will be converted to sRGB)

Items Per Post: Up to 10 media items per post (carousel). If you provide more than 10 items, they are automatically distributed across multiple posts (up to 10 items per post).

Thread Media Layout: Use the threads\_thread\_media\_layout parameter to control how media items are distributed across posts. For example, "5,5" splits 10 items into 2 posts of 5 each.

Instagram Photo Requirements

Media Type: The API supports "IMAGE" for feed posts and "STORIES".

General Guidance: Instagram supports: png, jpeg, gif formats.

For detailed specifications (resolution, aspect ratio, file size), please refer to the official Instagram documentation.

TikTok Photo Requirements

[Excerpt truncated in this topic reference; see the source URL for full page context.]

### docs/api/video-requirements.md

Video Format Requirements

This document outlines the video format requirements for uploading to various social media platforms via the API.

Automatic Video Transformation

Our API automatically transforms videos to adapt them to the specifications of each social network. However, if you use this feature, the upload will take longer because the transformation is performed first before uploading to the platforms.

If you prefer faster uploads, you can pre-process your videos according to the specific requirements of each platform outlined in this document.

Video Encoding Compatibility

Some video creation tools occasionally produce videos with encoding that Meta's systems don't accept. At times, their output needs to be re-encoded for compatibility.

One Solution: Re-encode with FFmpeg

If your video uploads are failing, try re-encoding the video using FFmpeg, an open-source tool for video processing:

This command converts your video to use the widely-compatible H.264 video codec and AAC audio codec, which Meta platforms accept.

Re-encoding "normalizes" your video to use standard encoding parameters that Meta's platforms are designed to process, without sacrificing quality. If you see these errors regularly, this simple step can save you frustration when sharing your creative content.

FFmpeg Installation and Usage

Installation instructions:

macOS: brew install ffmpeg

Windows: winget install ffmpeg

Linux: sudo apt install ffmpeg (Ubuntu/Debian) or sudo dnf install ffmpeg (Fedora)

Parameters:

-c:v libx264: Uses H.264 video codec

[Excerpt truncated in this topic reference; see the source URL for full page context.]
