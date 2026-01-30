# 🪞 Self-Reflection

A continuous self-improvement skill for AI agents. Track mistakes, log lessons learned, and build institutional memory over time.

## Why?

AI agents make mistakes. Without memory, they repeat them. This skill creates a structured feedback loop:

1. **Check** → Is it time to reflect?
2. **Read** → Review past lessons
3. **Log** → Record new insights
4. **Improve** → Apply learnings to future interactions

## Quick Start

```bash
# Install
git clone https://github.com/hopyky/self-reflection.git ~/.openclaw/skills/self-reflection

# Add to PATH
ln -sf ~/.openclaw/skills/self-reflection/bin/self-reflection ~/bin/self-reflection

# First check (will prompt for initial reflection)
self-reflection check
```

## Commands

| Command | Description |
|---------|-------------|
| `check` | Check if reflection is due (OK or ALERT) |
| `check --quiet` | Silent mode for scripts (outputs OK or ALERT only) |
| `log <tag> <miss> <fix>` | Log a new reflection |
| `read [n]` | Read last n reflections (default: 5) |
| `stats` | Show statistics and top tags |
| `reset` | Reset the timer |

## Usage Examples

### Check Status

```bash
$ self-reflection check
OK: Status good. Next reflection due in 45 minutes.

# Or when reflection is needed:
$ self-reflection check
ALERT: Self-reflection required. Last reflection was 65 minutes ago.
```

### Log a Reflection

```bash
$ self-reflection log "error-handling" "Forgot to handle API timeout" "Always add timeout parameter to requests"

Reflection logged successfully.
  Tag:  error-handling
  Miss: Forgot to handle API timeout
  Fix:  Always add timeout parameter to requests
```

### Read Past Lessons

```bash
$ self-reflection read 3
=== Last 3 reflections (of 12 total) ===

## 2026-01-30 14:30 | error-handling

**Miss:** Forgot to handle API timeout
**Fix:** Always add timeout parameter to requests

---

## 2026-01-30 10:15 | communication

**Miss:** Response was too verbose
**Fix:** Lead with the answer, then explain

---
```

### View Statistics

```bash
$ self-reflection stats
=== Self-Reflection Statistics ===

Last reflection: 2026-01-30 14:30:00
Total reflections: 12

Entries in memory: 12

Top tags:
  error-handling: 4
  communication: 3
  api: 2
  performance: 2
  ux: 1

Threshold: 60 minutes
Memory file: /home/user/workspace/memory/self-review.md
```

## Configuration

Create `~/.openclaw/self-reflection.json`:

```json
{
  "threshold_minutes": 60,
  "memory_file": "~/clawd/memory/self-review.md",
  "state_file": "~/.openclaw/self-review-state.json",
  "max_entries_context": 5
}
```

| Option | Default | Description |
|--------|---------|-------------|
| `threshold_minutes` | 60 | Minutes between required reflections |
| `memory_file` | `~/clawd/memory/self-review.md` | Where reflections are stored |
| `state_file` | `~/.openclaw/self-review-state.json` | Timer state file |
| `max_entries_context` | 5 | Default entries shown by `read` |

## Integration with AI Agents

### For OpenClaw / Claude Code

Add to your `AGENTS.md` or system prompt:

```markdown
## Self-Reflection (required)

At the start of each session, run:
```bash
self-reflection check
```

If the result is "ALERT", you MUST:
1. Run `self-reflection read` to review past lessons
2. Reflect on recent interactions - identify mistakes or improvements
3. Log insights with `self-reflection log "<tag>" "<miss>" "<fix>"`
4. Then continue with the user's request
```

### Recommended Tags

| Tag | Use for |
|-----|---------|
| `error-handling` | Missing try/catch, unhandled edge cases |
| `communication` | Verbose responses, unclear explanations |
| `api` | API usage mistakes, wrong endpoints |
| `performance` | Slow code, inefficient algorithms |
| `ux` | Poor user experience decisions |
| `security` | Security oversights |
| `testing` | Missing tests, untested edge cases |

## Memory Format

Reflections are stored in human-readable Markdown:

```markdown
# Self-Review Log

This file contains lessons learned and improvements for continuous growth.

---

## 2026-01-30 14:30 | error-handling

**Miss:** Forgot to handle API timeout
**Fix:** Always add timeout parameter to requests

---

## 2026-01-30 10:15 | communication

**Miss:** Response was too verbose
**Fix:** Lead with the answer, then explain

---
```

## Hook Integration (Optional)

For automatic reminders, create a hook that runs on session start:

```bash
#!/usr/bin/env bash
# ~/.openclaw/hooks/self-reflection-check.sh

status=$(self-reflection check --quiet 2>/dev/null)

if [[ "$status" == "ALERT" ]]; then
  echo ""
  echo "---"
  echo "SELF-REFLECTION REMINDER: Time for self-review."
  echo "Run: self-reflection read → then → self-reflection log"
  echo "---"
fi
```

## Dependencies

- `bash` (4.0+)
- `jq` (JSON processing)
- `date` (GNU coreutils)

## File Structure

```
self-reflection/
├── bin/
│   └── self-reflection      # Main CLI script
├── self-reflection.example.json
├── README.md
├── SKILL.md                 # OpenClaw skill manifest
└── LICENSE
```

## How It Works

```
┌─────────────────────────────────────────────────┐
│  Agent starts session                           │
│  ↓                                              │
│  Runs: self-reflection check                    │
│  ↓                                              │
│  < threshold  →  "OK" → Continue working        │
│  > threshold  →  "ALERT" → Reflect first        │
│                    ↓                            │
│              Read past lessons                  │
│                    ↓                            │
│              Identify improvements              │
│                    ↓                            │
│              Log new insights                   │
│                    ↓                            │
│              Continue working (improved!)       │
└─────────────────────────────────────────────────┘
```

## Philosophy

> "The only real mistake is the one from which we learn nothing." — Henry Ford

This skill implements a simple but powerful idea: **structured reflection leads to continuous improvement**. By regularly reviewing mistakes and documenting fixes, agents build institutional memory that persists across sessions.

## License

MIT License - See [LICENSE](LICENSE) for details.

## Author

Created by [hopyky](https://github.com/hopyky)

## Contributing

Issues and PRs welcome at [github.com/hopyky/self-reflection](https://github.com/hopyky/self-reflection)
