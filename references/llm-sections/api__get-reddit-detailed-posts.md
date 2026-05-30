---
source_id: llm-section:docs/api/get-reddit-detailed-posts.md
public_url: https://docs.upload-post.com/api/get-reddit-detailed-posts
raw_source: llm.txt
collected: 2026-05-30
---

GET /api/uploadposts/reddit/detailed-posts/

Retrieves detailed posts from a Reddit account connected to a profile, including complete media information (images, galleries, and videos).

Method: GET

Endpoint URL: https://api.upload-post.com/api/uploadposts/reddit/detailed-posts/

Authentication:

A valid JSON Web Token (JWT) is required for authentication. The token must be included in the Authorization header as a Bearer token.

Authorization: Bearer \<YOUR\_JWT\_TOKEN>

Query Parameters:

| Parameter          | Type   | Required | Description                                                      |
| ------------------ | ------ | -------- | ---------------------------------------------------------------- |
| profile\_username | string | Yes      | Username of the profile that has the Reddit account connected.   |

Example Request:

Example Successful Response (200 OK):

Response Fields:

| Field         | Type     | Description                                                   |
| ------------- | -------- | ------------------------------------------------------------- |
| id          | string   | Unique identifier of the Reddit post.                         |
| title       | string   | Title of the post.                                            |
| subreddit   | string   | Name of the subreddit where the post was published.           |
| body        | string   | Content of the post or URL if it's a link post.               |
| likes       | integer  | Number of upvotes on the post.                                |
| comments    | integer  | Number of comments on the post.                               |
| impressions | integer  | Number of views (uses view\_count if available, otherwise score). |
| has\_image   | boolean  | Indicates if the post contains images.                        |
| has\_video   | boolean  | Indicates if the post contains video.                         |
| media       | array    | Array of media objects attached to the post.                  |
| url         | string   | Direct URL to the post on Reddit.                             |
| created\_at  | string   | ISO 8601 timestamp of when the post was created.              |
| thumbnail   | string   | URL of the post's thumbnail image.                            |

Media Types:

| Type             | Description                      | Additional Fields               |
| ---------------- | -------------------------------- | ------------------------------- |
| image          | Individual or gallery image      | width, height               |
| video          | Video hosted on Reddit           | width, height, duration   |
| external\_video | External video (YouTube, etc.)   | thumbnail, provider         |

Error Responses:

| Code | Message                                              | Description                                      |
| ---- | ---------------------------------------------------- | ------------------------------------------------ |
| 400  | Query parameter "profile\_username" is required.   | The required parameter is missing.               |
| 400  | Profile 'X' has no Reddit account connected.      | The profile does not have a linked Reddit account. |
| 400  | Reddit account 'X' not found.                     | The Reddit account does not exist for the user.  |
| 500  | An internal server error occurred.                | Internal server error.                           |

Notes:

Supports automatic pagination, fetching up to 2000 posts (20 pages × 100 posts).

Gallery posts include all images in the media array.

Image URLs are already unescaped (no \&amp;).

impressions uses view\_count if available, otherwise uses score as a fallback.
