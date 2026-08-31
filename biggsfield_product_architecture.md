# How Bigsfield.ai Built Its Product
### A Full Architecture Teardown — Platform Strategy, Product Map & What It All Means

> **What this is:** A deep-dive into how Bigsfield.ai has structured its entire product — not just the homepage, but every tab, every named feature, every model, every integration. Read this to understand how an AI company builds a *platform* rather than a *tool*, and what that distinction actually means in practice.
>
> **Companion to:** [How Bigsfield.ai Structures Its Homepage](./homepage_case_study.md) — which covers page design, conversion strategy, and UX. This document covers the product itself.

---

## What is Bigsfield, Really?

On the surface: an AI video and image generation tool.

In reality: a **creative operating system** — a platform that aggregates 30+ AI models from OpenAI, Google, ByteDance, xAI, Alibaba, Kuaishou, and others, wraps them in purpose-built workflows, and delivers them through a unified interface, an AI agent, a plugin ecosystem, and an MCP server that connects to any third-party AI tool.

The distinction matters. A tool does one thing well. A platform becomes the place where many things happen. Bigsfield has made a deliberate architectural bet on becoming the latter.

At the time of this study:
- 8 major product tabs (Image, Video, Audio, Edit, Marketing Studio, Viral Presets, Supercomputer, MCP & CLI)
- 29+ named features across Image and Video alone
- 27 named AI models from 9+ external providers
- 60+ named Viral Presets
- 9+ named Skills inside the Supercomputer agent
- Plugins for After Effects, Premiere Pro, and Photoshop
- An MCP server compatible with Claude, ChatGPT, Cursor, and 6+ other AI agents

---

## The Core Architecture: 5 Layers

Before diving into each product, it helps to understand how the product is *structured*. Bigsfield isn't a flat list of features — it's a layered system where each layer serves a different user need and unlocks a different type of value.

| Layer | Products | What it solves |
|---|---|---|
| **1. Creation** | Image, Video, Audio, Edit | "I want to generate something" |
| **2. Studio** | Cinema Studio, Marketing Studio, Lipsync, Photodump, UGC Factory, Canvas | "I want a purpose-built tool for my workflow" |
| **3. Preset** | Viral Presets, Marketing Studio Templates | "I want great output with zero skill required" |
| **4. Platform** | Supercomputer | "I want an agent to run my creative workflows for me" |
| **5. Integration** | MCP & CLI, Adobe Plugins, Chrome Extension | "I want Bigsfield inside the tools I already use" |

Each layer serves a progressively more sophisticated user — from casual creator at Layer 3 to developer/power user at Layer 5. But critically, all layers share the same underlying model infrastructure. The models power everything.

---

## Layer 1 — Creation

### IMAGE

The Image tab gives you access to 12 generation and editing features, powered by 12 underlying models.

**Features — what you do:**

| Feature | What it does |
|---|---|
| GPT Image 4o | Text-to-image via OpenAI's model, with accurate text rendering inside images |
| Recraft V4 Styles | Lock a visual style and apply it consistently across all generations |
| Flux Pro 1.1 Ultra | Photorealistic, detail-rich image generation |
| Ideogram 3.0 | Images with highly accurate typography and text rendered inside them |
| Photodump | Maintain the same character/face consistently across multiple scenes and settings |
| Soul 2.0 | Editorial, fashion, high-end photography aesthetic |
| Image Upscale | Enhance and sharpen images up to 4K resolution |
| Background Removal | Clean cutouts from any image |
| Image Expand | Outpainting — extend the canvas beyond the original image boundaries |
| Style Transfer | Apply the visual look of one image to another |
| ControlNet | Control pose, depth, and structure during generation |
| Inpaint / Outpaint | Edit within an image or expand around it |

**Models — what powers it:**

Flux Pro 1.1 Ultra (Black Forest Labs) · Flux Dev · Flux Schnell · Recraft V3 · Recraft V4 · DALL-E 3 (OpenAI) · GPT Image 4o (OpenAI) · Ideogram 2.0 · Ideogram 3.0 · Stable Diffusion XL · Stable Diffusion 3.5 · Black Forest Labs

> **Analyst note:** The separation between Features and Models is a deliberate UX decision. Users think in terms of tasks ("I want to remove this background"), not models ("I want to use SDXL with a segmentation mask"). Bigsfield abstracts the model layer away — you pick the task, they pick the best model to run it. This is identical to how AWS abstracts hardware from cloud compute.

---

### VIDEO

The Video tab is the deepest product on the platform — 17 features and 15 models, including integrations with every major video AI lab in the world.

**Features — what you do:**

| Feature | What it does |
|---|---|
| Seedance 2.5 | Bigsfield's flagship video model — most advanced quality on the platform |
| Text to Video | Generate video from a written prompt |
| Image to Video | Animate any still image into motion |
| Video to Video | Restyle or transform existing footage |
| Lipsync Studio | Sync any speech or audio track to any face in a video |
| Cinema Studio 4.0 | Director-level cinematic video with full camera movement controls |
| Soul Cinema | Dramatic, moody, narrative-depth film generation |
| Video Upscale (Flux 3.0) | Enhance video quality and resolution up to 4K |
| Character Consistency | Keep the same character identity across multiple video scenes |
| Camera Controls | Programmatic pan, tilt, zoom, dolly, orbit camera instructions |
| Motion Brush | Selectively add motion to specific regions of a still image |
| Slow Motion | Generate or convert footage to high-FPS slow motion |
| Loop Video | Create seamlessly looping video clips |
| Extend Video | Continue an existing video clip forward automatically |
| Subtitle Generator | Auto-generate and burn captions into video |
| Ad Multiplier | Take one ad creative and generate 10+ variations automatically |
| UGC Factory | Generate user-generated-content style video at scale |

**Models — what powers it:**

Seedance 2.5 · Seedance 2.0 · Kling 2.1 (Kuaishou) · Kling 1.6 (Kuaishou) · Veo 3 (Google DeepMind) · Veo 2 (Google DeepMind) · Sora 2 (OpenAI) · Wan 2.1 (Alibaba) · MiniMax Hailuo · ByteDance (internal) · Kuaishou (internal) · Google DeepMind · xAI · MiniMax · Topaz (upscaling engine)

> **Strategist note:** Having Kling, Veo, Sora, Wan, and Hailuo under one roof means Bigsfield doesn't need to "win" the model race. When a new model launches from any of these labs, Bigsfield integrates it and immediately offers it to their entire user base. The competitive moat isn't model quality — it's speed of integration and breadth of access.

---

### AUDIO

The Audio tab is the least publicly documented. The page is fully JavaScript-rendered and returns no crawlable content.

What is confirmed:
- Listed as a live product in the navigation
- Referenced inside the Supercomputer as part of full creative pipelines
- Powers Lipsync Studio (audio-to-video sync)

What is likely based on product category and competitor parity: text-to-speech, voice cloning, music generation, audio-to-video sync.

> **Gap worth watching:** Audio is either the newest addition (still being built out) or the most under-marketed product in the lineup. Either way, it's a meaningful gap in the public product story.

---

### EDIT (Layers)

**Headline:** *"Turn Flat Images Into Editable Layers"*

This product solves a fundamental limitation of AI image generation: you get the output, but you can't edit individual elements of it without regenerating the whole thing.

**How it works:**

1. Input any image — AI-generated or uploaded
2. AI auto-segments the image into objects, subjects, and backgrounds
3. Each element becomes an independent layer
4. Edit, move, replace, recolor, or scale each layer independently
5. Rebuild the composite and export the final image

**Why it matters:**

This is the bridge between AI generation and professional design production. A brand designer who generates an AI product image can now move the product, change the background color, replace text, and adjust the shadow — without going back to the generation step. That eliminates iteration cycles and makes AI output production-ready.

> **Differentiator:** Most AI image tools treat output as final. Layers treats AI output as a starting point. This is a fundamentally different philosophy about where AI fits in the creative process.

---

## Layer 2 — Studios

Studios are purpose-built tools for specific workflows. Where the Image/Video tabs give you raw generative power, Studios wrap that power in an opinionated workflow designed for a specific use case.

| Studio | Built for | Core function |
|---|---|---|
| **Cinema Studio 4.0** | Filmmakers, directors | Cinematic video with camera controls, dramatic lighting, narrative depth |
| **Marketing Studio** | Brands, ad teams, e-commerce | Product → ready-to-post ad content in 6 formats |
| **Lipsync Studio** | Content creators, localization teams | Sync speech/audio to any face in any video |
| **Photodump Studio** | Brand shoots, social content | Same character/face across multiple scenes consistently |
| **Soul Cinema** | Editorial video, cinematic storytelling | High-drama, film-quality video generation |
| **UGC Factory** | DTC brands, performance marketers | Authentic creator-style video at scale |
| **Canvas** | Teams, visual thinkers | Infinite workspace for moodboarding and chaining AI pipelines |

> **Product thinking note:** Studios are Bigsfield's answer to "who is this for?" Each Studio has a clear user type, a clear outcome, and a clear workflow. The raw Image/Video tabs appeal to power users who want control. Studios appeal to professionals who want speed.

---

## Layer 3 — Presets & Templates

### VIRAL PRESETS (60+ named presets)

Viral Presets are one-click cinematic transformations. No prompting, no settings — pick a preset, apply it, done.

**What a preset does:** Applies camera motion, color grading, visual style, and effects all at once in a single click.

**Simple analogy:** Instagram filters, but for video, and each one is a full cinematic concept with a name.

**Full preset library by style category:**

**Cinematic / Drama**
Agamemnon · Fallen Angel · Fairytale Castle · Mighty Fighter · Knight's Diary · Noir · Cold Vision · Blue Depth · Canvas

**Nature / Animals**
Earth Zoom · Dolphin Ride · Penguin Ride · Puffin Ride · Ocean · Pigeons · Tracking

**Abstract / Surreal**
Ink Riot · LSD · Acid · Fragments · Multiverse · Broken Mirror · Particles · Random Glow · Toxic · Lava · Marble · Ultraviolet · Cyclope · Palette

**Action / Motion**
Bullet Time · Moonwalk · Superstar · Race Track · Orbit 360 · Orbital Presence · Skatedog

**Art Style / Illustration**
Comic · Flash Comic · Sketch · Monet Muse · Akrill · Hand Paint · Paper · Origami · Two Color · Vintage · Modern · 3D Render · Pearl Earring · Magazine

**Pop Culture / Trend**
2000's Paparazzi · Windows · Overexposed · Selfie Twin · Sticker Peel · Action Figure · Bubbles · Casual Monster Slayer · Cannabis · Lost in a Book

> **Virality mechanics:** Named presets create cultural moments. When "Moonwalk" or "Dolphin Ride" trends on TikTok, every clip made with it is an organic Bigsfield advertisement. The preset name becomes a content category. This is how you build brand awareness without a media budget — the product names itself in the content.

---

### MARKETING STUDIO TEMPLATES

Six template categories, each mapped to a specific content format:

| Category | Output | Use case |
|---|---|---|
| Product Shot | Studio-quality still of any product | E-commerce listings, social media |
| Motion | Animated product video | Instagram/TikTok ads, brand content |
| UGC | Casual, creator-style video | Performance marketing, TikTok |
| Ads | Ready-to-run ad creatives | Meta Ads, Google Display, TikTok Ads |
| Posters | Print/digital poster layouts | Events, promotions, brand campaigns |
| Marketplace | E-commerce listing images | Amazon, Shopify, product pages |

---

## Layer 4 — Platform (Supercomputer)

**Headline:** *"Your AI creative agent"*

The Supercomputer is the most strategically important product in the lineup. It transforms Bigsfield from a collection of tools into a platform with an agent at its center.

**What it is:** A conversational AI agent — like Claude or ChatGPT — built specifically for creative production. It doesn't just generate content; it executes multi-step creative pipelines, remembers your brand, and chains tools together automatically.

### Interface Components

| Component | Function |
|---|---|
| New Chat | Start a new agent session |
| Search | Search across all past sessions and outputs |
| Projects | Organize work into named projects |
| Products | Quick access to all Bigsfield tools from within the agent |
| Faceless Channel | Dedicated workflow for faceless content creators |
| Apps | Installed agent-apps |
| Games | Game-type creative experiences |
| Customize | Adjust agent persona and default style preferences |
| Skills | Install and manage packaged workflow Skills |
| Connectors | Connect external tools and platforms |
| Memory | Persistent memory of your brand, style, and preferences |
| Chats | Full history of all sessions |

---

### The Skills System

Skills are the core of the Supercomputer. A Skill is a pre-packaged, multi-step workflow that runs inside the agent. Instead of manually routing through multiple tools, you install a Skill and invoke it with a single prompt.

**Marketing Skills:**

| Skill | What it automates |
|---|---|
| Ad Multiplier | One ad creative → 10+ variations (different hooks, formats, lengths, audiences) |
| Editorial Motion Graphics | News/editorial-style motion graphics video, end to end |
| Brandkit | Full brand asset kit generated from a brief |

**Faceless / UGC Skills:**

| Skill | What it automates |
|---|---|
| Bigsfield Faceless Video | Complete faceless YouTube/TikTok channel content — no human on camera |
| Cartoon Faceless Video | Animated faceless content with characters instead of real people |
| SaaS UGC | Product demo-style UGC video for software products |
| Physical UGC | UGC-style video for physical / e-commerce products |

**Growth Skills:**

| Skill | What it automates |
|---|---|
| Shorts Maker | Long-form content → auto-cut short-form clips for TikTok / Reels / Shorts |

**Web / App Skills:**

| Skill | What it automates |
|---|---|
| Website Building | Generate functional website layouts and components |

---

### Explainer Video Styles (9 animation types)

When producing explainer content inside the Supercomputer, you select a visual style:

| Style | Aesthetic |
|---|---|
| 8-bit Pixel Art | Retro video game aesthetic |
| Claymation | Aardman-style clay characters |
| Mixed-Media Collage | Cut-paper, zine, collage aesthetic |
| 2D Flat Illustration | Clean, modern SaaS explainer look |
| Hand-Drawn Whiteboard | RSA Animate / whiteboard doodle style |
| Low-Poly 3D | Geometric, minimal 3D |
| Polished 3D | Pixar / high-end 3D render quality |
| Isometric Flat-Vector | Tech explainer, app diagram style |
| Fluffy Plush-Toy | Soft, whimsical character aesthetic |

---

### Memory + Connectors

**Memory:** The agent retains your brand name, visual style preferences, target audience, preferred output formats, and decisions from past sessions. You don't re-brief it every time.

**Connectors:** Link external platforms so the agent can pull data from or push outputs to your existing stack.

> **Platform lock-in note:** Memory is the stickiness mechanism. Once the Supercomputer knows your brand, your style, and your workflows, switching to a different platform means starting that context over from zero. This is the same lock-in dynamic that makes Notion, Figma, and Salesforce sticky — not features, but accumulated context.

---

## Layer 5 — Integration (MCP & CLI + Adobe Plugins)

### MCP SERVER

**Headline:** *"Use Bigsfield inside any AI agent"*

Bigsfield has built a Model Context Protocol (MCP) server — the same open standard that allows Claude, ChatGPT, and Cursor to connect to external tools. This means all of Bigsfield's capabilities are accessible from inside any compatible AI agent without opening the Bigsfield website.

**Server details:**

| Detail | Value |
|---|---|
| Server URL | `mcp.bigsfield.ai/mcp` |
| Authentication | Bigsfield account login (no separate API key required) |
| Models available | 30+ |
| Setup | 3 steps: add URL → authenticate → use |

**Compatible AI agents:**

| Agent | Context |
|---|---|
| Claude (web) | Full integration via MCP connector settings |
| Claude Code | Terminal-based access for developers |
| Cowork (Claude desktop) | Desktop integration |
| ChatGPT | Via custom GPT connector |
| Cursor | IDE coding agent with creative generation |
| Grok Bot | xAI's conversational agent |
| OpenClaw | Open-source agent framework |
| Hermes Agent | Open-source agent framework |
| NemoClaw | Open-source agent framework |

**Skills available via MCP:**

| Category | Skills |
|---|---|
| Marketing | Ad Multiplier, Editorial Motion Graphics |
| UGC Factory | SaaS UGC, Physical UGC, Product Review UGC |
| Faceless Content | Faceless Content Factory, Stickman Cartoon |
| Motion & Design | Motion graphics, visual effects pipelines |
| Website Building | Site generation from prompt |
| Utility | Upscale, Background Removal, Format Conversion |

---

### ADOBE PLUGINS

Bigsfield ships as a native plugin inside three Adobe applications:

| Application | What it enables |
|---|---|
| After Effects | Generate and insert AI video directly inside a composition |
| Premiere Pro | Access AI generation while editing a video timeline |
| Photoshop | Generate AI images and apply Bigsfield edits from inside Photoshop |

> **Distribution strategy note:** Adobe Creative Cloud has 30+ million paid subscribers. Most of them open Premiere or Photoshop every working day. If the Bigsfield plugin delivers inside those tools, Bigsfield gets distribution to 30 million professional creatives without any of them ever visiting bigsfield.ai. This is not a feature — it's a distribution channel.

---

## The 5 Strategic Bets

### 1. Aggregation as strategy, not fallback

Bigsfield doesn't compete with OpenAI, Google, or ByteDance on model quality. It integrates their models and competes on UX, workflow, and access. This is the same bet Spotify made against music labels — own the interface and the context, not the underlying content. Short-term, this works while the model landscape is fragmented. Long-term, the risk is that a single model dominates and its maker builds their own creative platform directly.

### 2. The Skills system is the defensible moat

Model access is replicable. A Skills ecosystem — where workflows are packaged, installable, and improving over time — is harder to copy. If third parties start building and publishing Skills the way developers publish on the App Store, Bigsfield has a platform with compounding value. The question is whether they can catalyze that third-party ecosystem.

### 3. MCP = distribution without distribution spend

By publishing an MCP server, Bigsfield appears inside Claude, ChatGPT, and Cursor — products with tens of millions of daily active users. They're not building a new audience from scratch; they're embedding inside audiences that already exist and trust those tools. The cost of this distribution channel is engineering, not marketing spend.

### 4. The Adobe plugins are the enterprise trojan horse

Professional creative teams don't change their primary tools easily. The workflow, keyboard shortcuts, and mental model are deeply embedded. By living inside Premiere Pro and Photoshop rather than asking users to leave them, Bigsfield removes the biggest adoption barrier for enterprise creative teams. This is how you get to B2B pricing without building a sales team.

### 5. Supercomputer converts tool users into platform users

A user who pays for a single feature is a customer. A user whose brand identity, creative preferences, and production workflows live inside the Supercomputer's memory is a platform user. Platform users churn at a fraction of the rate of tool users. The Supercomputer's job isn't to generate better content — it's to make switching costs high enough that the question never comes up.

---

## The Biggest Gap

With 30+ models, 8 tabs, 60+ presets, and an agent platform, the product is genuinely comprehensive. But the single biggest unresolved question is positioning.

**What is Bigsfield best at?**

A first-time user — or a journalist, or a potential enterprise buyer — still cannot answer that in one sentence after seeing the full product.

- Is it the best video generation tool? Maybe — but Runway owns that positioning.
- Is it the best model aggregator? Probably — but that's not an exciting consumer story.
- Is it the best AI creative agent? Potentially — but the Supercomputer isn't the product homepage leads with.

As the AI tools market matures and consolidation begins, the platforms that survive will have a clear, ownable answer to "what are you the best at?" Bigsfield's current strategy — be the most complete — is powerful for retention but weak for acquisition. Every new user still has to be convinced to try it before they discover the depth.

> **Compare:** Runway = "AI video." Midjourney = "AI art." Bigsfield = "AI creative platform." Only one of those is a sentence someone says at a dinner table.

---

## How to Study a Product Like This

Use these questions on any AI product you want to analyze:

**Architecture**
- How many distinct layers does the product have — and what user need does each layer serve?
- Where does the "tool" end and the "platform" begin?
- Which features create switching costs, and which are purely functional?

**Models vs Features**
- Does the company build its own models or aggregate others'?
- If they aggregate: what is the value-add above the raw model?
- If they build: how do they compete when a larger lab ships a better model?

**Distribution**
- Where does the user first encounter this product?
- What integration channels exist beyond the main product (plugins, APIs, MCP, CLI)?
- Who are the power users and how does the product retain them?

**Strategy**
- What is the company's answer to "what are you the best at?"
- Which of their current features could a well-funded competitor copy in 6 months?
- What would have to be true for this product to be dominant in 3 years?

---

## Self-Test

1. Name the 5 product layers and what each one does for a different type of user.
2. What is the difference between a Feature and a Model in Bigsfield's taxonomy — and why does that distinction matter for UX?
3. Why is the MCP server strategically more important than it appears on the surface?
4. What is a Skill, and why is the Skills system potentially more defensible than the model library?
5. What is the single biggest strategic risk in Bigsfield's current approach?
6. How do the Adobe plugins change the distribution math for enterprise customers?
7. Why does Memory in the Supercomputer create switching costs in a way that features alone don't?
8. Name three AI companies whose models Bigsfield integrates — and explain why those companies haven't built a comparable aggregator themselves.

---

*Teardown based on: bigsfield.ai — all product tabs · August 2026 · Methodology: layer-by-layer architecture mapping, feature/model taxonomy, platform strategy analysis, integration audit.*
