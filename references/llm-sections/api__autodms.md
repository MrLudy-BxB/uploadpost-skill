---
source_id: llm-section:docs/api/autodms.md
public_url: https://docs.upload-post.com/api/autodms
raw_source: llm.txt
collected: 2026-05-30
---

AutoDM Monitors

Set up persistent monitors that automatically send private DMs to users who comment on your Instagram posts. Monitors run in the background 24/7 — no need to keep polling manually.

Start a Monitor

Create a new AutoDM monitor for an Instagram post. The monitor will check for new comments at regular intervals and send a private reply (DM) to each new commenter.

Endpoint

Headers

| Name          | Value                    | Description                     |
|---------------|--------------------------|---------------------------------|
| Authorization | Apikey <YOUR_API_KEY> | Your API key for authentication |

Body Parameters (JSON)

| Name                | Type           | Required | Description                                                                                     |
|---------------------|----------------|----------|-------------------------------------------------------------------------------------------------|
| post\_url            | String         | Yes      | The Instagram post URL to monitor for comments.                                                |
| reply\_message       | String         | Yes      | The DM message to send to each matching commenter.                                             |
| profile\_username    | String         | Yes      | Profile username (as configured in Upload-Post). Must have Instagram connected.                |
| monitoring\_interval | Integer        | No       | Minutes between comment checks. Default: 15. Minimum: 15.                                 |
| trigger\_keywords    | Array / String | No       | Keywords to filter comments. Only comments containing at least one keyword will receive a DM. Case-insensitive and accent-insensitive ("guide" matches "GUIDE", "guía", "güide", etc.). If omitted, all commenters receive a DM. Accepts a single string or an array of strings. |

Limits

2 new monitors per profile per day. You can create up to 2 monitors per profile in a 24-hour period.

No duplicate posts. If there's already an active monitor for a post URL, you must stop it before creating a new one.

Auto-expiration. Monitors automatically stop after 15 days.

Daily DM limits per plan. Free: 10 DMs/day. Paid: 500 DMs/day. When the limit is reached, the monitor pauses and resumes the next day.

Example Request

Responses

200 OK (monitor started)

400 Bad Request (missing fields, no Instagram connected, or duplicate post)

429 Too Many Requests (daily limit reached)

Get Monitor Status

Retrieve the status of AutoDM monitors for your account. By default returns active monitors (running / paused). Pass include\_inactive=true to also receive stopped and expired monitors so you can recover monitor IDs without keeping your own database.

Endpoint

Headers

| Name          | Value                    | Description                     |
|---------------|--------------------------|---------------------------------|
| Authorization | Apikey <YOUR_API_KEY> | Your API key for authentication |

Query Parameters

| Name             | Type   | Required | Description                                                                                                       |
|------------------|--------|----------|-------------------------------------------------------------------------------------------------------------------|
| include\_inactive | Bool   | No       | When true, also returns stopped and expired monitors. Deleted monitors are always excluded. Default: false.   |

Example Requests

Responses

200 OK

Status values: running, paused, resuming, stopped, expired

running — thread is currently checking comments and replying.

paused — temporarily halted (data preserved, can be resumed).

resuming — monitor was active in DB but no live thread; one was just started.

stopped — manually stopped via POST /autodms/stop. Only returned when include\_inactive=true.

expired — auto-stopped after the 15-day lifetime. Only returned when include\_inactive=true.

Response fields:

total\_active — count of monitors with status running, paused, or resuming. Unchanged by include\_inactive so existing dashboards keep working.

total — count of every monitor in the response.

stopped\_at / stop\_reason — only set for monitors with is\_active: false.

Get Monitor Logs

Retrieve activity logs for a specific monitor.

Endpoint

Headers

| Name          | Value                    | Description                     |
|---------------|--------------------------|---------------------------------|
| Authorization | Apikey <YOUR_API_KEY> | Your API key for authentication |

Query Parameters

| Name       | Type   | Required | Description              |
|------------|--------|----------|--------------------------|
| monitor\_id | String | Yes      | The monitor ID to query. |

Example Request

Responses

200 OK

Pause a Monitor

Temporarily pause a monitor without losing its configuration. The monitor stops checking for comments but can be resumed later.

Endpoint

Headers

| Name          | Value                    | Description                     |
|---------------|--------------------------|---------------------------------|
| Authorization | Apikey <YOUR_API_KEY> | Your API key for authentication |

Body Parameters (JSON)

| Name       | Type   | Required | Description              |
|------------|--------|----------|--------------------------|
| monitor\_id | String | Yes      | The monitor ID to pause. |

Example Request

Responses

200 OK

Resume a Monitor

Resume a previously paused monitor. It will continue checking for new comments from where it left off.

Endpoint

Headers

| Name          | Value                    | Description                     |
|---------------|--------------------------|---------------------------------|
| Authorization | Apikey <YOUR_API_KEY> | Your API key for authentication |

Body Parameters (JSON)

| Name       | Type   | Required | Description               |
|------------|--------|----------|---------------------------|
| monitor\_id | String | Yes      | The monitor ID to resume. |

Example Request

Responses

200 OK

Stop a Monitor

Deactivate a monitor. The monitor stops running but its data is preserved.

Endpoint

Headers

| Name          | Value                    | Description                     |
|---------------|--------------------------|---------------------------------|
| Authorization | Apikey <YOUR_API_KEY> | Your API key for authentication |

Body Parameters (JSON)

| Name       | Type   | Required | Description             |
|------------|--------|----------|-------------------------|
| monitor\_id | String | Yes      | The monitor ID to stop. |

Example Request

Responses

200 OK

Delete a Monitor

Permanently delete a monitor and all its data.

Endpoint

Headers

| Name          | Value                    | Description                     |
|---------------|--------------------------|---------------------------------|
| Authorization | Apikey <YOUR_API_KEY> | Your API key for authentication |

Body Parameters (JSON)

| Name       | Type   | Required | Description               |
|------------|--------|----------|---------------------------|
| monitor\_id | String | Yes      | The monitor ID to delete. |

Example Request

Responses

200 OK

404 Not Found

Important Notes

Instagram only. AutoDM monitors currently support Instagram only, using Meta's official Private Replies API.

One DM per comment. Each comment can only receive one private reply. Duplicate attempts are automatically prevented.

7-day comment window. Private replies can only be sent to comments less than 7 days old.

Daily DM limits. Upload-Post enforces daily DM limits per account (varies by plan). When the limit is reached, the monitor pauses and resumes the next day.

Auto-expiration. Monitors automatically stop after 15 days to prevent stale monitors from running indefinitely.

Rate limits. Meta enforces a limit of 200 DMs per hour per Instagram account. The monitor includes built-in delays between DMs to stay within limits.

How It Works

You start a monitor with a post URL and reply message.

Every monitoring\_interval minutes, the monitor checks for new comments.

For each new comment, it sends a private DM using Meta's Private Replies API.

It tracks which comments have already been replied to, avoiding duplicates.

After 15 days, the monitor automatically stops.

Related Endpoints

Instagram Comments — Read comments and send one-off private replies manually.

Instagram Direct Messages — Send follow-up DMs and read conversations.

Media List — Find post IDs and URLs for your Instagram content.
