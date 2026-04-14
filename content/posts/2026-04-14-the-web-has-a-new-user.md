---
title: "The Web Has a New User. It Isn't Human."
date: 2026-04-14T08:00:00+01:00
images: ["/images/posts/the-web-has-a-new-user.png"]
tags: ["AI", "Agents", "Commerce", "Protocols", "Open Source", "OCP"]
categories: ["AI", "Agents", "Commerce", "Open Source"]
draft: false
---

There's a quiet shift happening on the web, and most e-commerce teams haven't noticed yet.

Users aren't just opening browsers anymore. They're asking ChatGPT, Claude, Gemini, and a growing zoo of autonomous shopping agents to *go do the shopping for them*. The humans still approve the purchase — but the discovery, the comparison, the "what's the best one under $150" part? That's being delegated.

And here's the uncomfortable question every merchant should be asking in 2026:

**When an agent goes looking for your product, can it actually see your store?**

If you're on Shopify, probably yes. Shopify has been quietly wiring up agent-readable endpoints for months. If you're on WooCommerce, Magento, BigCommerce, a headless Next.js build, or — honestly — *anything else*, the answer is almost certainly no.

That's the problem I've been poking at for a while. Today I'm putting out one possible approach — and opening it up for others to poke at.

It's called [**OCP — the Open Commerce Protocol**](https://opencommerceprotocol.org).

---

## The Same Movie, Different Decade

I've written before about how every tech revolution follows the same playbook: hype, projection, panic, investment, reality check, and then — slowly — the real value emerges in places nobody predicted.

The agentic commerce wave is no different. The trillion-dollar headlines are already here. The "AI will run your store" decks are already being shopped around boardrooms. Meanwhile, the actual infrastructure problem is embarrassingly mundane:

**Agents can't read most of the web.**

Not because the tech is hard. Because there's no standard way for a website to say *"here's my catalog, here's how to search it, here's how to add something to a cart."*

We've seen this exact movie before. Before sitemaps.xml, search engines crawled blind. Before RSS, you had to visit every site manually. Before Schema.org, Google had to guess what a product page actually was. Every time, the fix was the same: a small, boring, open protocol that everyone could adopt in an afternoon.

Agentic commerce needs that moment. Right now.

---

## The Shopify Problem

Let me be blunt about why this matters.

When an agent searches "best hiking boots under $150," it can only recommend stores it can *read*. Shopify stores surface. The rest of the web doesn't. That's not a fairness problem — that's a distribution collapse.

The long tail of commerce — the independent brands, the niche shops, the DTC founders, the local stores running on WooCommerce plugins older than some of their customers — is staring down a future where they're structurally invisible to the next dominant shopping channel.

You can't solve that by telling every small merchant to rewrite their backend against five competing agent protocols. MCP, UCP, ACP, A2A, WebMCP — every framework has its own opinion about how an agent should talk to a store. If you're a founder running a single-person brand, you are not going to implement five of them. You're going to implement zero.

That's where the protocol wars lose their customers.

---

## Bridge, Don't Replace

Here's the design principle OCP is built on — and the part I think matters most:

> **One OCP setup can produce all the other protocols automatically.**

Not "OCP instead of MCP." Not "OCP vs ACP." OCP sits underneath them and translates.

```
Your Store
    │
    ▼
┌─────────────────────────────────────────────────────────┐
│                  OCP Core Setup                          │
│   .well-known/ocp.json  │  ocp/products.jsonl  │ ocp.md │
└──────┬──────────┬────────┬───────────┬──────────┬───────┘
       │          │        │           │          │
       ▼          ▼        ▼           ▼          ▼
   MCP Server  UCP      ACP        A2A Card   WebMCP
   (Claude,    Manifest  Endpoints  (Google    (Chrome
    Cursor)    (Google   (OpenAI    agent-to-  browser
               checkout) agents)    agent)     runtime)
```

You publish **two static files**. A bridge command turns them into MCP servers, UCP manifests, ACP endpoints, A2A agent cards, and WebMCP tool registrations. When the next protocol ships — and there will *always* be a next protocol — you add one bridge, not one integration.

I think this is the kind of design that has a shot at surviving the next five years. Because the next five years are going to be *loud*.

---

## The Five-Minute Setup

The whole point of OCP is that the minimum viable implementation is stupidly small. That's not a design compromise — it's a deliberate bet.

Two files on any web server:

**`/.well-known/ocp.json`** — a manifest describing your store.

**`/ocp/products.jsonl`** — one product per line, plain text.

That's it. No backend. No API keys. No framework lock-in. No ongoing maintenance beyond regenerating the feed when your catalog changes.

If your site already has Schema.org markup on product pages, you don't even have to write those files yourself:

```bash
npx @opencommerceprotocol/cli crawl https://mystore.com
```

The crawler walks your site, reads the structured data you already published for Google, and generates the OCP files for you. I've tested it on stores ranging from a single-developer Astro build to a 40,000-SKU WooCommerce instance. So far, it's held up.

If you want more — interactive tools for agents, a conversational chat widget for your human shoppers — you can progressively enhance. Drop in one script tag and your handlers are exposed to WebMCP (Chrome 146+), fall back to `window.__ocp` for older agent frameworks, and simultaneously power an embedded chat assistant for human visitors.

That last part is the thing that quietly sold me on shipping this. **The chat widget is the merchant's first tangible ROI.** Agents are still warming up. Human shoppers are here *right now*, and they'll happily talk to a shopping assistant that actually knows your catalog. You get the human-facing win immediately, and the agent-facing win arrives on the same rails a few weeks later as crawlers index your manifest.

---

## What "Measured" Looks Like Here

I've been pretty loud on this blog about my skepticism toward "slap AI on it" strategies. I stand by that. OCP is not a magic AI feature. It's an attempt at boring, foundational infrastructure.

That's the point.

The revolutionary applications — AI that runs your entire store, negotiates with suppliers, reprices in real time — those are years away and will require regulation, trust frameworks, and social consent that doesn't exist yet. Meanwhile, some practical applications seem available today for anyone willing to ship something small:

- An independent bookstore getting recommended by an AI reading assistant when someone asks for "a good book on systems thinking."
- A DTC coffee brand getting surfaced when an agent is helping someone restock their pantry.
- A B2B parts supplier being discoverable to a procurement agent that would otherwise default to the three giants everyone already knows.

None of this is trillion-dollar-projection stuff. It's just merchants not being invisible. And right now, *that's the whole game*.

---

## Why It Has To Be Open

There's a version of this project that ships as a SaaS — "we'll make your store agent-readable for $49/month" — and the unit economics would probably work. That's exactly why I think it would be the wrong move.

**Protocols tend to die when they're owned.** The web works because nobody owns HTTP. Schema.org works because nobody owns Schema.org. A commerce layer for agents only becomes a default if no single company sits on top of it extracting rent — otherwise platforms route around it, and we end up back where we started with five competing stacks.

**The long tail can't afford a middleman.** The whole point of this is that small merchants are structurally at risk of being invisible to agents. Putting a paywall on the fix reproduces the exact problem the protocol is trying to solve.

**Open is likely the only way it outlives any one maintainer.** The useful version of OCP is the one other people feel ownership of — fork it, argue with it, ship adapters for platforms I've never heard of, eventually make it better than it starts out. That requires Apache 2.0 and a public spec, not a licensing agreement.

So it's all open. Spec, CLI, runtime, bridges, adapters for Shopify, WooCommerce, and a generic Schema.org flow. If you want to contribute an adapter for your platform, the repo is waiting.

---

## See It Running

Abstract protocols are easy to hand-wave about, so the repo ships a working example for every flavor of store you're likely to have. All of them are open source under `examples/` and most are deployed live:

- **[static-site.opencommerceprotocol.org](https://static-site.opencommerceprotocol.org)** — Acme Widgets. The absolute minimum: three static files, no build step, no server. If you only look at one example, look at this one.
- **[bookstore.opencommerceprotocol.org](https://bookstore.opencommerceprotocol.org)** — PageTurner Books. A static HTML bookstore showing Discovery + Interact layers with zero backend.
- **[express-store.opencommerceprotocol.org](https://express-store.opencommerceprotocol.org)** — a full store on Hono + Cloudflare Workers, including cart, checkout, and live bridges to ACP, UCP, and A2A.
- **[nextjs-store.opencommerceprotocol.org](https://nextjs-store.opencommerceprotocol.org)** — Next.js 15 / React 19 storefront with server-generated OCP manifests and the client runtime wired in.
- **[pet-store.opencommerceprotocol.org](https://pet-store.opencommerceprotocol.org)** — Pawsome Pets. A standalone Node.js/Express implementation with no workspace dependencies — `npm install && npm start` and you have an agent-readable store.
- **[store.opencommerceprotocol.org](https://store.opencommerceprotocol.org)** — UrbanThread. A clothing store that walks through the full OCP shopping flow with real UI components.
- **[browser-llm.opencommerceprotocol.org](https://browser-llm.opencommerceprotocol.org)** — an AI shopping assistant that runs entirely in the browser, no cloud API tokens. Useful for seeing what a client-side agent looks like against an OCP catalog.
- **[analytics.opencommerceprotocol.org](https://analytics-dashboard.opencommerceprotocol.org)** — a ready-to-run dashboard for visualising which agents are actually showing up at your store.
- **[event-ticketing.opencommerceprotocol.org](https://event-ticketing.opencommerceprotocol.org)** — StageHop. A Next.js 15 event-ticketing store running on Cloudflare Pages, with seat tiers, 8-minute holds, and mandatory human approval on non-refundable checkout.
- **[gift-finder.opencommerceprotocol.org](https://gift-finder.opencommerceprotocol.org)** — Giftly. A browser-only gift finder that fans `search_products` out across every OCP store in parallel and shows a unified ranked grid — zero backend, one HTML file served from Cloudflare Workers.

Clone any of them, point the CLI at them, or just read the manifests. They're deliberately small so you can skim the whole thing in a single sitting.

---

## What's Actually In The Box

For anyone who wants the technical rundown:

- **`@opencommerceprotocol/spec`** — the protocol itself. Versioned, JSON-schema-validated, deliberately minimal.
- **`@opencommerceprotocol/cli`** — crawl, init, validate, bridge. The one command a merchant ever has to learn.
- **`@opencommerceprotocol/runtime`** — ~8KB gzipped browser runtime. Registers tools with WebMCP, falls back for older frameworks, powers the human chat widget.
- **`@opencommerceprotocol/validator`** — makes sure your manifest and feed are actually valid before agents see them.
- **`@opencommerceprotocol/bridge-mcp`**, **`bridge-acp`**, **`bridge-ucp`**, **`bridge-a2a`** — the translation layers to every other agent protocol currently in flight.
- **Adapters** for Shopify, WooCommerce, and a generic Schema.org flow for everyone else.
- **`@opencommerceprotocol/registry`** — optional discovery layer for agents that want to browse participating stores.
- **`@opencommerceprotocol/analytics`** — because if you're going to ship this in production, you need to know which agents are actually showing up.

All of it lives at **[opencommerceprotocol.org](https://opencommerceprotocol.org)**, with the full spec, documentation, examples, and quickstart guides. The source is on GitHub under Apache 2.0.

---

## The Ask

If you run an e-commerce site — try it. `npx @opencommerceprotocol/cli crawl` against your own domain. It takes less time than reading this post. Tell me what breaks.

If you build agent frameworks — look at the bridges. Tell me what I got wrong about your protocol's semantics. I'd rather fix that now than after stores are depending on it.

If you work on a platform (Shopify, Wix, Squarespace, WooCommerce, Magento, anything) — let's talk about first-class support. This gets radically better the moment platforms ship it in the box.

And if you just want to argue about whether any of this is the right design — good. That's what open protocols are for. Open an issue. File a PR. Poke holes in the spec. That's how this gets better.

---

## Final Thought

The agentic web is not going to be turnkey. No revolution ever is.

But the companies and independent merchants that make it through this transition are going to be the ones who did the boring, infrastructural work early. Not the ones who wrote "AI-powered" on a slide. The ones who made their catalog *legible* to machines while everyone else was still debating which model to use.

Two files. Five minutes. One proposal.

The web has a new user. It isn't human. Time to let it in.

---

### 📚 Further Reading

- [Open Commerce Protocol — opencommerceprotocol.org](https://opencommerceprotocol.org)
- [The spec on GitHub](https://github.com/jjoaquim/agentic-commerce-protocol)
- [WebMCP explainer](https://github.com/webmachinelearning/webmcp)
- [Model Context Protocol](https://modelcontextprotocol.io)
