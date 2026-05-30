---
source_id: llm-section:docs/api/get-google-business-locations.md
public_url: https://docs.upload-post.com/api/get-google-business-locations
raw_source: llm.txt
collected: 2026-05-30
---

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

The gbp\_location\_id parameter tells the API which location to post to. First list the available locations using the endpoint above, then include the selected location's name as gbp\_location\_id in your upload request.

| Parameter        | Type   | Description                                              | Required |
| :--------------- | :----- | :------------------------------------------------------- | :------- |
| gbp\_location\_id | string | The location to post to. Must be a valid location name from the locations list. | No\*  |

This parameter works with all upload endpoints (/api/upload, /api/upload\_photos, /api/upload\_text).

Auto-select fallback: If gbp\_location\_id is not provided, the API will automatically query your available locations. If your account has exactly one location, it will be used automatically. If you have multiple locations, the API returns an error asking you to select one. If you have zero locations, the API returns an error indicating no locations were found.

Example:

Additional Notes:

Locations are queried live from the Google Business API each time you call this endpoint.

The endpoint handles token refresh automatically if the stored access token has expired.

Connect your Google account via OAuth first — the connection stores your credentials, and this endpoint uses them to fetch locations in real-time.

Works the same way as Facebook pages: connect once, then select which location to post to on each upload.
