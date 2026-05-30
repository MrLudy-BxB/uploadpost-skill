---
source_id: llm-section:docs/api/webhooks.md
public_url: https://docs.upload-post.com/api/webhooks
raw_source: llm.txt
collected: 2026-05-30
---

Upload-Post allows you to receive real-time notifications about upload statuses and social account connection changes. This eliminates the need to poll endpoints for status updates.

Configuration

You can configure notifications in the Upload-Post Dashboard:

Configure Notifications

You can choose to receive notifications via:

Webhook: A POST request sent to your server with a JSON payload.

Telegram: A message sent to a configured Telegram chat.

Configuration via API

You can also configure your notification preferences programmatically using the API.

Endpoint: POST https://app.upload-post.com/api/uploadposts/users/notifications

Authentication: Requires a valid API Key.

Request Body:

Response:

Webhook Events

You can subscribe to specific event types using the webhook\_events object. Set each event key to true to receive it, or false to disable it. If omitted, all events are enabled by default.

| Event | Description |
| :--- | :--- |
| upload\_completed | Fired when an upload process completes (success or failure). |
| social\_account\_connected | Fired when a social account is connected or reconnected. |
| social\_account\_disconnected | Fired when a social account is disconnected (manually or automatically). |
| social\_account\_reauth\_required | Fired when a social account requires re-authentication. |

Webhook Payloads

upload\_completed

Sent when an upload process completes (whether successfully or with a failure).

Field Descriptions

| Field | Type | Description |
| :--- | :--- | :--- |
| event | string | The type of event: upload\_completed. |
| job\_id | string | The persistent job identifier returned when the post was created or scheduled. Use this to correlate webhook events with your original API requests. Only present when the upload was triggered via the API with a job\_id. |
| user\_email | string | The email address of the user who initiated the upload. |
| profile\_username | string | The username of the profile associated with the upload. |
| platform | string | The social platform where the post was uploaded (e.g., instagram, youtube, tiktok). |
| media\_type | string | The type of media uploaded (video, photo, or text). |
| title | string | The title provided for the post. |
| caption | string | The caption or description of the post. |
| result | object | An object containing the outcome of the upload attempt. |
| result.success | boolean | true if the upload was successful, false otherwise. |
| result.url | string | The direct URL to the published post (if available and successful). |
| result.publish\_id | string | The ID assigned to the post by the platform. |
| result.error | string | A description of the error if the upload failed. |
| created\_at | string | The timestamp of the event in ISO 8601 format. |

social\_account\_connected

Sent when a social account is successfully connected or reconnected via OAuth.

social\_account\_disconnected

Sent when a social account is disconnected. This can happen due to:

Manual disconnection by the user

Automatic disconnection due to persistent authentication failures (account blocked)

social\_account\_reauth\_required

Sent when a social account's access token can no longer be refreshed and the user must re-authenticate. This typically happens when:

The refresh token has expired

The user revoked access on the platform

Multiple consecutive token refresh attempts have failed

Connection Status Event Fields

| Field | Type | Description |
| :--- | :--- | :--- |
| event | string | The event type: social\_account\_connected, social\_account\_disconnected, or social\_account\_reauth\_required. |
| user\_email | string | The email address of the account owner. |
| platform | string | The social platform (e.g., instagram, youtube, tiktok, x, linkedin, facebook, threads, pinterest, reddit, bluesky, snapchat, google\_business, tiktok\_business). |
| account\_name | string | The account identifier on the platform (e.g., username, channel ID). |
| status | string | The new connection status: connected, disconnected, or reauth\_required. |
| profile\_username | string | The Upload-Post profile associated with this account (if applicable). |
| reason | string | Additional context for the status change (e.g., manual\_disconnect, account\_blocked, token\_refresh\_threshold\_exceeded, max\_auth\_strikes). Only present for disconnected and reauth\_required events. |
| created\_at | string | The timestamp of the event in ISO 8601 format. |

Usage Notes

Idempotency: While we strive to deliver each notification exactly once, you should handle potential duplicate events based on your own unique identifiers if necessary (though post\_id or publish\_id can serve this purpose for successful posts).

Security: Ensure your webhook endpoint is secure (HTTPS) and verify the data as needed for your application logic.

Event Filtering: Use the webhook\_events field in your notification settings to subscribe only to the events you need. If not specified, all events are enabled by default.

Replacing Polling: If you were previously polling the profile endpoint to check connection status, subscribe to social\_account\_connected, social\_account\_disconnected, and social\_account\_reauth\_required events instead.
