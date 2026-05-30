---
source_id: llm-section:docs/api/upload-document.md
public_url: https://docs.upload-post.com/api/upload-document
raw_source: llm.txt
collected: 2026-05-30
---

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

Success Response

Error Response

How Documents Appear on LinkedIn

When you upload a document:

Native Viewer: LinkedIn displays the document in a native carousel/viewer format

Page Navigation: Users can swipe or click through pages

Preview: LinkedIn generates thumbnail previews for each page

Download: Depending on visibility settings, users may be able to download the document

Platform Limitations

| Platform | Document Support |
|----------|------------------|
| LinkedIn | Yes (native carousel/viewer) |
| Facebook | No |
| Instagram | No |
| TikTok | No |
| X (Twitter) | No |
| YouTube | No |
| Pinterest | No |
| Threads | No |
| Bluesky | No |
| Reddit | No |

Notes

Document processing may take a few seconds on LinkedIn's side before the post becomes fully visible

The document title appears as the post's media title

The description/commentary appears as the post text above the document

For company pages, ensure the authenticated LinkedIn account has admin access to the page

LinkedIn may compress or optimize documents for viewing

Related Endpoints

Upload Video - Upload videos to multiple platforms

Upload Photo - Upload images to multiple platforms

Upload Text - Post text-only content

Get LinkedIn Pages - List available LinkedIn pages for your account
