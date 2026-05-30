---
source_id: llm-section:docs/api/user-profiles.md
public_url: https://docs.upload-post.com/api/user-profiles
raw_source: llm.txt
collected: 2026-05-30
---

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

403 Forbidden: Profile limit reached for the current plan (error\_code: PROFILE\_LIMIT\_REACHED).

409 Conflict: A profile with the provided username already exists.

Get User Profiles

Retrieves a list of all user profiles created under your API key.

Method: GET

Headers:

Authorization: Apikey YOUR\_API\_KEY

Query Parameters: None

Success Response (200 OK):

limit: The maximum number of profiles allowed by the current plan.

plan: The subscription plan associated with the API key.

profiles: An array of user profile objects.

created\_at: Timestamp of profile creation.

social\_accounts: An object detailing connected social media accounts. Each key is the platform name (e.g., facebook, instagram, tiktok). The value can be an object with details (display\_name, social\_images, username) or an empty string/null if not fully connected.

username: The unique identifier for the profile.

success: Indicates successful retrieval.

Error Responses:

401 Unauthorized: Invalid or missing API Key.

Get a Specific User Profile

Retrieves information for a single user profile using its username.

Method: GET

Endpoint: /api/uploadposts/users/{username}

Path Parameters

| Parameter  | Type   | Description                                            |
| :--------- | :----- | :----------------------------------------------------- |
| username | string | Required. The username of the profile to retrieve. |

Success Response (200 OK)

If the profile is found, the API will return a JSON object with the profile details.

Error Response (404 Not Found)

If no profile is found with the specified username, the API will return:

Delete User Profile

Deletes an existing user profile and its associated data (like social connections).

Method: DELETE

Headers:

Authorization: Apikey YOUR\_API\_KEY

Content-Type: application/json

Body Parameters:

| Name     | Type   | Required | Description                                      |
|----------|--------|----------|--------------------------------------------------|
| username | String | Yes      | The unique identifier of the profile to delete. |

Example Body:

Success Response (200 OK):

Error Responses:

400 Bad Request: Missing or invalid username.

401 Unauthorized: Invalid or missing API Key.

404 Not Found: No profile found with the provided username.

JWT Management

Generate and validate JWTs for the secure social account linking process.

Endpoint: Generate JWT URL

Generates a secure, single-use URL containing a JWT. Your user visits this URL to link their social media accounts.

Method: POST

Headers:

Authorization: Apikey YOUR\_API\_KEY

Content-Type: application/json

Body Parameters:

| Name         | Type    | Required | Description                                                                                      |
|--------------|---------|----------|--------------------------------------------------------------------------------------------------|
| username     | String  | Yes      | The identifier for the user profile for which the JWT is being generated.                        |
| redirect\_url | String  | No       | (Optional) The URL to which the user will be redirected after linking their social account.      |
| logo\_image   | String  | No       | (Optional) A URL to a logo image to display on the linking page for branding purposes.           |
| redirect\_button\_text | String | No | (Optional) The text to display on the redirect button after linking. Defaults to "Logout connection". |
| connect\_title | String | No | (Optional) Custom title text for the connection page. Defaults to "Connect Social Media Accounts". |
| connect\_description | String | No | (Optional) Custom description text for the connection page. Defaults to "Connect your social media accounts to manage your posts.". |
| platforms    | Array   | No       | (Optional) List of platforms to show for connection. Possible values: 'tiktok', 'instagram', 'linkedin', 'youtube' (not working, waiting for audit), 'facebook', 'x', 'threads', 'google\_business'. Defaults to all supported platforms. |
| show\_calendar | Boolean | No       | (Optional) Whether to show the calendar view on the connection page. Defaults to true.         |
| readonly\_calendar | Boolean | No   | (Optional) When true, shows only a read-only calendar view. The user cannot edit, delete, or create posts, and cannot connect or disconnect social accounts. Ideal for sharing a content calendar with end clients. Defaults to false. |

Example Body:

Success Response (200 OK):

access\_url: The secure URL your user needs to visit. Redirect your user to this URL.

success: Always true if the request was successful.

duration: The validity period of the generated JWT (48 hours).

Example Request (curl):

Example Request with Calendar Disabled (curl):

Example Request with Read-Only Calendar for Clients (curl):

This generates a link where the client only sees the calendar with scheduled posts (social channel, date/time, visual, text) but cannot edit anything or access other sections.

Example Success Response (200 OK):

Calendar Deep Link: If you want users to land directly on the shared calendar view, replace the path with /connect/calendar while keeping the token intact, e.g. https://app.upload-post.com/connect/calendar?token=GENERATED\_JWT\_TOKEN. The page will automatically fall back to /connect when the profile has show\_calendar disabled. When readonly\_calendar is true, the user is automatically redirected to the calendar view regardless of the URL path.

Error Responses:

400 Bad Request: Missing or invalid username.

401 Unauthorized: Invalid or missing API Key.

403 Forbidden: Profile exists but is blocked by plan limits (error\_code: PROFILE\_BLOCKED).

404 Not Found: No profile found with the provided username (error\_code: PROFILE\_NOT\_FOUND).

Integration tip: If JWT generation returns 404, call GET /api/uploadposts/users first to confirm the profile exists and that profile creation did not fail due to plan limits.

Mobile OAuth Compatibility

When users open the connection page on a mobile device (iOS or Android), the
operating system may intercept OAuth URLs (e.g. instagram.com, accounts.google.com)
and open the corresponding native app instead of keeping the flow in the browser.
Because native apps cannot handle the OAuth authorization URL, the connection fails.

Upload-Post automatically detects mobile browsers and routes OAuth redirects through
a secure intermediate page (/api/uploadposts/oauth/bounce) that performs the
redirect via JavaScript. This bypasses Universal Links (iOS) and App Links (Android)
interception so the OAuth flow stays entirely in the mobile browser.

No action is required from API consumers — the mobile-safe redirect is applied
automatically when the user accesses the access\_url on a mobile device.

Endpoint: Validate JWT

(Optional) Allows you to validate a JWT token. The primary validation occurs automatically when the user accesses the access\_url.

Method: POST

Headers:

Authorization: Bearer YOUR\_JWT\_TOKEN

Body Parameters: None

Example Request (curl):

Success Response (200 OK - Valid Token): Returns the profile details associated with the token.

profile: Contains details about the user profile linked to the token.

social\_accounts: An object showing the connection status for various platforms (e.g., null if not connected, or details if connected).

username: The unique identifier provided when the profile was created.

success: Indicates the token is valid.

Success Response (200 OK - Invalid Token):

Error Responses:

401 Unauthorized: Invalid, expired, or missing JWT token in the Authorization header.

Facebook Pages

Retrieve Facebook page IDs associated with user profiles to enable posting to Facebook pages.

Endpoint

Get Facebook Pages

Fetches Facebook page IDs associated with a profile. You can use this endpoint to connect and start posting on Facebook pages.

Method: GET

Headers:

Authorization: Apikey YOUR\_API\_KEY

Query Parameters:

| Name    | Type   | Required | Description                                                                          |
|---------|--------|----------|--------------------------------------------------------------------------------------|
| profile | String | No       | The unique identifier of the profile. If not specified, returns all pages for your account. |

Example Request (curl):

Example Request (without profile parameter):

Success Response (200 OK):

pages: Array of Facebook page objects associated with the profile(s).

page\_id: The Facebook page ID that can be used for posting.

page\_name: The display name of the Facebook page.

profile: The profile identifier associated with this page.

success: Indicates successful retrieval.

Error Responses:

401 Unauthorized: Invalid or missing API Key.

404 Not Found: No profile found with the provided identifier (if profile parameter is specified).
