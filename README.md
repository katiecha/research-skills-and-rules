# Research Skills for Claude Code

A collection of skills for reading and analyzing research papers using Claude Code.

## Skills

| Skill | Purpose |
|-------|---------|
| **read-paper** | Read and analyze papers using Keshav's three-pass method |
| **task-router** | Route requests to the appropriate skill |
| **gather-context** | Gather background before reading a paper |
| **check-format** | Verify output has required sections |
| **check-evidence** | Label claims as verified/inference/unable to verify |
| **fallback-handler** | Handle failed retrievals or errors |
| **review-conversation** | Analyze past conversations for improvements |

## Usage

Place this repo in your project or reference it in your Claude Code settings. The `read-paper` skill supports three depth levels:

- **Pass 1**: Bird's-eye view (5-10 min) — category, context, contributions, recommendation
- **Pass 2**: Grasp content (1 hr) — argument structure, figures, evidence assessment
- **Pass 3**: Deep understanding (4-5 hrs) — virtual re-implementation, assumption audit, critique

Also supports **literature survey mode** for surveying a research field.

## Example

```
Read this paper: https://arxiv.org/abs/2301.00001
```

```
Survey the literature on transformer architectures
```

```
Do a pass 3 analysis on this PDF: /path/to/paper.pdf
```
