# Limits, Errors, Requirements, and Troubleshooting

Collected: 2026-05-30

## Source Traceability

| Source | Public URL |
| --- | --- |
| `docs/guides/error-handling.md` | https://docs.upload-post.com/guides/error-handling |
| `docs/guides/limit-of-uploads.md` | https://docs.upload-post.com/guides/limit-of-uploads |
| `docs/guides/rate-limits.md` | https://docs.upload-post.com/guides/rate-limits |
| `docs/guides/youtube-quota-explained.md` | https://docs.upload-post.com/guides/youtube-quota-explained |
| `docs/resources/common-errors.md` | https://docs.upload-post.com/resources/common-errors |
| `docs/resources/character-limits.md` | https://docs.upload-post.com/resources/character-limits |
| `docs/resources/faq.md` | https://docs.upload-post.com/resources/faq |
| `docs/resources/support.md` | https://docs.upload-post.com/resources/support |

## Endpoint Routing

No OpenAPI endpoint group is assigned to this topic; use the source excerpts and concept index.

## Practical Notes

- Treat platform session errors as reconnect/permission problems first.
- Respect `X-RateLimit-*` headers and wait until reset after HTTP 429.
- YouTube quota and upload limits are documented separately from generic API rate limits.

## Source Excerpts

### docs/guides/error-handling.md

API Error Handling: Upload Endpoints

This document explains the structure of responses you will receive from the Video Upload (POST /api/upload) and Photo Upload (POST /api/upload\_photos) endpoints, including how success and errors are indicated.

1\. Successful Request Processing (HTTP 200 OK)

When your upload request is successfully processed by our server (meaning your authentication was valid, input was generally okay, and usage limits weren't exceeded before starting), you will always receive an HTTP 200 OK status code.

The JSON response body will look like this:

Key Points:

"success": true indicates the API server processed your request.

"results": This dictionary is crucial. It contains the outcome for each individual platform you requested.

Platform Success: If results\[platform].success is true, the upload to that platform likely succeeded. Additional platform-specific IDs (like publish\_id, container\_id, post\_id) may be included.

Platform Failure: If results\[platform].success is false, the upload to that specific platform failed. The results\[platform].error field will contain a message explaining the reason (e.g., token expired, API error from the platform, file issue).

Important: An error on one platform (like LinkedIn in the example) does not stop attempts on other platforms. Always check the success flag for each platform in the results.

"usage": Provides information about your current API usage count and limit after this request.

2\. Request Failure (Non-200 HTTP Status Codes)

[Excerpt truncated in this topic reference; see the source URL for full page context.]

### docs/guides/limit-of-uploads.md

Limit of uploads

Social Hard Caps Per Network

To protect your connected accounts and stay compliant with each social network, Upload-Post enforces platform hard caps using a rolling 24-hour window. When a cap is reached for a specific account on a given network, further posts to that account/network are rejected until the window rolls over.

What counts toward the cap: only successful publishes recorded for that account/network in the last 24 hours.

Scope: Per connected social account. These limits are NOT global to your Upload-Post user.

Example: If you manage 5 Profiles, and each Profile has its own TikTok account connected, you get the full limit for each TikTok account. (e.g., 5 TikTok accounts × 15 posts = 75 posts per day total).

Scheduled posts: caps are re-checked at execution time; if the cap is already reached, the publish will be rejected then.

Recommended and enforced daily caps

| Social Network     | Hard Cap (posts per 24h) |
| :----------------- | -----------------------: |
| Instagram          |                       50 |
| TikTok             |                       15 |
| LinkedIn           |                      150 |
| YouTube            |                       10 |
| Facebook           |                       25 |
| X (Twitter)        |              See per-plan table below |
| Threads            |                       50 |
| Pinterest          |                       20 |
| Reddit             |                       40 |
| Bluesky            |                       50 |

X (Twitter) per-plan daily caps

[Excerpt truncated in this topic reference; see the source URL for full page context.]

### docs/guides/rate-limits.md

Rate Limits & Polling Best Practices

Upload-Post applies rate limits at multiple levels to protect the service and the social media platforms. This guide explains each limit and how to design your integration to work within them.

API Rate Limits

Every authenticated API response includes rate limit headers:

| Header | Description |
|--------|-------------|
| X-RateLimit-Limit | Maximum requests allowed in the current window |
| X-RateLimit-Remaining | Requests remaining in the current window |
| X-RateLimit-Reset | Unix timestamp when the window resets |

When you exceed the limit, the API returns HTTP 429 Too Many Requests. Wait until the X-RateLimit-Reset timestamp before retrying.

Upload Status Polling

When using async\_upload=true, you need to poll the Upload Status endpoint to get the result. Here are the recommended intervals:

Polling Strategy

Recommended Polling Intervals

| Scenario | Poll Interval | Max Wait |
|----------|--------------|----------|
| Photo uploads | Every 5–10 seconds | 2 minutes |
| Video uploads (small, < 50 MB) | Every 10 seconds | 5 minutes |
| Video uploads (large, > 50 MB) | Every 15 seconds | 10 minutes |
| Scheduled posts (after scheduled time) | Every 30 seconds | 10 minutes |
| Queued posts | Every 60 seconds | Until next queue slot |

Status Cache TTLs

The status endpoint uses internal caching. Here is how often the status value is refreshed:

| Status | Cache TTL | Meaning |
|--------|-----------|---------|
| queued | 2 seconds | The upload is waiting for a worker |
| pending | 2 seconds | Accepted but not yet started |

[Excerpt truncated in this topic reference; see the source URL for full page context.]

### docs/guides/youtube-quota-explained.md

Understanding YouTube API Quota Limits

⚠️ DEPRECATED:
We have now received our own dedicated YouTube API quota. You no longer need to configure your own Google Cloud project. All YouTube features are available directly from our platform without any extra setup.

We believe in being transparent with our community about the challenges we face and the solutions we implement. This document explains the current situation regarding YouTube's API quota and introduces a new feature that gives you more control.

The Challenge: YouTube's API Quota

Every application that interacts with YouTube, including ours, is subject to a daily API quota. This quota determines how many actions (like uploads, comments, or data requests) can be performed through our platform each day.

Due to the incredible growth of our user base, we are frequently reaching the limit of our current quota. This can sometimes result in temporary service disruptions for YouTube-related features.

What We Are Doing About It

For the past six months, we have been in ongoing discussions with YouTube's leadership team to request a significant increase in our daily quota. We believe a higher quota is essential to reliably serve our growing community.

Unfortunately, this process has been slower than anticipated, and we are still awaiting a final decision. We are persistently following up and providing all necessary information to make our case.

Our Solution: Use Your Own Google Cloud Project

[Excerpt truncated in this topic reference; see the source URL for full page context.]

### docs/resources/common-errors.md

Common Errors

This guide covers the most common errors you might encounter when using Upload-Post and how to resolve them. Errors are organized by category to help you quickly find solutions.

Session Expired {#session-expired}

Session and authentication errors occur when the connection between Upload-Post and your social media account has been broken. This is usually easy to fix by reconnecting your account.

Common Session Errors

| Error Message | Platform | Solution |
|---------------|----------|----------|
| "Your \[Platform] session has expired" | All | Reconnect your account |
| "Token expired and refresh failed" | All | Reconnect your account |
| "The session has been invalidated because the user changed their password" | Facebook/Instagram | Reconnect after password change |
| "Error validating access token" | Facebook/Instagram | Verify permissions and reconnect |
| "Your X session has expired and could not be refreshed" | X (Twitter) | Reconnect your X account |
| "User has not authorized application" | Various | Grant permissions and reconnect |
| "Unauthorized" | Various | Reconnect your account |

How to Fix Session Errors

Go to Manage Users

Find the affected account - Look for accounts with warning indicators

Disconnect the account - Click the disconnect/remove button next to the account

Reconnect the account - Click "Connect" and complete the authorization flow

Grant all permissions - Make sure to approve all requested permissions during reconnection

:::tip

[Excerpt truncated in this topic reference; see the source URL for full page context.]

### docs/resources/character-limits.md

Social Media Character Limits

This guide summarizes the most relevant text limits for each social network supported by Upload-Post. Keep these constraints in mind when building payloads so posts are accepted without truncation.

Platform-Specific Character Limits

Facebook Character Limits

| Property | Description |
| --- | --- |
| post | 63,206 characters maximum |
| title | Reels title – 255 characters maximum |

Instagram Character Limits

| Property | Description |
| --- | --- |
| post | 2,200 characters maximum |
| altText | 1,000 characters maximum per image |
| comment | 2,196 characters maximum |

LinkedIn Character Limits

| Property | Description |
| --- | --- |
| post | 3,000 characters maximum |
| title | 400 characters maximum |
| comment | 1,250 characters maximum |

TikTok Character Limits

| Property | Description |
| --- | --- |
| post | 2,200 characters maximum |
| title (photo posts) | 90 characters maximum |
| description (photo posts) | 4,000 characters maximum |

Pinterest Character Limits

| Property | Description |
| --- | --- |
| post | 500 characters maximum |
| title | 100 characters maximum |
| link | 2,048 characters maximum |
| altText | 500 characters maximum |

Reddit Character Limits

| Property | Description |
| --- | --- |
| post | 5,000 characters maximum |
| title | 300 characters maximum |
| comment | 10,000 characters maximum |

Threads Character Limits

| Property | Description |
| --- | --- |
| post | 500 characters maximum |

X (Twitter) Character Limits

| Property | Description |
| --- | --- |
| post | 280 characters maximum |

[Excerpt truncated in this topic reference; see the source URL for full page context.]

### docs/resources/faq.md

Frequently Asked Questions

This FAQ covers the most common questions about Upload-Post. Questions are organized by category to help you find answers quickly.

Connection & Authentication

Where do I find my API Key?

Log in to your Upload-Post Dashboard

Navigate to the "API Keys" section

Click "Generate New API Key"

Copy and securely store your API key

Include your API key in the Authorization header of all API requests:

How do I connect my TikTok/Instagram/Facebook/YouTube/LinkedIn account?

Go to Manage Users in your dashboard

Click "Connect" next to the platform you want to add

Follow the OAuth authorization flow for that platform

Grant all requested permissions when prompted

Once connected, the account will appear in your profile

Why do I get error 400 when connecting Instagram?

Error 400 when connecting Instagram usually means:

Account not confirmed: Your Facebook/Instagram account needs email or phone verification. Go to Facebook Account Quality to check for issues.

Missing permissions: During connection, make sure to approve ALL requested permissions.

Business account required: Instagram API requires a Business or Creator account linked to a Facebook Page.

Page not connected: Instagram must be linked to a Facebook Page. Create one at facebook.com/pages/create if needed.

How do I get my Facebook Page ID?

Use our API endpoint to retrieve all Facebook Pages associated with your account:

The response includes page\_id, page\_name, and the associated profile for each page.

:::info

[Excerpt truncated in this topic reference; see the source URL for full page context.]

### docs/resources/support.md

Support

We're here to help you succeed with the Upload-Post API. Here are the different ways you can get support:

Contact Options

Email Support

Support: info@upload-post.com

Community Support

Follow us on Twitter

Check our GitHub repository

Getting Help

Before Contacting Support

Check our documentation

Review our FAQ

Search for similar issues in our GitHub issues

When Contacting Support

Please include:

Your API key (masked)

Error messages or logs

Steps to reproduce the issue

Expected vs actual behavior

Any relevant code snippets

Bug Reports

If you've found a bug:

Check if it's already reported on GitHub

Create a new issue with:

Clear description

Steps to reproduce

Expected behavior

Actual behavior

Environment details

Feature Requests

We welcome feature requests! Submit them through:

GitHub issues

Email to info@upload-post.com
