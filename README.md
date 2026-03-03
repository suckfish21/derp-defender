# DERP Defender

**ADHD-friendly prompt clarification system.** Intercepts vague inputs — brain dumps, voice-to-text rambles, half-baked ideas — and produces precision-engineered prompts through structured interrogation.

Works with **Claude, ChatGPT, Grok, Gemini, and open-source models.**

---

## What It Does

Most AI failures aren't model failures — they're **input failures.** A vague prompt produces a confidently wrong result that wastes your session and requires re-work.

DERP Defender front-loads 3-5 minutes of clarification to save 30-60 minutes of correction:

1. **Detects** vague, ambiguous, or underspecified prompts (auto-skips clear ones)
2. **Calibrates** depth — baby DERP (2-3 Qs), mid DERP (4-6), alpha DERP (7-10)
3. **Interrogates** using a 7-layer framework (Identity, Context, Task, Constraints, Format, Examples, Verification)
4. **Assembles** a self-contained, precision prompt ready for any agent or model
5. **Protects momentum** — ADHD-friendly by design: one question at a time, options over open-ended questions, defaults when you're stuck

---

## The 7-Layer Framework

Every prompt is checked against these layers. Depth level determines how many get asked vs. inferred:

| Layer | Question It Answers | baby | mid | alpha |
|---|---|---|---|---|
| 1. Identity | Who executes this? | Ask | Ask | Ask |
| 2. Context | What background is needed? | Ask | Ask | Ask |
| 3. Task | What's the concrete deliverable? | Ask | Ask | Ask |
| 4. Constraints | What boundaries apply? | Infer | Ask | Ask |
| 5. Format | What shape should output take? | Infer | Ask | Ask |
| 6. Examples | What does good/bad look like? | Infer | Infer | Ask |
| 7. Verification | How do you know it's done? | Infer | Infer | Ask |

---

## Setup by Platform

The core system prompt lives in **`SKILL.md`**. Installation varies by platform.

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

**Explicit** — say these to activate:
- `"DERP this"` / `"derp defender"`
- `"Help me figure out what I need"`
- `"Make this prompt better"`
- `"I have an idea but it's messy"`

**Override depth:**
- `"baby DERP"` — 2-3 questions, ~2 min
- `"mid DERP"` — 4-6 questions, ~5 min
- `"alpha DERP"` — 7-10 questions, ~10 min

**Skip it:**
- `"just do it"` / `"skip DERP"` / `"I know what I want"`

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

## ADHD Momentum Rules

Built into the system prompt. DERP Defender will:

- Ask **one question at a time** (never stacks 3 in one message)
- **Offer options** instead of open-ended questions when possible
- **Propose defaults** when you say "I don't know" (never stalls)
- **Respect "just go"** — proceeds with what it has
- **Capture tangents** as follow-up tasks instead of derailing

---

## License

MIT
