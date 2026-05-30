---
source_id: llm-section:docs/guides/ai-shorts-uploader.md
public_url: https://docs.upload-post.com/guides/ai-shorts-uploader
raw_source: llm.txt
collected: 2026-05-30
---

AI Shorts Uploader

The AI Shorts Uploader is a built-in assistant in the Upload-Post dashboard that watches your short-form video and writes the title, description and hashtags for each platform you publish to — YouTube, Instagram and TikTok — in seconds.

It is available from the Shorts Uploader screen in the app via the Generate with AI button.

What it does

When you click Generate with AI, the platform:

Sends your video to a multimodal AI model (Gemini).

Analyses the visual content, pacing and on-screen text.

Returns optimized copy per platform:

YouTube — title (≤100 chars) + description (≤5,000 chars) with relevant hashtags.

Instagram — caption (≤2,200 chars) tuned for Reels.

TikTok — caption (≤150 chars) tuned for the For You feed.

You can also rewrite an already-generated set of captions into another language (Spanish, English, Japanese, …) without re-uploading the video. Each rewrite counts as one analysis.

Monthly quotas per plan

Each successful analysis (and each language rewrite) consumes one unit from your monthly quota. The counter resets every 30 days from your last reset.

| Plan          | Analyses per month |
| :------------ | -----------------: |
| Free          |                 10 |
| Basic         |                100 |
| Professional  |                300 |
| Advanced      |                600 |
| Business      |              1,000 |

When you reach your quota, the dashboard shows an in-app message and the API responds with HTTP 429. Quotas reset automatically at the start of your next billing cycle.

Video requirements

| Constraint   | Limit         |
| :----------- | :------------ |
| Max file size | 100 MB       |
| Max duration | 5 minutes     |
| Format       | .mp4 recommended |

These limits exist because the feature is tuned for short-form vertical content. For longer videos, generate captions manually or use the FFmpeg Video Editor API to trim a highlight first.

Error responses

When the monthly quota is exhausted:

HTTP status: 429 Too Many Requests.

If you also see a separate yellow banner reading "Too many requests. Please wait a moment before trying again", that is the global per-minute API rate limit (see Rate limits) — wait a few seconds and retry.

FAQ

Does retrying a failed analysis count twice? No. The counter is incremented only after a successful analysis is returned to your browser.

Can I see how many analyses I have left? The dashboard shows the remaining count in the response of every analysis. A standalone usage endpoint is on the roadmap.

Is the feature available on the public API? Today it is dashboard-only. API access for analyze-shorts is on the roadmap; reach out to support if you need early access.
