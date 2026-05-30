---
source_id: llm-section:docs/api/get-linkedin-pages.md
public_url: https://docs.upload-post.com/api/get-linkedin-pages
raw_source: llm.txt
collected: 2026-05-30
---

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

vanityName: The custom "vanity" URL of the page (e.g., the part that comes after linkedin.com/company/). Can be null.

Error Responses:

401 Unauthorized: If the Authorization header is missing or the token is invalid.

404 Not Found:

If the user associated with the token is not found.

If a profile username is provided but not found for that user.

If no LinkedIn accounts are connected to the user or the specified profile.

If no LinkedIn pages are found for the connected accounts.

500 Internal Server Error: If there's an issue communicating with the LinkedIn API or an unexpected server error occurs.
