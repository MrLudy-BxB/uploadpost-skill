---
source_id: llm-section:docs/guides/mcp-server-integration.md
public_url: https://docs.upload-post.com/guides/mcp-server-integration
raw_source: llm.txt
collected: 2026-05-30
---

MCP Server (Claude / Cursor)

Upload-Post ships an official Model Context Protocol (MCP) server. Connect it to Claude Desktop, Claude Code, Cursor, or any other MCP-compatible AI agent and your assistant can publish, schedule, analyze and manage social media on your behalf — without writing any code.

The server exposes 40 tools that map 1:1 to the public Upload-Post REST API.

Hosted endpoint: https://mcp.upload-post.com/mcp

Source: github.com/Upload-Post/upload-post-mcp (MIT)

How authentication works

The server is multi-tenant and stateless. Each MCP client sends its own Upload-Post API key in the Authorization header of every request. The server uses that key for the duration of the session and stores nothing.

Authorization: Bearer YOUR\_UPLOAD\_POST\_API\_KEY is also accepted, for clients that only allow Bearer tokens.

Get your API key

Sign in to app.upload-post.com.

Open the API Keys section.

Generate a new key and copy it.

Connect Claude Desktop / Claude Code / Cursor

Add the following entry to your MCP config (~/.claude/mcp.json, ~/.cursor/mcp.json, or your IDE's equivalent):

Restart your client. You should see 40 upload-post tools become available.

What the agent can do

| Group         | Tools |
|---------------|-------|
| Upload        | upload\_video, upload\_photos, upload\_text, upload\_document |
| Status        | get\_status, get\_job\_status, get\_history, get\_media |
| Schedule      | list\_scheduled, cancel\_scheduled, edit\_scheduled |
| Analytics     | get\_analytics, get\_total\_impressions, get\_post\_analytics, get\_platform\_metrics |
| Users         | get\_account\_info, list\_users, create\_user, delete\_user, generate\_jwt, validate\_jwt |
| Pages / boards| get\_facebook\_pages, get\_linkedin\_pages, get\_pinterest\_boards, get\_google\_business\_locations, select\_google\_business\_location, get\_reddit\_detailed\_posts |
| Comments      | get\_post\_comments, reply\_to\_comment, public\_reply\_to\_comment |
| DMs           | send\_dm, list\_dm\_conversations, manage\_autodms |
| FFmpeg        | submit\_ffmpeg\_job, get\_ffmpeg\_job, download\_ffmpeg\_result, get\_ffmpeg\_consumption |
| Queue         | get\_queue\_settings, update\_queue\_settings, preview\_queue |

Async uploads return a request\_id; the agent polls get\_status until success: true.

Example prompts

Once connected, try these in your AI client:

"List the users in my Upload-Post account."

"Publish this video URL to TikTok and Instagram under the profile marketing with the caption 'Spring launch'."

"Schedule a text post on LinkedIn for next Monday at 10:00 Madrid time."

"Show me the analytics for my profile marketing over the last month."

"Reply privately to the latest comment on my Instagram post."

The model decides which tools to call based on the request; you don't have to name them.

Self-hosting (optional)

If you prefer to run the server yourself — for an isolated network, custom auth proxy, or stricter compliance requirements — the source repository ships with a multi-stage Dockerfile and a one-click Coolify configuration. See the project README for instructions.

Local-only stdio mode

For single-user setups (no hosted server), you can run the MCP locally via npx. Once published to npm, the configuration becomes:

Both modes expose the same 40 tools.

Need assistance?

Open an issue at github.com/Upload-Post/upload-post-mcp/issues or contact our support team.
