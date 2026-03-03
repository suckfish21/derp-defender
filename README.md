
<p align="center">
  <img src="https://em-content.zobj.net/source/apple/391/shield_1f6e1-fe0f.png" width="80" alt="shield" />
</p>

<h1 align="center">DERP Defender</h1>

<p align="center">
  <strong>Your vague prompt ends here.</strong><br/>
  ADHD-friendly prompt clarification system for any AI model.
</p>

<p align="center">
  <a href="#setup-by-platform"><img src="https://img.shields.io/badge/Claude-Works-blueviolet?style=flat-square&logo=anthropic" alt="Claude" /></a>
  <a href="#chatgpt"><img src="https://img.shields.io/badge/ChatGPT-Works-green?style=flat-square&logo=openai" alt="ChatGPT" /></a>
  <a href="#grok"><img src="https://img.shields.io/badge/Grok-Works-black?style=flat-square&logo=x" alt="Grok" /></a>
  <a href="#google-gemini-gems"><img src="https://img.shields.io/badge/Gemini-Works-blue?style=flat-square&logo=google" alt="Gemini" /></a>
  <a href="#open-source-models-llama-mistral-etc"><img src="https://img.shields.io/badge/Open_Source-Works-orange?style=flat-square&logo=meta" alt="Open Source" /></a>
  <img src="https://img.shields.io/badge/license-MIT-gray?style=flat-square" alt="MIT License" />
</p>

---

## The Problem

```
  You                          The AI
  ┌─────────────┐              ┌─────────────────────────┐
  │ "do something│    ───►     │ *confidently produces    │
  │  with the    │             │  the wrong thing for     │
  │  onboarding" │             │  45 minutes*             │
  └─────────────┘              └─────────────────────────┘
                    😐 → 😤 → 🔄 → 😩

  You                          DERP Defender              The AI
  ┌─────────────┐              ┌───────────────┐          ┌──────────────────┐
  │ "do something│    ───►     │ 3-5 targeted  │   ───►   │ *nails it on the │
  │  with the    │             │ questions     │          │  first try*      │
  │  onboarding" │             │ (~3 min)      │          │                  │
  └─────────────┘              └───────────────┘          └──────────────────┘
                    😐 → 🤔 → 💡 → 🎯
```

Most AI failures aren't model failures — they're **input failures.** DERP Defender front-loads 3-5 minutes of clarification to save 30-60 minutes of correction.

---

## How It Works

```
                    ┌─────────────────────────────┐
                    │      YOUR MESSY INPUT        │
                    │  "i need to do the thing"    │
                    └──────────────┬──────────────┘
                                   │
                          ┌────────▼────────┐
                          │  STEP 0: DETECT  │
                          │  Already clear?  │
                          └────────┬────────┘
                            ┌──────┴──────┐
                          YES             NO
                            │              │
                     ┌──────▼──────┐  ┌───▼───────────┐
                     │  Auto-skip  │  │ STEP 1: DEPTH │
                     │  proceed ►  │  │ baby/mid/alpha│
                     └─────────────┘  └───┬───────────┘
                                          │
                               ┌──────────▼──────────┐
                               │  STEP 2: 7-LAYER    │
                               │  INTERROGATION      │
                               │                     │
                               │  1. Identity     ◄──┤
                               │  2. Context      ◄──┤
                               │  3. Task         ◄──┤
                               │  4. Constraints  ◄──┤
                               │  5. Format       ◄──┤
                               │  6. Examples     ◄──┤
                               │  7. Verification ◄──┤
                               └──────────┬──────────┘
                                          │
                               ┌──────────▼──────────┐
                               │  STEP 3: ASSEMBLE   │
                               │  precision prompt    │
                               └──────────┬──────────┘
                                          │
                               ┌──────────▼──────────┐
                               │  STEP 4: APPROVE    │
                               │  edit / send / save  │
                               └──────────┬──────────┘
                                          │
                                          ▼
                               ┌─────────────────────┐
                               │  🎯 DERP DEFENDED   │
                               │  PROMPT → AGENT     │
                               └─────────────────────┘
```

---

## Depth Levels

```
  baby DERP 🐣          mid DERP 🦊            alpha DERP 🐉
  ─────────────         ─────────────          ─────────────
  2-3 questions         4-6 questions          7-10 questions
  ~2 minutes            ~5 minutes             ~10 minutes
  Low stakes            Medium complexity       High stakes
  Single task           Multi-interpretation    Strategic/novel
```

Override with: `"baby DERP"`, `"mid DERP"`, or `"alpha DERP"` when invoking.

---

## The 7-Layer Framework

```
  Layer          Question It Answers         🐣    🦊    🐉
  ────────────── ─────────────────────────── ───── ───── ─────
  1. Identity    Who executes this?          Ask   Ask   Ask
  2. Context     What background is needed?  Ask   Ask   Ask
  3. Task        Concrete deliverable?       Ask   Ask   Ask
  4. Constraints What boundaries apply?      ~     Ask   Ask
  5. Format      Output shape/structure?     ~     Ask   Ask
  6. Examples    What does good look like?   ~     ~     Ask
  7. Verification How do you know it's done? ~     ~     Ask

  Ask = explicitly asked    ~ = inferred from context
```

---

## Setup by Platform

The core system prompt lives in **`SKILL.md`**. Copy it, paste it, go.

### Claude Code (CLI)

```bash
mkdir -p ~/.claude/skills/derp-defender
cp SKILL.md ~/.claude/skills/derp-defender/SKILL.md
```

Invoke with: `"DERP this"`, `"derp defender"`, or `/derp-defender`

### Claude.ai (Projects)

1. Open your Claude.ai project
2. Go to **Project Knowledge**
3. Upload `SKILL.md` as a knowledge file
4. Invoke in chat: `"DERP this"` or `"derp defender"`

### ChatGPT

**Option A — Custom GPT (recommended):**

1. Go to [chat.openai.com](https://chat.openai.com) → **Explore GPTs** → **Create**
2. Name: **DERP Defender**
3. In **Instructions**, paste the contents of `SKILL.md` (skip the YAML frontmatter between the `---` lines at the top)
4. Conversation starters:
   - `"DERP this"`
   - `"I have a messy idea"`
   - `"Make this prompt better"`
5. Save (private or public)

**Option B — Custom Instructions (all chats):**

1. **Settings** → **Personalization** → **Custom Instructions**
2. Paste into "How would you like ChatGPT to respond?"
3. Active across all conversations, but less depth than a dedicated GPT

### Grok

1. Start a new conversation on [grok.com](https://grok.com)
2. Paste `SKILL.md` contents into the system prompt / custom instructions
3. Add this constraint (Grok is chatty by default):
   ```
   Keep interrogation questions to one per message. No commentary between questions.
   ```
4. Invoke with: `"DERP this"` or `"derp defender"`

### Google Gemini (Gems)

1. Open [gemini.google.com](https://gemini.google.com)
2. **Gem Manager** → **New Gem**
3. Name: **DERP Defender**
4. Paste `SKILL.md` contents into the instructions
5. Save and start a conversation with the Gem

### Open-Source Models (Llama, Mistral, etc.)

Works with any model that accepts a system prompt:

```python
# Example with Ollama
import ollama

with open("SKILL.md") as f:
    system_prompt = f.read()

response = ollama.chat(
    model="llama3",
    messages=[
        {"role": "system", "content": system_prompt},
        {"role": "user", "content": "your vague idea here"}
    ]
)
```

**Note:** Models under 13B parameters may struggle with depth calibration. For smaller models, add:
```
Always use baby DERP depth (2-3 questions only).
```

### Cursor / Windsurf / Other AI IDEs

1. Add `SKILL.md` to your project's AI rules:
   - **Cursor:** `.cursor/rules/derp-defender.md`
   - **Windsurf:** `.windsurfrules` or project-level AI instructions
2. Invoke in chat: `"DERP this"` or `"derp defender"`

---

## Triggers

```
  ┌─────────────────────────────────────────────────┐
  │              SAY ANY OF THESE:                   │
  │                                                  │
  │   "DERP this"         "derp defender"            │
  │   "Make this prompt better"                      │
  │   "I have an idea but it's messy"                │
  │   "Help me figure out what I need"               │
  │                                                  │
  ├─────────────────────────────────────────────────┤
  │              OVERRIDE DEPTH:                     │
  │                                                  │
  │   "baby DERP"    → 🐣  2-3 Qs, ~2 min          │
  │   "mid DERP"     → 🦊  4-6 Qs, ~5 min          │
  │   "alpha DERP"   → 🐉  7-10 Qs, ~10 min        │
  │                                                  │
  ├─────────────────────────────────────────────────┤
  │              SKIP IT:                            │
  │                                                  │
  │   "just do it"   "skip DERP"   "I know what     │
  │                                  I want"         │
  └─────────────────────────────────────────────────┘
```

---

## ADHD Momentum Rules

```
  ┌──────────────────────────────────────────────────────────┐
  │                                                          │
  │  🧠  One question at a time (never stacks 3 in one msg) │
  │                                                          │
  │  🎯  Offers options instead of open-ended questions      │
  │                                                          │
  │  ⚡  Proposes defaults when you say "I don't know"       │
  │                                                          │
  │  🏃  Respects "just go" — proceeds with what it has      │
  │                                                          │
  │  🔀  Captures tangents as follow-ups, never derails      │
  │                                                          │
  └──────────────────────────────────────────────────────────┘
```

---

## Customization

### Adding Domain Context

If you work in a specific domain, add a context module after the "Cross-Environment Notes" section in `SKILL.md`:

```markdown
## Domain Context Module

When DERP Defender detects [your domain]-related content, automatically inject:

- **Company:** [what you do]
- **Audience:** [who your users are, their sophistication level]
- **Messaging rules:** [tone, terminology, positioning constraints]
- **Key terms:** [domain glossary the model should know]
```

### Agent Routing

If you use multiple AI agents or GPTs, add a routing table:

```markdown
## Agent Routing

| Task Domain | Recommended Agent |
|---|---|
| Code & architecture | Coding agent |
| Content & copy | Writing agent |
| Data & analysis | Analytics agent |
| Strategy & planning | Planning agent (use alpha DERP) |
```

---

## Before / After

```
  BEFORE                              AFTER
  ──────                              ─────

  "make the app faster"         →     "Profile the /api/search endpoint,
                                       identify the top 3 bottlenecks by
                                       response time, and propose fixes.
                                       Target: p95 under 200ms."

  "do something with auth"      →     "Add JWT refresh token rotation to
                                       the login flow. Store refresh tokens
                                       in HttpOnly cookies. Return 401 with
                                       a specific error code when expired."

  "help with the landing page"  →     "Rewrite the hero section headline
                                       and subhead. Audience: technical
                                       founders. Tone: direct, no hype.
                                       Include 2 variants for A/B testing."
```

---

<p align="center">
  <strong>Stop re-explaining. Start DERPing.</strong><br/><br/>
  <code>Just say "DERP this" and paste your messy idea.</code>
</p>

---

## License

MIT — use it however you want.
