---
source_id: llm-section:docs/resources/common-errors.md
public_url: https://docs.upload-post.com/resources/common-errors
raw_source: llm.txt
collected: 2026-05-30
---

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
If you recently changed your password on a social media platform, you'll need to reconnect that account in Upload-Post.
:::

Account Blocked {#account-blocked}

These errors occur when there's an issue with your account on the social media platform itself. Upload-Post cannot fix these - you need to resolve them directly on the platform.

Common Account Status Errors

| Error Message | Platform | What It Means |
|---------------|----------|---------------|
| "Sessions for the user are not allowed because the user is not a confirmed user" | Facebook | Account needs verification |
| "The YouTube account of the authenticated user is suspended" | YouTube | Account suspended by YouTube |
| "Your account is temporarily locked" | X (Twitter) | X has locked your account |
| "The Instagram account is restricted or inactive" | Instagram | Instagram has restricted your account |
| "Action suspected as spam. Activity is restricted" | Instagram | Spam detection triggered |
| "The user used for authentication is suspended" | Various | Account suspended on platform |

How to Fix Account Blocked Errors

These issues must be resolved directly on the social media platform:

Facebook/Instagram Account Not Confirmed

Go to facebook.com and log in

Check for any verification prompts or security notices

Go to Settings & Privacy > Settings > Personal Details

Verify your email and phone number are confirmed

Visit Facebook Account Quality to check for restrictions

After resolving issues, reconnect in Upload-Post

X (Twitter) Account Locked

Go to twitter.com or x.com and log in

Follow the prompts to unlock your account (may require phone verification)

Once unlocked, reconnect in Upload-Post

YouTube Account Suspended

Go to YouTube and sign in

Check the YouTube Help Center for suspension appeals

Follow YouTube's process to restore your account

Instagram Restricted

Open the Instagram app and log in

Look for any notification banners or prompts

Follow Instagram's instructions to verify your identity

After restrictions are lifted, reconnect in Upload-Post

:::warning
Upload-Post cannot bypass platform restrictions. You must resolve these issues directly with the social media platform before reconnecting.
:::

Configuration & Permissions {#configuration-permissions}

These errors occur when your account is connected but requires additional configuration or permissions.

Common Configuration Errors

| Error Message | Platform | Solution |
|---------------|----------|----------|
| "No Facebook Pages found for your account" | Facebook | Connect a Facebook Page (personal profiles not supported) |
| "Multiple Facebook Pages found. Please select a Page" | Facebook | Select which Page to post to |
| "Facebook Page ID is required for text posts" | Facebook | Configure your profile to select a Page |
| "Couldn't get a Facebook Page access token for Page ID..." | Facebook | Reconnect with proper Page permissions |
| "Pinterest account not found or not configured" | Pinterest | Configure Pinterest in your profile |
| "Board not found" | Pinterest | Select a valid Pinterest board |
| "You are not permitted to access that resource" | Various | Check your role/permissions on the account |
| "This site doesn't allow you to save Pins" | Pinterest | The target site blocks Pinterest pins |

How to Fix Configuration Errors

Facebook Page Issues

Facebook requires you to post to a Facebook Page, not a personal profile. The Facebook API does not support posting to personal profiles.

Create a Facebook Page (if you don't have one):

Go to Create a Page

Follow the setup wizard

Connect your Page to Upload-Post:

Go to Manage Users

Disconnect your Facebook account

Reconnect and select the Page you want to post to

Make sure you have Admin or Editor role on the Page

Select the correct Page in your profile:

Go to your Upload-Post profile settings

Select the Facebook Page from the dropdown

:::info Required Permissions
To post to a Facebook Page, you need:

Admin or Editor role on the Page

Grant "pages\_manage\_posts" permission during connection

Grant "pages\_read\_engagement" permission
:::

Pinterest Board Issues

Make sure you have at least one board in your Pinterest account

Go to your Upload-Post profile and select the correct board

If the board was recently created, try disconnecting and reconnecting Pinterest

Content Format {#content-format}

These errors occur when your media doesn't meet the platform's requirements for size, format, or aspect ratio.

Common Content Errors

| Error Message | Platform | Solution |
|---------------|----------|----------|
| "Unsupported image size" | Various | Use supported dimensions |
| "Invalid image aspect ratio" | Instagram | Use ratio between 4:5 and 1.91:1 |
| "Video longer than 2 minutes" | X (Twitter) | Shorten video or upgrade X account |
| "TikTok rejected the media format" | TikTok | Use MP4/MOV format |
| "One or more tags are invalid" | Various | Remove special characters from tags |
| "Your post must contain post flair" | Reddit | Add required flair to your post |
| "Media could not be fetched from the provided URL" | Various | Use a publicly accessible URL |
| "Downloaded file is too small" | Various | Check URL points to valid media |
| "Collaborator usernames are invalid" | Instagram | Use valid public usernames without @ |

Media Requirements by Platform

| Platform | Image Format | Max Image Size | Video Format | Max Video Duration |
|----------|--------------|----------------|--------------|-------------------|
| Instagram | JPG, PNG | 8 MB | MP4, MOV | 60 min (Feed), 90 sec (Reels) |
| Facebook | JPG, PNG, GIF | 10 MB | MP4, MOV | 240 min |
| TikTok | - | - | MP4, MOV | 10 min |
| X (Twitter) | JPG, PNG, GIF | 5 MB | MP4 | 2 min 20 sec\* |
| LinkedIn | JPG, PNG | 8 MB | MP4 | 10 min |
| YouTube | - | - | MP4, MOV, AVI | 12 hours |
| Pinterest | JPG, PNG | 32 MB | MP4, MOV | 15 min |

\*X Premium users may have longer video limits

Instagram Aspect Ratio Guidelines

Square: 1:1

Portrait: 4:5 (recommended for Feed)

Landscape: 1.91:1

Stories/Reels: 9:16

Images outside the 4:5 to 1.91:1 range will be rejected.

:::tip
For best results, use 1080x1350 pixels (4:5 ratio) for Instagram feed posts.
:::

Fixing Media URL Issues

If you're getting "Media could not be fetched" errors:

Check the URL is publicly accessible - Open it in an incognito browser window

Don't use private/restricted URLs - Google Drive links must be set to "Anyone with the link"

Use direct file URLs - The URL should end with a file extension like .jpg or .mp4

Check file size - Very small files (< 1KB) often indicate a broken link

Rate Limits {#rate-limits}

These errors occur when you've hit posting limits or the platform is experiencing temporary issues.

Common Rate Limit Errors

| Error Message | Platform | Solution |
|---------------|----------|----------|
| "Daily upload limit exceeded for this channel" | YouTube | Wait until tomorrow (UTC midnight) |
| "Service Unavailable (503)" | Various | Retry in a few minutes |
| "Temporary issue. We retried 4 times but it still failed" | Instagram | Platform issue - retry later |
| "Fatal" / "Unexpected error" | Various | Platform issue - retry later |
| "Rate limit reached" | Various | Wait before making more requests |

Platform Daily Limits

| Platform | Daily Posting Limit |
|----------|---------------------|
| Instagram | ~50 posts per day |
| TikTok | 15-20 videos per day |
| LinkedIn | ~150 posts per day |
| Pinterest | 25 pins per day |
| Reddit | Varies by subreddit |
| YouTube | 100 videos per day |

Handling Rate Limits

Wait and retry - Most limits reset at midnight UTC

Spread posts throughout the day - Avoid posting many items at once

Use scheduling - Schedule posts to spread them out automatically

Check your usage - View your posting history to track activity

:::info Automatic Rescheduling
When you hit a daily limit, Upload-Post may automatically reschedule your post for the next day. Check your scheduled posts to confirm.
:::

Getting Help

If your error isn't listed here or you need additional assistance:

Check our FAQ for general questions

Review our API Error Handling Guide for technical details

Contact support at info@upload-post.com

When contacting support, please include:

The exact error message you received

The platform you were posting to

The type of content (text, photo, video)

When the error occurred

Platform Help Centers

If you need to resolve issues directly with a social media platform:

Facebook Help Center

Instagram Help Center

TikTok Support

YouTube Help

X (Twitter) Help

LinkedIn Help

Pinterest Help

Reddit Help
