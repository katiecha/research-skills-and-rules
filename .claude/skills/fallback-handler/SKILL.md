---
name: fallback-handler
description: Handle failed retrievals or tool errors.
---

# Fallback Handler

When a tool fails or returns insufficient results:

| Failure | Action |
|---|---|
| Empty search | Retry with broader terms |
| Fetch error | Retry once, then mark `[unable to verify]` |
| Missing input | Ask user for the input |

## Output

```
failure_type: [type]
fallback_action: [what was tried]
escalation_needed: yes / no
```
