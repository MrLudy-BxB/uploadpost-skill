---
source_id: llm-section:docs/api/get-facebook-pages.md
public_url: https://docs.upload-post.com/api/get-facebook-pages
raw_source: llm.txt
collected: 2026-05-30
---

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
