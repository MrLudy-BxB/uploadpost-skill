---
source_id: llm-section:docs/resources/character-limits.md
public_url: https://docs.upload-post.com/resources/character-limits
raw_source: llm.txt
collected: 2026-05-30
---

Social Media Character Limits

This guide summarizes the most relevant text limits for each social network supported by Upload-Post. Keep these constraints in mind when building payloads so posts are accepted without truncation.

Platform-Specific Character Limits

Facebook Character Limits

| Property | Description |
| --- | --- |
| post | 63,206 characters maximum |
| title | Reels title – 255 characters maximum |

Instagram Character Limits

| Property | Description |
| --- | --- |
| post | 2,200 characters maximum |
| altText | 1,000 characters maximum per image |
| comment | 2,196 characters maximum |

LinkedIn Character Limits

| Property | Description |
| --- | --- |
| post | 3,000 characters maximum |
| title | 400 characters maximum |
| comment | 1,250 characters maximum |

TikTok Character Limits

| Property | Description |
| --- | --- |
| post | 2,200 characters maximum |
| title (photo posts) | 90 characters maximum |
| description (photo posts) | 4,000 characters maximum |

Pinterest Character Limits

| Property | Description |
| --- | --- |
| post | 500 characters maximum |
| title | 100 characters maximum |
| link | 2,048 characters maximum |
| altText | 500 characters maximum |

Reddit Character Limits

| Property | Description |
| --- | --- |
| post | 5,000 characters maximum |
| title | 300 characters maximum |
| comment | 10,000 characters maximum |

Threads Character Limits

| Property | Description |
| --- | --- |
| post | 500 characters maximum |

X (Twitter) Character Limits

| Property | Description |
| --- | --- |
| post | 280 characters maximum |
| post (Premium) | 25,000 characters maximum for Premium and Premium Plus accounts |
| altText | 1,000 characters maximum per image |
| subTitleName | 150 characters maximum |

:::warning URLs are automatically removed from X posts
Upload-Post strips every URL that X would turn into a clickable link from any
caption, title, description, or first\_comment sent to X (Twitter), before
the post is published. This applies to video, photo, and text uploads via
every endpoint and via the scheduler.

Why: X charges $0.200 per "Content: Create (with URL)" request versus
$0.015 for posts without a URL — a 13× surcharge. To keep pricing predictable,
every parseable link is removed before the tweet leaves Upload-Post.

What gets stripped: anything X's parser recognises as a URL:

Schemed URLs: https://…, http://…, ftp://…, ws://…

www.host.tld\[/path]

Common shorteners: t.co/…, bit.ly/…, tinyurl.com/…, lnkd.in/…,
youtu.be/…, etc.

Bare hostnames with a path: example.com/posts/1

IPv4 with a path: 192.168.1.1:8080/admin

Markdown link syntax: \[text]\(https://…) (the URL portion is removed)

What is NOT stripped: anything that X does not parse as a link does not
trigger the surcharge, so we leave it alone — e.g. example\[.]com,
example(dot)com, hxxp://…, unicode-dot lookalikes (example．com), or
bare domains with no path. These display as plain text in the tweet and are
billed at the normal $0.015 rate.

If you need to share a link, put it in your X profile bio or render it visibly
inside an image/video.
:::

Bluesky Character Limits

| Property | Description |
| --- | --- |
| post | 300 characters maximum |
| images | Up to 4 images per post |
| altText | Supported |

YouTube Character Limits

| Property | Description |
| --- | --- |
| post | 5,000 characters maximum |
| youTubeOptions > title | 100 characters maximum |
| youTubeOptions > tags | 500 characters total, 2+ characters each |
| youTubeOptions > subTitleName | 150 characters maximum |

Snapchat Character Limits

| Property | Description |
| --- | --- |
| Spotlight description | 500 characters maximum |
| Saved Story title | 45 characters maximum |

Notes:

Stories are ephemeral (24 hours) and don't support text captions

Saved Stories are permanent on your public profile

Spotlight posts are permanent and reach wider audiences

Only one media item (video or image) allowed per post

Hashtags are supported and clickable in Spotlight posts

Content Restrictions

Banned Hashtags

Google Business Profile Character Limits

| Field | Limit |
|-------|-------|
| Post summary (title) | 1,500 characters |
| Event title | 58 characters |
| Offer coupon code | 58 characters |
| Offer terms | 1,000 characters |
| CTA URL | Standard URL length |

Notes:

Google Business Profile supports one photo per post via the API.

Video uploads are not supported via the API.

Product posts cannot be created via the API.

Upload-Post validates content against a list of prohibited hashtags before posting to Instagram. Posts containing any of these hashtags will be rejected with a validation error. The complete list of banned hashtags includes:

A: anorexia, alone, a$$, antivax, abdl, addmysc, adulting, always, armparty, asiagirl

B: beautyblogger, bikinibody, boho, blogladrona, brain, besties, bikinibod

C: costumes, curvygirls, cancer

D: date, dating, desk, dm

E: elevator, edm, endme

F: followtrain, followtrains

G: graffitiigers, girlsonly, gloves

H: hardworkpaysoff, happythanksgiving, humpday, hustler, hotgirls

I: iphonegraphy, italiano, ifb

K: kansas, killingit, kissing, kill, killme, killyourself, kys

M: master, models, mustfollow, milf, midget

N: nasty, newyearsday

P: petite, petitegirls, pushups, payme

S: saltwater, shit, shower, single, singlelife, skype, snap, snapchat, snapchatme, snowstorm, sopretty, stranger, streetphoto, sunbathing, swole, suicide, suicideawareness

T: tag4like, tanlines, teens, teen, thought, todayimwearing

U: undies, unbalanced

V: valentinesday

W: workflow

Y: youngmodel, yolo

If your content includes any of these hashtags, remove them before submitting your request to avoid validation errors.

API Considerations

Upload-Post validates payload sizes before sending them to social networks whenever limits are known. Requests that exceed the documented limits return a validation error.

Some platforms might truncate overlong text instead of rejecting it (Meta products and YouTube occasionally do this). Inspect the per-platform response inside results to confirm the final content.

For channels with strict limits such as X, consider shortening URLs in your application prior to calling the Upload-Post API.

Updates and Changes

Social networks regularly adjust their limits. We keep this page aligned with the latest behavior we observe in production, but you should also:

Check Upload-Post API responses for detailed error messages about rejected posts.

Subscribe to our release notes for platform updates.

Revisit this reference periodically, especially before large content campaigns.
