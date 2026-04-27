---
name: review-conversation
description: Analyze past conversations to find failure patterns and suggest improvements.
---

# Conversation Reviewer

Analyze past conversations for failures:

| Signal | Example |
|---|---|
| Wrong skill | Used freeform when read-paper applied |
| Overclaiming | Stated verified without citing source |
| Missing section | Pass 2 output lacked required table |
| Retrieval miss | Summarized without reading paper |

## Output

```
### Failure clusters
[pattern] — [N instances]
  Root cause: [why]

### Suggestions
[specific changes to skills or routing]
```
