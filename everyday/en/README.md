<div align="center">

> **Version:** v1.2.0 · Stand: Juni 2026 · [Changelog](../../CHANGELOG.md)

# 🧠 The Ultimate Claude Guide — English
### *From zero to expert — understandable for everyone*

</div>


## 📚 Table of Contents

- [🏠 Part 1 — The Perfect Everyday Setup](#-part-1--the-perfect-everyday-setup)
- [🧪 Part 2 — Secret Control Commands](#-part-2--secret-control-commands)
- [✍️ Part 3 — Making Text Sound Human](#️-part-3--making-text-sound-human)
- [🛡️ Part 4 — Preventing Hallucinations](#️-part-4--preventing-hallucinations)
- [📋 Templates](./templates.md)


## 🏠 Part 1 — The Perfect Everyday Setup

### 1.1 Personal Preferences — Claude's Permanent Memory Card

Imagine hiring a new assistant. Every single day you'd have to re-explain who you are and what you need — unless you handed them a reference card they always kept with them.

That reference card is **Personal Preferences** in Claude.

How to find it:
```
Click your initials (bottom left) → "Profile"
→ Fill in the fields ("What should Claude call you?" / "What best describes your work?")
```

Whatever you write there, Claude reads **at the start of every single conversation** — automatically, without you ever mentioning it again.

> 💡 **The key insight:** Write what you want Claude to *stop* doing — not just what to do. That's what makes the real difference.

#### ✅ What belongs there?

| Category | What to write |
|---|---|
| 🧑 Who you are | Job, background, experience level |
| 🎯 Your goal | What you mostly use Claude for |
| 📝 Your style | Formal, casual, brief, detailed |
| 🚫 What you hate | Filler phrases, long intros, bullet overload |
| 🌍 Language | Always respond in English |

→ **Ready-to-copy templates:** [templates.md](./templates.md)


### 1.2 Projects — Separate Workspaces for Everything

Projects are like **separate drawers** on your desk. Each drawer has its own label, its own documents, and its own memory.

What projects are good for:

```
📁 Work          → documents, company context, colleague names
📁 Home          → recipes, shopping lists, budget spreadsheets
📁 Hobby         → project descriptions, research, notes
📁 Learning      → study materials, notes, exam prep
📁 Writing       → style examples, characters, plot notes
```

How to create a project:
```
claude.ai → left sidebar → "New Project"
→ Enter name → Write "Project Instructions"
→ Upload documents (PDF, Word, Excel up to 30 MB)
```

The trick with project instructions:  
Write briefly and precisely what Claude should always know in THIS project. Task-specific instructions go in the actual chat message.

> 💡 **Pro tip:** Upload important documents once to a project (your resume, company style guide, a lease agreement) — Claude knows them in every conversation within that project, without you uploading them each time.


### 1.3 Skills — Claude's Reusable Mini-Assistants

Skills (available from Pro plan) are like **recipes** you write once and reuse forever.

**Examples of useful skills:**

| Skill Name | What it does |
|---|---|
| 📧 Email Writer | Writes emails in your personal style |
| 🔍 Fact Checker | Verifies sources, flags uncertain claims |
| ✏️ Humanizer | Makes AI text sound more human (more in Part 3) |
| 📊 Meeting Notes | Converts rough notes into structured minutes |

How to create a skill:
```
claude.ai → Settings → Skills → "New Skill"
→ Enter name, description, and instructions
→ Optional: upload example files
```


### 1.4 Connectors — Claude Talking to the Outside World

Claude can connect to other apps and pull information or take actions there.

**Available connectors (selection):**

| Connector | What Claude can then do |
|---|---|
| 🔍 Web Search | Pull current information from the internet |
| 📁 Google Drive | Read and summarize your documents |
| 📝 Google Docs | Write directly into documents |
| 📅 Google Calendar | Read and create appointments |
| 💬 Slack | Read and write messages |
| 📓 Notion | Retrieve and update notes |

How to activate connectors:
```
claude.ai → Settings → Connectors (or "Connections")
→ Select desired service → Connect → Allow
```

> ⚠️ **Important:** Claude will ask for your approval the first time it uses a connector. That's intentional — you stay in control.


## 🧪 Part 2 — Secret Control Commands

### 2.1 XML Tags — Claude's Secret Language

This is the **most powerful trick** of all, and it's as simple as using headings in a Word document.

XML tags are words in angle brackets that tell Claude what role each part plays.  
Words in angle brackets that tell Claude: *"Hey, this part has a specific role."*

```xml
<role>Write this here</role>
<task>What Claude should do</task>
<context>The background information</context>
```

Why it works:  
Claude was trained on millions of XML documents. It "understands" this structure at a deeper level than plain text — similar to how a human immediately knows a numbered list means separate items.

With and without tags — the difference:

❌ **Without tags** — Claude guesses what you mean:
```
You are a marketing expert. Analyze this text: [product description] 
and then write an email campaign about it.
```

✅ **With tags** — Claude knows exactly what's what:
```xml
<role>You are an experienced marketing expert for B2B software.</role>

<context>
We're launching a new project management tool for small teams.
Target audience: freelancers and small agencies.
</context>

<document>
[Insert product description here]
</document>

<task>
1. Analyze the strengths and weaknesses of the product description.
2. Then write a 3-part email campaign (subject + body for each).
</task>

<output_format>
Analysis: 3-5 bullet points
Emails: Numbered, with "Subject:" and "Body:" separated
</output_format>
```

Anthropic measured it: Structured prompts with XML tags deliver measurably more consistent results (Anthropic documents up to 40% hallucination reduction in internal tests) than unstructured text.

#### 🏷️ The 8 Most Important Tags for Everyday Use

| Tag | What it's for | Example |
|---|---|---|
| `<role>` | Who Claude is | `<role>Tax attorney with 15 years experience</role>` |
| `<context>` | Background info | `<context>I'm a freelancer, tax year 2025</context>` |
| `<task>` | What Claude should do | `<task>Explain which expenses I can deduct</task>` |
| `<instructions>` | Rules and constraints | `<instructions>Only US tax law. No speculation.</instructions>` |
| `<document>` | Inserted text/content | `<document>Here's the letter from the IRS...</document>` |
| `<examples>` | Sample responses | `<examples>Here's how the answer should look: ...</examples>` |
| `<thinking>` | Let Claude think out loud | `<thinking>Show me your reasoning process</thinking>` |
| `<output_format>` | How the answer should look | `<output_format>Table with 3 columns: Expense, Amount, Reason</output_format>` |


### 2.2 Forcing Deep Thinking

Claude thinks fast by default. For complex tasks, you want it to work slower and more thoroughly. These three techniques work immediately.

#### Level 1 — The Quick Trick (1 sentence)
Just append to any complex prompt:
```
Think step by step before answering.
```

#### Level 2 — Guide the Reasoning Path
```xml
<task>
I'm considering whether to buy or continue renting my apartment.
Monthly rent: $1,500. Purchase price: $450,000. Down payment available: $75,000.
</task>

<instructions>
Analyze in this order:
1. Calculate monthly mortgage payment (assume 6.5% rate).
2. Compare rent vs. buy costs over 10 and 20 years.
3. List the 3 most important factors favoring renting.
4. List the 3 most important factors favoring buying.
5. Then give your recommendation.
Only conclude after step 5.
</instructions>
```

#### Level 3 — Separate Thinking from Answer (for maximum clarity)
```xml
<instructions>
Show your reasoning in <thinking> tags.
Write the final answer for me in <answer> tags.
I only read the <answer>.
</instructions>
```

Claude then responds like this:
```xml
<thinking>
Okay, I need to first consider... The purchase price of $450,000 minus
$75,000 down = $375,000 mortgage. At 6.5% over 30 years that's
approximately $2,370/month...
</thinking>

<answer>
Based on your numbers:
**Monthly mortgage:** ~$2,370
**Extra cost vs. renting:** $870/month
...
</answer>
```

> 💡 **When is Level 3 worth it?** Whenever you want to copy the answer directly or show it to someone — without Claude's deliberations mixed in.


### 2.3 Controlling Response Format

Tell Claude exactly how you want the answer structured using `<output_format>`:

```xml
<output_format>
Start your answer directly with: "The three key points are:"
No introduction. No summary at the end.
Use emojis as bullet markers (✅, ⚠️, ❌).
Maximum 5 sentences per point.
</output_format>
```

More format commands that work:

| What you want | What you write |
|---|---|
| JSON format | `Respond exclusively in JSON format. No text before or after.` |
| Table | `Create a markdown table with columns: Name, Pros and Cons, Recommendation.` |
| Short and sharp | `Maximum 3 sentences. No bullet points. Direct answer.` |
| Step-by-step | `Numbered list. Each step: max 2 sentences. Start with step one.` |
| Email format | `Write a complete email. Subject: [...]. No placeholders, no square brackets.` |

> ⚠️ **Note on prefilling:** Some older guides suggest "prefilling" Claude's response (writing the first word of its answer yourself). This technique no longer works on current Claude models — it returns an error. The format control method above is the current best practice and works better anyway.


### 2.4 The Role Trick — Turning Claude Into an Expert

Assigning a concrete role is one of the most powerful tricks of all.

Vague role — barely any difference:
```
You are an expert. Help me with taxes.
```

Precise role — noticeable difference:
```xml
<role>
You are a certified public accountant with 20 years of experience in the US,
specializing in freelancers and self-employed professionals in the creative industry.
You explain tax law so that non-lawyers immediately understand it.
You explicitly flag when something enters territory where a professional should be consulted.
</role>
```

Why it works: Claude learned from the writing of millions of experts. A precise role activates the right patterns — vocabulary, caution level, depth of explanation.


### 2.5 Examples — The Underrated Superpower

Show Claude **3 examples** of what an answer should look like — and quality rises measurably.

```xml
<examples>
  <example>
    <input>Product: Bamboo coffee mug</input>
    <output>Your morning coffee, reimagined. Sustainable, lightweight, dishwasher-safe — the mug you'll still be using in retirement.</output>
  </example>

  <example>
    <input>Product: Running shoes for women</input>
    <output>Built for your rhythm. Not for photos. For miles.</output>
  </example>

  <example>
    <input>Product: Standing desk</input>
    <output>Eight hours sitting is so last year. Your back already voted.</output>
  </example>
</examples>

<task>Now write a tagline for: Noise-canceling headphones for commuters</task>
```


## ✍️ Part 3 — Making Text Sound Human

### 3.1 Why AI Text Sounds Like AI Text

Claude writes statistically. It always picks the most common words — which produces text that's correct but predictable. Like a school essay that checks every box but excites nobody.

The result: All Claude texts sound similar. AI detectors spot this. Humans do too — they sense it without knowing why.

The fix: Tell Claude what NOT to do. That's more effective than telling it what to do.


### 3.2 The Banned List — Eliminating AI Phrases

Add this list directly to your prompt or Personal Preferences:

```
Avoid the following phrases and patterns completely:

BANNED PHRASES:
- "plays a crucial role"
- "serves as a testament to"
- "It is important to note that"
- "In conclusion" / "To summarize"
- "In today's fast-paced world"
- "Certainly!" / "Of course!" / "Absolutely!"
- "I hope this helps"
- "If you have any further questions"
- "continues to captivate"
- "underscores the importance"
- "rich cultural heritage"
- "groundbreaking" / "revolutionary"
- "essential" / "multifaceted"

BANNED STRUCTURES:
- Two-word headers with colons ("Challenges and Opportunities:")
- Bullet lists at the start without being asked
- Bold text everywhere without real purpose
- Horizontal rules (---) between every section
- Letter openings like "Dear valued reader"

BANNED FORMATTING:
- Equal-length paragraphs
- Consistent sentence length throughout
- Always the same sentence start pattern (Subject + Verb)
```


### 3.3 The Humanizer Prompt — The All-Rounder

Write the text with Claude first, then run it through this prompt:

```xml
<role>
You are an experienced editor who specializes in removing AI patterns from text
to make writing sound natural and human.
Your job is not to improve the text — but to humanize it.
</role>

<task>
Rewrite the following text so it sounds like it was written by a real person —
with natural imperfections, varying sentence lengths, and genuine personality.
Preserve the core message completely.
</task>

<instructions>
REMOVE:
- All AI stock phrases (see banned list above)
- Excessive bold text
- Artificially two-part headings
- Even paragraph lengths
- Consistent sentence structure

ADD:
- Varying sentence lengths (sometimes 3 words, sometimes 30)
- An occasional tangent or aside
- Natural tone that fits the intended audience
- Real opinions instead of balanced neutrality

CHECK AT THE END:
Ask yourself: "What still makes this text obviously AI-generated?"
Answer briefly and then fix it.
</instructions>

<document>
[INSERT YOUR TEXT HERE]
</document>
```


### 3.4 Voice Matching — Claude Learning Your Style

The strongest humanizer trick: Give Claude a sample of your own writing.

```xml
<role>
You are an editor who analyzes and imitates my writing style.
</role>

<my_writing_style>
[Insert 2-3 paragraphs of your own writing here — an old email, 
a social media post, a message, anything you've already written]
</my_writing_style>

<task>
First analyze my style briefly (sentence length, tone, quirks).
Then rewrite this text in my style:
</task>

<text_to_rewrite>
[The AI text you want to humanize]
</text_to_rewrite>
```


### 3.5 Structural Anti-AI Patterns

These writing instructions immediately produce less detectable AI text:

```
VARY sentence lengths:
- Sometimes 3-4 words. Period.
- Sometimes a longer sentence with multiple clauses that stretches across half a line,
  because some thoughts just need space to breathe.
- Then short again.

VARY sentence starts:
- Not always: "Subject + Verb"
- Sometimes: "And then..."
- Sometimes: "Actually..."
- Sometimes start with a question.
- Sometimes jump straight into the middle of a thought.

NO EVEN PARAGRAPHS:
- Sometimes one sentence.
- Sometimes seven.
- No pattern — like real people write.
```


## 🛡️ Part 4 — Preventing Hallucinations

### 4.1 Why Claude Sometimes Makes Things Up

Claude doesn't have a search engine. It "remembers" patterns from training — and when it's unsure, it sometimes fills gaps with plausible-sounding information. This is called hallucination.

Watch out especially with:
- 📚 Sources, quotes, books, studies (these get fabricated!)
- 📅 Events after Anfang–Mitte 2025 (ältere Modelle) bis Januar 2026 (Opus 4.7) (knowledge cutoff)
- 🔢 Precise statistics and numbers
- 👤 Biographical details about lesser-known people


### 4.2 The Hallucination Protection Formulas

Formula 1 — demand uncertainty signals:
```
If you're not 100% certain about a claim, 
write "[UNVERIFIED]" right next to it.
Don't invent sources. If you don't know a source: say so.
```

Formula 2 — explicitly ban sources:
```xml
<instructions>
Do NOT name any books, studies, URLs, or person quotes
unless I have provided them in this conversation.
If you need an example, create a fictional one and mark it as "[EXAMPLE]".
</instructions>
```

Formula 3 — force recency awareness:
```
If this information might be outdated after Anfang–Mitte 2025 (ältere Modelle) bis Januar 2026 (Opus 4.7):
Explicitly tell me and recommend where I should check.
```

Formula 4 — build in self-verification:
```xml
<instructions>
Write your answer.
Then check yourself: Are there any claims you're uncertain about?
Mark those with ⚠️ and briefly explain why.
</instructions>
```


### 4.3 Activating the Fact-Checker Mode

When you want to fact-check a piece of text:

```xml
<role>
Professional fact-checker with journalistic training.
</role>

<task>
Analyze the following text sentence by sentence for factual claims.
</task>

<instructions>
For each claim:
- ✅ LIKELY CORRECT: If you know this from training with high confidence
- ⚠️ VERIFY: If you're unsure or it might be outdated → recommend a source
- ❌ WRONG: If you know it's incorrect → explain why

YOUR OWN RULES:
- Don't invent counter-claims
- If you recommend a source, make it a real one (Wikipedia / official government sites / etc.)
- No fabricated statistics, even as placeholders
</instructions>

<document>
[Insert the text to be fact-checked here]
</document>
```


<div align="center">

## 🎯 At a Glance — The Most Important Rules

| # | Rule | Why |
|---|---|---|
| 1 | Fill in Personal Preferences | Claude then knows you permanently |
| 2 | XML tags for complex prompts | measurably better results *(community experience value)* |
| 3 | Banned list for human-sounding style | AI patterns get eliminated |
| 4 | Never use sources without checking | Hallucination protection |
| 5 | Enable web search for current topics | Facts instead of guesses |

</div>


*→ [Templates & Prompts to Copy](./templates.md)*  
*← [Back to main page](../README.md)*

---

> **⚖️ Legal Notices**
>
> **Trademark disclaimer:** This guide is independently created and has no affiliation with Anthropic PBC. Claude™ is a trademark of Anthropic. All other trademarks belong to their respective owners.
>
> **Privacy:** Consumer users (free / Pro) should be aware that Claude conversations may be used for model improvement by default. Since the policy change in August 2025, an active opt-out in Settings is required. "Confidential" in the project context does not automatically mean Anthropic has no access — Enterprise plans offer stronger guarantees.
>
> **Humanizer & AI fact-check:** AI-generated fact-checks do not replace professional verification. Using humanizers in academic contexts may violate institutional or exam policies — users are solely responsible.
