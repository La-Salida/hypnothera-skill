---
name: hypnothera-personal-session
description: Create a personalized hypnosis session or multi-day journey on hypnothera.ai from what this agent already knows about the user. Use when the user asks for a hypnosis, relaxation, sleep, focus, or confidence session "based on what you know about me", a personalized wind-down, mental rehearsal for an upcoming event, or a multi-day self-improvement journey. Never sends chat logs — builds a short anonymous brief the user approves first.
---

# Hypnothera Personal Session

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
    "title": "Short session title, max 120 chars",
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
- Outline titles ≤ 120 chars; descriptions and directives ≤ 400 chars.

## Step 4 — Approval gate (mandatory)

Before building any URL, show the user the brief in plain language: the
goal text, session type, style, and the day-by-day outline if there is
one. Ask if they'd like changes. Apply edits and show it again. Only
proceed on explicit approval. If the user declines, stop — never retry
silently.

## Step 5 — Open the link

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
   hypnothera.ai, sign in (new accounts include free credits that cover
   the first session), and their audio is generated from there. For
   journeys, Day 1 is created first; later days use 1 credit each.

## Failure handling

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
