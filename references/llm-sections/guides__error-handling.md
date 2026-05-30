---
source_id: llm-section:docs/guides/error-handling.md
public_url: https://docs.upload-post.com/guides/error-handling
raw_source: llm.txt
collected: 2026-05-30
---

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

If there's a fundamental problem with your request before we attempt to upload to individual platforms, you will receive an HTTP status code other than 200 OK. The response body will typically look like this:

Here are common error status codes and their meanings:

400 Bad Request

Meaning: Your request was malformed or missing required information.

Common Causes: Missing video or photos file, missing title, invalid platform name, missing user identifier in the form data when required.

message examples: "Video file and title are required", "Title cannot be empty", "Username required", "Invalid platforms: \[platform\_name]", "Username not associated with any profile".

401 Unauthorized

Meaning: Authentication failed.

Common Causes: Missing Authorization header, using an invalid or expired API Key or Bearer Token.

message examples: "Authorization header required", "Invalid or expired token", "Invalid API key", "API key expired".

404 Not Found

Meaning: The user associated with your authentication could not be found in our system.

message example: "User not found".

429 Too Many Requests

Meaning: You have exceeded an API usage limit.

Common Causes: Reaching your monthly upload limit, or (for Professional plan users) reaching the daily upload limit for a specific social media account.

message examples: "You have reached your monthly limit of X uploads", "You have reached the daily limit of 5 uploads for: Instagram (account\_name)".

500 Internal Server Error

Meaning: An unexpected error occurred on our server while processing your request.

message example: Usually contains technical details about the error. If you encounter this repeatedly, please contact support.

In summary: Always check the HTTP status code first. If it's 200 OK, examine the success flag within the results dictionary for each platform. If it's not 200 OK, check the message field in the response body for the reason.
