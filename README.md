# Hypnothera Personal Session — Claude Code plugin

A plugin for Claude Code (and a portable skill for other AI agents): turn what
your assistant already knows about you into a personalized hypnosis session or
multi-day journey on [hypnothera.ai](https://hypnothera.ai).

Your conversations never leave your assistant. The skill distills a short,
anonymous brief, shows it to you for approval, and only the approved brief
travels to hypnothera.ai — in the link's URL fragment, which browsers don't
send to servers.

## Install (Claude Code — recommended)

This repo is its own plugin marketplace. Inside Claude Code:

```
/plugin marketplace add La-Salida/hypnothera-skill
/plugin install hypnothera@hypnothera
```

Then ask: *"Make me a hypnosis session based on what you know about me."*

**Already have a Hypnothera account?** The plugin bundles a remote MCP server.
Run `/mcp`, select **hypnothera**, and choose **Authenticate** — a browser
opens to connect your account, and sessions are then created directly in your
library without leaving the conversation.

## Install (manual skill file)

For claude.ai (upload as a skill under Settings → Capabilities, or add to a
project), Codex, or any agent that reads skill files, use
[`skills/personal-session/SKILL.md`](skills/personal-session/SKILL.md), or:

```bash
mkdir -p ~/.claude/skills/hypnothera-personal-session && \
curl -fsSL https://hypnothera.ai/skills/hypnothera-session/SKILL.md \
  -o ~/.claude/skills/hypnothera-personal-session/SKILL.md
```

More: https://hypnothera.ai/tools/agent-skill

## Notes

- The canonical copy of the skill lives at
  `skills/personal-session/SKILL.md` in this repo and is mirrored at
  `https://hypnothera.ai/skills/hypnothera-session/SKILL.md` (web repo
  `public/skills/hypnothera-session/SKILL.md`). Keep the two in sync when
  editing.
- Hypnothera is a wellness and self-improvement tool for relaxation, focus,
  sleep quality, and confidence — not a medical service.
