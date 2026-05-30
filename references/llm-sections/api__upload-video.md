---
source_id: llm-section:docs/api/upload-video.md
public_url: https://docs.upload-post.com/api/upload-video
raw_source: llm.txt
collected: 2026-05-30
---

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
| timezone | String (IANA) | No | Optional timezone identifier (e.g., "Europe/Madrid", "America/New\_York"). If provided, scheduled\_date is interpreted in this timezone. Defaults to UTC if omitted. See IANA Time Zone Database for valid values. |
| request\_id | String | No | Client-provided request identifier. If omitted, the server generates one. Returned in every response and used to track the upload via Upload Status. Useful when async\_upload=true and the HTTP response might be lost (e.g., timeout). Can also be sent as an X-Request-Id header. |
| async\_upload | Boolean | No | If true, the request returns immediately with a request\_id and processes in the background. See Upload Status. |
| add\_to\_queue | Boolean | No | If true, automatically schedules the post to your next available queue slot. Cannot be used with scheduled\_date. See Queue System. |
| max\_posts\_per\_slot | Integer | No | Override the profile's max posts per slot setting for this request. Only used when add\_to\_queue=true. See Queue System. |
| first\_comment | String | No | Automatically post a first comment after publishing. Supported on Instagram, Facebook, Threads, Bluesky, Reddit, X, YouTube, and LinkedIn. On X (Twitter) and Threads, this creates a reply to the main post. For X threads, the comment is posted as a reply to the last tweet in the thread. On YouTube, it posts as a top-level comment on the video. |
| first\_comment\_media\[] | File(s) | No | Image files to attach to the first comment as inline images. Currently supported on Reddit. Not available for scheduled or queued posts. |

Important: If you set async\_upload to false but the upload takes longer than 59 seconds, it will automatically switch to asynchronous processing to avoid timeouts. In that case, use the request\_id with the Upload Status endpoint to check the upload status and result.

Scheduling behavior: When you provide scheduled\_date, the API responds with 202 Accepted and includes a job\_id. That same job\_id will later appear in Upload History so you can correlate the scheduled job with the eventual publish record. You can also use the job\_id with the Upload Status endpoint to check the execution status of the scheduled post.

Platform-Specific First Comments

The first\_comment parameter serves as a fallback. To set a custom first comment for a particular platform, use the optional \[platform]\_first\_comment parameter. If provided, it will override the main first\_comment for that platform.

Example Optional Parameters:

instagram\_first\_comment: "Follow for more content! #photography"

facebook\_first\_comment: "Let me know your thoughts in the comments!"

x\_first\_comment: "Thread incoming! 🧵"

threads\_first\_comment: "First comment on Threads!"

youtube\_first\_comment: "Subscribe for more videos!"

reddit\_first\_comment: "Source in the comments."

bluesky\_first\_comment: "More details in the replies."

linkedin\_first\_comment: "Source article in the comments."

Platform-Specific Titles

The title parameter serves as a fallback. To set a custom title for a particular platform, use the optional \[platform]\_title parameter. If provided, it will override the main title for that platform.

Example Optional Parameters:

instagram\_title: "Check out my latest reel on Instagram! #reels"

facebook\_title: "Excited to share this new video with my Facebook friends and family."

tiktok\_title: "New TikTok video just dropped! 🔥"

linkedin\_title: "A professional insight on the latest industry trends, discussed in this video."

x\_title: "New video out now! 📢"

youtube\_title: "My new YouTube video is live!"

pinterest\_title: "An inspiring video pin."

reddit\_title: "Check out this video!"

Platform-Specific Parameters

TikTok

For more information about Tiktok API parameters, visit the Tiktok API documentation.

| Name | Type | Required | Description | Default |
|------|------|----------|-------------|---------|
| tiktok\_title | String | No | Specific title for the TikTok post (max 90 characters for photo posts, 2200 for video). Fallbacks to title. | title |
| privacy\_level | String | No | Privacy setting ("PUBLIC\_TO\_EVERYONE", "MUTUAL\_FOLLOW\_FRIENDS", "FOLLOWER\_OF\_CREATOR", "SELF\_ONLY") | "PUBLIC\_TO\_EVERYONE" |
| disable\_duet | Boolean | No | Disable duet feature | false |
| disable\_comment | Boolean | No | Disable comments | false |
| disable\_stitch | Boolean | No | Disable stitch feature | false |
| post\_mode | String | No | DIRECT\_POST: Directly post the content to TikTok user's account or MEDIA\_UPLOAD: Upload content to TikTok for users to complete the post using TikTok's editing flow. Users will receive an inbox notification. | DIRECT\_POST |
| cover\_timestamp | Integer | No | Timestamp in milliseconds for video cover | 1000 |
| brand\_content\_toggle | Boolean| No       | Set to true for paid partnerships that promote third-party brands.        | false   |
| brand\_organic\_toggle | Boolean| No       | Set to true when promoting the creator's own business.                    | false   |
| is\_aigc | Boolean | No | Indicates if content is AI-generated | false |

Note on Draft Mode (MEDIA\_UPLOAD): When using MEDIA\_UPLOAD mode (Draft), TikTok does not allow setting a title, caption, privacy settings, or other metadata via the API. The video is simply uploaded to your TikTok inbox/drafts, and you must add the title, caption, and settings manually within the TikTok app before publishing.

The global description field is ignored for TikTok uploads.

Instagram

For more information about Instagram API parameters, visit the Instagram Graph API documentation.

| Name | Type | Required | Description | Default |
|------|------|----------|-------------|---------|
| instagram\_title | String | No | Specific title for the Instagram post. Fallbacks to title. | title |
| media\_type | String | No | Type of media ("REELS" or "STORIES") | "REELS" |
| share\_mode | String | No | Reel posting mode. See Trial Reels below for details. | "CUSTOM" |
| share\_to\_feed | Boolean | No | Whether to share to feed (only for regular Reels, not Trial Reels) | true |
| collaborators | String | No | Comma-separated list of collaborator usernames (not available for Trial Reels) | - |
| cover\_url | String | No | URL for custom video cover. You can also send a binary image via the cover\_image field (see below). | - |
| cover\_image | File | No | Binary cover image file (JPEG, ≤ 8MB). Uploaded to a public URL automatically. If both cover\_image and cover\_url are provided, cover\_url takes precedence. | - |
| audio\_name | String | No | Name of the audio track embedded in your video | - |
| user\_tags | String | No | Users to tag on the Reel. Accepts a comma-separated list (e.g., "@user1, user2") — Instagram does not require coordinates for video tags. | - |
| location\_id | String | No | Instagram location ID | - |
| thumb\_offset | String | No | Timestamp offset for video thumbnail, expressed in milliseconds | - |

Tagging on photo posts is different. For /upload\_photos, Instagram requires x/y coordinates in a JSON array and silently drops username-only tags. See Upload Photo → Instagram user\_tags.

Trial Reels (share\_mode)

Trial Reels allow you to test content with non-followers first to see how it performs before sharing with your followers. This feature is available for public Instagram accounts with at least 1,000 followers.

Available share\_mode values:

| Value | Description |
|-------|-------------|
| CUSTOM | Regular Reel (default) - Shown to all followers immediately |
| TRIAL\_REELS\_SHARE\_TO\_FOLLOWERS\_IF\_LIKED | Trial Reel with auto-share - Shown to non-followers first. If it performs well within 72 hours, Instagram automatically shares it with your followers |
| TRIAL\_REELS\_DONT\_SHARE\_TO\_FOLLOWERS | Trial Reel without auto-share - Shown only to non-followers. You decide later in the Instagram app if you want to share with followers |

Important notes about Trial Reels:

Only you can see that a Reel is marked as a Trial. To everyone else, it appears as a regular Reel.

Your followers won't see the Trial on your profile or in their feeds unless you (or Instagram, if auto-share is enabled) choose to share it.

Collaborators cannot be added to Trial Reels.

There may be limits on how many Trial Reels you can publish within a certain period.

Note on Instagram audio\_name

Scope: Reels only, and only for the original audio embedded in your uploaded video. It does not let you pick licensed/trending music from Instagram’s library via API.

Limit: You can rename only once (when creating the Reel via API, or later from the audio page if you are the audio owner).

Behavior: The Reel is published using the audio embedded in your video and displays the name you provide in audio\_name.

The global description field is ignored for Instagram video uploads.

LinkedIn

For more information about LinkedIn API parameters, visit the LinkedIn Marketing API documentation.

| Name                    | Type   | Required | Description                                          | Default     |
|-------------------------|--------|----------|------------------------------------------------------|-------------|
| linkedin\_title          | String | No       | Specific title for the LinkedIn post. Fallbacks to title. | title |
| linkedin\_description or description    | String | No       | Sent as the LinkedIn commentary. If omitted, we reuse title. | title |
| visibility              | String | Yes      | Visibility setting ("CONNECTIONS", "PUBLIC", "LOGGED\_IN", "CONTAINER") | "PUBLIC"    |
| target\_linkedin\_page\_id | String | No       | LinkedIn page ID to upload videos to an organization | "107579166" |

YouTube

For more information about YouTube API parameters, visit the YouTube Data API documentation.

| Name | Type | Required | Description | Default |
|------|------|----------|-------------|---------|
| youtube\_title | String | No | Specific title for the YouTube video. Fallbacks to title. | title |
| youtube\_description or description | String | No | Populates snippet.description. If omitted, we send title. | title |
| tags | Array | No | Array of tags | \[] |
| categoryId | String | No | Video category | "22" |
| privacyStatus | String | No | Privacy setting ("public", "unlisted", "private") | "public" |
| embeddable | Boolean | No | Whether video is embeddable | true |
| license | String | No | Video license ("youtube", "creativeCommon") | "youtube" |
| publicStatsViewable | Boolean | No | Whether public stats are viewable | true |
| thumbnail | File | No | Custom thumbnail image to set after upload. Accepts a multipart image file or a public URL. Formats: JPG/PNG/GIF/BMP. Max 2 MB. If both thumbnail (file) and thumbnail\_url are provided, the file takes precedence. YouTube custom thumbnails are not supported for Shorts; they only apply to standard YouTube videos. | - |
| thumbnail\_url | String (URL) | No | Alternative to provide the thumbnail as a public URL. | - |
| selfDeclaredMadeForKids | Boolean | No | Explicit declaration that the video is made for children | false |
| containsSyntheticMedia | Boolean | No | Declaration that the video contains synthetic or AI-generated content | false |
| defaultLanguage | String | No | Language of title and description (BCP-47 code, e.g., "es", "en") | - |
| defaultAudioLanguage | String | No | Language of the video audio (BCP-47 code, e.g., "es-ES", "en-US") | - |
| allowedCountries | String | No | Comma-separated list of country codes where the video is allowed (e.g., "US,CA,MX") | - |
| blockedCountries | String | No | Comma-separated list of country codes where the video is blocked (e.g., "CN,RU") | - |
| hasPaidProductPlacement | Boolean | No | Declaration that the video includes paid product placements | false |
| recordingDate | String | No | Recording date and time of the video (ISO 8601 format, e.g., "2024-01-15T14:30:00Z") | - |
| youtube\_subtitle\_file | File | No | Subtitle file to upload to the video. Supported formats: SRT, VTT, SBV, SUB, ASS, SSA, TTML, DFXP. Must be accompanied by youtube\_subtitle\_language. | - |
| youtube\_subtitle\_language | String | No | BCP-47 language code for the subtitle file (e.g., "en", "es", "fr"). Required when uploading subtitles. | - |
| youtube\_subtitle\_name | String | No | Display name for the subtitle track (e.g., "English", "Español"). Defaults to the language code if not provided. | - |
| youtube\_subtitle\_file\_{N} | File | No | Indexed subtitle file for multiple tracks (N = 0, 1, 2...). Use with youtube\_subtitle\_language\_{N}. | - |
| youtube\_subtitle\_language\_{N} | String | No | Language code for indexed subtitle track N. | - |
| youtube\_subtitle\_name\_{N} | String | No | Display name for indexed subtitle track N. | - |

Important: YouTube custom thumbnails are not supported for Shorts; they only apply to standard YouTube videos.

Notes about YouTube parameters:

Subtitles: You can upload multiple subtitle files by using indexed fields (youtube\_subtitle\_file\_0, youtube\_subtitle\_language\_0, youtube\_subtitle\_file\_1, youtube\_subtitle\_language\_1, etc.). Each subtitle file requires a corresponding language code. Subtitles are uploaded after the video is processed using the YouTube Captions API.

Region restrictions: allowedCountries and blockedCountries cannot be used simultaneously. Country codes must be ISO 3166-1 alpha-2 (e.g., "US", "CA", "MX").

Language settings: defaultLanguage affects title and description display, while defaultAudioLanguage specifies the spoken language in the video. Use BCP-47 codes (e.g., "es" for Spanish, "es-ES" for Spain Spanish).

Legal declarations: selfDeclaredMadeForKids is used for COPPA compliance. containsSyntheticMedia provides transparency for AI-generated content. hasPaidProductPlacement ensures FTC compliance.

Facebook

For more information about Facebook API parameters, visit the Facebook Graph API documentation and the Facebook Video API Publishing Guide.

| Name             | Type   | Required | Description                                                       | Default     |
|------------------|--------|----------|-------------------------------------------------------------------|-------------|
| facebook\_title   | String | No       | Specific title for the Facebook post. Fallbacks to title. Note: If facebook\_media\_type is "STORIES", this field is ignored.       | title |
| facebook\_description or description      | String | No       | Sent as description for the video. Note: If facebook\_media\_type is "STORIES", this field is ignored. | title |
| facebook\_page\_id | String | Yes      | Facebook Page ID where the video will be posted                   | -           |
| facebook\_media\_type | String | No | Type of media: "REELS" (short-form 9:16), "STORIES" (24h ephemeral), or "VIDEO" (normal page video, any aspect ratio, up to 4 hours) | "REELS" |
| video\_state      | String | No       | Desired state of the video ("DRAFT", "PUBLISHED")    | "PUBLISHED" |
| thumbnail\_url    | String | No       | Public URL of an image to set as the video thumbnail. Only supported when facebook\_media\_type is "VIDEO". Uses the Facebook Video Thumbnails API. | -           |

Normal page videos (VIDEO): Use facebook\_media\_type=VIDEO to upload regular videos to a Facebook Page (not Reels or Stories). These videos have no forced 9:16 aspect ratio, support durations up to 4 hours, and support custom thumbnails via the thumbnail\_url parameter.

Note: If facebook\_page\_id is not provided, we will automatically use the user's only connected Page (if exactly one exists). If multiple Pages are connected, the API returns a helpful error with an available\_pages list so you can choose one. Posting to personal Facebook profiles via API is not supported by Meta; only Pages can be posted to.

Threads

For more information about Threads API parameters, visit the Threads API documentation.

| Name | Type | Required | Description | Default |
|------|------|----------|-------------|---------|
| threads\_title | String | No | Specific title for the Threads post. Fallbacks to title. | title |
| threads\_topic\_tag | String | No | A topic tag for the post (1-50 characters). Cannot contain periods (.) or ampersands (&). One tag per post. Helps increase reach. | - |

The global description field is ignored for Threads video uploads.

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

Why: X bills $0.200 per post that contains a URL vs $0.015 without —
13× more. Stripping happens on every X path (video, photo, text, scheduled
and retried). See the
Character Limits page
for details.
:::

For more information about X API parameters, visit the X API Post Creation documentation.

| Name                        | Type    | Required | Description                                                                                                                           | Default     |
|-----------------------------|---------|----------|---------------------------------------------------------------------------------------------------------------------------------------|-------------|
| x\_title                      | String  | No       | Specific title for the tweet. Fallbacks to title.                                                                                   | title     |
| x\_long\_text\_as\_post          | Boolean | No       | When true, publishes long text as a single post. Otherwise, creates a thread.                                                      | false     |
| reply\_settings               | String  | No       | Controls who can reply to the tweet ("following", "mentionedUsers", "subscribers", "verified")                                       | -           |
| geo\_place\_id                 | String  | No       | Place ID for adding geographic location to the tweet                                                                                  | -           |
| nullcast                     | Boolean | No       | Whether to publish without broadcasting (promotional/promoted-only posts)                                                             | false     |
| for\_super\_followers\_only     | Boolean | No       | Tweet exclusive for super followers                                                                                                   | false     |
| community\_id                 | String  | No       | Community ID for posting to specific communities                                                                                      | -           |
| share\_with\_followers         | Boolean | No       | Share community post with followers                                                                                                   | false     |
| direct\_message\_deep\_link     | String  | No       | Link to take the conversation from public timeline to private Direct Message                                                         | -           |
| tagged\_user\_ids              | Array   | No       | Array of user IDs to tag in the media (max 10 users)                                                                                  | \[]          |
| reply\_to\_id                  | String  | No       | ID of the tweet to reply to. Creates a reply to the specified tweet.                                                                  | -           |
| exclude\_reply\_user\_ids       | Array   | No       | Array of user IDs to exclude from replying to this tweet. Requires reply\_to\_id.                                                    | \[]          |

The global description field is ignored for X uploads.

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

For posts that include photos or videos, all media is attached only to the first tweet of the thread. The subsequent tweets in the thread will be text-only replies.

Pinterest

| Name                                   | Type   | Required | Description                                                                              | Default |
|----------------------------------------|--------|----------|------------------------------------------------------------------------------------------|---------|
| pinterest\_title                        | String | No       | Specific title for the Pinterest Pin. Fallbacks to title.                              | title |
| pinterest\_description or description    | String | No       | Populates pin.description. If omitted, we reuse title.                                | title |
| pinterest\_board\_id                     | String | Yes       | Pinterest board ID to publish the video to.                                              | -       |
| pinterest\_alt\_text   | String | No       | Alt text for the video.                      | -       |
| pinterest\_link                         | String | No       | Destination link for the video Pin.                                                      | -       |
| pinterest\_cover\_image\_url              | String | No       | URL of an image to use as the video cover.                                               | -       |
| pinterest\_cover\_image\_content\_type     | String | No       | Content type of the cover image (e.g., image/jpeg, image/png), used if pinterest\_cover\_image\_data is provided. | -       |
| pinterest\_cover\_image\_data             | String | No       | Base64 encoded cover image data, used if pinterest\_cover\_image\_content\_type is provided. | -       |
| pinterest\_cover\_image\_key\_frame\_time | Integer| No       | Time in milliseconds of the video frame to use as cover.                                 | -       |

Bluesky

| Name | Type | Required | Description | Default |
|---|---|---|---|---|
| bluesky\_title | String | No | Specific text for the Bluesky video post. Fallbacks to title. | title |

Note: Video uploads to Bluesky are limited to 10GB per day/user and 25 videos per day, 100MBs Maximum, and up to 3 minutes (180 seconds) in duration. Supported formats: .mp4, .mpeg, .webm, .mov.

Reddit

| Name | Type | Required | Description | Default |
|---|---|---|---|---|
| reddit\_title | String | No | Specific title for the Reddit post. Fallbacks to title. | title |
| subreddit | String | Yes | Name of the subreddit to post to (without "r/"). | - |
| flair\_id | String | No | ID of the flair to apply to the post. | - |

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

Upload a Video to TikTok

Upload a Video to YouTube Using URL

Upload a Video to YouTube With Custom Thumbnail

Upload to YouTube with thumbnail file

Upload to YouTube with subtitle files

Responses

200 OK (synchronous, finished fast)

200 OK (asynchronous/background started, including sync→background fallback)

202 Accepted (scheduled)

400 Bad Request

Missing user, platform\[], video file/URL, invalid scheduled\_date, invalid platform values, Pinterest without pinterest\_board\_id.

401 Unauthorized

403 Forbidden (e.g., TikTok on Free plan)

404 Not Found (e.g., user not found after auth)

429 Too Many Requests (monthly limit exceeded; includes current usage)

500 Internal Server Error

Notes

When async or when sync falls back to background, use GET /api/uploadposts/status?request\_id={request\_id} to poll progress.

Per-platform results may include: url, publish\_id, container\_id, post\_id, video\_urn, video\_reel\_id, video\_id, image\_urns, post\_ids, video\_was\_transcoded, changes, prevalidation\_metadata, or error.
