---
source_id: llm-section:docs/resources/faq.md
public_url: https://docs.upload-post.com/resources/faq
raw_source: llm.txt
collected: 2026-05-30
---

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
Facebook API does not support posting to personal profiles—only Pages can be posted to. If you don't have a Page, create one at facebook.com/pages/create.
:::

How does JWT work for white-label integrations?

JWT (JSON Web Token) enables white-label integrations where your users connect their social accounts through your platform:

Create a profile for your user via POST /api/uploadposts/users

Generate a secure URL via POST /api/uploadposts/users/generate-jwt

Redirect your user to the returned access\_url

The user connects their accounts through Upload-Post's interface

Use the API with the user's username to post on their behalf

The JWT URL is valid for 48 hours. See the White-label Integration Guide for full details.

How long do tokens last before I need to reconnect?

Token validity varies by platform:

| Platform | Typical Duration | Notes |
|----------|------------------|-------|
| TikTok | ~60 days | Auto-refreshes if used regularly |
| Instagram | ~60 days | May expire after password changes |
| Facebook | ~60 days | May expire after password changes |
| LinkedIn | ~60 days | Auto-refreshes if used regularly |
| YouTube | ~6 months | May require re-authorization |
| X (Twitter) | Long-lived | Usually stable unless revoked |

When a token expires, you'll receive a "session expired" error. Simply reconnect the account at Manage Users.

Limits & Quotas

How many posts can I make per day on each platform?

Upload-Post enforces platform hard caps using a rolling 24-hour window to protect your accounts:

| Platform | Daily Limit (per account) |
|----------|---------------------------|
| Instagram | 50 posts |
| TikTok | 15 posts |
| LinkedIn | 150 posts |
| YouTube | 10 videos |
| Facebook | 25 posts |
| X (Twitter) | 50 posts |
| Threads | 50 posts |
| Pinterest | 20 pins |
| Reddit | 40 posts |
| Bluesky | 50 posts |

What does error 429 (Too Many Requests) mean?

Error 429 indicates you've hit a rate limit. This can happen when:

Monthly upload limit reached: Your plan's monthly quota is exhausted

Daily platform cap reached: You've hit the 24-hour limit for a specific platform/account

Example response:

Solution: Wait for the 24-hour window to roll over, or spread posts across multiple connected accounts.

Are limits per account or per profile?

Limits are per connected social account, not per Upload-Post user or profile.

Example: If you manage 5 Profiles, and each has its own TikTok account connected, you get the full limit for each TikTok account (5 TikTok accounts × 15 posts = 75 TikTok posts per day total).

What does "unlimited uploads" mean in the Basic plan?

"Unlimited uploads" refers to the number of API calls you can make per month through Upload-Post. However, you're still subject to:

Platform daily caps (enforced per social account to protect against bans)

Profile limits (based on your plan tier)

Platform-specific restrictions (each social network's own rules)

How many profiles can I connect on each plan?

Check upload-post.com/pricing for the current limits per plan. You can see your current limit and usage via the GET /api/uploadposts/users endpoint, which returns a limit field showing your maximum allowed profiles.

Video Uploads

How do I upload YouTube Shorts vs regular videos?

YouTube automatically determines if a video is a Short based on:

Duration: 60 seconds or less

Aspect ratio: Vertical (9:16) or square (1:1)

Simply upload your video normally to the /api/upload endpoint. YouTube will classify it as a Short if it meets these criteria.

:::warning
Custom thumbnails are not supported for YouTube Shorts—they only apply to standard YouTube videos.
:::

What is the maximum video file size?

Maximum file sizes vary by platform:

| Platform | Max File Size | Max Duration |
|----------|---------------|--------------|
| TikTok | 4 GB | 10 minutes |
| Instagram | 300 MB | 15 minutes |
| YouTube | 256 GB | 12 hours |
| LinkedIn | 5 GB | 10 minutes |
| Facebook | ~1 GB | 240 minutes |
| X (Twitter) | 1 GB+ (Premium) | 4 hours (Premium) |
| Threads | 1 GB | 5 minutes |
| Pinterest | 1 GB | 15 minutes |
| Reddit | 1 GB | 15 minutes |
| Bluesky | 100 MB | 3 minutes |

See Video Requirements for detailed format specifications.

Can I use a video URL instead of uploading the file?

Yes! Instead of uploading a file, you can pass a direct URL to your video:

Requirements for video URLs:

URL must be publicly accessible (test in an incognito browser)

Should be a direct link to the file (ending in .mp4, .mov, etc.)

Google Drive links must be set to "Anyone with the link"

File must not be too small (< 1KB often indicates a broken link)

Why is my video stuck in "processing"?

Videos may remain in processing status for several reasons:

Long upload: If sync upload takes > 59 seconds, it automatically switches to async processing

Platform processing: The social network is processing your video (especially for large files)

Encoding issues: The video format may need transcoding

To check status, use the Upload Status endpoint:

Status values: pending → in\_progress → completed

How do I upload videos to TikTok drafts (MEDIA\_UPLOAD mode)?

Use the post\_mode parameter set to MEDIA\_UPLOAD:

:::info
In MEDIA\_UPLOAD (Draft) mode, TikTok does not allow setting title, caption, privacy, or other metadata via API. The video uploads to your TikTok inbox/drafts, and you must add all details manually in the TikTok app before publishing.
:::

Photo & Carousel Uploads

How do I upload carousels to Instagram/TikTok/Facebook?

Upload multiple photos using the /api/upload\_photos endpoint with the photos\[] array:

Mixed carousels (photos + videos) are supported on Instagram and Threads only:

How many photos can I include in a carousel?

| Platform | Max Photos per Post |
|----------|---------------------|
| Instagram | 10 items (photos/videos mixed) |
| Threads | 10 items (photos/videos mixed) |
| TikTok | Multiple (photo slideshow) |
| Facebook | Multiple |
| Pinterest | 5 carousel images |
| Bluesky | 4 images |
| X (Twitter) | 4 images |
| Reddit | 1 image per post |

For X (Twitter), use x\_thread\_image\_layout to control how images are distributed across tweets when posting more than 4 images. For Threads, use threads\_thread\_media\_layout to control how media items are distributed across posts when posting more than 10 items.

What image resolution/size should I use?

Recommended specifications by platform:

| Platform | Recommended Size | Max File Size | Formats |
|----------|------------------|---------------|---------|
| Instagram | 1080x1350 (4:5) | 8 MB | JPG, PNG |
| TikTok | 1080x1920 (9:16) | — | JPG, JPEG, WEBP |
| Facebook | 1200x630 | 10 MB | JPG, PNG, GIF, WebP |
| LinkedIn | 1200x627 | 8 MB | JPG, PNG, GIF |
| Pinterest | 1000x1500 (2:3) | 20 MB | JPG, PNG, GIF, WEBP |
| Threads | 1440px max width | 8 MB | JPG, PNG |
| Bluesky | — | 1 MB per image | JPG, PNG, GIF, WEBP |
| Reddit | — | 10 MB | JPG, PNG, GIF, WEBP |

See Photo Requirements for detailed specifications.

How do I add automatic music to TikTok photos?

Use the auto\_add\_music parameter:

TikTok will automatically add background music to your photo slideshow.

Scheduling

How do I schedule a post for later?

Add the scheduled\_date parameter (ISO-8601 format) to any upload request:

The API returns a job\_id that you can use to check status, edit, or cancel the scheduled post.

Constraints:

Must be in the future

Maximum 365 days ahead

What timezone does the API use?

By default, the API uses UTC. To use a different timezone, add the timezone parameter:

Use any valid IANA timezone identifier (e.g., Europe/Madrid, America/Los\_Angeles, Asia/Tokyo).

How do I check the status of a scheduled post?

Use the Upload Status endpoint with the job\_id:

To list all scheduled posts:

To cancel a scheduled post:

See Manage Scheduled Posts for full details.

n8n Integration

How do I configure credentials in n8n?

Add an HTTP Request Node to your workflow

Set Method to POST

Set URL to https://api.upload-post.com/api/upload

Under Headers, add:

Name: Authorization

Value: Apikey YOUR\_API\_KEY

Set Body to multipart/form-data

Add required parameters: user, title, platform\[]

Security tip: Use n8n's Credentials store instead of hardcoding your API key.

How do I pass binary files in n8n?

In the HTTP Request node, use the formBinaryData option:

This passes the binary data from a previous node (like a Google Drive trigger) as the video file.

Alternative: You can pass a video URL instead of binary data:

Where do I find the Upload-Post n8n community node?

Currently, Upload-Post integration is done via the standard HTTP Request node. We provide:

Complete JSON node configuration

Example workflow template on n8n.io

See our n8n Integration Guide for step-by-step setup instructions.

Common Errors

Error: "Username required in form data"

The user parameter is missing from your request. This field identifies which Upload-Post profile to use.

Error: "Video URL is not accessible"

The provided video URL cannot be fetched. Check that:

The URL is publicly accessible (test in incognito browser)

The URL points directly to a video file (not a webpage)

For Google Drive: sharing is set to "Anyone with the link"

The file exists and isn't too small (< 1KB indicates broken link)

Error: "TikTok photo upload timed out"

TikTok photo uploads can be slow. Try:

Use async\_upload=true to avoid timeout

Reduce image file sizes

Upload fewer photos at once

Check TikTok service status

Error: "File name too long"

Some platforms reject files with very long names. Rename your file to something shorter (under 100 characters) before uploading.

Error 502/504 Gateway Timeout

These indicate server-side timeouts, usually for large files or slow platform responses.

Solutions:

Use async\_upload=true for large uploads

Reduce file size if possible

Retry the request

If persistent, contact support

Error: "Your \[Platform] session has expired"

Your social account connection has expired. Go to Manage Users, disconnect the account, and reconnect it.

See Common Errors for a complete error reference.

Platform-Specific Features

Can I add a first comment automatically?

Yes! Use the first\_comment parameter:

Supported on: Instagram, Facebook, Threads, Bluesky, Reddit, X, YouTube, and LinkedIn.

For platform-specific first comments, use \[platform]\_first\_comment:

How do I upload Stories to Instagram/Facebook?

Use the media\_type or facebook\_media\_type parameter set to "STORIES":

Instagram Stories (video):

Instagram Stories (photo):

Facebook Stories:

Can I add custom thumbnails?

Yes, for YouTube (standard videos only, not Shorts):

Requirements: JPG/PNG/GIF/BMP, max 2 MB.

Can I add a custom cover image for Instagram Reels?

Yes, for Instagram Reels you can provide a cover image via URL or file upload:

Requirements: JPEG format, max 8 MB, recommended aspect ratio 9:16.

How do I mark content as AI-generated?

For TikTok, use the is\_aigc parameter:

For YouTube, use containsSyntheticMedia:

Is there an API to read/respond to comments?

Currently, Upload-Post focuses on content publishing. Reading and responding to comments is not supported in the API. For comment management, use each platform's native interface or their direct APIs.

White-Label Integration

How does white-label integration work?

White-label allows you to integrate Upload-Post into your own platform, so your users can connect their social accounts through your interface:

Create profiles for your users via API

Generate secure URLs (JWT) for account linking

Users connect their accounts through the Upload-Post interface (customizable branding)

You manage their content via API using their profile username

See the White-label Integration Guide for implementation details.

How do I generate connection URLs for my users?

The response includes an access\_url valid for 48 hours. Redirect your user to this URL.

Can I use my own domain for the connection page?

Currently, the connection page is hosted at app.upload-post.com. You can customize:

Logo: via logo\_image parameter

Title: via connect\_title parameter

Description: via connect\_description parameter

Button text: via redirect\_button\_text parameter

Redirect URL: via redirect\_url parameter

Visible platforms: via platforms array

Calendar visibility: via show\_calendar parameter

Custom domain support is not currently available.

Billing, Invoices & Payments

How do I access my invoices?

You can access all your invoices through the Stripe Billing Portal:

Log in to your Upload-Post Dashboard

Go to My Profile

Click the "Open Billing Portal" button in the Invoices & Billing card at the top

In the Stripe Portal, click "Invoice History" to view and download all invoices

You can also access the Billing Portal from the profile dropdown menu in the navigation bar on any page.

How do I update my payment method?

Go to My Profile in the dashboard

Click "Open Billing Portal"

In the Stripe Portal, click "Payment methods"

Add a new card or update your existing payment method

What are the plan prices?

Visit our Pricing page for current prices. We offer:

Basic - For individuals getting started

Professional - For power users and small teams

Advanced - For agencies and larger teams

Business - For enterprise with usage-based billing

All plans are available with monthly or yearly billing. Yearly plans include a significant discount.

Do you charge VAT/IVA/taxes?

Upload-Post uses Stripe for payment processing. Depending on your country and tax regulations:

EU customers: VAT may be applied automatically based on your billing address. Stripe handles EU VAT compliance.

Non-EU customers: Taxes depend on your local regulations. Stripe may collect applicable taxes based on your billing address.

Your invoices in the Stripe Billing Portal will show any applied taxes. If you need a tax ID (VAT number) added to your invoices, you can add it directly in the Stripe Portal under your billing details.

How do I cancel my subscription?

To cancel your subscription:

Log in to your Upload-Post Dashboard

Go to My Profile

Click "Cancel Subscription" and follow the steps

Your access continues until the end of the current billing period. You can also cancel directly from the Stripe Billing Portal.

How do I request a refund?

Contact our support team at info@upload-post.com with:

Your account email

Reason for the refund request

Any relevant details

Refund eligibility depends on your subscription terms and usage.

Can I pause my subscription instead of canceling?

Yes! When you click "Cancel Subscription", you'll be offered the option to pause your subscription for 1 or 3 months instead. During the pause:

Your data and connected accounts are preserved

No charges during the pause period

You can resume at any time from your profile

How do I change my account email?

You can change your email directly from the dashboard:

Go to My Profile

Click "Change email" next to your current email

Enter your new email address

Click "Send Verification"

Check your current email inbox and click the confirmation link

Then check your new email inbox and click the second confirmation link

Your email will be updated and you'll need to log in again

This double-confirmation process protects your account from unauthorized changes.

How do I contact human support?

Email: info@upload-post.com

Twitter/X: @HiImg2html

GitHub: github.com/upload-post

When contacting support, please include:

Your API key (masked)

Exact error messages

Steps to reproduce the issue

Platform(s) affected

Can I buy additional profiles?

Yes! You can add extra profiles as add-ons to your existing subscription:

Go to My Profile in the dashboard

Under Subscription, click "Manage Extra Profiles"

Choose the add-on size that fits your needs

Alternatively, you can upgrade to a higher tier plan at upload-post.com/pricing for more base profiles.

What happens to my data if I cancel?

When you cancel your subscription:

Your data and connected accounts remain until the end of the billing period

After the billing period ends, your account reverts to the free plan (limited uploads)

Your connected social accounts and profile data are preserved

You can resubscribe at any time to restore full access

Still Have Questions?

If your question isn't answered here:

Check our detailed API Reference

Review the Common Errors guide

Contact support at info@upload-post.com
