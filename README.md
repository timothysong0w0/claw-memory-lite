# claw-memory-lite

> Lightweight Long-Term Memory for OpenClaw — SQLite-Powered, Zero External Dependencies, Millisecond Queries

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![OpenClaw](https://img.shields.io/badge/Built%20for-OpenClaw-blue)](https://github.com/openclaw/openclaw)

## Why claw-memory-lite?

OpenClaw's native `memory/*.md` approach works great initially, but as memory files accumulate:

- ❌ Every session loads all markdown files — slow and token-heavy
- ❌ Text-based search is inefficient
- ❌ No structured indexing or categorization

**claw-memory-lite** solves this with:

- ✅ **SQLite Storage** — Query in <10ms, no external vector DB needed
- ✅ **L0/L1/L2 Hierarchy** — Inspired by OpenViking, but lightweight (~200 lines)
- ✅ **Auto-Extraction** — Cron/heartbeat-based, zero manual maintenance
- ✅ **Zero External Dependencies** — Pure Python `sqlite3` (built-in)
- ✅ **Privacy-First** — All data stays local, no API calls

## Quick Start (Recommended)

### 1. Installation

The easiest way is to add it as a standard OpenClaw Skill:

```bash
npx skills add timothysong0w0/claw-memory-lite --agent openclaw
```

### 2. Initialize Database

```bash
# Run extraction script once (creates database automatically)
python ~/.openclaw/extensions/claw-memory-lite/scripts/extract_memory.py
```

### 3. Configure Automation

Add the following to your `HEARTBEAT.md` to enable daily memory extraction:

```bash
python ~/.openclaw/extensions/claw-memory-lite/scripts/extract_memory.py
```

---

## Manual Installation (Alternative)

If you prefer to manage scripts manually:

```bash
# Clone the repository
git clone https://github.com/timothysong0w0/claw-memory-lite.git

# Copy scripts to your workspace
cp claw-memory-lite/scripts/*.py /home/node/.openclaw/workspace/scripts/
```

Usage for manual installation:
- Search: `python scripts/db_query.py [keyword]`
- Extract: `python scripts/extract_memory.py`

---

## Usage (Skill Mode)

### Search by Keyword
```bash
python ~/.openclaw/extensions/claw-memory-lite/scripts/db_query.py [SEARCH_TERM]
```

### Filter by Category
```bash
python ~/.openclaw/extensions/claw-memory-lite/scripts/db_query.py --category Skill
```

## Categories

| Category | Description |
|----------|-------------|
| `System` | Session configuration, model aliases, compatibility rules |
| `Environment` | Workspace paths, backup rules, tool policies |
| `Skill` | Skill configurations, API endpoints, known issues |
| `Project` | Project status, strategy parameters, TODOs |
| `Comm` | Channel mappings, notification rules, bot configs |
| `Security` | Access control principles, audit log locations |

## Roadmap

- [ ] Add `--export` flag to dump DB to JSON/Markdown
- [ ] Integration with OpenClaw's native `memory_search` tool

## License

MIT License — see [LICENSE](LICENSE) for details.

---

**Built with 🐯 for OpenClaw users who value speed, privacy, and simplicity.**
