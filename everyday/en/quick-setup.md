# ⚡ Quick Setup — Ready in 30 Minutes

Three steps that separate "nice chatbot" from "actual assistant". No technical knowledge required.

## Step 1 — Download & Register Claude

**Browser (no install needed):**
Go to [claude.ai](https://claude.ai) and create a free account with email or Google login.

**Desktop app (recommended):**
```
claude.ai → click your profile picture (top right) → "Download Desktop App"
→ install .dmg (Mac) or .exe (Windows) → log in
```

**Mobile:**
Search for the Claude app in the App Store (iOS) or Play Store (Android) → install → log in.

**Which plan?**

| Plan | Price | Who it's for |
|---|---|---|
| Free | $0 | Trying it out, occasional use |
| Pro | ~$17–20/month | Daily use, more messages |
| Max | ~$100–200/month | Heavy use, highest limits |
| Team | $20/seat/month | 3+ people with shared projects |

> 💡 Free is fine to start. Upgrade to Pro when you keep hitting the message limit regularly.

## Step 2 — Teach Claude Who You Are

This is the most important step. Without it, Claude guesses who you are and how you work every single conversation. After this step, it knows.

**Where to do it:**
```
Click your initials (bottom left)
→ Open "Profile"
→ Fill in the fields: name, job, working style
```

These are Claude's "permanent memory" — they apply to all conversations on your account.

**But that's not enough.** For truly precise preferences, use the onboarding prompt below. Start a new conversation, paste it in, and Claude will ask you 50 targeted questions about yourself — one at a time.

### 🎯 The Onboarding Prompt (copy as-is)

```
Ask me exactly one question at a time — 50 questions total.
For each question, offer 3–5 answer options with checkboxes where possible,
plus an open-ended option.

Cover these areas:
- Who I am (job, background, experience level)
- What I do (main tasks, typical projects, goals)
- How I work (style, pace, preferences, tools)
- What I hate in outputs (filler phrases, formats, length, tone)
- What great work looks like to me (concrete examples, success criteria)

If my answer is vague or generic:
Push back and ask for a concrete example or a specific situation.

When you've asked all 50 questions:
Compile everything into a single, cleanly structured .md file
that I can paste directly into my Claude settings or project instructions.
No intro, no summary — just the finished config file.
```

When Claude is done, you get a `.md` file with your personal profile. You can then:
- Paste it into the profile fields (Step above)
- Use it as a project instruction (→ Step 3)
- Attach it as context in any new conversation

## Step 3 — Set Up Projects & Custom Instructions

Projects are separate workspaces. Each project has its own memory, its own documents, and its own instructions.

**Creating a project:**
```
Left sidebar → "New Project" → give it a name
→ Write "Project Instructions"
→ Upload documents (optional)
```

**What to keep in mind:**

Project instructions should be short and precise — half a page maximum. Everything Claude should permanently know in this project: context, tone, constraints. Task-specific instructions go in the actual message, not here.

A common mistake: putting everything into project instructions, which quickly fills the context budget and leaves Claude less room for the actual task.

**What belongs in project instructions:**

```
I am a [role] at [company/context].
Target audience for my texts: [description].
Tone: [formal / casual / technical].
Always respond in English.
No filler phrases like "Certainly!" or "I hope this helps".
Documents in this project are confidential — don't quote from them.
```

**What does NOT belong in project instructions:**
- The actual task ("Write me a blog post about...") → goes in the message
- Highly specific formatting rules for individual tasks → goes in the message
- Long background texts → better uploaded as a document

**Free plan limit:** Maximum 5 projects. Pro and above: unlimited.

**Recommended base structure:**

```
📁 Work          → company, colleagues, products, tone of voice
📁 Personal      → hobbies, home, personal goals
📁 Learning      → current topic / course / exam
📁 Writing       → blog, book, newsletter — with style examples
```

> 💡 Upload important documents once to a project — resume, style guide, contracts. Claude knows them in every conversation within that project without re-uploading.

**Context window in projects:**

| Plan | Context in chat |
|---|---|
| All paid plans | 500K tokens with Opus 4.6+ / Sonnet 4.6 |
| Claude Code | 1M tokens (credits required) |

> ⚠️ Known bug: RAG mode (automatic document search) sometimes activates as early as ~13 files, even when plenty of context space remains. Retrieval quality can vary with many small files.

← [Back to Everyday Guide](./README.md)
← [Back to home](../../README.md)
