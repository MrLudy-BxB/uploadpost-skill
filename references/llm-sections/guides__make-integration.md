---
source_id: llm-section:docs/guides/make-integration.md
public_url: https://docs.upload-post.com/guides/make-integration
raw_source: llm.txt
collected: 2026-05-30
---

Make Integration

Upload-Post provides seamless integration with Make (formerly Integromat) for automated video publishing workflows. This guide walks you through connecting your Upload-Post account with Make in 3 simple steps.

Getting Started with Upload-Post

Create an account or log in to your existing Upload-Post account

Navigate to the "API Keys" section

Generate an API key for your Make integration

API Configuration

For Make integration, you'll need to configure an HTTP module with the following parameters:

Note: Find your API key in your Upload-Post Manage API Keys section.

:::info
URL Support for Media Files: You can now pass URLs for both photo and video uploads instead of binary files. Simply provide the direct URL to your media file in the video or photos\[] parameter.
:::

Form Data Configuration & Make.com Setup

Configure your Make HTTP module with these parameters:

| Field | Value | Required |
| ----- | ----- | -------- |
| title | Your video title | Optional |
| user | Your username | Required |
| platform\[] | tiktok | Required |
| video | Binary file | Required |

Make.com Configuration Steps:

Add an HTTP Module: In your Make.com scenario, add an HTTP module and choose the "Make a Request" action.

Configure the Request Settings:

Method: Set to POST.

URL: Enter https://api.upload-post.com/api/upload.

Headers: Add a header with:

Key: Authorization

Value: Apikey \[YOUR\_API\_KEY]

Set the Request Body: Change the body type to multipart/form-data and add the following form fields:

title: Set the value to your desired title (you can use a variable if needed).

user: Enter your username you set in Upload-Post.

platform\[]: Set the value to tiktok.

video: Attach the binary file (your video file). Make sure this field is mapped to the binary data you want to send.

Save and Test: Save your scenario and run a test to ensure that the video upload works correctly via the API.

Advanced Configuration Options

For Instagram Uploads

To upload to Instagram instead, simply change the platform value to instagram in your form data.

Uploading to Multiple Platforms

To upload to both TikTok and Instagram simultaneously, add both platform values by creating multiple fields with the same name platform\[] in the Make.com HTTP module.

Securely Storing API Keys in Make.com

For better security, avoid hardcoding your API key directly in scenarios:

Create an App Key in Make.com

Store your Upload-Post API key as a constant

Reference the constant in your HTTP module headers

When sharing scenarios, use scenario blueprints which do not expose your keys

Example of referencing an API key constant in Make:

Need more guidance? Check out this detailed forum post: Make.com Community Tutorial

Need Assistance?

For additional help with your Make integration, contact our support team.
