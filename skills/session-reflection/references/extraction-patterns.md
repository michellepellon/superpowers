# Extraction Patterns for Periodic Mode

## Session File Location

Claude Code stores session data at `~/.claude/projects/<project-slug>/<session-uuid>.jsonl`.

The project slug is the absolute path with `/` replaced by `-` and a leading `-`:
- `/Users/mpellon/dev/superpowers` → `-Users-mpellon-dev-superpowers`

### Discovering the Project Directory

```bash
# Auto-detect from current working directory
PROJECT_DIR="$HOME/.claude/projects/-$(pwd | sed 's|^/||; s|/|-|g')"

# Verify it exists
if [ -d "$PROJECT_DIR" ]; then
    echo "Found: $PROJECT_DIR"
    ls -la "$PROJECT_DIR"/*.jsonl 2>/dev/null | wc -l
else
    echo "No session folder found. Available projects:"
    ls ~/.claude/projects/
fi
```

## JSONL Message Schema

Each line is a JSON object with a `type` field:

| Type | Description | Useful for Analysis |
|---|---|---|
| `user` | User message or tool result | Yes — contains requests and tool outputs |
| `assistant` | Agent response or tool call | Yes — contains decisions and tool invocations |
| `system` | System messages | Minimal — timestamps only |
| `progress` | Streaming progress updates | No — skip |
| `file-history-snapshot` | File state snapshots | No — skip (large) |
| `last-prompt` | Final prompt record | No — skip |

### Message Content Structure

**User messages** have `.message.content` as either:
- A string (plain text from user)
- An array of content blocks including `{type: "tool_result", tool_use_id: "...", content: "..."}`

**Assistant messages** have `.message.content` as an array of:
- `{type: "text", text: "..."}` — agent's text output
- `{type: "tool_use", name: "Read", input: {file_path: "..."}}` — tool invocation
- `{type: "thinking", ...}` — reasoning (skip — large, not useful for pattern detection)

### Key Fields

| Field | Location | Purpose |
|---|---|---|
| `.timestamp` | Top level | Sequence and timing analysis |
| `.type` | Top level | Message type filtering |
| `.message.content` | User/assistant | Content for analysis |
| `.message.role` | User/assistant | `"user"` or `"assistant"` |
| `.sessionId` | Top level | Group messages by session |

## Summary Extraction Command

This extracts ~2% of raw token volume while preserving the signals needed for pattern detection:

```bash
PROJECT_DIR="$HOME/.claude/projects/-$(pwd | sed 's|^/||; s|/|-|g')"
OUTPUT="/tmp/session-summary.jsonl"

cat "$PROJECT_DIR"/*.jsonl 2>/dev/null | jq -c '
select(.type == "user" or .type == "assistant") |
{
  type,
  ts: .timestamp,
  sid: .sessionId,
  content: (
    if .message.content | type == "string" then
      .message.content[0:300]
    elif .message.content | type == "array" then
      [.message.content[] |
        if .type == "text" then {t: "text", v: .text[0:200]}
        elif .type == "tool_use" then {t: "tool", name: .name, input_keys: (.input | keys)}
        elif .type == "tool_result" then {t: "result", len: (.content | tostring | length)}
        elif .type == "thinking" then empty
        else empty
        end
      ]
    else null
    end
  )
}' > "$OUTPUT" 2>/dev/null

echo "Summary: $(wc -l < "$OUTPUT") messages, $(wc -c < "$OUTPUT" | xargs) bytes"
```

### What This Preserves

- User message text (first 300 chars) — what was requested
- Tool call names and input key shapes — what tools were used and on what
- Tool result sizes (not content) — how much data was returned
- Timestamps — sequence and duration analysis
- Session IDs — group by session for cross-session patterns

### What This Discards

- Full tool results (huge, not needed for pattern detection)
- Thinking blocks (private reasoning, very large)
- Progress updates (streaming noise)
- File history snapshots (file state, not behavioral)
- Full tool input values (only keys preserved — enough to detect "Read file_path" patterns)

## Filtering by Date

```bash
# macOS: last N days only
DAYS_BACK=7
find "$PROJECT_DIR" -name "*.jsonl" -mtime -${DAYS_BACK} -exec cat {} + 2>/dev/null | jq -c '
  select(.type == "user" or .type == "assistant") |
  # ... same filter as above
' > "$OUTPUT"
```

## Extracting Tool Call Patterns

For detecting repeated file reads and unnecessary tool calls:

```bash
# Count tool calls by name and target
cat "$OUTPUT" | jq -r '
  .content[]? |
  select(.t == "tool") |
  .name
' | sort | uniq -c | sort -rn

# Find repeated Read targets (need full extraction for this)
cat "$PROJECT_DIR"/*.jsonl 2>/dev/null | jq -r '
  select(.type == "assistant") |
  .message.content[]? |
  select(.type == "tool_use" and .name == "Read") |
  .input.file_path
' | sort | uniq -c | sort -rn | head -20
```

## Extracting Decision Reversals

Look for patterns where assistant text contains correction signals:

```bash
# Find user corrections (messages following assistant that redirect)
cat "$OUTPUT" | jq -c '
  select(.type == "user") |
  select(.content | tostring | test("no|not that|instead|actually|wrong|stop"; "i"))
'
```

## Prerequisite Check

Before running periodic mode, verify `jq` is available:

```bash
if ! command -v jq &> /dev/null; then
    echo "jq is required for periodic mode but not installed"
    echo "Install: brew install jq (macOS) or apt install jq (Linux)"
    echo "Falling back to session mode (current context only)"
fi
```
