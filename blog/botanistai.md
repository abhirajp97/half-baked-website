# BotanistAI: An AI Plant Doctor in a Weekend

*Half-Baked #1*

---

My plants keep dying. Not from neglect — I do everything right, water on schedule, check the care tags, move them to better light. They die anyway.

The problem is diagnosis. By the time a leaf looks wrong, I'm already googling "yellow leaves houseplant" and getting contradictory answers. So I spent a weekend building something that skips the googling.

---

## The Idea

**Take a photo of your plant. Find out what's wrong.**

Not "this might be overwatering" — actually specific:

- What plant is this?
- Is it healthy? (1–10)
- What's wrong right now?
- What do I do today?
- How do I prevent it next time?

---

## What I Built

**BotanistAI** is a web app:

1. Open it on your phone
2. Take a photo of your plant (or upload one)
3. Get an instant health report

No account. No subscription. Just point and diagnose.

Stack:
- **React + Vite** for the UI
- **Gemini 2.5 Flash** for AI vision analysis
- **LocalStorage** for scan history

---

## The Prompt Engineering

The naive prompt — "what's wrong with this plant?" — gives you generic answers.

The fix is a persona + structured output:

```
You are an expert botanist and plant pathologist.
Analyze this plant image and provide:
1. Plant identification (common and scientific name)
2. Health assessment (score 0-100)
3. Current diagnosis (be specific)
4. Immediate care instructions (actionable steps)
5. Preventive measures

Return your response as JSON with these exact fields...
```

Structured output means I can parse and display the results cleanly instead of rendering a wall of text.

---

## What Works

✅ **Identification is solid.** Correctly identifies most common houseplants, even from weird angles.

✅ **Health scoring feels right.** Healthy plants score 85–95. Struggling ones score 40–60. Dying ones go below 30.

✅ **Care advice is specific.** Not "water more" — "water thoroughly until it drains, then wait until the top 2 inches of soil are dry."

✅ **It's fast.** Analysis takes 2–3 seconds.

---

## What's Half-Baked

🚧 **No accounts.** History lives in localStorage. Clear your cache and it's gone.

🚧 **No cloud sync.** Can't access scan history from another device.

🚧 **No reminders.** It tells you to water in 3 days but won't ping you when the time comes.

🚧 **No verification.** The AI can be wrong. There's no confidence scoring or expert review.

🚧 **No tracking over time.** You can't upload photos weekly and see if your plant is recovering.

---

## Ideas for Taking This Further

**Make it a real product**
- Auth with Supabase or Firebase
- Cloud-stored plant history
- Subscription model
- Native mobile apps

**Make it smarter**
- Fine-tune on plant disease datasets
- Multi-photo health trend tracking
- Weather/location-aware advice
- Confidence scoring

**Make it B2B**
- Nursery inventory management
- Greenhouse monitoring
- Agricultural pest detection

---

## The Code

MIT licensed, everything on GitHub:

**→ [github.com/abhirajp97/botanistai](https://github.com/abhirajp97/botanistai)**

Fork it, extend it, ship it.

---

## Why I'm Doing This

I have a lot of half-finished ideas. Most stay that way. **Half-Baked** is my attempt to change that — build one thing, release it, move on. Some will be useful. Some won't. All will be incomplete enough that there's room for someone else to take them further.

If that's you, go for it.

---

*Made by [Abhiraj Parikh](https://github.com/abhirajp97) as part of [Half-Baked](../), a weekly series of open-source prototypes.*
