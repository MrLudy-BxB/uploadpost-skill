---
source_id: llm-section:docs/guides/async-uploads.md
public_url: https://docs.upload-post.com/guides/async-uploads
raw_source: llm.txt
collected: 2026-05-30
---

Avoid Timeouts with Asynchronous Uploads

Are your requests taking too long and resulting in timeouts? For video, photo, or text post uploads that may require more processing time (file processing, social network publishing queues, etc.), use the async\_upload parameter to make your request asynchronously.

How does it work?

Send your request with async\_upload=true to the appropriate upload endpoint.

The API will immediately respond with a request\_id.

Use this request\_id to check the progress and result at the status endpoint.

Checking Status

The status endpoint supports two different identifier types:

request\_id: Returned by upload endpoints when async\_upload=true

job\_id: Returned when you schedule posts with scheduled\_date

For Async Uploads

For Scheduled Posts

After scheduling a post with scheduled\_date, the API returns a job\_id. Use it to check the status after the scheduled time:

Relevant Endpoints

Text upload: POST /api/upload\_text

Video upload: POST /api/upload

Photo upload: POST /api/upload\_photos

Upload status: GET /api/uploadposts/status?request\_id=\<REQUEST\_ID> or ?job\_id=\<JOB\_ID>

Quick Example: Asynchronous Video Upload
