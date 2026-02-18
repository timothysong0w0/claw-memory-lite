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

## Quick Start

### 1. Installation

```bash
# Clone into your OpenClaw workspace
cd /home/node/.openclaw/workspace
git clone https://github.com/timothysong0w0/claw-memory-lite.git

# Copy scripts to your workspace
cp claw-memory-lite/scripts/*.py scripts/
```

### 2. Initialize

```bash
# Run extraction script once (creates database automatically)
python3 scripts/extract_memory.py
```

### 3. Configure Heartbeat (Optional)

Edit `HEARTBEAT.md` to add automated daily extraction:

```bash
python3 /home/node/.openclaw/workspace/scripts/extract_memory.py
```

## Usage

### Search by Keyword

```bash
python3 scripts/db_query.py backup
```

### Filter by Category

```bash
python3 scripts/db_query.py --category Skill
```

### Combined Query

```bash
python3 scripts/db_query.py uv --category Environment
```

### Auto-Extraction (Preview Mode)

```bash
python3 scripts/extract_memory.py --review
```

### Auto-Extraction (Execute)

```bash
python3 scripts/extract_memory.py
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

## Database Schema

**Table**: `long_term_memory`

| Column | Type | Description |
|--------|------|-------------|
| `id` | INTEGER | Primary key (auto-increment) |
| `category` | TEXT | Category (indexed) |
| `content` | TEXT | Memory content |
| `keywords` | TEXT | Keyword index (indexed) |
| `source_file` | TEXT | Source daily memory file |
| `created_at` | TIMESTAMP | Creation timestamp |
| `updated_at` | TIMESTAMP | Last update timestamp |

## L0/L1/L2 Hierarchy

claw-memory-lite adopts a simplified 3-tier structure inspired by OpenViking:

### L0 — Abstract (One-Line Summary)

A single sentence capturing the core essence. Used for quick scanning.

**Example**: `[2026-02-18] System: ModelScope integration complete with Qwen/Kimi aliases`

### L1 — Overview (Category Index)

Categorized summaries (2-3 sentences) for decision-making during planning.

**Example**:
```markdown
### 🛠️ Skills
- **tvscreener**: TradingView data query (HK/A-share/US)
- **humanizer**: AI writing pattern detection
- **Tavily**: Disabled since 2026-02-17 (OAuth failure)
```

### L2 — Details (Full Content in DB)

Complete factual records stored in SQLite, queryable on demand.

**Example**:
```
[2026-02-18 05:57:03] Skill: Model `Qwen/Qwen3.5-397B-A17B` → alias `qwen35plus`
```

## Comparison: claw-memory-lite vs OpenViking

| Feature | claw-memory-lite | OpenViking |
|---------|------------------|------------|
| **Target** | OpenClaw-specific | General Agent context |
| **Dependencies** | None (sqlite3 built-in) | Embedding + VLM models |
| **Storage** | SQLite | Vector DB + Filesystem |
| **Retrieval** | SQL + Category Filter | Vector search + Directory recursion |
| **Complexity** | Low (~200 LOC) | High (full framework) |
| **Token Optimization** | Query-on-demand (no pre-loading) | L0/L1/L2 layered loading |
| **Best For** | Conversation memory, config logs | Document/codebase management |

## Integration with OpenClaw

### Option A: OpenClaw Skill (Recommended)

```bash
npx skills add timothysong0w0/claw-memory-lite --agent openclaw
```

### Option B: Manual Copy

```bash
# Copy scripts
cp claw-memory-lite/scripts/*.py /home/node/.openclaw/workspace/scripts/

# Copy templates
cp claw-memory-lite/templates/*.md /home/node/.openclaw/workspace/

# Add cron job for daily extraction
# (See docs/integration.md for details)
```

## API Reference

### db_query.py

```bash
# Usage
python3 scripts/db_query.py [SEARCH_TERM] [--category CATEGORY]

# Arguments
SEARCH_TERM          Keyword to search (optional)
-c, --category CAT   Filter by category (optional)
```

### extract_memory.py

```bash
# Usage
python3 scripts/extract_memory.py [--review]

# Arguments
--review    Preview mode (don't write to DB)
```

## Performance Benchmarks

| Operation | Time |
|-----------|------|
| Database query (keyword) | <5ms |
| Database query (category) | <2ms |
| Auto-extraction (per file) | ~50ms |
| Initial DB creation | ~100ms |

*Benchmarked on Linux x64 with 30+ memory records*

## Roadmap

- [ ] Add `--export` flag to dump DB to JSON/Markdown
- [ ] Support for custom category definitions
- [ ] Web UI for browsing/searching memories
- [ ] Integration with OpenClaw's native `memory_search` tool

## Contributing

1. Fork the repo
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.

## Acknowledgments

- [OpenClaw](https://github.com/openclaw/openclaw) — The AI agent framework this is built for
- [OpenViking](https://github.com/volcengine/OpenViking) — Inspiration for L0/L1/L2 hierarchy

---

**Built with 🐯 for OpenClaw users who value speed, privacy, and simplicity.**
