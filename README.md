<div align="center">

<img src="assets/heymetra-mcp.png" alt="HeyMetra — a remote MCP server for marketing and revenue data" width="100%">

# HeyMetra

**Manage your business with your AI assistant.**

A remote MCP server for the accounts your business already runs on — ads, analytics, store, CRM, mobile subscriptions. Connect them once, then ask about all of them together from Claude, ChatGPT, Cursor or whichever assistant you already pay for.

[![MCP Registry](https://img.shields.io/badge/MCP_Registry-com.heymetra%2Fheymetra-1f6feb)](https://registry.modelcontextprotocol.io/v0/servers/com.heymetra%2Fheymetra/versions)
[![Transport](https://img.shields.io/badge/transport-Streamable_HTTP-444)](https://modelcontextprotocol.io/specification/draft/basic/transports)
[![Auth](https://img.shields.io/badge/auth-OAuth_2.1-444)](https://heymetra.com/security/)
[![Website](https://img.shields.io/badge/heymetra.com-1f6feb)](https://heymetra.com/)

```
https://mcp.heymetra.com/mcp
```

</div>

---

## What this is

Most MCP servers cover one vendor. Connect the Google Ads one and it will tell you what you spent; it cannot tell you what you earned, because the money arrived in Stripe, or WooCommerce, or Adapty, and that is a different server with a different login.

HeyMetra is one server over all of them. Spend, traffic, store and revenue answer the same question at the same time, which is the only way a question like *"did that campaign pay for itself?"* has an answer at all.

Two consequences worth knowing before you read further:

- **The model is yours.** HeyMetra has no chat interface and buys no inference. Your assistant calls our tools using the subscription you already have, which is why a seat costs what it costs.
- **The tool list is yours too.** A workspace sees tools for the accounts it has connected and nothing else. There is no fixed catalogue of 62 tools sitting in your assistant's context — only the ones that can actually answer.

Of those tools, **35 read and 27 write**. Every one of the 27 stops and asks a person first. See [Writes stop for a person](#writes-stop-for-a-person).

## Who it's for

- **Founders and small teams** with no analyst. "How did we do last week?" should not require a dashboard tour.
- **Marketing and growth** with budget split across platforms. "Where are we burning money?" is a cross-account question by definition.
- **Agencies** where every client wants their own report, yesterday.
- **Mobile app owners** — ad spend lives in one place and subscription revenue in another. This is the gap HeyMetra was built around: AppsFlyer, Adapty, RevenueCat and App Store Connect sit beside Google Ads, on one connection.

Not for you if you want a chat product or a dashboard. HeyMetra is neither. It is a paid product with no free plan, and every plan starts with a 7-day free trial; the card is taken at sign-up and the first charge comes on day 8. The current plans are on the [pricing page](https://heymetra.com/pricing/).

## Connect it

The address is the same everywhere:

```
https://mcp.heymetra.com/mcp
```

Your assistant will send you through a sign-in once, and HeyMetra never sees a password for any connected account — authorisation happens at the provider.

### One command

```bash
npx add-mcp https://mcp.heymetra.com/mcp
```

[`add-mcp`](https://www.npmjs.com/package/add-mcp) is a third-party installer that writes the configuration for Claude Code, Codex, Cursor, Antigravity, VS Code and seventeen other agents — it infers the name from the address, so the server lands as `heymetra`. For the assistants that keep the setting in their own UI rather than a file, use the steps below.

### Or by hand

<details>
<summary><b>Claude</b> — Settings → Customize → Connectors → Add custom connector</summary>

Anthropic calls a connected MCP server a *connector*. Add it from Claude's own settings. On Team and Enterprise plans, an owner adds the connector for the whole organization under Organization settings → Connectors, and each member then connects to it.

Full walkthrough: [heymetra.com/mcp/claude/](https://heymetra.com/mcp/claude/)
</details>

<details>
<summary><b>ChatGPT</b> — chatgpt.com/plugins → + → Add custom MCP server</summary>

Add it on chatgpt.com in a browser, because the mobile app cannot add one. Every ChatGPT plan can, Free included. In a Business, Enterprise or Edu workspace, an admin or owner decides whether members can add custom MCP servers. ChatGPT needs the full address, including `/mcp` at the end.

Full walkthrough: [heymetra.com/mcp/chatgpt/](https://heymetra.com/mcp/chatgpt/)
</details>

<details>
<summary><b>Grok</b> — grok.com/connectors → New Connector → Custom</summary>

On Grok Business and Enterprise, a team admin has to provision the connector in xAI's cloud console before members can use it.

Full walkthrough: [heymetra.com/mcp/grok/](https://heymetra.com/mcp/grok/)
</details>

<details>
<summary><b>Perplexity</b> — Plugins → Add custom → Connector → Add MCP connector → Remote</summary>

Available on Pro, Max and Enterprise. In an organization, members can add a custom connector only after an admin turns on *Allow members to add custom connectors*, which is off by default.

Full walkthrough: [heymetra.com/mcp/perplexity/](https://heymetra.com/mcp/perplexity/)
</details>

<details>
<summary><b>Claude Code</b> — one command</summary>

```bash
claude mcp add --transport http heymetra https://mcp.heymetra.com/mcp
```

Or a `.mcp.json` in the project root. `/mcp` inside a session shows what connected.

Full walkthrough: [heymetra.com/mcp/claude-code/](https://heymetra.com/mcp/claude-code/)
</details>

<details>
<summary><b>Codex</b> — <code>~/.codex/config.toml</code></summary>

```toml
[mcp_servers.heymetra]
url = "https://mcp.heymetra.com/mcp"
```

Then `codex mcp login heymetra`.

Full walkthrough: [heymetra.com/mcp/codex/](https://heymetra.com/mcp/codex/)
</details>

<details>
<summary><b>Cursor</b> — <code>~/.cursor/mcp.json</code>, or <code>.cursor/mcp.json</code> in a project</summary>

```json
{
  "mcpServers": {
    "heymetra": { "url": "https://mcp.heymetra.com/mcp" }
  }
}
```

Leave the static OAuth fields empty — they exist for servers that cannot register themselves, and this one can.

Full walkthrough: [heymetra.com/mcp/cursor/](https://heymetra.com/mcp/cursor/)
</details>

<details>
<summary><b>Antigravity</b> — <code>~/.gemini/config/mcp_config.json</code>, or <code>.agents/mcp_config.json</code> in a project</summary>

```json
{
  "mcpServers": {
    "heymetra": { "serverUrl": "https://mcp.heymetra.com/mcp" }
  }
}
```

Antigravity's documented key for the address is `serverUrl`, though it also accepts `url`.

Full walkthrough: [heymetra.com/mcp/antigravity/](https://heymetra.com/mcp/antigravity/)
</details>

New to any of this? [What an MCP server is, and how it differs from an API](https://heymetra.com/mcp/).

## What you can ask

Questions that need two accounts at once are the point:

> Which channel actually paid for itself last month — spend against subscription revenue, not installs?

> Search Console clicks fell last week. Which queries lost them, and did orders move with them?

> Compare cost per trial across platforms for the last 28 days.

> Which leads in the CRM came from the campaign that started on Monday?

> Cut the budget on the campaign with the worst cost per purchase by 20%.

The last one does not run. It comes back as a change for you to approve.

## Writes stop for a person

A tool that would change an account returns the proposed change instead of executing it. You approve it, and the payload you approved is the payload that runs — the model does not get to reconsider after the fact.

The bounds are values in code, not instructions in a prompt:

| Bound | Value |
|---|---|
| Budget change in one action | ±50% |
| Absolute budget ceiling | 10,000 in the account's currency |
| Campaigns touched by one action | 5 |
| Account-changing writes per rolling day | 20 |
| Messages sent per rolling day | 20, counted separately |
| Approval expires after | 30 minutes |

The last one matters more than it looks: an approval card left open overnight is a decision made against yesterday's numbers.

What HeyMetra does not claim: no SOC 2, no ISO certification, no penetration test. [The security page](https://heymetra.com/security/) says so in those words, along with what is actually in place.

## Connectors

Every connector in the catalogue, by category. The ones marked *coming soon* cannot be connected yet; each connector's own page carries its current state.

Every connector you can connect today, apart from Email, also has a repository of its own, marked <sup>repo</sup> below: setup steps, what you can ask it, what it may and may not touch, and what to do when the provider refuses. Each one is generated from the same catalogue and refreshed daily.

**Ads** — [Google Ads](https://heymetra.com/connectors/google-ads/) <sup>[repo](https://github.com/zeisoft/google-ads-mcp)</sup> · [Meta](https://heymetra.com/connectors/meta-ads/) (coming soon)

**Analytics** — [Google Analytics 4](https://heymetra.com/connectors/google-analytics-4/) <sup>[repo](https://github.com/zeisoft/google-analytics-4-mcp)</sup> · [Google Search Console](https://heymetra.com/connectors/google-search-console/) <sup>[repo](https://github.com/zeisoft/google-search-console-mcp)</sup> · [PostHog](https://heymetra.com/connectors/posthog/) <sup>[repo](https://github.com/zeisoft/posthog-mcp)</sup>

**Ecommerce** — [Shopify](https://heymetra.com/connectors/shopify/) (coming soon) · [Trendyol](https://heymetra.com/connectors/trendyol/) <sup>[repo](https://github.com/zeisoft/trendyol-mcp)</sup> · [Hepsiburada](https://heymetra.com/connectors/hepsiburada/) (coming soon) · [n11](https://heymetra.com/connectors/n11/) <sup>[repo](https://github.com/zeisoft/n11-mcp)</sup> · [Çiçeksepeti](https://heymetra.com/connectors/ciceksepeti/) <sup>[repo](https://github.com/zeisoft/ciceksepeti-mcp)</sup> · [Amazon Seller Central](https://heymetra.com/connectors/amazon-seller-central/) <sup>[repo](https://github.com/zeisoft/amazon-seller-central-mcp)</sup> · [ikas](https://heymetra.com/connectors/ikas/) <sup>[repo](https://github.com/zeisoft/ikas-mcp)</sup> · [WooCommerce](https://heymetra.com/connectors/woocommerce/) <sup>[repo](https://github.com/zeisoft/woocommerce-mcp)</sup>

**Revenue & CRM** — [Stripe](https://heymetra.com/connectors/stripe/) <sup>[repo](https://github.com/zeisoft/stripe-mcp)</sup> · [HubSpot](https://heymetra.com/connectors/hubspot/) (coming soon) · [Zoho CRM](https://heymetra.com/connectors/zoho-crm/) <sup>[repo](https://github.com/zeisoft/zoho-crm-mcp)</sup> · [Zoho SalesIQ](https://heymetra.com/connectors/zoho-salesiq/) <sup>[repo](https://github.com/zeisoft/zoho-salesiq-mcp)</sup> · [Zoho Marketing Automation](https://heymetra.com/connectors/zoho-marketing-automation/) <sup>[repo](https://github.com/zeisoft/zoho-marketing-automation-mcp)</sup>

**Mobile** — [AppsFlyer](https://heymetra.com/connectors/appsflyer/) <sup>[repo](https://github.com/zeisoft/appsflyer-mcp)</sup> · [RevenueCat](https://heymetra.com/connectors/revenuecat/) <sup>[repo](https://github.com/zeisoft/revenuecat-mcp)</sup> · [Adapty](https://heymetra.com/connectors/adapty/) <sup>[repo](https://github.com/zeisoft/adapty-mcp)</sup> · [App Store Connect](https://heymetra.com/connectors/app-store-connect/) <sup>[repo](https://github.com/zeisoft/app-store-connect-mcp)</sup>

**Work** — [Jira](https://heymetra.com/connectors/jira/) <sup>[repo](https://github.com/zeisoft/jira-mcp)</sup>

**Channels** (delivery, not data) — [Slack](https://heymetra.com/connectors/slack/) <sup>[repo](https://github.com/zeisoft/slack-mcp)</sup> · [Telegram](https://heymetra.com/connectors/telegram/) <sup>[repo](https://github.com/zeisoft/telegram-mcp)</sup> · [Email](https://heymetra.com/connectors/email/)

Missing one you need? Ask for it inside the assistant — HeyMetra can pass a request or a correction to us in your own words — or [write to us](https://heymetra.com/contact/).

## How it works

- **Transport** — Streamable HTTP. SSE is not offered.
- **Authorisation** — OAuth 2.1. The server publishes an RFC 9728 protected-resource document, so a client discovers where to sign in rather than being told. Nothing is pasted into a config file except the address above.
- **Per-workspace surface** — tools are resolved per request from the accounts that workspace has connected. A revoked token stops working on the next call, not whenever the transport session happens to end.
- **Change notifications** — connect an account mid-conversation and the tool list updates in place.

Published in the [Official MCP Registry](https://registry.modelcontextprotocol.io/v0/servers/com.heymetra%2Fheymetra/versions) as `com.heymetra/heymetra`.

## Links

| | |
|---|---|
| Product | [heymetra.com](https://heymetra.com/) |
| Pricing | [heymetra.com/pricing/](https://heymetra.com/pricing/) |
| Connectors | [heymetra.com/connectors/](https://heymetra.com/connectors/) |
| Setup per assistant | [heymetra.com/mcp/](https://heymetra.com/mcp/) |
| Security and limits | [heymetra.com/security/](https://heymetra.com/security/) |
| Alternatives | [heymetra.com/alternatives/](https://heymetra.com/alternatives/) |
| Writing | [heymetra.com/blog/](https://heymetra.com/blog/) |
| About | [heymetra.com/about/](https://heymetra.com/about/) |
| Contact | [hello@heymetra.com](mailto:hello@heymetra.com) |

This repository is documentation. HeyMetra itself is a hosted service and its source is not public; there is nothing to install or run from here. Corrections and questions are welcome as [issues](https://github.com/zeisoft/heymetra-mcp/issues) — a wrong setup line is worth reporting, because the reader follows it, fails, and concludes the product is broken.

<div align="center">
<sub>Built by <a href="https://zeisoft.com">Zeisoft Yazılım Limited Şirketi</a> — İzmir, Türkiye</sub>
</div>
