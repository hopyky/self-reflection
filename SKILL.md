---
name: self-reflection
description: Continuous self-improvement through structured reflection and memory
version: 1.0.0
metadata: {"openclaw":{"emoji":"🪞","requires":{"bins":["jq","date"]}}}
---

# 🪞 Self-Reflection

A skill for continuous self-improvement. The agent tracks mistakes, lessons learned, and improvements over time.

## Quick Start

```bash
# Check if reflection is needed
self-reflection check

# Log a new reflection
self-reflection log "error-handling" "Forgot timeout on API call" "Always add timeout=30"

# Read recent lessons
self-reflection read

# View statistics
self-reflection stats
```

## How It Works

1. Agent runs `self-reflection check` at session start
2. If > threshold minutes since last reflection → returns ALERT
3. Agent performs self-analysis and logs insights
4. Lessons are stored in a Markdown file for persistence

## Commands

| Command | Description |
|---------|-------------|
| `check [--quiet]` | Check if reflection is due (OK or ALERT) |
| `log <tag> <miss> <fix>` | Log a new reflection |
| `read [n]` | Read last n reflections (default: 5) |
| `stats` | Show reflection statistics |
| `reset` | Reset the timer |

## Configuration

Create `~/.openclaw/self-reflection.json`:

```json
{
  "threshold_minutes": 60,
  "memory_file": "~/workspace/memory/self-review.md",
  "state_file": "~/.openclaw/self-review-state.json",
  "max_entries_context": 5
}
```

## Integration

Add to your agent's system prompt or AGENTS.md:

```
At session start, run `self-reflection check`.
If ALERT:
1. Run `self-reflection read` to review past lessons
2. Identify mistakes or improvements from recent interactions
3. Log insights with `self-reflection log "<tag>" "<miss>" "<fix>"`
```

## Author

Created by [hopyky](https://github.com/hopyky)

## License

MIT
