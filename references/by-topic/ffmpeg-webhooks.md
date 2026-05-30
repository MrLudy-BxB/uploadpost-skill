# FFmpeg Editor and Webhooks

Collected: 2026-05-30

## Source Traceability

| Source | Public URL |
| --- | --- |
| `docs/api/ffmpeg-editor.md` | https://docs.upload-post.com/api/ffmpeg-editor |
| `docs/api/webhooks.md` | https://docs.upload-post.com/api/webhooks |
| `openapi.json` | https://docs.upload-post.com/openapi.json |

## Endpoint Routing

| Method | Path | Operation | Summary | Required fields |
| --- | --- | --- | --- | --- |
| `POST` | `/uploadposts/ffmpeg/jobs/upload` | `createFfmpegJob` | Create FFmpeg job | file, full_command, output_extension |
| `GET` | `/uploadposts/ffmpeg/jobs/{job_id}` | `getFfmpegJobStatus` | Get FFmpeg job status | - |
| `GET` | `/uploadposts/ffmpeg/jobs/{job_id}/download` | `downloadFfmpegResult` | Download FFmpeg result | - |
| `GET` | `/uploadposts/ffmpeg/consumption` | `getFfmpegConsumption` | Get FFmpeg consumption | - |
| `POST` | `/uploadposts/users/notifications` | `configureNotifications` | Configure webhook notifications | - |

## Practical Notes

- Use the endpoint index for exact method/path lookup and this reference for task-level routing.

## Source Excerpts

### docs/api/ffmpeg-editor.md

FFmpeg Editor API

Process and transform media using your own FFmpeg command safely on our infrastructure. Submit a job with your media and a command template, then poll the job until it finishes and download the result.

Endpoint

Headers

| Name          | Value                    | Description                      |
|---------------|--------------------------|----------------------------------|
| Authorization | Apikey <YOUR_API_KEY> | Your API key for authentication. |

Parameters

| Name              | Type          | Required | Description |
|-------------------|---------------|----------|-------------|
| file              | File (binary) | Yes      | Media file to process. |
| full\_command      | String        | Yes      | FFmpeg command template that MUST use {input} and {output} placeholders. Example: ffmpeg -y -i {input} -c:v libx264 -crf 23 {output} |
| output\_extension  | String        | Yes      | Desired output file extension (e.g., mp4, wav, mp3, mov, webm). |

Note: If the duration of the input media cannot be detected, the system assumes 60 seconds for quota calculation.

Command Template Rules

For security and reliability, only safe FFmpeg commands are accepted.

Use placeholders: {input} (or indexed {input0}, {input1}, …) for input files and {output} for the output file; do not hardcode filenames. The first input can be referenced as {input} or {input0}.

Allowed pattern starts with ffmpeg and may include typical flags (e.g., -y, -i, -c:v, -c:a, -r, -b:v, filters, etc.).

[Excerpt truncated in this topic reference; see the source URL for full page context.]

### docs/api/webhooks.md

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

[Excerpt truncated in this topic reference; see the source URL for full page context.]
