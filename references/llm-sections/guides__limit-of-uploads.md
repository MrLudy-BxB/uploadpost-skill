---
source_id: llm-section:docs/guides/limit-of-uploads.md
public_url: https://docs.upload-post.com/guides/limit-of-uploads
raw_source: llm.txt
collected: 2026-05-30
---

Limit of uploads

Social Hard Caps Per Network

To protect your connected accounts and stay compliant with each social network, Upload-Post enforces platform hard caps using a rolling 24-hour window. When a cap is reached for a specific account on a given network, further posts to that account/network are rejected until the window rolls over.

What counts toward the cap: only successful publishes recorded for that account/network in the last 24 hours.

Scope: Per connected social account. These limits are NOT global to your Upload-Post user.

Example: If you manage 5 Profiles, and each Profile has its own TikTok account connected, you get the full limit for each TikTok account. (e.g., 5 TikTok accounts × 15 posts = 75 posts per day total).

Scheduled posts: caps are re-checked at execution time; if the cap is already reached, the publish will be rejected then.

Recommended and enforced daily caps

| Social Network     | Hard Cap (posts per 24h) |
| :----------------- | -----------------------: |
| Instagram          |                       50 |
| TikTok             |                       15 |
| LinkedIn           |                      150 |
| YouTube            |                       10 |
| Facebook           |                       25 |
| X (Twitter)        |              See per-plan table below |
| Threads            |                       50 |
| Pinterest          |                       20 |
| Reddit             |                       40 |
| Bluesky            |                       50 |

X (Twitter) per-plan daily caps

X uploads have a per-plan hard cap (one cap per connected X profile, per 24-hour rolling window) instead of a single flat limit. All other platforms keep the flat caps shown above.

| Plan         | X Hard Cap (posts per profile per 24h) |
| :----------- | -------------------------------------: |
| Default (free) |                                   10 |
| Basic        |                                     10 |
| Professional |                                     20 |
| Advanced     |                                     30 |
| Business     |                                     30 |

Error response when the cap is reached

Status: 429 Too Many Requests

Body example:

What else the verifier checks

Duplicate/similar content within 48h (per account/network) to reduce spam risk and shadow bans.

Mention limits to avoid spammy behavior (e.g., excessive mentions or repeating the same handle too frequently).

Media and content sanity checks evolve over time to align with network guidelines.
