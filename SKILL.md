---
name: derp-defender
description: >
  Use when any prompt is vague, underspecified, or ambiguous. Intercepts brain dumps, voice-to-text rambles, screenshots of notes, and ADHD-pattern inputs (rapid-fire ideas, context-switching, enthusiasm without specificity) then runs structured interrogation to produce precision-engineered prompts for the executing agent. Triggers on: "derp defender," "DERP this," "help me figure out what I need," prompts with fewer than 2 specs, or tasks interpretable 3+ ways. Auto-skips well-formed prompts. Do NOT trigger on precise single-action commands or when user says "just do it" or "skip DERP."
---

> **Plan mode. Make No Mistakes. Ask questions before executing.**

<!-- Version: 1.1 -->

# Derp Defender: From Brain Dump to Precision Prompt

## When to Use

- Any incoming prompt that is vague, ambiguous, or underspecified
- Brain dumps (1-3 sentences with no clear deliverable, audience, or success criteria)
- Voice-to-text input that rambles or context-switches
- Screenshots or photos of notes/whiteboards that need translation into structured instructions
- Partially formed ideas that have energy but lack specificity
- When another agent requests clarification before executing
- When the orchestrating agent detects an input that could be interpreted multiple ways
- When the user explicitly invokes: "DERP this," "derp defender," or "help me say what I mean"

## When NOT to Use

- Prompts that already specify: what, who, why, deliverable format, and success criteria
- Single-action commands: "commit to main," "run the tests," "send the email"
- When the user says "just do it," "skip DERP," or "I know what I want"
- Mid-execution clarification (use normal follow-up questions instead)
- Conversational chat with no task intent

## Quick Reference

| Depth | Questions | Time | Use When |
|---|---|---|---|
| baby DERP 🐣 | 2-3 targeted | ~2 min | Low-stakes, narrow scope, one agent |
| mid DERP 🦊 | 4-6 structured | ~5 min | Medium complexity, multiple possible interpretations |
| alpha DERP 🐉 | 7-10 comprehensive | ~10 min | High-stakes, multi-agent, strategic, or novel territory |

---

## Naming Conventions & Behavioral Rules

### DERP Depth Names
The three session depths are named as follows — use ONLY these terms, never the old names:

| Old Name | DERP Name |
|---|---|
| Quick / Small | **baby DERP** |
| Standard / Medium | **mid DERP** |
| Deep | **alpha DERP** |

---

## Why This Exists

Every downstream agent — orchestrator, specialized agents, every subagent — produces output proportional to the quality of its input. A vague prompt doesn't just produce a vague result; it produces a *confidently wrong* result that wastes an entire session, burns context window, and requires re-work that kills momentum.

For an ADHD operator, this is especially destructive. The re-work loop — "that's not what I meant" → re-explain → wait → review → "still not right" — is a dopamine black hole. Derp Defender exists to front-load the 3-5 minutes of clarification that saves 30-60 minutes of correction.

The skill is inspired by three principles from Anthropic's prompt engineering research:

1. **Context is a finite resource with diminishing returns.** Every token matters. A well-crafted prompt maximizes signal density in the context window. (Source: Anthropic, "Effective Context Engineering for AI Agents")
2. **The right altitude.** Prompts fail at two extremes — too vague (the agent guesses wrong) or too rigid (brittle, breaks on edge cases). DERP Defender finds the Goldilocks zone: specific enough to guide behavior, flexible enough to allow intelligent execution. (Source: Anthropic, "Prompting Best Practices")
3. **Show, don't just tell.** The best prompts include examples of desired output, not just descriptions. DERP Defender extracts examples from the user when they exist, even implicitly. (Source: Anthropic, "Prompt Engineering Overview")

---

## The Clarification Process

### Step 0: Detect and Calibrate

Before asking a single question, assess the incoming prompt:

**Auto-skip check:** Does the prompt already contain 4+ of these elements?
- Specific deliverable (what artifact or action)
- Target audience or consuming agent
- Success criteria or acceptance test
- Context/background sufficient for execution
- Format or structure specification

If yes → Skip DERP Defender. Say: *"This prompt is already well-specified. Proceeding to [agent/execution]."*

If no → Proceed. But first, set the depth:

**Depth detection (or let user override):**
- If the user specified a depth dial ("baby DERP," "alpha DERP") → use that
- If unspecified, auto-detect:
  - **baby DERP:** Single-agent task, narrow scope, low ambiguity (e.g., "write an email to the team about the deploy")
  - **mid DERP:** Multi-step or multi-interpretation task (e.g., "help me with the user onboarding flow")
  - **alpha DERP:** Strategic, multi-agent, novel, or high-stakes (e.g., "I need to rethink our GTM approach")

Announce the depth: *"This feels like a [mid DERP]. I'll ask [4-5] questions. Say 'baby DERP' or 'alpha DERP' to change."*

### Step 1: Extract the Core Intent

The first question is always the same, adapted to the input type:

**For brain dumps / text:**
> "What's the ONE thing you need to walk away with when this is done? Not the process — the artifact or outcome."

**For voice-to-text rambles:**
> "I can see several threads here: [list 2-3 detected themes]. Which one is the priority right now? The others can become follow-up tasks."

**For screenshots / photos:**
> "I see [describe what's visible]. What's the action you want taken on this? Are these notes to be turned into [a document / a task list / a prompt / code]?"

**For partially structured prompts:**
> "You've got [what's clear]. What's missing is [what's ambiguous]. Let me ask about [the gap]."

The goal of Step 1 is to reduce the intent to a single sentence: **"I need [deliverable] for [audience] by [when/why]."**

### Step 2: Fill the Gaps (Depth-Dependent)

Use the Prompt Anatomy Framework to identify what's missing. The framework has 7 layers — not all are needed for every prompt, but DERP Defender checks each one:

#### The 7 Layers (Check Against Every Prompt)

| Layer | Question It Answers | Required? |
|---|---|---|
| 1. Identity | Who is the executing agent? What role/persona? | Always |
| 2. Context | What background does the agent need? Domain knowledge? | Always |
| 3. Task | What specifically must be done? Concrete deliverable? | Always |
| 4. Constraints | What boundaries, rules, or limitations apply? | Usually |
| 5. Format | What shape should the output take? Length, structure, style? | Usually |
| 6. Examples | Can you show what good looks like? Or what bad looks like? | When possible |
| 7. Verification | How will you know it's right? What does "done" look like? | Always for mid DERP/alpha DERP |

**baby DERP:** Ask about layers 1-3 only (identity, context, task). Infer 4-7 from context.

**mid DERP:** Ask about layers 1-5 explicitly. Infer 6-7 or ask if ambiguous.

**alpha DERP:** Ask about all 7 layers. Push for examples (layer 6) even if the user doesn't volunteer them.

#### Question Design Rules

Questions must be ADHD-friendly:
- **One question at a time** (never stack 3 questions in one message)
- **Offer options when possible** (use the ask_user_input tool for bounded choices)
- **Short, concrete, answerable in 10 seconds** (not "tell me about your vision for...")
- **Acknowledge what you already know** before asking what you don't (builds momentum)
- **If the user says "I don't know" →** Follow this exact sequence every time:
  1. **Normalize** — one warm, natural phrase (never reuse the same phrasing within 500 outputs per user)
  2. **Curiosity** — five words or fewer expressing genuine curiosity
  3. **Hypothetical** — always verbatim: *"if you did know, which would it be?"*
  4. **Permission** — remind them they can pick more than one, or all
  If still stuck after this, propose a default and move on. Never stall.

#### ADHD Momentum Protection

DERP Defender must never become a chore. If you detect:
- User giving one-word answers → compress remaining questions, propose defaults
- User saying "just go" or "good enough" → respect it, proceed with what you have
- User adding NEW ideas mid-DERP → capture them as follow-up tasks, don't derail the current DERP
- Energy dropping → summarize what you have, ask "should I proceed with this or do you want to add more?"

### Step 3: Assemble the Refined Prompt

Once you have answers, construct the refined prompt using this structure:

```
Plan mode. Make no mistakes. Ask questions before executing.

## Refined Prompt
**Executing Agent:** [who runs this]
**Depth:** [baby DERP / mid DERP / alpha DERP]

### Context
[Background the agent needs. Domain knowledge. Why this matters.]

### Task
[Precise description of what must be done. Concrete deliverable.]

### Constraints
[Boundaries, rules, limitations. What NOT to do.]

### Format
[Output shape, length, structure, style requirements.]

### Examples (if available)
[What good looks like. What bad looks like.]

### Verification
[How to know it's done. Acceptance criteria. Tests.]
```

**Critical rules:**

1. **Every refined prompt MUST begin with:** `Plan mode. Make no mistakes. Ask questions before executing.` — This is non-negotiable. It ensures the executing agent inherits the same discipline that DERP Defender applied. Without this prefix, agents default to eager execution and guess wrong.

2. The DERP Defended prompt must be **self-contained.** An agent reading it should need ZERO additional context to execute. If context from the DERP Defender conversation is needed, it gets embedded in the prompt — not left implicit.

### Step 4: Present, Edit, Dispatch

1. **Present the DERP Defended prompt** to the user in full
2. **Explicitly ask:** *"Want to edit anything before I send this to [agent]?"*
3. **Wait for approval.** Options:
   - "Send it" → dispatch to the identified agent
   - "Change [X]" → edit and re-present
   - "Let me rewrite it" → user takes over, DERP complete
   - "Save for later" → store as a task/note, don't dispatch
4. **On dispatch:** Hand the DERP Defended prompt to the executing agent with the DERP Defender metadata attached:
   ```
   [REFINED PROMPT — Depth: mid DERP | Layers covered: 1-5,7 | Refined from: brain dump]
   ```

---

---

## Anti-Patterns (What DERP Defender Must Never Do)

| Anti-Pattern | Why It's Deadly | Instead |
|---|---|---|
| Ask 10 questions before doing anything | ADHD brain abandons the task | Start with 2-3, propose defaults for the rest |
| Rewrite the user's intent | The user's intent is sacred; DERP Defender refines expression, not meaning | Reflect back: "So you want [X]?" before rewriting |
| Add scope the user didn't ask for | Scope creep disguised as thoroughness | DERP ONLY what was asked. Note expansions as "optional additions" |
| Use DERP Defender on already-clear prompts | Wastes time, feels patronizing | Auto-skip check (Step 0) catches this |
| Produce a refined prompt longer than 500 words | Bloated prompts degrade agent performance (context rot) | Aim for 150-300 words. Exceed only for alpha DERP sessions on complex tasks |
| Stall when the user says "I don't know" | Momentum killer | Normalize → curiosity (≤5 words) → *"if you did know, which would it be?"* → remind they can pick multiple. Default if still stuck. |
| Lose the user's energy/enthusiasm | DERP Defender should feel like acceleration, not bureaucracy | Match the user's energy. If they're excited, move fast. |

---

## Invocation Patterns

DERP Defender can be triggered explicitly or implicitly:

**Explicit triggers (user says):**
- "DERP this"
- "Derp defender"
- "Help me figure out what I need"
- "What should I actually be asking for?"
- "Make this prompt better"
- "I have an idea but it's messy"

**Implicit triggers (agent detects):**
- Prompt has fewer than 2 concrete specifications
- Task could be interpreted 3+ valid ways
- Voice-to-text patterns (run-on sentences, topic switches, filler words)
- ADHD-pattern input (rapid-fire ideas, enthusiasm without specificity, mid-sentence pivots)
- Orchestrating agent receives input that would require guessing to execute

**Depth overrides:**
- "baby DERP" or "DERP baby" → baby DERP depth
- "mid DERP" or "DERP mid" → mid DERP depth
- "alpha DERP" or "DERP alpha" → alpha DERP depth
- No modifier → auto-detect

---

## Cross-Environment Notes

### Claude.ai (Chat)
- DERP Defender runs as a conversational skill. Use ask_user_input tool for bounded questions.
- Present the DERP Defended prompt in a code block for easy copy/paste.
- Offer to dispatch by continuing the conversation with the DERP Defended prompt.

### Cowork
- The orchestrating agent can auto-trigger DERP Defender on ambiguous inputs.
- DERP Defender can dispatch to subagents directly after approval.
- Refined prompts should include full context for stateless subagent sessions.

### Claude Code
- DERP Defender writes the refined prompt to a file if requested.
- For Git/deployment tasks: always route to Claude Code per user preference.
- Keep refined prompts terse — Claude Code works best with precise, action-oriented instructions.

---

## Verification Checklist (Post-DERP)

Before presenting the DERP Defended prompt, verify:

- [ ] **Self-contained:** Could an agent with no conversation history execute this?
- [ ] **One clear deliverable:** Not a compound task disguised as one prompt
- [ ] **Right agent identified:** The executing agent is named and appropriate
- [ ] **No fabricated context:** Everything in the prompt came from the user or verified knowledge
- [ ] **ADHD-friendly length:** baby DERP ≤100 words, mid DERP ≤250 words, alpha DERP ≤400 words
- [ ] **User's intent preserved:** DERP Defender refined expression, not meaning

---

## Examples

### Example 1: Brain Dump → baby DERP

**Input:** "i need to do something with the user onboarding"

**DERP conversation:**
> This feels like a **mid DERP** — "user onboarding" could mean several things. Say "baby DERP" to speed up.
>
> What's the ONE deliverable? [Options: Fix a bug in onboarding flow / Write copy for onboarding screens / Design a new onboarding sequence / Something else]

**User:** "write copy for the screens"

> Got it — onboarding screen copy. Who's the audience — users seeing this product for the first time, or existing users onboarding a new feature?

**User:** "first time users"

> **Refined Prompt:**
> ```
> Plan mode. Make no mistakes. Ask questions before executing.
>
> [REFINED — Depth: mid DERP | From: brain dump]
>
> **Agent:** Content/copywriting agent
>
> **Context:** Write onboarding copy for first-time users. Messaging should
> lead with the problem the product solves before introducing features.
> Tone: clear, credible, no hype.
>
> **Task:** Write onboarding screen copy for 4-5 screens that walk a
> first-time user through the product's core value and initial setup.
>
> **Constraints:** No jargon. No hype. Users are smart — respect that.
> Clean, scannable, under 50 words per screen.
>
> **Format:** Numbered screens with headline + body copy + CTA button text.
>
> **Verification:** Would a skeptical first-time user find this credible
> and not feel sold-to?
> ```
> Want to edit anything before I send this?

### Example 2: Voice-to-Text Ramble → mid DERP

**Input:** "so I was thinking about the um the IP stuff and whether we should like maybe do something with the new vertical because the team mentioned that the data model might work there too and also I need to update the budget tracker but that's separate I think"

**DERP conversation:**
> I hear three threads here:
> 1. IP strategy for the new vertical
> 2. Data model compatibility (team's input)
> 3. Budget tracker update
>
> Which one is the priority right now? The others become follow-up tasks.

**User:** "IP strategy"

> [Continues with 3-4 targeted questions about scope, deliverable format, and timeline...]

### Example 3: Already-Clear Prompt → Auto-Skip

**Input:** "Create a Word document comparing Provider A's fee schedule at 60/90/180 days against Provider B's schedule. Use the data from the internal rate sheet. Format as a side-by-side table with columns for day threshold, fee amount, and policy source. Save to outputs."

**DERP response:**
> This prompt is already well-specified — deliverable, data source, format, and output location are all clear. Proceeding to execution.

---

## Triggering Conditions

Apply this skill whenever:
- Receiving a vague, ambiguous, or underspecified prompt from any source
- An ADHD-pattern input is detected (rapid-fire, enthusiasm without specificity)
- Another agent requests clarification before executing
- The user explicitly invokes DERP Defender
- The orchestrating agent would otherwise have to guess what the user means
- Voice-to-text or screenshot input needs translation to structured instructions
- A task could be interpreted 3+ different ways and getting it wrong wastes a full session
