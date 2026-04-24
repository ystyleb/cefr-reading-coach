# CEFR Reading Coach

> A Claude Code skill that turns your daily 15 minutes into a personalized English reading practice — at exactly the right difficulty.

Most reading apps either bore you (too easy) or burn you out (too hard). This coach pulls **real recent articles** (Anthropic blog, Hacker News, books, fiction — your pick), **rewrites them to your CEFR level**, and runs a tight feedback loop with you. It calibrates your level on first run and adjusts based on your comprehension over time.

## Why another reading tool?

Four design decisions that I haven't seen combined elsewhere:

1. **12 CEFR sub-levels (A1.1 → C2.2), not 6** — adjusts in 0.5-step increments, so progress feels gradual instead of jumping cliffs.
2. **Rewrite, don't generate from scratch** — preserves the original article's structure and voice. AI-generated text always feels formulaic; rewriting keeps the *real* texture.
3. **Overwrite-style "next session focus"** — every session leaves one sentence about what tripped you up, and the next session **deliberately rewrites around that exact structure** so you re-encounter it in friendly context. This is the targeted-practice mechanism.
4. **Inline IPA + one-click TTS audio** — every new word gets US IPA pronunciation and an embedded mp3 player (powered by Microsoft Edge neural voices via `edge-tts`).

## How it feels

```
You: /english-read

Coach: Today's direction? (AI / Book / Twitter / Fiction / Surprise me)
       Recommended: AI — your last session left "could already serve" as a focus point.

You: AI

Coach: [Finds a recent Simon Willison post on Hacker News]
       [Rewrites it to your B1.2 level, ~280 words, with `could already`
        woven in 1-2 spots]
       [Lists 4 key vocab with /IPA/ + ▶ audio button]
       Read it and tell me in your native language what you got.

You: [3-sentence summary, possibly missing one nuance]

Coach: ✅ You caught X, Y, Z.
       ❌ You read "could already serve 30x more users" as
          "is already serving" — it's actually a virtual modality
          ("has the unused capacity to"). [explains the structure]

       Score: 82/100. Holding at B1.2 (need 1 more pass to step up).
       Next session focus: relative clauses with "whose".

       [Auto-archives reading log + appends vocab + generates audio
        + ticks today's checkbox + adds journal entry — all silent]
```

## Setup

### 1. Install the skill

```bash
# In your Claude Code project (or globally in ~/.claude/skills/)
git clone https://github.com/<your-username>/cefr-reading-coach.git \
  .claude/skills/cefr-reading-coach
```

### 2. Install TTS engine (one-time)

```bash
# uv (recommended — fast)
uv tool install edge-tts

# OR pipx
pipx install edge-tts
```

Verify: `edge-tts --voice en-US-AriaNeural --text "hello" --write-media test.mp3`

### 3. Create a project folder in your Obsidian vault

Use the template in [`examples/README.example.md`](examples/README.example.md). The skill expects:

```
<your-vault>/
└── english-reading/                  # any name you like
    ├── README.md                     # level profile, check-in table, next-session focus
    ├── reading-logs/                 # one .md per session
    ├── vocabulary.md                 # accumulated vocab (deduplicated by date)
    └── audio/                        # mp3 cache (auto-deduplicated by word)
```

See [`docs/INSTALL.md`](docs/INSTALL.md) for full setup including optional daily-journal integration.

### 4. Run it

```
/english-read
```

Or just say "I want to practice English reading" — the skill auto-triggers.

First run is the **calibration** (5-paragraph gradient test, takes ~5 min). After that, every run is the **daily cycle** (~15 min).

## Examples

The `examples/` folder contains real (cleaned) records from my own usage:

- [`examples/README.example.md`](examples/README.example.md) — project README template
- [`examples/reading-log.example.md`](examples/reading-log.example.md) — a real Day 03 reading log
- [`examples/vocabulary.example.md`](examples/vocabulary.example.md) — real vocab entries with IPA + audio embeds

## Design philosophy

See [`docs/WHY.md`](docs/WHY.md) for the longer-form rationale on why each design decision was made — including what didn't work.

## Requirements

- [Claude Code](https://claude.com/code) (the CLI, IDE extensions, or web app)
- [Obsidian](https://obsidian.md/) — the skill writes Markdown with `![[]]` wikilinks for audio embeds
- macOS / Linux — `edge-tts` works on both (Windows untested)
- Optional: `defuddle` for cleaner web extraction (`brew install defuddle` or `npm i -g @kepano/defuddle-cli`)

## 中文说明

这是一个 Claude Code skill，把每天 15 分钟变成个性化英语阅读练习——难度刚好。

四个核心设计：
1. **12 个 CEFR 子档**（A1.1 → C2.2），0.5 子档为单位升降，避免大跳级
2. **改写而非生成** — 保留原文结构和语气，AI 生成的文本总有套路感，改写保住真实质地
3. **覆盖式"下次重点提示"** — 每次结束时记一条最重要的弱点，下次刻意把这个结构编进改写文本里，让你在熟悉语境再碰一次
4. **行内 IPA + 一键播放音频** — 每个新词带美式音标 + 内嵌 mp3 迷你播放器（用 edge-tts 微软神经语音）

详见上方英文版。

## License

MIT — see [LICENSE](LICENSE).

## Credits

Built collaboratively with [Claude Opus 4.7](https://www.anthropic.com/claude) on Claude Code. Inspired by Paul Nation's "optimal challenge" concept (2-5% unknown word rate) and the CEFR sub-level framework.
