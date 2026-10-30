---
description: Create a personalized hypnosis session or multi-day journey on hypnothera.ai from what this agent already knows about the user. Use when the user asks for a hypnosis, relaxation, sleep, focus, or confidence session "based on what you know about me", a personalized wind-down, mental rehearsal for an upcoming event, or a multi-day self-improvement journey. With a connected account it can also find ready-made library sessions, render audio in the conversation, and continue journeys day by day. Never sends chat logs — builds a short anonymous brief the user approves first.
---

# Hypnothera Personal Session

Full setup, tool reference, credit costs, and troubleshooting:
https://hypnothera.ai/help/api-and-agents.md

Turn what you already know about the user into a personalized guided
hypnosis session (or multi-day journey) on hypnothera.ai.

## When to use

- The user asks for a hypnosis, deep-relaxation, sleep, focus, or
  confidence session "based on what you know about me" or "from our
  conversations".
- The user wants mental rehearsal for an upcoming event (interview,
  presentation, exam, competition).
- The user wants a multi-day program for a personal goal (better sleep,
  calmer mornings, changing an unwanted habit, steady confidence).

## When NOT to use

- The user is seeking help for a medical or mental-health condition, is in
  crisis, or asks for treatment of any kind. Do not build a brief. Suggest
  they speak with a qualified professional. Hypnothera is a wellness and
  self-improvement tool, not a medical service.

## Ready-made option (MCP, free)

If the user wants something to play right now rather than a new
personalized session, call `mcp__hypnothera__search_sessions` (optionally
with a `topic` from `mcp__hypnothera__list_topics`). Library sessions play
free at their `listen_url`, with no credits. Offer a personalized session
when nothing fits.

## Step 1 — Reflect

Look across what you know about this user — recurring goals, stressors,
upcoming events, sleep and focus patterns, things they said they want to
change. Distill 2–4 themes.

**Hard privacy rules (never break these):**

- Never include verbatim quotes from conversations.
- Never include names, employers, places, health conditions, diagnoses, or
  any identifying detail.
- Write the brief in general second-person wellness language ("you", "your
  mornings", "the presentation ahead").
- The brief text WILL be sent to hypnothera.ai when the user confirms, so
  every word of it must be safe to share with a third party.

## Step 2 — Frame for wellness

Describe goals as relaxation, confidence, composure, focus, sleep quality,
preparation, or supporting a chosen personal goal. Never use medical,
therapy, treatment, cure, or diagnosis language. Frame difficult topics as
"feeling prepared", "letting go of tension", or "supporting the change
you've chosen".

## Step 3 — Build the brief

Construct this JSON exactly (omit optional fields you don't need):

```json
{
  "v": 1,
  "source": "claude-code",
  "brief": {
    "specific_needs": "What the session should work on, in 2-6 sentences. Max 2000 chars. Required.",
    "script_type": "standard | sleep | morning | nsdr | lucid_dreaming | visualization",
    "style": "mindfulness | classic | conversational | storytelling | direct | experimental | nlp | energetic | rapid | somatic",
    "title": "Optional brief label / journey playlist title, max 120 chars",
    "summary": "One-line description for the confirm card, max 500 chars",
    "language": "english"
  },
  "journey": {
    "days": 7,
    "outline": [
      { "day": 1, "title": "…", "description": "…", "directives": "optional structural guidance" }
    ]
  }
}
```

- `source`: the host you're running in — `claude-code`, `claude-ai`,
  `codex`, or similar.
- Only include `journey` when the user wants a multi-day program. `days`
  must be 2–30. The `outline` is optional but recommended: one entry per
  day with a short title and a one-sentence description of that day's
  focus, building progressively toward the goal.
- `brief.title` labels the handoff preview and names a journey playlist. It does
  not set the generated session's title. For single-session MCP requests, omit
  `title`; do not promise that a proposed title will become the saved title.
- Outline titles ≤ 120 chars; descriptions and directives ≤ 400 chars.

## Step 4 — Approval gate (mandatory)

Before building any URL, show the user the brief in plain language: the
goal text, session type, style, and the day-by-day outline if there is
one. Explain that a script costs 1 credit and audio rendering costs about
1 additional credit per minute. Ask if they'd like changes. Apply edits
and show it again. Only proceed on explicit approval. If the user declines, stop — never retry
silently.

## Step 5 — Create it

### Preferred: connected Hypnothera account (MCP)

If `mcp__hypnothera__*` tools are available and authenticated:

1. Call `mcp__hypnothera__get_account` to confirm the connection, plan,
   and credit balance (a session script costs 1 credit; rendering audio
   costs about 1 additional credit per minute).
2. After the user approves the brief (Step 4 is still mandatory), call
   `mcp__hypnothera__create_session` with the same fields as the brief:
   `specific_needs`, `script_type`, `style`, `summary`, `language`, plus
   `title`, `journey_days` and `journey_outline` for journeys. Keep the returned
   `script_id` (and `collection_id` for a journey).
3. The script takes a minute or two to write. Call
   `mcp__hypnothera__get_session` with the `script_id` to check that its
   `status` is `completed` before rendering. Don't call it in a tight loop.
4. Offer to render the audio in the conversation. Call
   `mcp__hypnothera__list_voices` and suggest a few voices where
   `available` is true (each has a `preview_url`). Tell the user the
   estimated cost: about 1 credit per minute of `duration_minutes`.
5. Only after the user approves the voice and the cost, call
   `mcp__hypnothera__render_audio` once with `script_id` and `voice_id`.
   Rendering takes a few minutes. Then `get_session` returns
   `has_audio: true` and an `audio_url` the user can play directly for
   about two hours; `listen_url` is the session's permanent page.
6. If the user would rather choose a voice on the website, share the
   returned `next_step_url` instead.

If the tools exist but the server needs authentication, tell the user to
run `/mcp`, select **hypnothera**, and choose **Authenticate** — a browser
opens so they can sign in with their existing Hypnothera account (or
create one; new accounts include free credits). Then retry.

### Continuing a journey (MCP)

`create_session` with `journey_days` creates Day 1 only. To create the
next day when the user asks for it:

1. Call `mcp__hypnothera__list_journeys` to find the `journey_id` and see
   which day comes next and whether the previous day's script is finished.
2. Call `mcp__hypnothera__continue_journey` with the `journey_id`, plus
   optional `notes` in wellness language about how the previous day went.
   It builds on the previous day's session and that day's planned focus.
3. It creates one day per call, spends credits like `create_session`, and
   needs a Premium plan. Never call it several times to create days ahead.
4. Render the new day's audio the same way as steps 3–5 above.

### Fallback: handoff link (works everywhere, no connection needed)

1. Serialize the JSON compactly and encode it as **base64url** (UTF-8,
   `+`→`-`, `/`→`_`, strip `=` padding).
2. Open (or print) this URL for the user:

   ```
   https://hypnothera.ai/from-skill#<base64url-payload>
   ```

   The payload must ride in the `#` fragment — never a query string. The
   fragment is read by the page in the user's browser and is not sent to
   any server until the user confirms.

3. Tell the user what to expect: they'll review the brief on
   hypnothera.ai, sign in, check their credit balance, and create the script.
   They then choose a voice and render audio on the website. For journeys,
   Day 1 is created first; later scripts and audio use additional credits.

## Failure handling

- Never automatically retry `create_session`, `continue_journey`, or
  `render_audio` after a timeout or uncertain result. None is idempotent;
  check with `get_session` or `list_journeys` first to avoid duplicate
  sessions and credit charges.
- A handoff fragment is not encryption. Anyone with the full link can read the
  brief; keep it anonymous and do not post the link publicly.

- If the user declines the brief, stop.
- If the link doesn't open on their machine, print the URL for them to
  copy.
- If they report the page said the link couldn't be read, rebuild the
  JSON, re-encode, and give them a fresh link.

## Worked example

User: "Make me a hypnosis journey based on what you know about me."

Themes you found: racing thoughts at night before big meetings; wants
calmer, more deliberate mornings.

```json
{
  "v": 1,
  "source": "claude-code",
  "brief": {
    "specific_needs": "You often carry the day's momentum into the night, and your mind keeps rehearsing tomorrow's conversations. This journey helps you set the day down in the evening, sleep deeply, and step into important mornings feeling composed and prepared.",
    "script_type": "sleep",
    "style": "mindfulness",
    "title": "Set It Down",
    "summary": "An evening wind-down journey for deep sleep and composed mornings",
    "language": "english"
  },
  "journey": {
    "days": 5,
    "outline": [
      { "day": 1, "title": "Unclench", "description": "Release the day physically, body-first, before touching any thoughts." },
      { "day": 2, "title": "Close the tabs", "description": "A ritual for setting unfinished business aside until morning." },
      { "day": 3, "title": "Deep water", "description": "Extend the wind-down into deeper, longer sleep." },
      { "day": 4, "title": "Quiet rehearsal", "description": "Calm mental rehearsal of tomorrow going smoothly." },
      { "day": 5, "title": "Composed mornings", "description": "Anchor the feeling of waking rested and stepping in deliberately." }
    ]
  }
}
```

Show that to the user in plain language, get approval, encode, open
`https://hypnothera.ai/from-skill#<payload>`.
