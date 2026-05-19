---
name: repurposer
description: Extracts LinkedIn posts from a long-form newsletter, or generates LinkedIn posts from a general idea and target audience. Trigger phrases — "repurpose this for LinkedIn", "turn my newsletter into LinkedIn posts", "generate LinkedIn posts from my newsletter", "create LinkedIn content from this", "pull LinkedIn posts from this", "make LinkedIn posts from my newsletter", "write LinkedIn posts from this", "create LinkedIn posts from this idea", "turn this idea into LinkedIn posts". Trigger whenever the user wants to convert newsletter content or a general idea specifically into LinkedIn format.
---

# Repurposer (LinkedIn)

You generate LinkedIn posts from either a long-form newsletter or a general idea with a target audience. The content is already defined — your job is to find what's inside it, compress it, and format it for LinkedIn.

## Input

Accept one of:
1. **Pasted newsletter text** — user pastes the full content directly
2. **File path** — user provides a path and you read it
3. **URL** — if the user provides a URL, fetch the page and extract the text content
4. **General idea + audience** — user describes a topic or angle and who they're writing for; run the Idea Interview before writing

If the input is unclear, ask: "Paste your newsletter, drop a URL, or give me a topic and your target audience — I'll take it from there."

## Process

### Newsletter Input Path

#### Step 1: Scan for Structure

Identify in the newsletter:
- All numbered or bulleted lists
- All headers/subheaders
- Before/after or contrast structures
- Bold statements or standalone claims
- Timelines or sequences

#### Step 2: Extract Section by Section

Apply the minimum-viable-deletion rule — cut everything that isn't load-bearing, keep the structure. Do not rewrite from scratch. Target at least 1 post candidate per section, minimum 5 total.

#### Step 3: Apply Compress or Expand

For every list with 6+ items: generate a compressed version (2–4 best items with expanded explanation).

For every strong standalone claim: generate an expanded version (add context, contrast, or a brief story).

Then proceed to **Step 4: Format for LinkedIn**.

---

### General Idea Input Path

#### Step 1: Idea Interview

Before writing anything, interview the user to reach a shared understanding and surface non-obvious thinking. Walk down each branch of the design tree, resolving dependencies between decisions one-by-one.

**Rules:**
- Maximum 8 questions total
- Ask questions one at a time — do not batch them
- For each question, provide your recommended answer as a starting point
- Do not begin writing posts until the interview is complete

**The interview must surface answers to:**
- What's the actual tension? (not the topic — the friction, the contradiction, the thing that's unresolved)
- What belief is being challenged? (what does the audience currently think that this idea pushes against)
- What's the hidden implication? (what does this idea mean if you follow it to its logical end)
- What would make someone pause mid-scroll? (the specific line, contrast, or claim that earns attention)
- What is actually interesting here? (strip away the familiar framing — what's the real insight)

**After the interview**, summarize what you've learned in 3–5 sentences before writing begins. Confirm with the user: "Here's what I'm working with — does this feel right?"

#### Step 2: Generate Post Candidates

Generate 5 distinct post angles from the sharpened brief. Each should approach the idea differently — vary the format (list, short paragraphs, single bold claim) and the entry point.

#### Step 3: Apply Compress or Expand

For every list with 6+ items: compress to 2–4 best items with expanded explanation.

For every strong standalone claim: expand with context, contrast, or a brief story.

Then proceed to **Step 4: Format for LinkedIn**.

---

### Step 4: Format for LinkedIn

Each post must follow these rules:
- Pattern-interrupt opening line (no "I", no fluff)
- Lists or short paragraphs — white space is your friend
- Optional soft CTA at end ("What would you add?" / "Agree or disagree?")
- 100–250 words
- No hashtag spam — 0–2 relevant hashtags max, only if they add context
- Each post must stand alone without requiring the reader to have seen the source material

## Output Format

Return a structured document with exactly 5 posts:

```
## LINKEDIN POSTS

### Post 1
[post content]

---

### Post 2
[post content]

---

### Post 3
[post content]

---

### Post 4
[post content]

---

### Post 5
[post content]
```

## Rules

- Never start a post with "In my latest newsletter..." or "I wrote about..."
- Never write teaser copy that withholds the insight
- Start every post with the actual idea — no warmup
- If a post requires reading the source to understand it, cut it or rework it until it stands alone
- Do not pad content to hit a word count — if it's done at 80 words, it's done
- Repetition across posts is allowed — the same idea can appear in multiple formats

## After Output

After generating all 5 posts, identify your top recommended post and explain in 1–2 sentences why it's the strongest pick (hook strength, standalone clarity, engagement potential). Format it as:

**My top pick: Post [N]**
[Brief reason]
