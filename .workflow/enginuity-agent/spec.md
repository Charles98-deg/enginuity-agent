# EnginuityAgent: technical spec (personal profile intentionally omitted)

The "ABOUT ME" block (name, brand, voice samples, brand colours) is NOT stored here because the repo is meant to be public. Ask the user for it when needed, or read it from the encrypted store.

Ideal clients, skills and goal (generic, safe to store): founders / CEOs / brand managers of Web3 and AI startups, especially in Nigeria and Africa. Skills: conversion copy, landing pages, threads, content strategy, research articles, pitch decks, video scripts. Visual identity: Minecraft/voxel pixel aesthetic (gradient brown, grey, blue; exact hex values TBD). Goal: less time on social media, more time studying, while LinkedIn grows and brings client DMs and bounty income.

## Agent: "EnginuityAgent", one repo, two modules

### Module A: SCOUT (cron 06:00 and 18:00 UTC)
1. Fetch live Superteam Earn bounties and projects. Read the official docs at https://superteam.fun/earn/agents first; use the current documented endpoint. Never guess endpoints.
2. Filter: open, deadline at least 48h away, reward >= [MIN REWARD, e.g. 100] USDC, writing/content/thread/research/design-copy categories.
3. Skip already-seen IDs.
4. LLM scores fit 1-10 with a one-line reason.
5. Score >= 7: winning angle, outline, first draft of the hook in the owner's voice, submission checklist.
6. One Telegram digest (title, reward, deadline, score, link, draft). None: "No strong matches today."

### Module B: STUDIO (LinkedIn)
Inputs via Telegram, anytime: `/log <what I built or learned>`, `/idea <post idea>`, `/win <result>`, plus photos/screenshots of work. Scout findings and wins can become post material.

Draft run (cron 05:00 UTC weekdays; POSTS_PER_WEEK configurable, default 5):
1. Pillar of the day: Mon build in public (with screenshots) / Tue copy teardown (before/after rewrite, anonymize brands) / Wed lesson or story / Thu Web3/AI insight for African founders (carousel) / Fri proof, results or soft offer.
2. Write the post with the LINKEDIN PLAYBOOK.
3. Visual for EVERY post: a carousel of 5-10 slides at 1080x1350 exported as one PDF (document post) plus PNG previews, or a 1080x1350 image card. Render from HTML/CSS templates (Playwright) or Pillow, following apple-design + voxel brand. Use real screenshots when relevant. No paid image APIs.
4. Send to Telegram: text + previews + inline buttons APPROVE / EDIT / SKIP / REGENERATE. EDIT: owner replies with instructions; revise and resend.

Approval poll (cron every 30 min, 05:00-21:00 UTC):
- Read updates via Telegram getUpdates (no webhook, no server).
- APPROVE: publish to personal LinkedIn at POST_TIME (default 08:00 Africa/Lagos), or immediately if that time has passed.
- Then send the post link + "Golden hour: reply to every comment in the next 60 minutes with a real answer."
- NOTHING EVER POSTS WITHOUT EXPLICIT APPROVE.

LinkedIn publishing:
- Self-serve "Share on LinkedIn" product (w_member_social), versioned Posts API (POST /rest/posts), /rest/documents for PDFs, /rest/images for images. Check current docs and the LinkedIn-Version header before coding.
- Tokens last 60 days with no refresh token: store creation date, warn on Telegram at day 50 and day 57 with exact re-auth steps.
- Tell the owner exactly what LinkedIn requires to create a developer app, and the simplest phone-friendly way to get the token and person URN.
- The API can't read comments or analytics: build nothing that depends on them.

### LINKEDIN PLAYBOOK (store in prompts.py; apply to every post)
- Dwell time is the main signal: hook in the first 2 lines (before "see more"), short lines, white space, one idea per post.
- Specific beats generic: every post uses a real experience, number, tool or screenshot from the inbox. NEVER invent facts, results, clients or numbers. Empty inbox: write a teardown or insight post and tell the owner.
- Comments matter most: end with one specific question people can answer from their own experience. No "Agree?" bait.
- No external links in the body; if needed, tell the owner to post the link as the first comment.
- Max 3 relevant hashtags.
- Carousels: slide 1 = bold hook; under ~30 words per slide; last slide = takeaway + follow CTA.
- Show, don't tell: clients respond to visuals of real work, not technical explanations.
- One soft client CTA per week (e.g. "DM me 'COPY' if your landing page isn't converting").
- Ban AI-sounding phrases: "in today's fast-paced world", "delve", "game-changer", "unlock", emoji walls, heavy em-dash use.

### One-time deliverable
`profile_kit.md`: 3 headline options, an About section, Featured section ideas, and a rendered banner PNG that tells clients what the owner does and how to reach them.

## Tech rules
- Python 3.11, minimal dependencies.
- GitHub Actions in a PUBLIC repo: scout.yml, studio_draft.yml, studio_poll.yml, each with workflow_dispatch for manual tests.
- Commit state every run (keeps the repo active so schedules never get disabled).
- ENCRYPT all stored state and media with a key from GitHub Secrets: nothing personal in plain text.
- LLM via the OpenAI-compatible client with LLM_BASE_URL, LLM_API_KEY, LLM_MODEL (default: a current free-tier Gemini Flash/Flash-Lite model), swappable to Groq or OpenRouter with no code changes.
- Secrets: LLM_BASE_URL, LLM_API_KEY, LLM_MODEL, TELEGRAM_BOT_TOKEN, TELEGRAM_CHAT_ID, LINKEDIN_ACCESS_TOKEN, LINKEDIN_PERSON_URN, LINKEDIN_TOKEN_DATE, DATA_KEY.
- Accept Telegram commands ONLY from TELEGRAM_CHAT_ID.
- Retry with backoff on 429s; if the LLM fails, still send what you can.
- DRY_RUN=true: drafts go to Telegram, nothing posts to LinkedIn.
- Total running cost: $0.

## Safety
Never submit to bounties. Never post without APPROVE. Never invent facts.

## Deliverables
Full code + workflows, README with numbered phone-only setup, exact git commands for every change, and a "how to know it's working" checklist per module.

## Definition of Done
1. Scout sends a real digest twice daily.
2. Studio sends a draft with a visual every weekday morning.
3. The APPROVE tap publishes it to LinkedIn with no other action from the owner.
