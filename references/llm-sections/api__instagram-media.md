---
source_id: llm-section:docs/api/instagram-media.md
public_url: https://docs.upload-post.com/api/instagram-media
raw_source: llm.txt
collected: 2026-05-30
---

Media List

Retrieve a list of recent media (posts, reels, videos, pins, tweets, etc.) from a connected social media account. Supports all major platforms: Instagram, TikTok, YouTube, LinkedIn, Facebook, X (Twitter), Threads, Pinterest, Bluesky, and Reddit.

Useful for building post selectors, displaying recent content, or getting media IDs for other API calls.

Get User Media

Endpoint

Headers

| Name          | Value                    | Description                     |
|---------------|--------------------------|---------------------------------|
| Authorization | Apikey <YOUR_API_KEY> | Your API key for authentication |

Query Parameters

| Name     | Type   | Required | Description                                                      |
|----------|--------|----------|------------------------------------------------------------------|
| platform | String | Yes      | The platform to retrieve media from. See Supported Platforms. |
| user     | String | Yes      | Profile username (as configured in Upload-Post).                 |
| page\_urn | String | No       | LinkedIn only. Selects which LinkedIn page to fetch posts from. Accepts a numeric organization ID (e.g., 12345), a full URN (e.g., urn:li:organization:12345), or me to force the connected member's personal profile. If omitted, the endpoint targets the page that was active when the account was linked — for accounts connected as an organization admin, the first administered organization is auto-resolved; otherwise the personal profile is used. Use the LinkedIn Pages endpoint to list available organizations. |

Supported Platforms

| Platform    | Value        | Description                                |
|-------------|--------------|--------------------------------------------|
| Instagram   | instagram  | Posts, reels, and carousels                |
| TikTok      | tiktok     | Videos                                     |
| YouTube     | youtube    | Videos from the channel's uploads playlist |
| LinkedIn    | linkedin   | Posts (text, images, articles)             |
| Facebook    | facebook   | Page/profile posts                         |
| X (Twitter) | x          | Tweets with media information              |
| Threads     | threads    | Thread posts                               |
| Pinterest   | pinterest  | Pins                                       |
| Bluesky     | bluesky    | Posts (skeets)                             |
| Reddit      | reddit     | Submissions                                |

Example Requests

Instagram:

TikTok:

YouTube:

LinkedIn (personal profile):

LinkedIn (organization page):

LinkedIn (force the personal profile of an account connected as an org admin):

Responses

200 OK

400 Bad Request

500 Internal Server Error

Response Fields

All platforms return media items with a consistent structure:

| Field           | Type        | Description                                              |
|-----------------|-------------|----------------------------------------------------------|
| id            | String      | Platform-specific unique identifier for the media item   |
| caption       | String      | Text content, caption, or title of the post              |
| media\_type    | String      | Type of media. See Media Types           |
| media\_url     | String/null | Direct URL to the media file (image or video). See Media URL Availability |
| permalink     | String/null | Direct URL to the post on the platform                   |
| timestamp     | String/null | ISO 8601 timestamp of when the post was created          |
| thumbnail\_url | String/null | URL of the thumbnail/preview image (if available)        |

Media Types

| Type             | Description                                    |
|------------------|------------------------------------------------|
| IMAGE          | Single photo post or image pin                 |
| VIDEO          | Video post, reel, or video pin                 |
| CAROUSEL\_ALBUM | Multi-image/video post (carousel)              |
| TEXT           | Text-only post (no media attached)             |

Media URL Availability

The media\_url field returns a direct URL to the media file (image or video) when available. Support varies by platform:

| Platform    | media\_url Support | Details                                                  |
|-------------|---------------------|----------------------------------------------------------|
| Instagram   | Yes                 | Direct image/video URL. Not available for CAROUSEL\_ALBUM parent (use children). May be omitted for copyrighted content. URLs are temporary. |
| Threads     | Yes                 | Direct image/video URL, same behavior as Instagram.       |
| Facebook    | Yes                 | Image URL or playable video URL via attachments.          |
| X (Twitter) | Yes                 | Direct photo URL. For videos, returns the preview image URL. |
| LinkedIn    | Yes                 | Resolved via Images/Videos API. URLs are signed and temporary. |
| Reddit      | Yes                 | Direct i.redd.it image URL, v.redd.it video URL (video-only, no audio), or first gallery image URL. |
| Bluesky     | Yes                 | fullsize CDN image (up to 2000px) or HLS playlist URL (.m3u8) for videos. |
| Pinterest   | Yes                 | Largest available image URL (up to 1200px or original). Video URLs are restricted by Pinterest. |
| TikTok      | No                  | TikTok API does not expose direct video file URLs. Use permalink instead. |
| YouTube     | No                  | YouTube API does not provide direct video URLs (prohibited by ToS). Use permalink instead. |

:::warning
Media URLs from most platforms are temporary and will expire after some time (hours to days). Do not store them permanently — re-fetch from the API when needed, or download the media file to your own storage.
:::

Common Use Cases

Post selector UI: Display the user's recent posts so they can pick one for comment monitoring or AutoDMs.

Get media IDs: Use the id field from the response as the post\_id parameter in the Comments endpoint.

Content overview: Show a dashboard of recent content across all platforms with permalinks and captions.

Cross-platform analytics: Aggregate media from multiple platforms to display a unified content calendar.
