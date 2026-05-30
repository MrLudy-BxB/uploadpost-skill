---
source_id: llm-section:docs/api/video-requirements.md
public_url: https://docs.upload-post.com/api/video-requirements
raw_source: llm.txt
collected: 2026-05-30
---

Video Format Requirements

This document outlines the video format requirements for uploading to various social media platforms via the API.

Automatic Video Transformation

Our API automatically transforms videos to adapt them to the specifications of each social network. However, if you use this feature, the upload will take longer because the transformation is performed first before uploading to the platforms.

If you prefer faster uploads, you can pre-process your videos according to the specific requirements of each platform outlined in this document.

Video Encoding Compatibility

Some video creation tools occasionally produce videos with encoding that Meta's systems don't accept. At times, their output needs to be re-encoded for compatibility.

One Solution: Re-encode with FFmpeg

If your video uploads are failing, try re-encoding the video using FFmpeg, an open-source tool for video processing:

This command converts your video to use the widely-compatible H.264 video codec and AAC audio codec, which Meta platforms accept.

Re-encoding "normalizes" your video to use standard encoding parameters that Meta's platforms are designed to process, without sacrificing quality. If you see these errors regularly, this simple step can save you frustration when sharing your creative content.

FFmpeg Installation and Usage

Installation instructions:

macOS: brew install ffmpeg

Windows: winget install ffmpeg

Linux: sudo apt install ffmpeg (Ubuntu/Debian) or sudo dnf install ffmpeg (Fedora)

Parameters:

-c:v libx264: Uses H.264 video codec

-preset medium: Balance between encoding speed and quality

-profile:v high -level 4.0: Compatibility settings

-pix\_fmt yuv420p: Standard pixel format for maximum compatibility

-b:v 5000k: Video bitrate (adjust as needed for quality)

-c:a aac: AAC audio codec

-b:a 192k: Audio bitrate

-movflags +faststart: Optimizes file for web streaming

TikTok Video Requirements

Supported Formats: MP4 (recommended), WebM, MOV

Supported Codecs: H.264 (recommended), H.265, VP8

Framerate: Minimum: 23 FPS, Maximum: 60 FPS

Picture Size: Minimum: 360 pixels (height and width), Maximum: 4096 pixels (height and width)

Duration: Maximum via API: 10 minutes. (Note: All TikTok creators can post 3-minute videos. Some creators have access to 5-minute or 10-minute videos. Users may trim videos in the TikTok app.)

Size: Maximum: 4GB

Instagram Video Requirements

Container Format: MOV or MP4 (MPEG-4 Part 14)

No edit lists

Moov atom at the head of the file

Audio Codec: AAC

Maximum sampling rate: 48 kHz

1 or 2 channels (mono or stereo)

Audio bitrate: 128 kbps

Video Codec: HEVC or H264

Progressive scan

Closed GOP

Chroma subsampling: 4:2:0

Video bitrate: VBR, maximum 25 Mbps

Frame Rate: 23-60 FPS

Image Size:

Maximum horizontal pixels: 1,920

Aspect ratio: 0.01:1 to 10:1

Recommended aspect ratio: 9:16 (to avoid cropping or white space)

Duration & Size:

Maximum duration: 15 minutes (Instagram increased from 90 seconds)

Minimum duration: 3 seconds

Maximum file size: 300 MB

Note: Instagram now supports Reels up to 15 minutes in duration. The previous 90-second limit has been removed.

YouTube Video Requirements

File Size: Maximum: 256 GB

Accepted MIME Types: video/\*, application/octet-stream

Important: Custom thumbnails are not supported for YouTube Shorts; they only apply to standard YouTube videos.

LinkedIn Video Requirements

File Size: Minimum: 75 KB, Maximum: 5 GB

Duration: Minimum: 3 seconds, Maximum: 10 minutes

Resolution:

Range: 256 x 144 to 4,096 x 2,304

Aspect ratio: 1:2.4 to 2.4:1

Technical Specs:

Frame rate: 10-60 fps

Bitrate: 192 kbps - 30 Mbps

Supported Formats: AAC, ASF, FLV, MP3, MP4, MPEG-1, MPEG-4, MKV, WebM, H264/AVC, Vorbis, VP8, VP9, WMV2, WMV3

Facebook Video Requirements

Reels

File Format: MP4 (recommended)

Resolution & Aspect Ratio:

Recommended: 1080 x 1920 pixels

Minimum: 540 x 960 pixels

Aspect ratio: 9:16

Duration:

3-90 seconds

Maximum 60 seconds for page stories

Video Settings:

Frame rate: 24-60 fps

Chroma subsampling: 4:2:0

Closed GOP (2-5 seconds)

Compression: H.264, H.265, VP9, AV1

Progressive scan

Audio Settings:

Bitrate: 128 kbps+

Channels: Stereo

Codec: AAC (low complexity)

Sample rate: 48 kHz

Normal Page Videos

Use facebook\_media\_type=VIDEO to upload regular videos to a Facebook Page. Normal page videos have more permissive requirements than Reels:

File Format: MP4 (recommended), MOV, AVI, and other common formats

Resolution: Up to 4096x4096 pixels, any aspect ratio

Duration: Up to 4 hours

File Size: Up to 10 GB

Thumbnails: Custom thumbnails are supported via the thumbnail\_url parameter. See the Facebook Video Thumbnails API.

X (Twitter) Video Requirements

Recommended Codec & Profile:

Video: H264 High Profile

Audio: AAC LC (Low Complexity)

Frame Rates:

Recommended: 30 FPS, 60 FPS

Maximum: 60 FPS

Resolution:

Recommended: 1280x720 (landscape), 720x1280 (portrait), 720x720 (square)

Dimensions: 32x32 to 1920x1920

Bitrate:

Minimum Video: 5,000 kbps

Minimum Audio: 128 kbps

Aspect Ratio:

Recommended: 16:9 (landscape/portrait), 1:1 (square)

Range: 1:3 to 3:1

Pixel Aspect Ratio: 1:1

Duration & File Size:

Duration: 0.5 seconds - 14,400 seconds (4 hours) for Premium

Max File Size: 1 GB+ for Premium/Amplify

Technical Video Specs:

Pixel Format: YUV 4:2:0

GOP: Must not be open

Scan Type: Progressive scan

Technical Audio Specs:

Channels: Mono or Stereo (not 5.1 or greater)

High-Efficiency AAC: Not supported

Custom Thumbnails: Supported for Premium/Amplify users

Threads Video Requirements

Container: MOV or MP4

No edit lists

moov atom at the front

Audio Codec: AAC

48kHz sample rate maximum

1 or 2 channels (mono/stereo)

Bitrate: 128 kbps

Video Codec: HEVC or H264

Progressive scan

Closed GOP

4:2:0 chroma subsampling

Frame Rate: 23-60 FPS

Picture Size:

Max columns (horizontal pixels): 1920

Aspect ratio: 0.01:1 to 10:1 (9:16 recommended)

Video Bitrate: VBR, 100 Mbps maximum

Duration:

Max: 300 seconds (5 minutes)

Min: > 0 seconds

File Size: 1 GB maximum

Pinterest Video Requirements

File Size: Maximum: 1 GB

Supported Formats: MP4, MOV, M4V

Duration: Minimum: 4 seconds, Maximum: 15 minutes

Aspect Ratio: Taller than 1.91:1 and shorter than 1:2. Recommended for standard video: 1:1 (square) or 2:3, 4:5 or 9:16 (vertical)

Reddit Video Requirements

File Size: Maximum: 1 GB

Supported Formats: MP4, MOV

Duration: Maximum: 15 minutes

Aspect Ratio: 1:1, 4:5, 9:16, or 16:9

Frame Rate: Up to 30 FPS recommended

Bluesky Video Requirements

File Size: Maximum: 100 MB

Supported Formats: MP4, MPEG, WebM, MOV

Duration: Minimum: 1 second, Maximum: 3 minutes (180 seconds)

Frame Rate: 10-60 FPS

Resolution: Minimum: 360x360 px, Maximum: 1920x1920 px

Aspect Ratio: Automatically detected and passed as metadata

Daily Limit: 50 uploads per day (combined photos and videos)
