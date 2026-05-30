---
source_id: llm-section:docs/api/queue-system.md
public_url: https://docs.upload-post.com/api/queue-system
raw_source: llm.txt
collected: 2026-05-30
---

Queue System

The queue system allows you to automatically schedule posts to predefined time slots. Instead of specifying an exact date/time with scheduled\_date, you can use add\_to\_queue=true to have the system automatically assign your post to the next available slot.

How It Works

The queue is always active with default time slots (9am, 12pm, 5pm Eastern Time). You can customize these slots, timezone, and active days through the Queue Settings endpoints.

When you upload content with add\_to\_queue=true:

The system finds the next available slot based on your queue configuration

Your post is automatically scheduled to that slot

You receive a job\_id to track the scheduled post

Multiple Posts Per Slot

By default, each slot accepts 1 post. You can increase this with the max\_posts\_per\_slot setting to allow multiple posts in the same time slot. This is useful when you want to post to different platforms at the same time (e.g., an Instagram post and a Facebook post both at 9am).

You can also mark individual slots as full to prevent new posts from being added, even if they haven't reached the maximum capacity.

Using the Queue in Uploads

Add the add\_to\_queue parameter to any upload endpoint:

| Name | Type | Required | Description |
|------|------|----------|-------------|
| add\_to\_queue | Boolean | No | If true, automatically schedules the post to your next available queue slot. Cannot be used together with scheduled\_date. |
| max\_posts\_per\_slot | Integer | No | Override the profile's max\_posts\_per\_slot setting for this request. Only used when add\_to\_queue=true. |

Example Request

Example with Multiple Posts Per Slot

Success Response 202 Accepted

Get Queue Settings

Retrieve the current queue configuration for a profile.

| | |
|---|---|
| Endpoint | GET /api/uploadposts/queue/settings |
| Authentication | Required. Authorization: Apikey \<token> |

Query Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| profile\_username | String | Yes | The profile to get settings for |

Success Response 200 OK

| Field | Type | Description |
|-------|------|-------------|
| timezone | String | IANA timezone for the queue slots (e.g., "America/New\_York", "Europe/Madrid") |
| slots | Array | Array of time slots with hour (0-23) and minute (0-59) |
| days\_of\_week | Array | Active days: 0=Monday, 1=Tuesday, ..., 6=Sunday |
| max\_posts\_per\_slot | Integer | Maximum number of posts allowed per time slot (default: 1) |
| full\_slots | Array | List of ISO 8601 datetimes that have been manually marked as full |

Update Queue Settings

Update the queue configuration for a profile.

| | |
|---|---|
| Endpoint | POST /api/uploadposts/queue/settings |
| Authentication | Required. Authorization: Apikey \<token> |

Body Parameters (JSON)

| Name | Type | Required | Description |
|------|------|----------|-------------|
| profile\_username | String | Yes | The profile to update settings for |
| timezone | String | No | IANA timezone (e.g., "Europe/London"). See valid timezones. |
| slots | Array | No | Array of slot objects: \[{ "hour": 9, "minute": 0 }, ...]. Max 24 slots. |
| days\_of\_week | Array | No | Array of active days (0-6). Example: \[0, 1, 2, 3, 4] for Monday-Friday. |
| max\_posts\_per\_slot | Integer | No | Maximum posts per slot (1-100). Default: 1. Set higher to allow multiple posts in the same time slot. |

Example Request

Success Response 200 OK

Get Queue Preview

Preview the next upcoming queue slots and their availability.

| | |
|---|---|
| Endpoint | GET /api/uploadposts/queue/preview |
| Authentication | Required. Authorization: Apikey \<token> |

Query Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| profile\_username | String | Yes | The profile to preview |
| count | Integer | No | Number of slots to return (default: 10, max: 50) |

Success Response 200 OK

| Field | Type | Description |
|-------|------|-------------|
| post\_count | Integer | Number of posts currently scheduled in this slot |
| max\_posts\_per\_slot | Integer | Maximum posts allowed per slot |
| is\_full | Boolean | true if the slot is at capacity or manually marked as full |
| manually\_full | Boolean | true if the slot was manually marked as full via the Mark Slot Full endpoint |
| scheduled\_posts | Array | List of all posts scheduled in this slot (when multiple posts per slot is enabled) |
| scheduled\_post | Object | First scheduled post in the slot (for backward compatibility) |

Mark Slot Full

Manually mark a specific queue slot as full, preventing new posts from being added to it even if it hasn't reached max\_posts\_per\_slot.

| | |
|---|---|
| Endpoint | POST /api/uploadposts/queue/slot-full |
| Authentication | Required. Authorization: Apikey \<token> |

Body Parameters (JSON)

| Name | Type | Required | Description |
|------|------|----------|-------------|
| profile\_username | String | Yes | The profile to update |
| slot\_datetime | String | Yes | ISO 8601 datetime of the slot to mark as full (UTC) |

Example Request

Success Response 200 OK

Unmark Slot Full

Remove the full mark from a slot, allowing new posts to be added again.

| | |
|---|---|
| Endpoint | DELETE /api/uploadposts/queue/slot-full |
| Authentication | Required. Authorization: Apikey \<token> |

Body Parameters (JSON)

| Name | Type | Required | Description |
|------|------|----------|-------------|
| profile\_username | String | Yes | The profile to update |
| slot\_datetime | String | Yes | ISO 8601 datetime of the slot to unmark (UTC) |

Example Request

Success Response 200 OK

Get Next Available Slot

Get the next available queue slot for a profile.

| | |
|---|---|
| Endpoint | GET /api/uploadposts/queue/next-slot |
| Authentication | Required. Authorization: Apikey \<token> |

Query Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| profile\_username | String | Yes | The profile to check |

Success Response 200 OK

If no slots are available within the next 30 days:

Default Configuration

If you haven't customized your queue settings, these defaults apply:

| Setting | Default Value |
|---------|---------------|
| Timezone | America/New\_York (Eastern Time) |
| Slots | 9:00 AM, 12:00 PM, 5:00 PM |
| Days of week | All days (Monday-Sunday) |
| Max posts per slot | 1 |

See Also

Upload Video - Video upload endpoint

Upload Photos - Photo upload endpoint

Upload Text - Text post endpoint

Schedule Posts - Manage scheduled posts

Upload Status - Check upload/job status
