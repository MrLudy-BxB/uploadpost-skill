# User Profiles, Current User, and JWT

Collected: 2026-05-30

## Source Traceability

| Source | Public URL |
| --- | --- |
| `docs/api/current-user.md` | https://docs.upload-post.com/api/current-user |
| `docs/api/user-profiles.md` | https://docs.upload-post.com/api/user-profiles |
| `docs/guides/user-profile-integration.md` | https://docs.upload-post.com/guides/user-profile-integration |
| `docs/guides/reached-active-user-cap-error.md` | https://docs.upload-post.com/guides/reached-active-user-cap-error |
| `openapi.json` | https://docs.upload-post.com/openapi.json |

## Endpoint Routing

| Method | Path | Operation | Summary | Required fields |
| --- | --- | --- | --- | --- |
| `POST` | `/uploadposts/users` | `createUserProfile` | Create user profile | username |
| `GET` | `/uploadposts/users` | `listUserProfiles` | List user profiles | - |
| `DELETE` | `/uploadposts/users` | `deleteUserProfile` | Delete user profile | username |
| `GET` | `/uploadposts/users/{username}` | `getUserProfile` | Get a specific user profile | username |
| `GET` | `/uploadposts/me` | `getCurrentUser` | Get current user | - |
| `POST` | `/uploadposts/users/generate-jwt` | `generateJwt` | Generate JWT URL | username |
| `POST` | `/uploadposts/users/validate-jwt` | `validateJwt` | Validate JWT | - |

## Practical Notes

- Use the endpoint index for exact method/path lookup and this reference for task-level routing.

## Source Excerpts

### docs/api/current-user.md

Current User API

Verify the validity of your API key and retrieve basic account information.

Authentication

Get Current User

Validates your API key and returns the associated email and subscription plan.

Endpoint

Headers

| Name          | Required | Description                |
|---------------|----------|----------------------------|
| Authorization | Yes      | Apikey YOUR\_API\_KEY      |

Example Request (curl)

Success Response (200 OK)

Response Fields:

| Field   | Type    | Description                                                        |
|---------|---------|--------------------------------------------------------------------|
| success | Boolean | Always true for successful requests                              |
| message | String  | Confirmation message                                               |
| email   | String  | The email address associated with the authenticated account        |
| plan    | String  | Current subscription plan (e.g., Basic, Professional, Business, Default) |
| preferences | Object | User preferences. See Preferences below.      |

Error Responses

401 Unauthorized - Invalid or missing authentication

500 Internal Server Error - Server-side error

Preferences

Manage user-level preferences via the preferences endpoint.

Get Preferences

Response (200 OK):

Update Preferences

Body (JSON):

| Field        | Type    | Description                                     |
|--------------|---------|-------------------------------------------------|
| weekStartDay | Integer | Calendar week start day. 0 = Sunday, 1 = Monday. |

[Excerpt truncated in this topic reference; see the source URL for full page context.]

### docs/api/user-profiles.md

User Profiles API

These endpoints allow you to integrate Upload-Post directly into your platform by managing user profiles and generating secure tokens for account linking.

See the User Profile Integration Guide for a conceptual overview and workflow.

Authentication

All API requests require authentication using your API Key. Include it in the Authorization header for every request:

Replace YOUR\_API\_KEY with the actual API key provided to you.

User Profile Management

Manage user profiles within Upload-Post that correspond to users on your platform.

Endpoint

Create User Profile

Creates a new profile linked to a user on your platform.

Method: POST

Headers:

Authorization: Apikey YOUR\_API\_KEY

Content-Type: application/json

Body Parameters:

| Name     | Type   | Required | Description                                                                 |
|----------|--------|----------|-----------------------------------------------------------------------------|
| username | String | Yes      | A unique identifier for the user on your platform (e.g., your internal ID). |

Example Body:

Success Response (201 Created):

profile: Contains details of the newly created profile.

created\_at: Timestamp of profile creation.

social\_accounts: Object showing connected accounts (initially empty or with placeholders).

username: The unique identifier provided.

success: Indicates successful creation.

Error Responses:

400 Bad Request: Missing or invalid username.

401 Unauthorized: Invalid or missing API Key.

[Excerpt truncated in this topic reference; see the source URL for full page context.]

### docs/guides/user-profile-integration.md

White-label Integration Guide

Profiles diagram

This guide explains how to integrate Upload-Post directly into your own platform. This allows your users to connect their social media accounts securely through Upload-Post, enabling your platform to manage their profiles and posts via the API on their behalf.

Integration Flow Overview

The core idea is to create a unique profile within Upload-Post for each user on your platform who wants to connect their social accounts. You then generate a special, secure URL that the user visits to link their accounts. Once linked, your platform can interact with the Upload-Post API using the user's unique identifier.

Step-by-Step Integration

Step 1: Create a User Profile

For each user on your platform, you need to create a corresponding profile in Upload-Post. This is done by making a POST request to the /api/uploadposts/users endpoint.

Requirement: You must provide a unique username in the request body. This username should be a stable identifier that links the Upload-Post profile back to the user on your platform (e.g., your internal user ID).

Authentication: Remember to include your Authorization: Apikey YOUR\_API\_KEY header.

Result: The API will respond with details of the created profile, confirming the username.

➡️ See details: Create User Profile API Reference

Step 2: Generate the Secure JWT URL

Once the profile exists, you need to generate a secure URL that your user will use to connect their social media accounts. Make a POST request to the /api/uploadposts/users/generate-jwt endpoint.

[Excerpt truncated in this topic reference; see the source URL for full page context.]

### docs/guides/reached-active-user-cap-error.md

Error: {"code":"reached\_active\_user\_cap"}

If you've encountered this error, don't worry. This is not an issue with your account, your content, or our platform's stability. It's a temporary limitation from the TikTok API.

This error means that the daily limit of active users allowed by TikTok for our application has been reached.

What is a "Daily Active User"?

In this context, a "daily active user" is anyone who uses our application to interact with the TikTok API on a given day. TikTok sets a cap on how many unique users can do this through a single application (like ours) within a 24-hour period.

What should you do?

Your account and content are safe. This is not a penalty or a block on your account.

The best solution is to wait and retry. The user cap is reset by TikTok every 24 hours. We recommend waiting a few hours before trying to post again.

If the error persists for more than 24 hours, please try again the next day.

Why does this happen?

To manage their platform's resources, TikTok imposes a daily usage quota on every application that connects to its API. Due to the rapid growth of our user community, we are sometimes hitting this maximum allowed number of daily users.

What are we doing about it?

We are actively working on a solution. We are in direct communication with TikTok's developer support team to request an increase in our daily user quota.

Unfortunately, the timeline for this increase is determined by TikTok, and we cannot expedite their internal review process. We appreciate your patience as we work to resolve this for good.

[Excerpt truncated in this topic reference; see the source URL for full page context.]
