---
name: creating-social-content
description: >
  Drafts platform-native social media posts (X, LinkedIn, TikTok, Instagram) in
  an authentic build-in-public founder voice. Use when a topic, event, or
  decision needs to become a post on a specific platform — the skill turns the
  raw input into platform-tuned copy with the right length, hook, and constraints.
  For marketing copy fragments (landing pages, emails, ads) use `copywriting`
  instead; for content calendar planning use `planning-content`.
version: 1.0.0
owner: HOUDINI
status: active
last_modified: 2026-05-24
blast_radius: read-only
locale_support:
  - en
requires: []
tags:
  - content
  - social-media
  - workflow
---

# creating-social-content

Workflow skill for drafting social media posts that pass the "could any other company post this?" test. KOOKY-authored; lives in the public `kooky-skills` repo as a showcase of how we author SPEC v1.2-compliant workflow skills.

## What this skill does

Takes a topic + target platform and returns ready-to-publish copy tuned to that platform's native form. Output is the post itself plus a small amount of metadata (character count, engagement notes, hashtag suggestions). The skill applies a single hard filter to every draft: if the post could be published by any other company without changing it, the draft is rejected and rewritten.

Platform-native means writing to the platform's actual conventions — not pasting the same text everywhere. An X thread is not a LinkedIn post is not a TikTok script.

## When to use it

- A topic, decision, or result needs to become a social post (specific platform identified).
- Adapting one piece of content across multiple platforms (each gets its own native draft, not a copy-paste).
- Drafting threads, reels, or single posts from a raw idea.
- Anchoring content to one of the four founder-voice pillars using the current 2026-Q2 mix: Case Studies & Proof (35%), Industry Hot Takes (30%), Founder's Journey (20%), Behind-the-Scenes Ops (15%).

> **Pillar mix note:** values current as of 2026-Q2. Scheduled revisit after 2026-06-26. Previous mix: 40/25/20/15 (Founder's / BTS / Case Studies / Hot Takes).

Do **not** use this skill when:

- The output needs to be paid-ad copy (different constraints; that's an ads skill, not this).
- The platform isn't named — pick a platform first, then come back.
- The deliverable is long-form (newsletter, article, blog) — wrong scope; use a long-form content skill.

## How to use it

1. **Identify the topic and target platform.** If the caller didn't specify a platform, ask before drafting — platform shape determines almost every other decision.
2. **Pick the content pillar** if not specified. Default to Founder's Journey unless the topic clearly belongs elsewhere.
3. **Draft to the platform's native form:**
   - **X single tweet:** ≤280 chars. No external links in the main tweet. Reply with link if needed.
   - **X thread:** Numbered (1/, 2/, 3/), 3–7 tweets. Hook-first opening. End with a question or open thread.
   - **LinkedIn:** Professional tone but not corporate. Line breaks every 1–2 sentences. Hook in the first line. 150–300 words. 3–5 hashtags at the bottom.
   - **TikTok script:** Hook in first 3 seconds. Face-to-camera. 45–90 seconds. Visual cues marked inline. Conversational, not scripted-sounding.
   - **Instagram Reels:** Cross-post selectively from TikTok. Separate caption text optimized for IG (different audience).
4. **Apply the brand voice:** direct, specific, honest, aspirational without hype. Concrete numbers and names beat abstractions. Lessons over flexing.
5. **Run the rejection filter:** could any other company post this exact thing? If yes, rewrite with more specificity until the answer is no.
6. **Return the output:** post copy ready to paste, labeled by platform, with character count (for X), engagement notes (timing, reply strategy), and 1–3 suggested follow-ups for threads.

## What to escalate

Stop and ask the operator before drafting when:

- The request involves naming or characterizing a real person, partner, or company in a way that could be defamatory or misrepresent them.
- The topic includes claims about results (revenue, growth, customer outcomes) that the operator hasn't confirmed are public — never fabricate numbers, even small ones.
- The request asks for content in a locale this skill doesn't support yet (currently `en` only — `locale_support: [en]`).
- The brand voice conflicts with what's being asked (e.g., request for hype-style language when the voice is "aspirational without hype") — confirm the operator wants the voice violated.
- The content would constitute legal, medical, or financial advice — return the topic for re-scoping.

## Output constraints

- Never fabricate numbers, dates, or results.
- Never use corporate jargon: leverage, synergy, disrupt, paradigm shift, ecosystem (as buzzword), unlock.
- Limit to 1–2 emoji per post maximum; zero is preferred for LinkedIn and X threads.
- Never prefix threads with "Thread:" or "🧵 Thread 👇" or similar.
- Always attribute voice to the founder (first person), not the company generically.
- Hashtags only on LinkedIn (3–5) and Instagram (5–10). Zero on X. None inline in body copy.

## References

- `interface.yaml` — machine-readable capability surface
- `PILOT_LEARNINGS.md` — workflow-archetype findings surfaced during SPEC v1.2 pilot
- SPEC v1.2 §2.3 — operator-centric body section convention
