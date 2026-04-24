# Installation

## Prerequisites

- [Claude Code](https://claude.com/code) (CLI / IDE extensions / web app — any client supporting `~/.claude/skills/`)
- [Obsidian](https://obsidian.md/) (any version supporting `![[]]` audio embeds)
- macOS or Linux
- Python 3.10+ (for `edge-tts`)

## Step 1: Install the skill

### Option A: global (available in all projects)

```bash
git clone https://github.com/<your-username>/cefr-reading-coach.git \
  ~/.claude/skills/cefr-reading-coach
```

### Option B: per-project (only available in one project)

```bash
cd /path/to/your/project
git clone https://github.com/<your-username>/cefr-reading-coach.git \
  .claude/skills/cefr-reading-coach
```

### Option C: as a Claude Code plugin

If your team distributes skills via plugins:

```bash
# Add to your plugin's marketplace.json or via /plugin add
```

## Step 2: Install TTS engine

```bash
# Recommended: uv (fast, isolated)
uv tool install edge-tts

# Alternative: pipx
pipx install edge-tts

# Verify
edge-tts --voice en-US-AriaNeural --text "hello world" --write-media /tmp/test.mp3
ls -lh /tmp/test.mp3   # should be ~10-15 KB
```

If `edge-tts` is not in your `PATH`, find its absolute path with `which edge-tts` and update the `TTS command` line in `SKILL.md` to use the absolute path.

## Step 3: Optional — install `defuddle` for cleaner web extraction

```bash
# Homebrew
brew install defuddle

# Or npm
npm i -g @kepano/defuddle-cli
```

The skill will fall back to `agent-browser` then `WebFetch` if `defuddle` isn't available, but `defuddle` gives the cleanest article extraction.

## Step 4: Create your reading project in Obsidian

In your Obsidian vault, create a folder for the project (any name):

```
<your-vault>/
└── english-reading/
    ├── README.md           # paste from examples/README.example.md and customize
    ├── reading-logs/       # empty, skill will fill
    ├── vocabulary.md       # empty file with frontmatter (see example)
    └── audio/              # empty, skill will fill
```

Copy [`examples/README.example.md`](../examples/README.example.md) into your project as `README.md`. Replace the example dates and stats with your own.

The "Level Profile" section's `Starting level` field should be **empty** on first run — that's how the skill detects it should run the calibration test.

## Step 5: First run

In Claude Code, navigate to your vault directory and say:

```
/english-read
```

If the skill is installed correctly, it'll respond by starting the 5-paragraph calibration test (Mode A). Takes about 5 minutes.

After that, every subsequent run enters the daily cycle (Mode B), about 15 minutes per session.

## Troubleshooting

### "Permission denied" when running `edge-tts`

If `edge-tts` is at `~/.local/bin/edge-tts` (uv's default), make sure that path is in your shell's `PATH`. Add to `.zshrc` / `.bashrc`:

```bash
export PATH="$HOME/.local/bin:$PATH"
```

### Audio doesn't play in Obsidian

Make sure:
1. The `audio/` folder is inside your vault (Obsidian only resolves `![[]]` within the vault)
2. The mp3 filename matches the embed exactly (case-sensitive on Linux)
3. You're in **Reading view** (Live Preview also works); Source view shows the raw `![[xxx.mp3]]` text

### Skill doesn't detect my project

The skill looks for the project README in the current Claude Code working directory tree. If your vault and your Claude Code session aren't in the same directory tree, either:
- `cd` into your vault before running, or
- Hardcode the project path in `SKILL.md`'s "Project Context" section (replace `<VAULT>/<PROJECT_DIR>`)

## Customizing

See `SKILL.md` directly — it's plain Markdown, edit any section. Common tweaks:

- **Change TTS voice**: edit the `--voice` flag (try `en-US-GuyNeural` for male, `en-GB-SoniaNeural` for British female; full list at [edge-tts voices](https://github.com/rany2/edge-tts#changing-voices))
- **Change rewrite length**: Step B3 default is 200-400 words; bump up if you want longer reads
- **Disable audio**: remove the B7-②a section and the `![[xxx.mp3]]` embed lines from B7-②b
- **Different journal integration**: edit Step B7 ④ to match your daily-note location
