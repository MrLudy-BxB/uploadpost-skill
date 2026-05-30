# Platform Data and Analytics

Collected: 2026-05-30

## Source Traceability

| Source | Public URL |
| --- | --- |
| `docs/api/get-analytics.md` | https://docs.upload-post.com/api/get-analytics |
| `docs/api/get-facebook-pages.md` | https://docs.upload-post.com/api/get-facebook-pages |
| `docs/api/get-google-business-locations.md` | https://docs.upload-post.com/api/get-google-business-locations |
| `docs/api/get-linkedin-pages.md` | https://docs.upload-post.com/api/get-linkedin-pages |
| `docs/api/get-pinterest-boards.md` | https://docs.upload-post.com/api/get-pinterest-boards |
| `docs/api/get-reddit-detailed-posts.md` | https://docs.upload-post.com/api/get-reddit-detailed-posts |
| `openapi.json` | https://docs.upload-post.com/openapi.json |

## Endpoint Routing

| Method | Path | Operation | Summary | Required fields |
| --- | --- | --- | --- | --- |
| `GET` | `/analytics/{profile_username}` | `getAnalytics` | Get analytics | profile_username, platforms |
| `GET` | `/uploadposts/facebook/pages` | `getFacebookPages` | Get Facebook pages | - |
| `GET` | `/uploadposts/linkedin/pages` | `getLinkedInPages` | Get LinkedIn pages | - |
| `GET` | `/uploadposts/pinterest/boards` | `getPinterestBoards` | Get Pinterest boards | - |
| `GET` | `/uploadposts/reddit/detailed-posts` | `getRedditDetailedPosts` | Get Reddit detailed posts | profile_username |
| `GET` | `/uploadposts/media` | `getUserMedia` | Get media list | platform, user |

## Practical Notes

- Use the endpoint index for exact method/path lookup and this reference for task-level routing.

## Source Excerpts

### docs/api/get-analytics.md

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

[Excerpt truncated in this topic reference; see the source URL for full page context.]

### docs/api/get-facebook-pages.md

This endpoint is crucial for uploads, as it provides you with the necessary ID to specify which Facebook Page you want to send your content to.

Get Facebook Pages

This endpoint allows you to get a list of all Facebook pages a user has access to through their connected accounts. This is a necessary step if you want to post to a specific page, as you will need its ID.

Method: GET

Endpoint: /api/uploadposts/facebook/pages

Authentication:

API Key in the Authorization header.

Authorization: Apikey \<YOUR\_API\_KEY>

Query Parameters:

| Parameter | Type   | Description                                                                                                                                                             | Required |
| :-------- | :----- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :--- |
| profile | string | Optional. The profile's username. If provided, the API will return only the Facebook pages associated with the Facebook account linked to that profile. | No   |

Successful Response (200 OK)

The response will include a list of objects, where each object represents a Facebook page.

Additional Notes:

To post on a Facebook page, you must pass the page id in the facebook\_page\_id parameter of the upload endpoint (/api/upload or /api/upload\_photos).

### docs/api/get-google-business-locations.md

This endpoint queries the Google Business Profile API in real-time to return all available locations for a connected account. Use this to let users choose which location to post to.

Get Google Business Locations

Method: GET

Endpoint: /api/uploadposts/google-business/locations

Authentication:

API Key in the Authorization header.

Authorization: Apikey \<YOUR\_API\_KEY>

Query Parameters:

| Parameter | Type   | Description                                                                                                                                                             | Required |
| :-------- | :----- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :--- |
| profile | string | Optional. The profile's username. If provided, the API will return only the locations associated with the Google Business account linked to that profile. | No   |

Successful Response (200 OK)

| Field      | Description                                                    |
| :--------- | :------------------------------------------------------------- |
| name     | The Google Business location identifier (used as gbp\_location\_id in uploads) |
| title    | Display name of the business location                          |
| account\_id | The internal Upload-Post account key                        |

Using Locations in Upload Requests

[Excerpt truncated in this topic reference; see the source URL for full page context.]

### docs/api/get-linkedin-pages.md

This endpoint is crucial for uploads, as it provides you with the necessary ID to specify which LinkedIn Page you want to send your content to.

Get LinkedIn Pages

Retrieves a list of LinkedIn company pages associated with the authenticated user's account(s).

Method: GET

Endpoint: /api/uploadposts/linkedin/pages

Authentication:

Type: Apikey Token

Header: Authorization: Apikey \<YOUR\_TOKEN>

Query Parameters:

| Parameter | Type   | Description                                                                                                                                                             | Required |
| :-------- | :----- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :--- |
| profile | string | Optional. The username of a specific profile. If provided, the endpoint will return only the LinkedIn pages associated with the LinkedIn account linked to that profile. If omitted, it will return pages from all LinkedIn accounts connected to the user. | No   |

Successful Response (200 OK)

A JSON object containing a list of the user's LinkedIn pages.

Field Descriptions:

id: The unique identifier (URN) for the LinkedIn organization. This is the value you should use when specifying a target\_linkedin\_page\_id in other API calls.

name: The display name of the LinkedIn page.

picture: The URL of the page's logo. Can be null.

account\_id: The internal identifier for the user's connected LinkedIn account in the Upload-Post system.

[Excerpt truncated in this topic reference; see the source URL for full page context.]

### docs/api/get-pinterest-boards.md

This endpoint is crucial for uploads, as it provides you with the necessary ID to specify which Pinterest Board you want to send your content to.

Get Pinterest Boards

This endpoint allows you to get a list of all boards (public and secret) from a connected Pinterest account. You will need a board ID to post a Pin to it.

Method: GET

Endpoint: /api/uploadposts/pinterest/boards

Authentication:

API Key in the Authorization header.

Authorization: Apikey \<YOUR\_API\_KEY>

Query Parameters:

| Parameter | Type   | Description                                                                                                                                                             | Required |
| :-------- | :----- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :--- |
| profile | string | Optional. The profile's username. If provided, the API will return only the boards from the Pinterest account linked to that specific profile.                 | No       |

Successful Response (200 OK)

The response will include a list of objects, where each object represents a Pinterest board.

Additional Notes:

To post a Pin, you must pass the board id in the pinterest\_board\_id parameter of the upload endpoint (/api/upload or /api/upload\_photos).

If a profile is not specified, the API will use the first Pinterest account it finds connected to the user. The response will tell you which account was used in the pinterest\_account\_used field.

### docs/api/get-reddit-detailed-posts.md

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

[Excerpt truncated in this topic reference; see the source URL for full page context.]
