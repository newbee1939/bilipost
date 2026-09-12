# bilipost

**Write in your own language. Post in English. Never open X.**

## Who it is for

People who want to post in English on X.
But the normal X makes them read posts and lose time, so they would rather not look at it.

## Experience

One screen. Just a field for your language and a field for English, side by side.
Write → translate → fix it if you want → post. Posting in your own language is fine too.

## Out of scope

- **Reading.** Other people's posts are not hidden — they do not exist. No timeline, likes, follows or analytics either.
- **Scheduling (v1).** It needs an always-on server. The design leaves room for it.

## Decisions

- **Open source, self-hosted** — App Store distribution breaks on scheduling, API keys and release cost
- **LLM for translation** — a translation engine cannot be told the constraints a post has to satisfy
- **English out, any language in** — the language setting disappears entirely
