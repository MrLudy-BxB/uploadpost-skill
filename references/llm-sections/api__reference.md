---
source_id: llm-section:docs/api/reference.md
public_url: https://docs.upload-post.com/api/reference
raw_source: llm.txt
collected: 2026-05-30
---

API Reference

The Upload-Post API provides comprehensive endpoints for content management across multiple social media platforms. All endpoints require authentication via API key in the Authorization header.

Core Upload APIs

Video Upload API

Upload videos to TikTok, Instagram, LinkedIn, YouTube, Facebook, X (Twitter), Threads, Pinterest, and Bluesky. Supports both synchronous and asynchronous uploads with scheduling capabilities.

Endpoint: POST /api/upload

Supported Platforms: TikTok, Instagram, LinkedIn, YouTube, Facebook, X (Twitter), Threads, Pinterest, Bluesky

Photo Upload API

Upload photos and image carousels to LinkedIn, Facebook, X (Twitter), Instagram, TikTok, Threads, Pinterest, and Bluesky. Perfect for visual content distribution across platforms.

Endpoint: POST /api/upload\_photos

Supported Platforms: LinkedIn, Facebook, X (Twitter), Instagram, TikTok, Threads, Pinterest, Bluesky

Text Upload API

Create and distribute text-only posts across social platforms. Ideal for announcements, updates, and text-based content.

Endpoint: POST /api/upload\_text

Supported Platforms: X (Twitter), LinkedIn, Facebook, Threads, Reddit, Bluesky

Upload Management APIs

Upload Status

Track the progress and results of asynchronous uploads initiated with async\_upload=true or scheduled posts. Essential for monitoring long-running upload operations and checking scheduled post execution status.

Endpoint: GET /api/uploadposts/status

Parameters: request\_id (for async uploads) or job\_id (for scheduled posts)

Use Case: Check status of background uploads and scheduled posts, get detailed results per platform

Upload History

Retrieve a paginated history of all your past uploads across platforms. Includes detailed metadata, success/failure status, and platform-specific information.

Endpoint: GET /api/uploadposts/history

Features: Pagination, filtering, comprehensive upload metadata

Schedule Management

Schedule posts for future publication across supported platforms. Manage your content calendar programmatically.

Endpoint: Various scheduling endpoints

Supported Platforms: X (Twitter), LinkedIn, Facebook, Instagram, TikTok, Bluesky, Threads, Pinterest, YouTube

Instagram Interactions

Media List

Retrieve a list of recent media (posts, reels, videos, pins, tweets, etc.) from any connected social media account. Supports Instagram, TikTok, YouTube, LinkedIn, Facebook, X, Threads, Pinterest, Bluesky, and Reddit.

Endpoint: GET /api/uploadposts/media

Returns: Media IDs, captions, media types, permalinks, timestamps, thumbnail URLs

Instagram Comments

Retrieve comments on Instagram posts and send private replies (DMs) to commenters. Supports both media IDs and post URLs.

Endpoints:

GET /api/uploadposts/comments - Get comments on an Instagram post

POST /api/uploadposts/comments/reply - Send a private reply DM to a commenter

Required Permission: instagram\_business\_manage\_comments

Instagram Direct Messages

Send direct messages to Instagram users and retrieve DM conversations. Supports customer support workflows and follow-up messaging within Instagram's 24-hour messaging window policy.

Endpoints:

POST /api/uploadposts/dms/send - Send a DM to an Instagram user by IGSID

GET /api/uploadposts/dms/conversations - Retrieve Instagram DM conversations

Required Permission: instagram\_business\_manage\_messages

AutoDM Monitors

Set up persistent monitors that automatically send private DMs to users who comment on your Instagram posts. Monitors run in the background 24/7 with built-in duplicate prevention and rate limiting.

Endpoints:

POST /api/uploadposts/autodms/start - Start a new comment monitor

GET /api/uploadposts/autodms/status - Get status of all monitors (supports ?include\_inactive=true to also return stopped/expired ones)

GET /api/uploadposts/autodms/logs - Get activity logs for a monitor

POST /api/uploadposts/autodms/pause - Pause a monitor

POST /api/uploadposts/autodms/resume - Resume a paused monitor

POST /api/uploadposts/autodms/stop - Stop a monitor

POST /api/uploadposts/autodms/delete - Delete a monitor

Limits: 2 monitors per profile per day, auto-expires after 15 days

Platform Integration APIs

Analytics API

Retrieve detailed analytics and performance metrics for your social media profiles across connected platforms.

Endpoint: GET /api/analytics/{profile\_username}

Supported Platforms: Instagram, TikTok, LinkedIn, Facebook, X (Twitter), YouTube, Threads, Pinterest, Reddit

Metrics: Followers, impressions, reach, profile views, time-series data, metric\_type

Total Impressions & Post Analytics

Get unified total impressions aggregated across platforms, per-post analytics, and platform metrics config. All analytics endpoints are consolidated in the Analytics API page.

Endpoints: GET /api/uploadposts/total-impressions/{profile\_username} · GET /api/uploadposts/post-analytics/{request\_id} · GET /api/uploadposts/post-analytics?platform\_post\_id=

Features: Date range filtering, per-platform breakdown, live post metrics, profile snapshots, analytics for organic posts via platform\_post\_id

Get Facebook Pages

Retrieve all Facebook pages accessible through connected accounts. Required for posting to specific Facebook pages.

Endpoint: GET /api/uploadposts/facebook/pages

Returns: Page IDs, names, profile pictures, account associations

Get LinkedIn Pages

Fetch LinkedIn company pages associated with your connected accounts. Essential for business page posting.

Endpoint: GET /api/uploadposts/linkedin/pages

Returns: Organization URNs, company names, vanity URLs, page logos

Get Google Business Locations

List and select Google Business Profile locations for accounts managing multiple business profiles. Essential for agencies and SaaS platforms where users manage multiple locations.

Endpoints: GET /api/uploadposts/google-business/locations, POST /api/uploadposts/google-business/locations/select

Returns: Location IDs, business names, selection status

Get Pinterest Boards

List all Pinterest boards (public and secret) from connected accounts. Required for targeting specific boards when pinning content.

Endpoint: GET /api/uploadposts/pinterest/boards

Returns: Board IDs, names, associated Pinterest accounts

User Management APIs

User Profiles API

Manage user profiles and generate JWTs for linking social accounts when integrating Upload-Post into your own platform. Essential for white-label integrations and multi-user applications.

Endpoints:

POST /api/uploadposts/users - Create user profiles

GET /api/uploadposts/users - Retrieve user profiles

DELETE /api/uploadposts/users - Delete user profiles

POST /api/uploadposts/users/generate-jwt - Generate authentication tokens

POST /api/uploadposts/users/validate-jwt - Validate tokens

See the User Profile Integration Guide for implementation workflow.

Current User API

Validate your API key and retrieve basic account information including email and subscription plan.

Endpoint: GET /api/uploadposts/me

Use Case: API key validation, plan verification, account confirmation

Content Requirements

Photo Requirements

Comprehensive format specifications, file size limits, aspect ratios, and technical requirements for photo uploads across all supported platforms.

Covers: Instagram, TikTok, Facebook, X (Twitter), LinkedIn, Threads, Pinterest, Reddit, Bluesky

Video Requirements

Detailed video format requirements, codec specifications, resolution limits, and encoding guidelines for optimal compatibility across platforms.

Covers: TikTok, Instagram, YouTube, LinkedIn, Facebook, X (Twitter), Threads, Pinterest, Bluesky

Includes: FFmpeg re-encoding solutions for compatibility issues

Getting Started

Authentication: All requests require an API key in the Authorization: Apikey <YOUR_API_KEY> header

Base URL: https://api.upload-post.com/api

Rate Limits: Free tier includes 10 uploads per month

Content Guidelines: Review platform-specific requirements before uploading

For implementation examples and integration guides, see our SDK Examples and Integration Guides.
