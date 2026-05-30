---
source_id: llm-section:docs/api/upload-text.md
public_url: https://docs.upload-post.com/api/upload-text
raw_source: llm.txt
collected: 2026-05-30
---

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
| description    | String | No       | Optional extended body used only on Reddit (becomes the post text). Ignored elsewhere. |
| request\_id | String | No | Client-provided request identifier. If omitted, the server generates one. Returned in every response and used to track the upload via Upload Status. Useful when async\_upload=true and the HTTP response might be lost (e.g., timeout). Can also be sent as an X-Request-Id header. |
| scheduled\_date | String (ISO-8601) | No | Optional date/time (ISO-8601) to schedule publishing, e.g., "2024-12-31T23:45:00Z". Must be in the future (≤ 365 days). Omit for immediate upload. |
| timezone | String (IANA) | No | Optional timezone identifier (e.g., "Europe/Madrid", "America/New\_York"). If provided, scheduled\_date is interpreted in this timezone. Defaults to UTC if omitted. See IANA Time Zone Database for valid values. |
| async\_upload  | Boolean | No      | If true, the request returns immediately with a request\_id and processes in the background. See Upload Status. |
| add\_to\_queue  | Boolean | No      | If true, automatically schedules the post to your next available queue slot. Cannot be used with scheduled\_date. See Queue System. |
| max\_posts\_per\_slot | Integer | No | Override the profile's max posts per slot setting for this request. Only used when add\_to\_queue=true. See Queue System. |
| first\_comment | String | No       | Automatically post a first comment after publishing. Supported on Facebook, Threads, Bluesky, Reddit, X, YouTube, and LinkedIn. On X (Twitter) and Threads, this creates a reply to the main post (threading). On YouTube, it posts as a top-level comment. Note: Instagram does not support text-only posts, so this parameter is not applicable for Instagram here. |
| first\_comment\_media\[] | File(s) | No | Image files to attach to the first comment as inline images. Currently supported on Reddit. Not available for scheduled or queued posts. |
| link\_url      | String | No       | URL to include as a link preview card. When provided, platforms that support link previews (LinkedIn, Bluesky, Facebook, Reddit) will display a rich preview card with the page's title, description, and thumbnail image. Platform-specific parameters (linkedin\_link\_url, bluesky\_link\_url, facebook\_link\_url, reddit\_link\_url) take priority over this generic parameter. |

Important: If you set async\_upload to false but the upload takes longer than 59 seconds, it will automatically switch to asynchronous processing to avoid timeouts. In that case, use the request\_id with the Upload Status endpoint to check the upload status and result.

Scheduling behavior: When you provide scheduled\_date, the API responds with 202 Accepted and includes a job\_id. That same job\_id will later appear in Upload History to correlate the scheduled job with the publish record. You can also use the job\_id with the Upload Status endpoint to check the execution status of the scheduled post.

This endpoint supports simultaneous text uploads to X (Twitter), LinkedIn, Facebook, Threads, Reddit, and Bluesky.

Platform-Specific First Comments

The first\_comment parameter serves as a fallback. To set a custom first comment for a particular platform, use the optional \[platform]\_first\_comment parameter. If provided, it will override the main first\_comment for that platform.

Example Optional Parameters:

facebook\_first\_comment: "Let me know your thoughts in the comments!"

x\_first\_comment: "Thread incoming! 🧵"

threads\_first\_comment: "First comment on Threads!"

reddit\_first\_comment: "Source in the comments."

bluesky\_first\_comment: "More details in the replies."

linkedin\_first\_comment: "Source article in the comments."

Platform-Specific Titles

The title parameter serves as a fallback. To set a custom title for a particular platform, use the optional \[platform]\_title parameter. If provided, it will override the main title for that platform.

Example Optional Parameters:

linkedin\_title: "A professional insight on the latest industry trends."

x\_title: "New update out now! 📢"

facebook\_title: "Excited to share this with my Facebook friends."

threads\_title: "Just posted something new on Threads!"

Platform-Specific Parameters

LinkedIn

| Name                    | Type   | Required | Description                                                                                                | Default     |
|-------------------------|--------|----------|------------------------------------------------------------------------------------------------------------|-------------|
| linkedin\_title          | String | No       | Specific text for the LinkedIn post. Fallbacks to title.                                                 | title     |
| target\_linkedin\_page\_id | String | No       | LinkedIn page ID to upload text to an organization's page. If not provided, posts to the user's personal profile. |             |
| linkedin\_link\_url       | String | No       | URL to include as a link preview card on the LinkedIn post. LinkedIn will display a rich preview with the page's title, description, and thumbnail. Overrides the generic link\_url parameter for LinkedIn. | link\_url  |

X (Twitter)

:::warning URLs are stripped from every X post
Every URL that X would turn into a clickable link is removed from the caption,
title, and first\_comment before the tweet is created — schemed URLs
(https://, http://, ftp://), www. hosts, shorteners (t.co, bit.ly,
…), bare hostnames with a path (example.com/foo), and IPs with a path.

Obfuscated forms (example\[.]com, hxxp://, unicode dots) are not
stripped because X does not parse them as links — they display as plain text
and are billed at the normal $0.015 rate.

Why: X charges $0.200 per "Content: Create (with URL)" vs $0.015 for
posts without a URL. Stripping happens on every X path (video, photo, text,
scheduled, retried) so usage stays on the cheap tier. See the
Character Limits page
for the full policy.
:::

| Name                        | Type    | Required | Description                                                                                                                           | Default     |
|-----------------------------|---------|----------|---------------------------------------------------------------------------------------------------------------------------------------|-------------|
| x\_title                     | String  | No       | Specific text for the tweet. Fallbacks to title. If the text is long, it will be split into a thread.                               | title     |
| x\_long\_text\_as\_post         | Boolean | No       | For X Premium users. When true, long text is published as a single post. When false (default), it creates a thread if text is long. | false     |
| reply\_settings              | String  | No       | Controls who can reply to the tweet ("following", "mentionedUsers", "subscribers", "verified")                                       | -           |
| quote\_tweet\_id              | String  | No       | ID of the tweet to quote in a quote tweet. Mutually exclusive with card\_uri, poll\_\*, and direct\_message\_deep\_link.             | -           |
| geo\_place\_id                | String  | No       | Place ID for adding geographic location to the tweet                                                                                  | -           |
| nullcast                    | Boolean | No       | Whether to publish without broadcasting (promotional/promoted-only posts)                                                             | false     |
| for\_super\_followers\_only    | Boolean | No       | Tweet exclusive for super followers                                                                                                   | false     |
| community\_id                | String  | No       | Community ID for posting to specific communities                                                                                      | -           |
| share\_with\_followers        | Boolean | No       | Share community post with followers                                                                                                   | false     |
| direct\_message\_deep\_link    | String  | No       | Link to take the conversation from public timeline to private Direct Message. Mutually exclusive with card\_uri, quote\_tweet\_id, and poll\_\*. | -           |
| card\_uri                    | String  | No       | Card URI for Twitter Cards/ads/promoted content. Mutually exclusive with quote\_tweet\_id, direct\_message\_deep\_link, and poll\_\*. | -           |
| poll\_options                | Array   | No       | Array of poll options (2-4 options, max 25 characters each). Mutually exclusive with card\_uri, quote\_tweet\_id, and direct\_message\_deep\_link. | \[]          |
| poll\_duration               | Integer | No       | Poll duration in minutes (5-10080, i.e., 5 minutes to 7 days)                                                                         | 1440        |
| poll\_reply\_settings         | String  | No       | Who can reply to poll ("following", "mentionedUsers", "subscribers", "verified"). Requires poll\_options.                                          | -           |
| reply\_to\_id                 | String  | No       | ID of the tweet to reply to. Creates a reply to the specified tweet.                                                                                | -           |
| exclude\_reply\_user\_ids      | Array   | No       | Array of user IDs to exclude from replying to this tweet. Requires reply\_to\_id. | \[]          |

Note: For Twitter uploads, specify the platform as "x" in the platform\[] array.

How Twitter Threads Are Created

If your text in the title field is longer than 280 characters, our API automatically creates a Twitter thread. You don't need to do anything special. By default, x\_long\_text\_as\_post is false.

How it works:

Our system creates natural-looking threads by intelligently splitting your text:

It groups paragraphs: The system combines as many paragraphs (text separated by a blank line) as possible into a single tweet without exceeding the character limit.

It splits long paragraphs: If a single paragraph is too long for one tweet, it's split into smaller parts. The system first tries to split by line breaks and then by words.

This process ensures your threads are easy to read.

Example of a thread creation

If you send this text in the title:

The API will create a thread like this:

Tweet 1:

This is the first paragraph. It is short.

This second paragraph is a bit longer. Our API tries to keep paragraphs together in one tweet.

Tweet 2:

This is a much longer third paragraph. It probably won't fit with the others. It might even be too long for a single tweet. If so, the API will split it. It will first look for line breaks.

Tweet 3:

If a single line is still too long, it will split it by words. This creates a readable and well-structured Twitter thread automatically.

Facebook

| Name             | Type   | Required | Description                                                       | Default |
|------------------|--------|----------|-------------------------------------------------------------------|---------|
| facebook\_title   | String | No       | Specific text for the Facebook post. Fallbacks to title.        | title |
| facebook\_page\_id | String | Yes      | Facebook Page ID where the text will be posted.                   | -       |
| facebook\_link\_url | String | No       | Optional URL to include for link preview in text posts. If provided, it's sent as link to the Graph API and Facebook may render a preview card. | -       |

Note: If facebook\_page\_id is not provided, we will automatically use the user's only connected Page (if exactly one exists). If multiple Pages are connected, the API returns a helpful error with an available\_pages list so you can choose one. Posting to personal Facebook profiles via API is not supported by Meta; only Pages can be posted to.

Threads

| Name                        | Type    | Required | Description                                                                                                                              | Default |
|-----------------------------|---------|----------|------------------------------------------------------------------------------------------------------------------------------------------|---------|
| threads\_title             | String  | No       | Specific text for the Threads post. Fallbacks to title.                                                                                | title |
| threads\_long\_text\_as\_post | Boolean | No       | If true, long text is published as a single post. If false (default), a thread is created if the text exceeds 500 characters.        | false |
| threads\_topic\_tag         | String  | No       | A topic tag for the post (1-50 characters). Cannot contain periods (.) or ampersands (&). One tag per post. Helps increase reach.        | -       |

Note: To upload content to Threads, specify the platform as "threads" in the platform\[] array.

How Threads Are Created

If the text you provide exceeds 500 characters and threads\_long\_text\_as\_post is false, our API will automatically create a thread on Threads, similar to how it works with X (Twitter).

How it works:

Our system creates natural-looking threads by intelligently splitting your text:

It groups paragraphs: The system combines as many paragraphs as possible into a single post without exceeding the character limit.

It splits long paragraphs: If a single paragraph is too long for a post, it is split into smaller parts, first trying to break by line breaks, and if that's not enough, by words.

This process ensures that your Threads are coherent and easy to read, replicating the functionality you already enjoy for X.

Reddit

| Name       | Type   | Required | Description                                                        | Default |
|------------|--------|----------|--------------------------------------------------------------------|---------|
| subreddit  | String | Yes      | Destination subreddit, without r/ (e.g., python).              | -       |
| flair\_id   | String | No       | ID of the flair template to apply to the post.                     | -       |
| reddit\_link\_url | String | No  | URL for creating a Reddit link post. When provided, creates a link post with a URL preview card instead of a self/text post. Overrides the generic link\_url parameter for Reddit. | link\_url |

If you provide the global description field, it becomes the Markdown body of the Reddit post; otherwise we post only the title.

Link posts: When reddit\_link\_url (or the generic link\_url) is provided, the post is created as a Reddit link post (kind: link) with a URL preview card. The description field is ignored for link posts since Reddit link posts don't have a text body.

Note: To upload content to Reddit, specify the platform as "reddit" in the platform\[] array.

Bluesky

| Name | Type | Required | Description | Default |
|---|---|---|---|---|
| bluesky\_title | String | No | Specific text for the Bluesky post. Fallbacks to title. | title |
| bluesky\_link\_url | String | No | URL to include as a link preview card on the Bluesky post. Bluesky will display a rich external embed with the page's title, description, and thumbnail. Overrides the generic link\_url parameter for Bluesky. | link\_url |
| reply\_to\_id | String | No | URL or AT-URI of the post to reply to. Creates a reply to the specified post. | - |

Note: To upload content to Bluesky, specify the platform as "bluesky" in the platform\[] array. The maximum character limit is 300 characters per post.

How Bluesky Threads Are Created

If your text exceeds 300 characters, our API automatically creates a Bluesky thread. This works similarly to X (Twitter) and Threads.

How it works:

Our system creates natural-looking threads by intelligently splitting your text:

It groups paragraphs: The system combines as many paragraphs (text separated by a blank line) as possible into a single post without exceeding 300 characters.

It splits long paragraphs: If a single paragraph is too long for one post, it's split into smaller parts. The system first tries to split by line breaks and then by words.

Example of a thread creation

If you send this text in the title:

The API will create a thread like this:

Post 1:

This is the first paragraph. It is short.

This second paragraph is a bit longer. Our API tries to keep paragraphs together.

Post 2:

This is a much longer third paragraph. It probably won't fit with the others. If so, the API will split it into multiple posts automatically.

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

Upload Text to X (Twitter)

Create a Twitter Thread

Upload Text to LinkedIn with Link Preview

Upload Text to LinkedIn (Personal Profile)

Upload Text to LinkedIn (Organization Page)

Upload Text to Facebook Page

Upload Text to Threads and Twitter (X)

Upload Text to Reddit

Upload Link Post to Reddit

Upload Text to Reddit with First Comment and Images

Upload Text to Bluesky

Upload Text to Bluesky with Link Preview

Create a Bluesky Thread

Upload Text to Multiple Platforms with Link Preview

Use the generic link\_url parameter to add a link preview card across all supported platforms at once:

Reply to a Tweet on X (Twitter)

Reply to a Tweet on X (Twitter) with Excluded Users

Responses

200 OK (synchronous, finished fast)

200 OK (asynchronous/background started or sync→background fallback)

202 Accepted (scheduled)

400 Bad Request

Missing title (content), user, platform\[], invalid platforms, Reddit without subreddit, invalid scheduled\_date. For Facebook without facebook\_page\_id, the per-platform result will include an error entry for facebook.

401 Unauthorized: { "success": false, "message": "Invalid or expired token" }

403 Forbidden (plan restrictions)

404 Not Found (e.g., user not found)

429 Too Many Requests (monthly limit exceeded; includes current usage)

500 Internal Server Error: { "success": false, "error": "Detailed error message" }

Notes

When async or when sync falls back to background, use GET /api/uploadposts/status?request\_id={request\_id} to poll progress.

Per-platform results are returned under results.{platform} and may include fields like url, platform-specific IDs, or error.
