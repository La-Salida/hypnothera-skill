# Hypnothera Personal Session

A plugin for Claude (and a portable skill for other AI agents) that writes a
personalized guided hypnosis script, or a multi-day journey of scripts, from
what your assistant already knows about your goals.

Everything happens in your conversation as text. The skill proposes a short
brief, free of identifying details, and writes the script only after you
approve it. It doesn't call any server, create audio, or send anything to
hypnothera.ai or anywhere else.

## Install (Claude Code)

This repo is its own plugin marketplace. Inside Claude Code:

```
/plugin marketplace add La-Salida/hypnothera-skill
/plugin install hypnothera@hypnothera
```

Then ask: *"Make me a hypnosis session based on what you know about me."*

## Install (manual skill file)

For claude.ai (upload as a skill, or add to a project), or any agent that
reads skill files, use
[`skills/personal-session/SKILL.md`](skills/personal-session/SKILL.md), or:

```bash
mkdir -p ~/.claude/skills/hypnothera-personal-session && \
curl -fsSL https://raw.githubusercontent.com/La-Salida/hypnothera-skill/main/skills/personal-session/SKILL.md \
  -o ~/.claude/skills/hypnothera-personal-session/SKILL.md
```

## What you get

- A brief to approve first: focus, session type (standard, sleep, morning,
  mental rehearsal, short reset), style, and length.
- A full script with settling in, induction, deepening, the personalized
  part, and a closing that either brings you back alert or lets you drift
  into sleep.
- For journeys, one day at a time, each building on the last.

Read it slowly, have someone read it to you, or record it in your own voice.

## About Hypnothera

[Hypnothera](https://hypnothera.ai) is a separate website and app from the
same team, with narrated sessions. This plugin doesn't connect to it.

Hypnothera is a wellness and self-improvement tool for relaxation, focus,
sleep quality, and confidence, not a medical service.
