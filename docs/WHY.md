# Why this skill is designed the way it is

Long-form rationale for the four core design decisions. Read this before forking — most "obvious improvements" have been tried and rejected for reasons listed here.

---

## 1. Why 12 sub-levels instead of 6 standard CEFR?

Standard CEFR (A1, A2, B1, B2, C1, C2) has 6 levels. The gap between B1 and B2 is enormous — typically 200-400 hours of study. If a learner struggles at B2 and you drop them back to B1, they'll feel patronized; if you keep them at B2, they'll burn out.

The 12-sub-level grid (A1.1 through C2.2) gives:
- **0.5-step adjustments** — half the gap, half the discouragement
- **Lexile cross-reference** — Lexile is widely used in graded readers, makes material discovery easier
- **Anchor metrics per level** — headword cap, sentence length, clause complexity — these constrain the AI's rewrite to actually hit the level instead of vaguely "writing simpler"

The headword cap is the strongest anchor. AI models have a strong prior toward fancy words; without an explicit cap, they revert to mean.

---

## 2. Why rewrite real articles instead of generate from scratch?

Three reasons:

**A. AI-generated text has a "smell"** — formulaic openings, hedged conclusions, "this highlights the importance of" patterns. A learner reading 30 days of AI-generated articles will internalize this voice instead of authentic English.

**B. Real articles have natural redundancy** — facts get repeated in different forms, names get re-introduced, transitions follow real-world logic. AI generates more efficient prose that's actually harder to parse for learners because there's no slack.

**C. Topic relevance** — the user picks "AI / tech" because they care about it. Real Anthropic blog posts and Hacker News threads are exactly what they want to read in their professional life. The skill is a bridge from "I can almost read this" to "I read this for work".

The downside: rewriting takes more skill (have to *preserve* original facts/voice while *changing* difficulty). But this is exactly what LLMs are good at — controlled paraphrase with constraints.

---

## 3. Why "next session focus" overwrite, not append?

Original design: each session's `feedback.next_session_suggestion` was logged in the reading log file (append-only). Problem: next session's Step 0 only reads the project README, not yesterday's reading log. So the suggestion **was never seen**.

Two fixes considered:
- **Read all reading logs every session** (rejected: O(n) context cost, grows over time)
- **Single overwrite slot in README** (chosen: O(1), stays focused)

The skill writes to a `## Next Session Focus` section in the project README. **Overwrite, not append** — only the most recent focus survives. Older focuses live in `reading-logs/*.md` for later retrieval.

This creates a tight loop:

```
Session N feedback → "Next Session Focus" in README
                         ↓
Session N+1 Step 0.2 reads it
                         ↓
Step B2 prefers articles matching topic suggestion
Step B3 deliberately rewrites in the grammar focus structure 1-2x
Step B4 tells user "🎯 today's focus: X"
                         ↓
User encounters the structure in friendly context, parses correctly
                         ↓
Session N+1 feedback → new focus (or "no new focus")
```

This is **active design of practice**, not passive feedback.

---

## 4. Why inline IPA + TTS audio?

Most vocabulary apps store words as text. Learners write down words → read them → think they "know" them → can't recognize them when listening. Three-point closure (form → sound → meaning) sticks better than two-point (form → meaning).

Implementation choices:
- **edge-tts** (Microsoft neural voices, free) over `say` (sounds robotic) over OpenAI TTS (best quality but costs money + needs key)
- **`en-US-AriaNeural`** voice — neutral US female, clear consonants, matches the US IPA we use in transcriptions
- **Per-word mp3, deduplicated by filename** — same word never gets generated twice; vocabulary file links via Obsidian wikilink
- **`![[word.mp3]]` embed**, not external link — Obsidian renders a mini player inline, point-and-click playback

What was rejected:
- **On-demand TTS** (generate audio when clicked) — adds latency, requires server
- **Inline phonetic spelling** instead of IPA — IPA is universal, phonetic spelling is regional
- **Cambridge / Forvo audio scraping** — URLs change, anti-scraping, less reliable than self-hosting

---

## What we *don't* do (and why)

### No SRS (Spaced Repetition System)

Anki/Memrise-style SRS works for memorizing isolated facts. Reading practice is different — comprehension comes from *re-encountering words in real text*, not from flashcard drills. The vocabulary file is for reference, not active recall.

### No "fill in the blank" / "multiple choice" exercises

These test surface recognition without comprehension. The skill's evaluation method (native-language summary) tests deep comprehension — if you can't summarize, you didn't understand, regardless of how many words you "knew".

### No fixed word lists / textbook progression

The skill follows the user's interests. If they always pick AI articles, they get an AI vocabulary. If they pick novels, they get literary vocabulary. The user's domain knowledge accelerates comprehension; forcing them through generic textbook topics destroys this advantage.

### No gamification / streaks / badges

The README check-in table is one tick per session — that's it. Reading is a long game; gamification creates short-term anxiety that often kills practice. The 30-day target is "≥ 20 check-ins, clear pain reduction" — not "unbroken streak".

---

## Open questions / known limitations

- **Single-language targeting** — currently English-only. The level system would transfer to other languages (Spanish has DELE levels, Chinese has HSK), but each language needs its own anchor data.
- **No listening practice** — TTS audio is per-word, not per-sentence/paragraph. Could be added: generate full-text mp3 alongside the rewrite for shadow-reading practice.
- **No writing practice** — pure input, no output. The English summary in Step B5 is optional and lightly evaluated; a separate writing-coach skill would make sense.
- **Obsidian dependency** — the audio embed `![[]]` is Obsidian-flavored Markdown. Other Markdown editors (Logseq, Bear, Typora) won't render the player. A cross-editor version would use `<audio>` HTML, but loses the inline UI.

---

If you fork this and end up making different decisions, please open a discussion — I'm interested in what doesn't generalize.
