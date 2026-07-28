# Lotus — AI Citation Intelligence (MCP Server)

Lotus is a Generative Engine Optimization (GEO) platform. It measures whether
your brand is actually cited as a source inside LLM-generated answers, and
hands your coding agent the exact code to fix what is missing.

This repository documents the **hosted Lotus MCP server**. The server is a
managed remote endpoint — there is nothing to install or self-host.

- **Endpoint:** `https://lotus.clicon.app/mcp/` (trailing slash required)
- **Transport:** streamable-http, stateless
- **Registry:** `app.clicon/lotus`
- **Website:** https://clicon.app

## What it does

Search engines told you when you ranked badly. Generative engines do not.
If an assistant hallucinates your product, ignores you, or cites a competitor
instead, you lose the business with no signal that it happened.

Lotus closes that loop in three steps:

1. **Measure** — real citation measurements across LLM answers for your domain,
   plus crawler activity and Search Console signals.
2. **Fix** — prioritized "quick wins" with ready-to-install code (JSON-LD
   structured data, `llms.txt`).
3. **Verify** — Lotus fetches your live site and confirms the fix is really
   there. A fix is not "applied" because an agent said so; it is applied
   because it was found in production.

## Tools

`analyze_geo` works without credentials, so any agent can evaluate a domain
before signing up. Every other tool requires an API key. Tools that take no
`domain` argument resolve the domain from the key itself.

### Preview

| Tool | Description |
|---|---|
| `analyze_geo` | GEO diagnosis for any domain: visibility score and estimated revenue at risk |

### Read (API key)

| Tool | Description |
|---|---|
| `get_quick_wins` | Pending fixes, each with title, category, priority, estimated impact and a `hash` |
| `get_quick_win_code` | Full, install-ready code for one quick win, by `hash` |
| `get_competitor_actions` | Competitor moves classified by AI-discoverability layer |
| `get_artifact` | Retrieve a generated artifact (`json_ld`, `llms_txt`) |
| `get_bot_activity` | AI crawler activity on your site over the last N days |
| `get_bleed_model` | Estimated business lost through the generative channel, with its provenance |
| `verify_installation` | Check whether your artifacts are live on your site right now |
| `get_citation_verdict` | Latest citation score plus the per-query measurements behind it |

### Write (API key)

| Tool | Description |
|---|---|
| `mark_applied` | Marks a quick win as applied — after verifying it against your live site |
| `generate_share_link` | Create a shareable report link |
| `regenerate_artifacts` | Regenerate your artifacts. Rate-limited; paid tiers only |

## Configuration

Add the server to any MCP-compatible client (Claude Code, Claude Desktop,
Cursor, VS Code, or your own agent):

```json
{
  "mcpServers": {
    "lotus": {
      "url": "https://lotus.clicon.app/mcp/",
      "headers": {
        "X-API-Key": "YOUR_LOTUS_API_KEY"
      }
    }
  }
}
```

The API key is sent as the `X-API-Key` **header** — never as a tool argument
or a query parameter. Keys are issued during onboarding at
[clicon.app](https://clicon.app). Without a key the server still responds to
`analyze_geo`.

Exact config syntax varies slightly per client; the shape above is what most
of them expect.

## Usage

Talk to your agent in plain language — it picks the tools:

> "Run the Lotus GEO analysis on my domain."

> "Show me the pending Lotus quick wins."

> "Get me the code for the highest-priority one and apply it to this repo."

> "I deployed. Mark it as applied in Lotus."

On that last step Lotus fetches your live site and looks for the schema. If it
is there, the fix turns green with a verification date. If it is not — because
the deploy has not landed, or the snippet went in wrong — Lotus refuses to mark
it. No false positives.

**Lotus generates and verifies. You deploy.** The server never writes to your
site or your repository.

## Support

Questions or access: https://clicon.app
