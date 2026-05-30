---
source_id: llm-section:docs/api/photo-requirements.md
public_url: https://docs.upload-post.com/api/photo-requirements
raw_source: llm.txt
collected: 2026-05-30
---

Photo Format Requirements

This document outlines the photo format requirements for uploading to various social media platforms via the API. For platforms where specific requirements are not listed, standard image formats like JPEG and PNG are generally accepted. However, for the most accurate and up-to-date information, please consult the official documentation of each respective platform.

Threads Photo Requirements

Format: JPEG, PNG

File Size: 8 MB maximum

Aspect Ratio: Limit: 10:1 (e.g., can be from 1:10 to 10:1)

Width:

Minimum: 320px (images narrower than 320px will be scaled up to 320px)

Maximum: 1440px (images wider than 1440px will be scaled down to 1440px)

Color Space: sRGB (images with other color spaces will be converted to sRGB)

Items Per Post: Up to 10 media items per post (carousel). If you provide more than 10 items, they are automatically distributed across multiple posts (up to 10 items per post).

Thread Media Layout: Use the threads\_thread\_media\_layout parameter to control how media items are distributed across posts. For example, "5,5" splits 10 items into 2 posts of 5 each.

Instagram Photo Requirements

Media Type: The API supports "IMAGE" for feed posts and "STORIES".

General Guidance: Instagram supports: png, jpeg, gif formats.

For detailed specifications (resolution, aspect ratio, file size), please refer to the official Instagram documentation.

TikTok Photo Requirements

While the API allows photo uploads to TikTok (e.g., for slideshows with auto\_add\_music), specific format requirements (resolution, aspect ratio, file size) are not detailed in the provided source.

General Guidance: Only image formats: JPG, JPEG, or WEBP are compatible.

Please refer to the official TikTok documentation for specific photo guidelines.

Facebook Photo Requirements

General Guidance: Facebook supports various image formats, including JPEG, PNG, GIF, and WebP.

The API upload-photo.md documentation notes that the description is applied only to the first photo uploaded.

For detailed specifications, please refer to the official Facebook documentation.

X (Twitter) Photo Requirements

General Guidance: X (Twitter) supports JPEG, PNG, GIF, and WEBP formats.

Max File Size: 5 MB per image

Images Per Tweet: Up to 4 images per tweet. If you provide more than 4 images, they are automatically distributed across a thread (up to 4 images per tweet).

Thread Image Layout: Use the x\_thread\_image\_layout parameter to control how images are distributed across tweets in the thread. For example, "4,4" puts 4 images in each of 2 tweets.

For detailed specifications, please refer to the official X/Twitter documentation.

LinkedIn Photo Requirements

General Guidance: LinkedIn supports JPEG, PNG, and GIF formats.

The API upload-photo.md common parameters apply. The caption is used as post commentary.

For detailed specifications, please refer to the official LinkedIn documentation.

Pinterest Photo Requirements

Max Image Size: 20 MB

Supported Formats: BMP, JPEG, PNG, TIFF, GIF, Animated GIF, WEBP

Recommended Size: 1000 x 1500 px

Aspect Ratio: 2:3

Minimum Size: 600 x 900 px

Maximum Size: 2000 x 3000 px

Content-Type: A valid media Content-Type such as image/jpeg, image/png, or image/webp returned by the hosting provider

Image Carousel:

Up to five carousel images

Images must be the same dimension

Reddit Photo Requirements

Max Image Size: 10 MB

Supported Formats: JPG, PNG, GIF, WEBP

Bluesky Photo Requirements

Max Images: 4 per post

Max File Size: 1 MB per image

Supported Formats: JPEG, PNG, GIF, WEBP

Alt Text: Supported and recommended

Daily Limit: 50 uploads per day (combined photos and videos)

Note: The information for Instagram, TikTok, Facebook, X (Twitter), and LinkedIn photo requirements above is general. The provided source code focused primarily on video specifications and Threads image specifications. Always check the official platform guidelines for the latest and most precise requirements.
