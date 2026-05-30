---
source_id: llm-section:docs/guides/n8n-integration.md
public_url: https://docs.upload-post.com/guides/n8n-integration
raw_source: llm.txt
collected: 2026-05-30
---

n8n Integration

Upload-Post provides seamless integration with n8n for automated video publishing workflows. This guide walks you through connecting your Upload-Post account with n8n.

Getting Started with Upload-Post

Create an account or log in to your existing Upload-Post account

Navigate to the "API Keys" section

Generate an API key for your n8n integration

API Configuration

For n8n integration, you'll need to configure an HTTP Request node with the following parameters:

:::info
URL Support for Media Files: You can now pass URLs for both photo and video uploads instead of binary files. Simply provide the direct URL to your media file in the video or photos\[] parameter.
:::

n8n Workflow Configuration

Configure your n8n HTTP Request node with these parameters:

| Field | Value | Required |
| ----- | ----- | -------- |
| title | Your video title | Optional |
| user | Your username | Required |
| platform\[] | tiktok | Required |
| video | Binary file | Required |

Node Configuration Steps:

Add an HTTP Request Node to your workflow

Configure the node settings:

Method: POST

URL: https://api.upload-post.com/api/upload

Headers: Authorization: Apikey \[YOUR\_API\_KEY]

Body: Set to multipart/form-data and add the required fields

Complete JSON Node Configuration

Below is the complete JSON configuration for the HTTP Request node in n8n:

For Instagram Uploads

To upload to Instagram instead, change the platform value:

Uploading to Multiple Platforms

To upload to both TikTok and Instagram simultaneously:

Security Best Practices

Never hardcode your API key directly in the workflow

Create a Credentials entry in n8n for your Upload-Post API key

Reference the credential in your HTTP Request node

For workflows that will be shared, export without credentials

Consider using environment variables or n8n's credential store

Example Workflow: AI-powered Social Media Publisher

This workflow automates video publishing with AI-generated descriptions:

Google Drive Trigger: Monitors a folder for new videos

OpenAI Transcription: Extracts audio and converts to text

OpenAI Description Generator: Creates engaging descriptions

Upload-Post HTTP Request: Uploads to multiple platforms

Error Handling: Sends notifications on completion/errors

This workflow is available as a template: View template on n8n.io

Need Assistance?

For additional help with your n8n integration, contact our support team.
