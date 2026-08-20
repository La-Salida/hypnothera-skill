# Hypnothera Personal Session — agent skill

A skill for Claude Code, claude.ai, and other AI agents: turn what your
assistant already knows about you into a personalized hypnosis session or
multi-day journey on [hypnothera.ai](https://hypnothera.ai).

Your conversations never leave your assistant. The skill distills a short,
anonymous brief, shows it to you for approval, and only the approved brief
travels to hypnothera.ai — in the link's URL fragment, which browsers don't
send to servers.

## Install

**Claude Code**

```bash
mkdir -p ~/.claude/skills/hypnothera-personal-session && \
curl -fsSL https://hypnothera.ai/skills/hypnothera-session/SKILL.md \
  -o ~/.claude/skills/hypnothera-personal-session/SKILL.md
```

**claude.ai** — upload `SKILL.md` as a skill (Settings → Capabilities) or add
it to a project.

**Other agents** — place `SKILL.md` wherever your agent reads skills or
standing instructions.

Then ask: *"Make me a hypnosis session based on what you know about me."*

More: https://hypnothera.ai/tools/agent-skill

## Notes

- The canonical copy of `SKILL.md` lives in this repo and is mirrored at
  `https://hypnothera.ai/skills/hypnothera-session/SKILL.md` (web repo
  `public/skills/hypnothera-session/SKILL.md`). Keep the two in sync when
  editing.
- Hypnothera is a wellness and self-improvement tool for relaxation, focus,
  sleep quality, and confidence — not a medical service.
