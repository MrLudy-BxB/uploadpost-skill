---
source_id: llm-section:docs/api/current-user.md
public_url: https://docs.upload-post.com/api/current-user
raw_source: llm.txt
collected: 2026-05-30
---

Current User API

Verify the validity of your API key and retrieve basic account information.

Authentication

Get Current User

Validates your API key and returns the associated email and subscription plan.

Endpoint

Headers

| Name          | Required | Description                |
|---------------|----------|----------------------------|
| Authorization | Yes      | Apikey YOUR\_API\_KEY      |

Example Request (curl)

Success Response (200 OK)

Response Fields:

| Field   | Type    | Description                                                        |
|---------|---------|--------------------------------------------------------------------|
| success | Boolean | Always true for successful requests                              |
| message | String  | Confirmation message                                               |
| email   | String  | The email address associated with the authenticated account        |
| plan    | String  | Current subscription plan (e.g., Basic, Professional, Business, Default) |
| preferences | Object | User preferences. See Preferences below.      |

Error Responses

401 Unauthorized - Invalid or missing authentication

500 Internal Server Error - Server-side error

Preferences

Manage user-level preferences via the preferences endpoint.

Get Preferences

Response (200 OK):

Update Preferences

Body (JSON):

| Field        | Type    | Description                                     |
|--------------|---------|-------------------------------------------------|
| weekStartDay | Integer | Calendar week start day. 0 = Sunday, 1 = Monday. |

Example:

Response (200 OK):

Error (400 Bad Request):

Use Cases

Token Validation: Verify that your API key or JWT is still valid before making other API calls

Plan Check: Determine the current subscription plan to understand available features and limits

Account Verification: Confirm which account is associated with your credentials
