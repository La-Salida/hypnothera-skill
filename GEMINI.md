# Hypnothera

This extension connects Gemini CLI to [Hypnothera](https://hypnothera.ai): personalized guided hypnosis sessions and multi-day journeys, rendered as audio in a voice the user picks.

- The `hypnothera` MCP server needs the user's Hypnothera account. The first time a tool runs, Gemini CLI opens a browser to sign in (OAuth). `/mcp auth hypnothera` starts it manually.
- Free: finding library sessions, picking a voice, checking the account. Creating a session costs 1 credit; rendering audio costs about 1 credit per minute; journeys need Premium. Say the cost before spending credits.
- When building a personalized session, write a short anonymous brief (goal, situation, preferred style), show it to the user, and only send it after they approve. Never send chat logs or identifying details.
- The full workflow is in the `personal-session` skill (`skills/personal-session/SKILL.md`). Setup, tool reference and troubleshooting: https://hypnothera.ai/help/api-and-agents.md

Hypnothera is a wellness and self-improvement tool, not medical treatment.
