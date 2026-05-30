---
source_id: llm-section:docs/guides/authentication.md
public_url: https://docs.upload-post.com/guides/authentication
raw_source: llm.txt
collected: 2026-05-30
---

Authentication

Upload-Post uses API keys to authenticate requests. This guide explains how to obtain and use your API key.

Getting Your API Key

Log in to your Upload-Post Dashboard

Navigate to the "API Keys" section

Click "Generate New API Key"

Copy and securely store your API key

Using Your API Key

Include your API key in the Authorization header of all API requests:

Example Request

Security Best Practices

Never share your API key: Keep your API key confidential

Use environment variables: Store your API key in environment variables

Rotate keys regularly: Generate new API keys periodically

Restrict access: Only share API keys with trusted team members

Monitor usage: Regularly check your API key usage in the dashboard

API Key Limits

Free tier includes 10 uploads per month

Additional uploads available through paid plans

Troubleshooting

If you receive a 401 Unauthorized error:

Verify your API key is correct

Check if your API key has expired

Ensure you're using the correct header format

Confirm your account is active

For additional help, contact our support team.
