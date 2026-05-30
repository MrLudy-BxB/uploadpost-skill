---
source_id: llm-section:docs/api/get-analytics.md
public_url: https://docs.upload-post.com/api/get-analytics
raw_source: llm.txt
collected: 2026-05-30
---

GET /api/analytics/profile\_username

Retrieves analytics data for a specified user profile across one or more social media platforms.

Method: GET

Endpoint URL: https://api.upload-post.com/api/analytics/profile\_username

Description:

This endpoint provides key analytics metrics for a given social media profile associated with a user's account. It allows fetching data for multiple platforms in a single request. The system is designed to be extensible, with support for more platforms planned for the future.

Authentication:

A valid JSON Web Token (JWT) is required for authentication. The token must be included in the Authorization header as a Apikey token.

Authorization: Apikey \<YOUR\_JWT\_TOKEN>

Parameters:

| Parameter          | Type   | Location      | Required | Description                                                                                             |
| ------------------ | ------ | ------------- | -------- | ------------------------------------------------------------------------------------------------------- |
| profile\_username | string | Path          | Yes      | The unique username of the profile for which you want to retrieve analytics.                            |
| platforms        | string | Query         | Yes      | A comma-separated list of platforms to fetch analytics for. E.g., ?platforms=instagram,youtube,threads,pinterest,reddit. |
| page\_id          | string | Query         | No       | Required for Facebook analytics. The ID of the Facebook Page.                                           |
| page\_urn         | string | Query         | No       | Optional for LinkedIn. Defaults to "me" (personal profile). Use Organization URN/ID for pages.          |

Supported Platforms:

Currently, the following platforms are supported:

instagram

tiktok

Linkedin

Facebook

X (Twitter)

youtube

threads

pinterest

reddit

bluesky

Support for additional platforms will be added in the future. If you request a platform that is not yet supported, the response will include a message indicating this for that specific platform.

Here is an example of how to call the endpoint to get analytics for the test profile on Instagram, YouTube, Threads, Pinterest, and Reddit.

Example Successful Response (200 OK):

The response is a JSON object where each key corresponds to a requested platform. The value is another object containing the specific analytics data for that platform.

Field Descriptions for Platform Analytics:

followers: Total number of followers.

reach: The number of unique accounts that have seen any of the profile's content.

views: Total content views (Instagram, YouTube, TikTok). For Instagram, this is the official "views" metric from the Instagram API, which replaced the deprecated "impressions" metric.

impressions: Alias for views on Instagram, YouTube, and TikTok. On other platforms (X, Pinterest, Threads), this represents content impressions. Kept for backwards compatibility.

profileViews: Total number of times the profile was viewed. For Instagram, this represents "Accounts Engaged" (unique accounts that interacted with the content).

likes: Total number of likes across the profile's content.

comments: Total number of comments across the profile's content.

shares: Total number of shares across the profile's content.

saves: Total number of saves across the profile's content.

pin\_clicks: Total number of clicks on pins (Pinterest only).

outbound\_clicks: Total number of clicks to external URLs from pins (Pinterest only).

reach\_timeseries: An array of objects showing the daily reach or views value over the last 30 days. The dashboard filters this data client-side based on the selected date range.

metric\_type: Indicates what the reach\_timeseries represents for this platform. Values: "reach" (Facebook, Instagram, LinkedIn), "views" (YouTube, TikTok, Threads), "impressions" (X, Pinterest), "score" (Reddit). Use this to avoid double-counting when aggregating across platforms.

primary\_impressions\_field: The field name used as the primary metric for aggregation on this platform (e.g., "reach" for Instagram, "impressions" for YouTube).

available\_metrics: Array of available metric keys for this platform.

metric\_labels: Object mapping metric keys to user-friendly labels for this platform.

For a unified total impressions metric across platforms, see the Total Impressions section below.

Error Responses:

400 Bad Request: The platforms query parameter is missing or invalid.

401 Unauthorized: The JWT is missing, invalid, or expired.

404 Not Found: The specified profile\_username does not exist for the authenticated user.

500 Internal Server Error: An unexpected error occurred on the server while fetching the data.

title: 'Total Impressions API'

GET /api/uploadposts/total-impressions/profile\_username

Returns a unified "total impressions" metric for a profile, aggregated from daily analytics snapshots across all connected platforms.

Method: GET

Endpoint URL: https://api.upload-post.com/api/uploadposts/total-impressions/profile\_username

Description:

This endpoint provides a single, deduplicated "total impressions" metric that intelligently combines reach and views data across platforms. Since different platforms report different types of impression metrics (Facebook and Instagram report "reach", YouTube and TikTok report "views"), this endpoint uses the most representative metric for each platform to avoid double-counting.

You can also request custom metrics aggregation by specifying which metrics to aggregate using the metrics parameter.

Authentication:

A valid JSON Web Token (JWT) is required. Include it in the Authorization header:

Authorization: Apikey \<YOUR\_JWT\_TOKEN>

Parameters:

| Parameter          | Type   | Location | Required | Description                                                                                                  |
| ------------------ | ------ | -------- | -------- | ------------------------------------------------------------------------------------------------------------ |
| profile\_username | string | Path     | Yes      | The unique username of the profile.                                                                          |
| date             | string | Query    | No       | Single date in YYYY-MM-DD format. If provided, returns impressions for that day only.                      |
| start\_date       | string | Query    | No       | Start of date range in YYYY-MM-DD format. Defaults to 30 days ago.                                        |
| end\_date         | string | Query    | No       | End of date range in YYYY-MM-DD format. Defaults to today.                                                 |
| period           | string | Query    | No       | Shortcut for date range: last\_day, last\_week, last\_month, last\_3months, last\_year. Overrides start\_date/end\_date. |
| platform         | string | Query    | No       | Comma-separated list of platforms to filter by. E.g., ?platform=youtube,tiktok.                            |
| breakdown        | string | Query    | No       | Set to true to include per-platform and per-day breakdown in the response.                                 |
| metrics          | string | Query    | No       | Comma-separated list of metrics to aggregate. E.g., ?metrics=likes,comments,shares. Available: followers, reach, views, impressions, likes, comments, shares, saves, profileViews, video\_count, following, pin\_clicks, outbound\_clicks. When provided, returns a metrics object instead of total\_impressions. |

Metric Selection per Platform (default mode):

| Platform   | Metric Used        | Reason                                        |
| ---------- | ------------------ | --------------------------------------------- |
| Facebook   | reach              | Reports unique account reach                  |
| Instagram  | reach              | Reports unique account reach. Note: Instagram renamed "impressions" to "views" in their API; both views and impressions fields are returned. |
| LinkedIn   | reach              | Reports unique impressions                    |
| YouTube    | impressions/views  | Reports video view counts                     |
| TikTok     | impressions/views  | Reports video view counts                     |
| X          | impressions        | Reports tweet impression counts               |
| Threads    | impressions/views  | Reports content view counts                   |
| Pinterest  | impressions        | Reports pin impression counts                 |
| Reddit     | impressions/score  | Reports post score as impressions proxy       |

Example Request:

Example Response (200 OK):

Example with Period Shortcut:

Example with Custom Metrics:

Example with Single Date:

Example with Platform Filter:

Error Responses:

400 Bad Request: Invalid date format (must be YYYY-MM-DD) or invalid metric name.

401 Unauthorized: The JWT is missing, invalid, or expired.

500 Internal Server Error: An unexpected error occurred on the server.

GET /api/uploadposts/post-analytics/request\_id

Returns analytics for a specific post across all platforms it was published to.

Method: GET

Endpoint URL: https://api.upload-post.com/api/uploadposts/post-analytics/request\_id

Description:

This endpoint provides per-post analytics by looking up the upload record and cross-referencing with stored analytics snapshots. It returns the post metadata along with platform-specific metrics at the time of posting and the latest available metrics.

Authentication:

A valid JSON Web Token (JWT) is required. Include it in the Authorization header:

Authorization: Apikey \<YOUR\_JWT\_TOKEN>

Parameters:

| Parameter    | Type   | Location | Required | Description                              |
| ------------ | ------ | -------- | -------- | ---------------------------------------- |
| request\_id | string | Path     | Yes      | The request ID of the upload.            |
| platform   | string | Query    | No       | Filter to a single platform (e.g., ?platform=x). When provided, only metrics for that platform are fetched, which is significantly faster than fetching all platforms. |

Example Request:

Example Request (single platform):

Example Response (200 OK):

Per-Platform Response Fields:

success: Whether the post was successfully published to this platform.

platform\_post\_id: The post's ID on the platform.

post\_url: Direct URL to the published post.

post\_metrics: Live metrics fetched from the platform's API for this specific post (views, likes, comments, shares, etc.).

post\_metrics\_source: Source of post metrics (currently "platform\_api").

post\_metrics\_error: If post-level metrics could not be fetched, this field contains a human-readable error message explaining why.

profile\_snapshot\_at\_post\_date: Profile-level metrics snapshot from the day the post was published.

profile\_snapshot\_latest: Most recent profile-level metrics snapshot.

profile\_snapshot\_latest\_date: Date of the latest snapshot.

Error Responses:

401 Unauthorized: The JWT is missing, invalid, or expired.

404 Not Found: No post found with the given request ID.

500 Internal Server Error: An unexpected error occurred on the server.

GET /api/uploadposts/post-analytics?platform\_post\_id=

Returns analytics for any post (including organically published posts) using its native platform ID instead of a request ID.

Method: GET

Endpoint URL: https://api.upload-post.com/api/uploadposts/post-analytics?platform\_post\_id=XXXXX\&platform=instagram\&user=profile\_username

Description:

This endpoint allows you to fetch live per-post analytics using the platform's native post ID (e.g., an Instagram media ID) rather than an Upload Post request\_id. This is useful for retrieving metrics on organically published posts that were not uploaded through the API. You can obtain platform\_post\_id values from the GET /api/uploadposts/media endpoint.

Authentication:

A valid JSON Web Token (JWT) is required. Include it in the Authorization header:

Authorization: Apikey \<YOUR\_JWT\_TOKEN>

Parameters:

| Parameter          | Type   | Location | Required | Description                              |
| ------------------ | ------ | -------- | -------- | ---------------------------------------- |
| platform\_post\_id | string | Query    | Yes      | The native post ID on the platform (e.g., Instagram media ID). |
| platform         | string | Query    | Yes      | The platform to query. One of: youtube, tiktok, instagram, facebook, linkedin, x, threads, pinterest, reddit. |
| user             | string | Query    | Yes      | The profile\_username of the profile that owns the social account. |

Example Request:

Example Response (200 OK):

Response Fields:

post.source: Either "organic" (post was not uploaded via the API) or "api\_uploaded" (post was uploaded via the API and has an associated request\_id).

post.request\_id: Only present when source is "api\_uploaded" — the original upload request ID.

platforms.\<platform>.post\_metrics: Live metrics fetched from the platform's API.

platforms.\<platform>.post\_metrics\_source: Source of post metrics (currently "platform\_api").

platforms.\<platform>.post\_metrics\_error: If metrics could not be fetched, a human-readable error message.

platforms.\<platform>.available\_metrics: List of metrics available for this platform.

Error Responses:

400 Bad Request: Missing or invalid query parameters.

401 Unauthorized: The JWT is missing, invalid, or expired.

404 Not Found: User or profile not found.

500 Internal Server Error: An unexpected error occurred on the server.

GET /api/uploadposts/platform-metrics

Returns the available metrics configuration for all supported platforms.

Method: GET

Endpoint URL: https://api.upload-post.com/api/uploadposts/platform-metrics

Description:

This public endpoint returns the metrics configuration for each social platform, including which metrics are available and which field is used as the primary "impressions" metric for aggregation.

Example Response (200 OK):
