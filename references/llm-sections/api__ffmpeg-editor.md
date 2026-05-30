---
source_id: llm-section:docs/api/ffmpeg-editor.md
public_url: https://docs.upload-post.com/api/ffmpeg-editor
raw_source: llm.txt
collected: 2026-05-30
---

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

Blocked characters/constructs to prevent command injection: ;, |, &, $, \\\`\`, $(, and destructive commands like rm/rmdir\`.

Newlines and carriage returns in the command string are automatically replaced with spaces (not blocked), so pasting multi-line commands works. To render multi-line text in the drawtext filter, use the literal escape \n (backslash + n) — in JSON, send \\\n.

If validation fails, the API returns 400 Bad Request with a helpful message.

Responses

202 Accepted (job created)

Check Job Status

Poll the job until it finishes.

Example response:

Statuses: PENDING, PROCESSING, FINISHED, ERROR.

Download Result

When status is FINISHED, download the processed file.

Response headers include the appropriate Content-Type and a Content-Disposition attachment filename (e.g., output.mp4, output.wav). The response body is the binary media.

Example: Convert to MP4 (H.264)

Example: Extract Audio to WAV

Concatenate/Merge multiple videos (NEW)

The API now supports multiple input files for operations like concatenation. You can use placeholders {input0}, {input1}, {input2}, etc., in full\_command.

Option A: Send multiple URLs (JSON endpoint)

Option B: Upload multiple files (multipart/form-data)

Concatenation examples

Simple concatenation (2 videos):

Concatenation with re-encoding (multiple videos):

Using the concat demuxer (no re-encoding — faster but requires identical formats):

First create a text file with the list of videos, upload it as file and the videos as file1, file2, etc.:

Important: The {input} placeholder still works as before (points to the first file). For multiple inputs, use {input0}, {input1}, {input2}, etc.

Limitation: The predefined presets (h264\_social, hevc\_social, copy\_mux) and ffmpeg\_args only support a single file. For multiple inputs, you must use full\_command.

Example: Draw Multi-Line Text on Video

Use the drawtext filter with \n for line breaks. In JSON, escape as \\\n:

JSON body example:

Quotas by Plan (minutes of media/month)

| Plan          | Minutes/Month |
|---------------|----------------|
| free          | 30             |
| basic         | 300            |
| professional  | 1000           |
| advanced      | 3000           |
| business      | 10000          |

Resets on the 1st of each month at 00:00 UTC.

Check Your FFmpeg Consumption

To check your current FFmpeg usage and remaining quota, use:

Example response:

You can also view your FFmpeg usage in the API Keys page or your Profile page.

Errors

400 Bad Request: Invalid or unsafe FFmpeg command, missing parameters.

401 Unauthorized: Invalid or expired API key.

404 Not Found: Job not found.

429 Too Many Requests: Monthly quota exceeded (response includes current usage when applicable).

500 Internal Server Error: Processing error.

Notes

Jobs are asynchronous; always poll the job status before attempting to download the output.

Quota checks use detected media duration to ensure fair usage across plans.
