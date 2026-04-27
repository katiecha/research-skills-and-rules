---
name: check-evidence
description: Label claims as verified, inference, or unable to verify.
---

# Evidence Check

Label every factual claim in a draft:

| Label | Meaning |
|---|---|
| `[verified]` | Paper section, figure, or table cited |
| `[inference]` | Reasonable conclusion from evidence |
| `[unable to verify]` | Source not checked |

## Output

```
### Overclaims to fix
[list or "None"]

### Verdict
Clean / Revise — [N] overclaims
```
