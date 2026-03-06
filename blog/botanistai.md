# BotanistAI: An AI Plant Doctor in a Weekend

*Half-Baked #1*

---

I have a confession: I'm a serial plant killer.

Not intentionally. I *want* to be a good plant parent. I buy the cute pots. I read the care tags. I set reminders to water. And yet, somehow, my fiddle leaf fig still ended up looking like it survived a drought, a flood, and possibly a small fire.

The problem isn't motivation — it's diagnosis. By the time I notice something's wrong, I'm already googling "why are my plant leaves turning yellow" and getting 47 different answers ranging from "overwatering" to "underwatering" to "your plant is sad."

So I built something.

---

## The Idea

**What if I could just take a photo of my plant and instantly know what's wrong?**

Not a generic "this might be root rot" answer, but actual, specific advice:

- What plant is this? (I forget what I bought)
- Is it healthy? (Scale of 1-10)
- What's wrong? (If anything)
- What should I do? (Right now, today)
- How do I prevent this? (For next time)

Modern AI vision models are scary good at this kind of thing. They can look at a leaf and notice spots, discoloration, wilting, pest damage — stuff that takes a trained eye to catch.

So I spent a weekend wiring it up.

---

## What I Built

**BotanistAI** is a simple web app:

1. Open it on your phone
2. Take a photo of your plant (or upload one)
3. Get an instant AI-powered health report

That's it. No account required. No subscription. Just point and diagnose.

**→ [Try the live demo](https://botanistai.vercel.app)**

Under the hood, it's:

- **React + Vite** for the UI
- **Vercel AI SDK** for multi-model AI integration
- **Llama 4 Scout via Groq** as the default model (free, open-source — no API key needed)
- **Vercel serverless functions** to keep API keys off the client
- **Tailwind (CDN)** for styling
- **LocalStorage** for keeping a history of your scans

The whole thing is maybe 600 lines of code. Most of the work was in crafting the right prompt — getting the AI to think like a botanist and return structured, actionable advice instead of rambling paragraphs.

---

## The Architecture

The original version called the Gemini API directly from the browser — which meant the API key was exposed in client-side code. That makes a public demo impossible.

The new setup routes everything through a Vercel serverless function:

```
Browser → POST /api/analyze { image, provider, apiKey? }
                    ↓
         api/analyze.ts (serverless)
                    ↓
         Vercel AI SDK → Groq (default) / Gemini / Claude / GPT-4o
                    ↓
         PlantAnalysis JSON → Browser
```

The server holds the `GROQ_API_KEY`, so the public demo works without users needing to sign up for anything. If someone wants to use their own model, they can hit the gear icon in the header, pick a provider, and paste in their own API key.

---

## The Prompt Engineering

This is where the magic happens. A naive prompt like "what's wrong with this plant?" gives you generic, unhelpful answers.

The trick is to make the AI adopt a persona and return structured data:

```
You are an expert botanist and plant pathologist.
Analyze the provided image of a plant.
Identify the plant, diagnose its health, and provide actionable
care instructions. Be encouraging but realistic.
```

The Vercel AI SDK's `generateObject` function returns the response as validated, typed JSON via a Zod schema — no manual parsing needed.

---

## What Works

✅ **Identification is solid.** The AI correctly identifies most common houseplants, even from weird angles.

✅ **Health scoring feels right.** A healthy plant gets 85-95. A struggling one gets 40-60. A dying one gets below 30.

✅ **Care advice is specific.** Not "water more" but "water thoroughly until it drains, then wait until the top 2 inches of soil are dry."

✅ **It's fast.** Analysis takes 2-4 seconds.

✅ **Multi-model.** Users can swap to Gemini, Claude, or GPT-4o from the gear icon if they have their own API key.

---

## What's Half-Baked

This is a prototype, not a product. Here's what I intentionally didn't build:

🚧 **No accounts.** Your history lives in your browser's localStorage. Clear your cache and it's gone.

🚧 **No cloud sync.** Can't access your plant history from another device.

🚧 **No reminders.** It tells you to water in 3 days but won't ping you when the time comes.

🚧 **No verification.** The AI might be wrong. There's no expert review or confidence scoring.

🚧 **No tracking over time.** You can't upload photos weekly and see if your plant is getting better or worse.

These are all solvable problems. I just didn't solve them.

---

## Ideas for Taking This Further

If this idea resonates with you, here's where I'd go next:

### Make it a real product

- Add auth with Supabase or Firebase
- Store plant history in the cloud
- Build a subscription model ($5/month for unlimited scans?)
- Native apps for iOS/Android

### Make it smarter

- Fine-tune on plant disease datasets
- Add multi-image tracking to show health trends over time
- Integrate weather/location for climate-aware advice
- Confidence scoring ("I'm 90% sure this is root rot")

### Make it social

- Plant parent communities
- Before/after recovery stories
- Expert botanist Q&A

### Make it B2B

- Nursery inventory management
- Greenhouse monitoring
- Agricultural pest detection

---

## The Code

Everything is open source and MIT licensed:

**→ [github.com/abhirajp97/botanistai](https://github.com/abhirajp97/botanistai)**

Clone it, fork it, ship it, sell it. If you build something cool, I'd love to hear about it.

---

## Why I'm Doing This

I have a lot of ideas. Too many, probably. Most of them sit in a notes app, slowly decomposing.

**Half-Baked** is my attempt to do something about that. Every week (or so), I'll take one idea, build a working prototype, and release it into the wild.

Some will be useful. Some will be weird. All will be unfinished — but finished *enough* that someone could pick them up and run with them.

If that sounds interesting, stick around. There's more coming.

---

*Made by [Abhiraj Parikh](https://github.com/abhirajp97) as part of [Half-Baked](../), a weekly series of open-source prototypes.*
