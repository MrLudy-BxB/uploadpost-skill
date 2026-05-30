---
source_id: llm-section:docs/api/upload-photo.md
public_url: https://docs.upload-post.com/api/upload-photo
raw_source: llm.txt
collected: 2026-05-30
---

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
| title      | String | Conditional | Default title/caption of the post. Required for Reddit. Optional for all other platforms (TikTok, Instagram, Facebook, LinkedIn, X, Threads, Bluesky, Pinterest). |
| description    | String | No       | Optional extended text used on TikTok photo descriptions, LinkedIn commentary, Facebook descriptions, Pinterest notes, and Reddit bodies. Ignored elsewhere. |
| request\_id | String | No | Client-provided request identifier. If omitted, the server generates one. Returned in every response and used to track the upload via Upload Status. Useful when async\_upload=true and the HTTP response might be lost (e.g., timeout). Can also be sent as an X-Request-Id header. |
| scheduled\_date | String (ISO-8601) | No | Optional date/time (ISO-8601) to schedule publishing, e.g., "2024-12-31T23:45:00Z". Must be in the future (≤ 365 days). Omit for immediate upload. |
| timezone | String (IANA) | No | Optional timezone identifier (e.g., "Europe/Madrid", "America/New\_York"). If provided, scheduled\_date is interpreted in this timezone. Defaults to UTC if omitted. See IANA Time Zone Database for valid values. |
| async\_upload  | Boolean | No      | If true, the request returns immediately with a request\_id and processes in the background. See Upload Status. |
| add\_to\_queue  | Boolean | No      | If true, automatically schedules the post to your next available queue slot. Cannot be used with scheduled\_date. See Queue System. |
| max\_posts\_per\_slot | Integer | No | Override the profile's max posts per slot setting for this request. Only used when add\_to\_queue=true. See Queue System. |
| first\_comment | String | No       | Automatically post a first comment after publishing. Supported on Instagram, Facebook, Threads, Bluesky, Reddit, X, YouTube, and LinkedIn. On X (Twitter) and Threads, this creates a reply to the main post. For X threads, the comment is posted as a reply to the last tweet in the thread. On YouTube, it posts as a top-level comment on the video. |
| first\_comment\_media\[] | File(s) | No | Image files to attach to the first comment as inline images. Currently supported on Reddit. Not available for scheduled or queued posts. |

Important: If you set async\_upload to false but the upload takes longer than 59 seconds, it will automatically switch to asynchronous processing to avoid timeouts. In that case, use the request\_id with the Upload Status endpoint to check the upload status and result.

Platform-Specific First Comments

The first\_comment parameter serves as a fallback. To set a custom first comment for a particular platform, use the optional \[platform]\_first\_comment parameter. If provided, it will override the main first\_comment for that platform.

Example Optional Parameters:

instagram\_first\_comment: "Follow for more content! #photography"

facebook\_first\_comment: "Let me know your thoughts in the comments!"

x\_first\_comment: "Thread incoming! 🧵"

threads\_first\_comment: "First comment on Threads!"

reddit\_first\_comment: "Source in the comments."

bluesky\_first\_comment: "More details in the replies."

linkedin\_first\_comment: "Source article in the comments."

Scheduling behavior: When you provide scheduled\_date, the API responds with 202 Accepted and includes a job\_id. That same job\_id will later appear in Upload History to correlate the scheduled job with the publish record. You can also use the job\_id with the Upload Status endpoint to check the execution status of the scheduled post.

Video Support (Mixed Carousels):

Instagram & Threads: You can upload videos in the photos\[] array to create mixed carousels (photos + videos).

All other platforms (Facebook, TikTok, LinkedIn, X, Pinterest): Do NOT upload videos to this endpoint. Use the Upload Video endpoint instead. Uploading videos here for these platforms will result in an error.

Platform-Specific Titles

The title parameter serves as a fallback. To set a custom title for a particular platform, use the optional \[platform]\_title parameter. If provided, it will override the main title for that platform.

Example Optional Parameters:

instagram\_title: "Check out my latest reel on Instagram! #reels"

facebook\_title: "Excited to share this new video with my Facebook friends and family."

tiktok\_title: "New TikTok video just dropped! 🔥"

linkedin\_title: "A professional insight on the latest industry trends, discussed in this video."

x\_title: "New video out now! 📢"

Platform-Specific Parameters

LinkedIn

| Name                    | Type   | Required | Description                                                    | Default     |
|-------------------------|--------|----------|----------------------------------------------------------------|-------------|
| linkedin\_title          | String | No       | Specific title for the LinkedIn post. Fallbacks to title.    | title     |
| linkedin\_description or description | String | No | Sent as the post commentary. If omitted, we reuse title. | title     |
| visibility              | String | No       | Visibility setting for the post (accepted value: "PUBLIC")     | PUBLIC      |
| target\_linkedin\_page\_id | String | No       | LinkedIn page ID to upload photos to an organization         | "107579166" |

Facebook

| Name             | Type   | Required | Description                                                       | Default |
|------------------|--------|----------|-------------------------------------------------------------------|---------|
| facebook\_title   | String | No       | Specific title for the Facebook post. Fallbacks to title.       | title |
| facebook\_page\_id | String | Yes      | Facebook Page ID where the photos will be posted                  | -       |
| facebook\_media\_type | String | No | Type of media ("POSTS" or "STORIES") | "POSTS" |

Note: The caption is applied only to the first photo uploaded. For correct posting on Facebook, ensure the Page is directly associated with your personal profile and not managed through a Business Portfolio.

Note: If facebook\_page\_id is not provided, we will automatically use the user's only connected Page (if exactly one exists). If multiple Pages are connected, the API returns a helpful error with an available\_pages list so you can choose one. Posting to personal Facebook profiles via API is not supported by Meta; only Pages can be posted to.

X (Twitter)

:::warning URLs are stripped from every X post
Upload-Post removes every URL that X would turn into a clickable link from
the caption, title, and first\_comment before sending the tweet — schemed
URLs (https://, http://, ftp://), www. hosts, shorteners (t.co,
bit.ly, …), bare hostnames with a path (example.com/foo), and IPs with a
path.

Obfuscated forms (example\[.]com, hxxp://, unicode dots) are not
stripped because X does not parse them as links — they display as plain text
and are billed at the normal $0.015 rate.

Why: X charges $0.200 per post containing a URL vs $0.015 without —
13× more. See the
Character Limits page
for details.
:::

| Name                        | Type    | Required | Description                                                                                                                           | Default     |
|-----------------------------|---------|----------|---------------------------------------------------------------------------------------------------------------------------------------|-------------|
| x\_title                      | String  | No       | Specific title for the tweet. Fallbacks to title.                                                                                   | title     |
| x\_long\_text\_as\_post          | Boolean | No       | When true, publishes long text as a single post. Otherwise, creates a thread.                                                      | false     |
| x\_thread\_image\_layout        | String  | No       | Comma-separated list of how many images to attach to each tweet in the thread. Each value must be 0-4, and the total must equal the number of images. Use 0 for a text-only tweet in the thread (e.g., "0,4" makes the first tweet text-only and attaches all 4 images to the second tweet). Example: "4,4" puts 4 images in each of 2 tweets; "2,3,1" puts 2 in the first, 3 in the second, 1 in the third. If omitted and more than 4 images are provided, defaults to auto-chunking into groups of 4. | auto        |
| reply\_settings               | String  | No       | Controls who can reply to the tweet ("following", "mentionedUsers", "subscribers", "verified")                                       | -           |
| geo\_place\_id                 | String  | No       | Place ID for adding geographic location to the tweet                                                                                  | -           |
| nullcast                     | Boolean | No       | Whether to publish without broadcasting (promotional/promoted-only posts)                                                             | false     |
| for\_super\_followers\_only     | Boolean | No       | Tweet exclusive for super followers                                                                                                   | false     |
| community\_id                 | String  | No       | Community ID for posting to specific communities                                                                                      | -           |
| share\_with\_followers         | Boolean | No       | Share community post with followers                                                                                                   | false     |
| direct\_message\_deep\_link     | String  | No       | Link to take the conversation from public timeline to private Direct Message                                                         | -           |
| tagged\_user\_ids              | Array   | No       | Array of user IDs to tag in the photos (max 10 users)                                                                                 | \[]          |
| reply\_to\_id                  | String  | No       | ID of the tweet to reply to. Creates a reply to the specified tweet.                                                                  | -           |
| exclude\_reply\_user\_ids       | Array   | No       | Array of user IDs to exclude from replying to this tweet. Requires reply\_to\_id.                                                    | \[]          |

Note: For Twitter uploads, specify the platform as "x" in the platform\[] array.

More than 4 images: X supports a maximum of 4 images per tweet. If you provide more than 4 images, the API will automatically create a thread, distributing images across multiple tweets (up to 4 images each). Use x\_thread\_image\_layout to control exactly how images are distributed across tweets.

The global description field is ignored for X photo uploads.

How X (Twitter) Thread Creation Works (Advanced Logic)

Note: The following describes the default thread creation logic. To override this and post long text as a single post, set the x\_long\_text\_as\_post parameter to true.

The system is engineered to create well-formatted, natural-looking threads on X (formerly Twitter). Instead of simply splitting text at every line break, it intelligently groups paragraphs to create more readable tweets.

Here's the step-by-step logic:

Intelligent Paragraph Grouping (Primary Method):

The function first identifies distinct paragraphs (any text separated by a blank line).
It then combines as many of these paragraphs as possible into a single tweet, filling it up to the 280-character limit without exceeding it. The double newline (\n\n) between combined paragraphs is preserved for formatting.
This results in fewer, more substantial tweets that flow naturally, just as if a person had written them.

Handling Exceptionally Long Paragraphs:

If a single paragraph is, by itself, longer than the 280-character limit, a more granular splitting logic is automatically triggered for that paragraph only:

Split by Line Break: The system first attempts to break the paragraph down by its individual line breaks (\n).

Split by Word: If any of those single lines are still too long, it will split them by words as a final resort.

Media Attachment:

Images are distributed across tweets according to the x\_thread\_image\_layout parameter. If not specified and more than 4 images are provided, they are automatically distributed in groups of 4. Text parts and image chunks are interleaved across the thread tweets.

TikTok

| Name                  | Type    | Required | Description                                                                                 | Default |
|-----------------------|---------|----------|---------------------------------------------------------------------------------------------|---------|
| tiktok\_title          | String  | No       | Specific title for the TikTok post (max 90 characters). Fallbacks to title.               | title |
| post\_mode             | String  | No      | Controls how the upload is handled. DIRECT\_POST publishes immediately; MEDIA\_UPLOAD sends the media to the TikTok inbox so users can finish editing in-app. | DIRECT\_POST       |
| privacy\_level         | String  | No | Accepted values: PUBLIC\_TO\_EVERYONE, MUTUAL\_FOLLOW\_FRIENDS, FOLLOWER\_OF\_CREATOR, SELF\_ONLY. | PUBLIC\_TO\_EVERYONE |
| auto\_add\_music        | Boolean | No       | Automatically add background music to photos                                                | false   |
| disable\_comment       | Boolean | No       | Disable comments on the post                                                                | false   |
| brand\_content\_toggle | Boolean| No       | Set to true for paid partnerships that promote third-party brands.        | false   |
| brand\_organic\_toggle | Boolean| No       | Set to true when promoting the creator's own business.                    | false   |
| photo\_cover\_index     | Integer | No       | Index (starting at 0) of the photo to use as the cover/thumbnail for the TikTok photo post  | 0       |
| tiktok\_description or description    | String  | No       | For photo posts, used as description inside post\_info (max 4,000 characters). | title   |

Note on Draft Mode (MEDIA\_UPLOAD): When using MEDIA\_UPLOAD mode (Draft), TikTok does not allow setting a title, caption, privacy settings, or other metadata via the API. The video is simply uploaded to your TikTok inbox/drafts, and you must add the title, caption, and settings manually within the TikTok app before publishing.

Instagram

| Name | Type | Required | Description | Default |
|------|------|----------|-------------|---------|
| instagram\_title | String | No       | Specific title for the Instagram post. Fallbacks to title. | title |
| media\_type | String | No | Type of media ("IMAGE" or "STORIES"). Automatically handles CAROUSEL/REELS logic if mixed media is detected. | "IMAGE" |
| collaborators | String | No | Comma-separated list of collaborator usernames. | - |
| user\_tags | String | No | Users to tag on the photo. Photo posts require x/y coordinates — see below. | - |
| location\_id | String | No | Instagram location ID. | - |

Note on Instagram user\_tags for photo posts

Instagram's Graph API requires x and y coordinates (floats between 0.0 and 1.0, marking the tag position on the image) whenever you tag a user on a photo post. Username-only tags are silently dropped by Instagram on photos — the post still publishes, but without the tag.

Send user\_tags as a JSON-encoded array of objects:

Tag multiple users by adding more objects, each with its own coordinates:

For carousels, the same tags are applied to every image in the carousel.

Reels/videos accept the simpler comma-separated form ("@user1, user2") because Instagram does not require coordinates for video tags. See Upload Video.

The global description field is ignored for Instagram uploads (title serves as caption).

Threads

| Name | Type | Required | Description | Default |
|---|---|---|---|---|
| threads\_title | String | No | Specific title for the Threads post. Fallbacks to title. | title |
| threads\_thread\_media\_layout | String | No | Comma-separated list of how many media items to include in each Threads post. Each value must be 0-10, and the total must equal the number of files. Use 0 for a text-only post in the thread (e.g., "0,5" makes the first post text-only and attaches all 5 media items to the second post). Example: "5,5" splits 10 items into 2 posts of 5 each; "3,4,3" splits 10 items into 3 posts. If omitted and more than 10 items are provided, defaults to auto-chunking into groups of 10. | auto |
| threads\_topic\_tag | String | No | A topic tag for the post (1-50 characters). Cannot contain periods (.) or ampersands (&). One tag per post. Helps increase reach. | - |

More than 10 items: Threads supports a maximum of 10 media items per post (carousel). If you provide more than 10 items, the API will automatically create multiple posts, distributing media across posts (up to 10 items each). Use threads\_thread\_media\_layout to control exactly how media items are distributed across posts.

The global description field is ignored for Threads photo uploads.

Pinterest

| Name                 | Type   | Required | Description                                  | Default |
|----------------------|--------|----------|----------------------------------------------|---------|
| pinterest\_title      | String | No       | Specific title for the Pinterest Pin. Fallbacks to title. | title |
| pinterest\_description or description | String | No | Populates the Pin description. If omitted, we reuse title. | title |
| pinterest\_board\_id   | String | Yes      | Pinterest board ID to publish the photo to.  | -       |
| pinterest\_alt\_text   | String | No       | Alt text for the image.                      | -       |
| pinterest\_link       | String | No       | Destination link for the photo Pin.          | -       |

Bluesky

| Name | Type | Required | Description | Default |
|---|---|---|---|---|
| bluesky\_title | String | No | Specific text for the Bluesky post. Fallbacks to title. | title |

Note: Bluesky supports up to 4 images per post.

Reddit

| Name | Type | Required | Description | Default |
|---|---|---|---|---|
| reddit\_title | String | No | Specific title for the Reddit post. Fallbacks to title. | title |
| subreddit | String | Yes | Name of the subreddit to post to (without "r/"). | - |
| flair\_id | String | No | ID of the flair to apply to the post. | - |

Note: Reddit photo posts support a single image. The image will be uploaded as a native Reddit image post.

Google Business Profile

| Name | Type | Required | Description | Default |
|------|------|----------|-------------|---------|
| gbp\_location\_id | String | No\* | The location to post to. Use Get Google Business Locations to list available locations. | Auto |
| gbp\_topic\_type | String | No | Post type: STANDARD (default), EVENT, or OFFER. | STANDARD |
| gbp\_cta\_type | String | No | Call-to-action button: BOOK, ORDER, SHOP, LEARN\_MORE, SIGN\_UP, CALL. | - |
| gbp\_cta\_url | String | Conditional | URL for the CTA button. Required if gbp\_cta\_type is set. | - |

Note: If gbp\_location\_id is not provided, the API will automatically use the account's only location (if exactly one exists). If multiple locations are connected, the API returns an error asking you to select one.

Event parameters (when gbp\_topic\_type is EVENT):

| Name | Type | Required | Description |
|------|------|----------|-------------|
| gbp\_event\_title | String | Yes | Title of the event. |
| gbp\_event\_start\_date | String | Yes | Start date in YYYY-MM-DD format. |
| gbp\_event\_start\_time | String | No | Start time in HH:MM format (24h). |
| gbp\_event\_end\_date | String | Yes | End date in YYYY-MM-DD format. |
| gbp\_event\_end\_time | String | No | End time in HH:MM format (24h). |

Offer parameters (when gbp\_topic\_type is OFFER):

| Name | Type | Required | Description |
|------|------|----------|-------------|
| gbp\_coupon\_code | String | No | Coupon or promo code. |
| gbp\_redeem\_url | String | No | URL where the offer can be redeemed. |
| gbp\_terms | String | No | Terms and conditions of the offer. |

Example Requests

Upload Photo and Video to Instagram (Carousel)

Upload Photos to Facebook

Upload Photo to Reddit

Responses

200 OK (synchronous, finished fast)

200 OK (asynchronous/background started or sync→background fallback)

202 Accepted (scheduled)

400 Bad Request

Missing user, platform\[], Pinterest without pinterest\_board\_id, Reddit without subreddit, invalid platforms, invalid scheduled\_date.

401 Unauthorized: { "success": false, "message": "Invalid or expired token" }

403 Forbidden (plan restrictions)

404 Not Found (e.g., user not found)

429 Too Many Requests (monthly limit exceeded; includes current usage)

500 Internal Server Error: { "success": false, "error": "Detailed error message" }

Notes

When async or when sync falls back to background, use GET /api/uploadposts/status?request\_id={request\_id} to poll progress.

Per-platform results may include fields like url, post\_id(s), and platform-specific metadata or error.
