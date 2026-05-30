---
source_id: llm-section:docs/api/get-pinterest-boards.md
public_url: https://docs.upload-post.com/api/get-pinterest-boards
raw_source: llm.txt
collected: 2026-05-30
---

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
