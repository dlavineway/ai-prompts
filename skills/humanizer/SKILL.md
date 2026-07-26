---
name: humanizer
description: Rewrite text to remove every detectable sign of AI-generated writing while preserving meaning and facts. Use this skill whenever the user says "humanize", "humanize this", "make this sound human", "remove AI tells", "de-AI this", "does this sound like AI", or pastes text and asks for it to be rewritten so it doesn't read as AI-written. Also use it when the user asks to check text for AI writing patterns before publishing.
---

# Humanizer

Rewrite AI-flavored text into natural human prose. Keep every fact, claim, and intended meaning. Add nothing new. Remove nothing substantive. Change only the wording, structure, and style.

## Core rules

1. Preserve meaning and facts exactly. No new claims, examples, statistics, or interpretations.
2. Do not editorialize, soften, or inflate. If the source says "the tool failed twice," don't turn it into "the tool experienced reliability challenges."
3. Output plain Markdown prose unless the source format demands otherwise.
4. If the input is already clean human writing, say so and make only minimal edits.
5. After rewriting, run the self-check (bottom of this file) before delivering.

## What to remove or rewrite

Work through these categories. They come from documented AI-writing tells (Wikipedia's "Signs of AI writing" field guide and related research).

### 1. Significance inflation
Delete or ground any sentence that puffs up importance: "stands as a testament to", "plays a crucial/pivotal/key role", "underscores its significance", "reflects broader trends", "marking a shift", "setting the stage for", "indelible mark", "evolving landscape", "focal point", "enduring legacy". If the sentence carries a real fact, keep the fact and drop the framing. If it carries nothing, delete it.

### 2. Superficial analysis via participle tails
Kill trailing "-ing" phrases that bolt vague analysis onto a fact: "..., highlighting the company's commitment to innovation", "..., ensuring continued growth", "..., fostering collaboration". Either delete the tail or, if it states something factual, make it its own plain sentence.

### 3. Promotional tone
Strip puffery: "boasts", "vibrant", "rich", "renowned", "groundbreaking", "nestled", "in the heart of", "diverse array", "showcasing", "exemplifies", "commitment to excellence", "natural beauty". Replace with neutral wording or nothing.

### 4. Weasel attribution and overgeneralization
Fix vague authority: "experts argue", "observers note", "industry reports suggest", "widely regarded as". Either name the actual source (only if it's in the input) or state the claim plainly or cut it.

### 5. AI vocabulary
Replace these words with plain alternatives when they appear in figurative or filler use: additionally (sentence-initial), align with, boasts, bolstered, crucial, delve, emphasizing, enduring, enhance, fostering, garner, highlight (verb), interplay, intricate, key (adjective), landscape (abstract), leverage (verb), meticulous, pivotal, robust, seamless, showcase, tapestry, testament, underscore (verb), utilize, valuable, vibrant, vital.

Also banned regardless of context (user preference): ensure, uncover, journey, embark, unleash, dive, discover, plethora, indulge, unlock, unveil, "look no further", "world of", realm, elevate, "whether you're", daunting, "unique blend", blend, "game changer", "stand out", stark, contrast, "more than just", "not just", whether (avoid where possible).

Plain swaps: utilize→use, leverage→use, enhance→improve, ensure→make sure/confirm (or restructure), crucial/vital→important (or cut), additionally→also (or cut), robust→solid/reliable, seamless→smooth (or cut).

### 6. Copula avoidance
Restore simple "is/are/has" constructions. "Serves as the exhibition arm" → "is the exhibition space". "Features four galleries" → "has four galleries". "Functions as", "operates as", "represents a", "refers to" (in definitions) → usually just "is".

### 7. Negative parallelisms
Rewrite "not just X, but Y", "it's not X, it's Y", "X rather than Y" (when used as rhetorical contrast rather than genuine comparison). State the point directly.

### 8. Rule-of-three padding
Break up formulaic triplets ("fast, reliable, and scalable"; three parallel short phrases). Keep the items that matter; vary the rhythm.

### 9. Forced synonym cycling (elegant variation)
If the text rotates through synonyms to avoid repeating a word ("the company... the firm... the organization"), pick one natural term and repeat it. Humans repeat words.

### 10. Structural tells
- Title Case headings → Sentence case headings.
- Excessive boldface, especially bolded lead-ins in bullets ("**Scalability:** the system...") → convert to prose or plain bullets.
- Bullet lists that should be prose → merge into sentences.
- "Challenges" / "Future outlook" / "Conclusion" / "In summary" sections and paragraph-ending restatements → cut or fold into the body.
- Em dashes → replace with commas, periods, colons, or parentheses. Never output an em dash.
- Curly quotes/apostrophes → straight quotes.
- Emoji used as decoration → remove.
- Thematic breaks (---) before headings → remove.

### 11. Chatbot residue
Delete anything addressed to a user rather than a reader: "I hope this helps", "Certainly!", "Would you like...", "It's important to note", "It's worth noting", knowledge-cutoff disclaimers, "while specific details are limited", placeholder text ([Company Name], 2025-xx-xx), and any markup artifacts (oaicite, turn0search, [cite: 1], contentReference, utm_source=chatgpt.com in URLs).

### 12. Hedging and didactic filler
Cut "it is important to remember", "keep in mind", "may vary", and safety-blanket qualifiers that add no information.

## What to add back (signs of human writing)

- Simple is/has phrasing: "there is a", "it has a".
- Plain verbs: wrote (not authored), used (not utilized), tried (not attempted), moved (not relocated), died (not passed away).
- Varied sentence length. Some short. Some longer, with subordinate clauses, the way a person actually drafts.
- Direct statements where warranted: "the first", "the only", when factually true in the source.
- Natural word repetition instead of synonym cycling.
- Occasional wordy human constructions where they read naturally: "as a result of", "in order to", "the fact that". Use sparingly.
- Active voice, direct address where the source genre allows.

## User style defaults

Apply these unless the user says otherwise:
- Direct, personable, casual-professional tone. Not upbeat or exuberant.
- Active voice. Short sentences and paragraphs in conversational text; thorough in documents.
- Sentence case headings. Markdown output. No em dashes ever.
- Minimize adjectives and adverbs.

## Workflow

1. Read the full input first. Identify the genre (email, doc, blog post, Jira ticket, Confluence page) and match its register.
2. Do a tell-scan: mark every instance of categories 1-12 above.
3. Rewrite paragraph by paragraph. Preserve the original structure unless the structure itself is a tell (rigid outline, Challenges/Conclusion sections, bolded bullet walls).
4. Run the self-check.
5. Deliver the rewrite. If the user asks, list what you changed and why, briefly. Otherwise just give the text.

## Self-check before delivering

- Zero em dashes, zero curly quotes, zero emoji.
- Zero words from the banned list in section 5.
- No sentence ends with a vague "-ing" analysis tail.
- No "not just X, but Y" constructions.
- Headings in sentence case.
- Every fact in the output traces to the input. Nothing added, nothing dropped.
- Read it aloud mentally: does any sentence sound like a press release or a chatbot? Fix it.
