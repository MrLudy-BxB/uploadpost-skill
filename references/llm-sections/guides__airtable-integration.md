---
source_id: llm-section:docs/guides/airtable-integration.md
public_url: https://docs.upload-post.com/guides/airtable-integration
raw_source: llm.txt
collected: 2026-05-30
---

Airtable Integration

Manage your video uploads to social media platforms directly from Airtable.

Set Up

Running Airtable Automation Scripts requires a paid Airtable plan that includes automations with scripts.

This guide shows you how to automatically upload videos to social media platforms from Airtable via Upload-Post.

Gather Your API Key

Start by getting your API Key from your Upload-Post account in the "API Keys" section. This key will be used in the script.

Be sure you have configured your social media accounts in Upload-Post before proceeding.

:::info
URL Support for Media Files: You can now pass URLs for both photo and video uploads instead of binary files. Simply provide the direct URL to your media file in the video or photos\[] parameter.
:::

Create an Airtable Workspace

In Airtable, create a new workspace with these fields:

Title as Single Line Text

Platforms as Multi Select with types: tiktok, instagram

Video as Attachment

User as Single Line Text

Status as Single Line Text

Enter Test Video Data

Add sample data to test the integration:

Title: Enter a title for your video

Platforms: Select one or more platforms (tiktok, instagram)

Video: Attach a video file (MP4 format recommended)

User: Enter your username

Status: Enter pending (lowercase)

Build an Automation Script

Let's create an Airtable automation script that uploads videos via Upload-Post:

Add Trigger

In your workspace, click on Automation then +New automation

Name the automation

Click Choose a Trigger

Select When a Record is Created

Select your table

Click Done

Add Action

Click Add Action

Select Run Script

Delete any default code in the script editor

Copy and paste this code:

Replace Your API Key with your actual Upload-Post API key.

Test the Script

In the script editor, press >Test

The script will run and process any pending records. If successful, you'll see your videos being uploaded to the selected platforms, and the Status field will update to "success".

Security Best Practices

Never share your API key

Consider using environment variables where possible

Review records before processing large batches
