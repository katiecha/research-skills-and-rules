---
name: read-paper
description: Read and analyze research papers using Keshav's three-pass method for efficient comprehension
---

# Read Paper

Apply Keshav's three-pass method for efficient paper reading. Each pass builds on the previous one.

## Procedure

### 1. Determine depth

If user specifies depth (1, 2, or 3), use that. Otherwise default to pass 2.

- **Pass 1**: Bird's-eye view. Use when screening papers or outside your specialty.
- **Pass 2**: Grasp content without details. Use for papers of interest but not core research.
- **Pass 3**: Full understanding. Use for papers you need to review, reproduce, or build upon.

### 2. Execute passes sequentially

Always start with pass 1, even if targeting a deeper pass.

---

## Pass 1: Bird's-Eye View (5-10 min equivalent)

Read only:
1. Title, abstract, introduction
2. Section and sub-section headings (skip body text)
3. Conclusions
4. References (note which you recognize)

### Output: The Five Cs

```markdown
## Pass 1 Summary

### Category
What type of paper is this?
- [ ] Measurement/empirical study
- [ ] Analysis of existing system
- [ ] Research prototype description
- [ ] Survey/tutorial
- [ ] Theoretical/formal methods
- [ ] Position/vision paper

**Type:** [one sentence]

### Context
- **Related papers:** [list 2-5 key related works mentioned]
- **Theoretical basis:** [frameworks, models, or prior results this builds on]
- **Research area:** [subfield and broader field]

### Correctness
- **Assumptions appear valid?** [yes/no/unclear]
- **Red flags:** [any obvious methodological concerns, or "none noted"]

### Contributions
- **Main contribution:** [one sentence]
- **Secondary contributions:** [bullet list if any]

### Clarity
- **Well written?** [yes/partially/no]
- **Structure quality:** [clear sections, logical flow, or issues noted]

---

### Recommendation
- [ ] **Read deeper** — relevant to my work, assumptions seem sound
- [ ] **Set aside** — interesting but outside current focus
- [ ] **Skip** — invalid assumptions / not relevant / poorly written
- [ ] **Need background first** — unfamiliar terminology or techniques

### References to follow up
- [list any unread references that seem important]
```

### GitHub & Code Links
- [List any GitHub repos, project pages, or dataset URLs mentioned in the paper, or "None found"]

If depth = 1, stop here.

---

## Pass 2: Grasp Content (up to 1 hour equivalent)

Read with greater care, but skip proofs and dense technical details.

Focus on:
1. **Figures, diagrams, graphs** — Are axes labeled? Error bars present? Do results support claims?
2. **Key arguments** — Jot down the logical structure
3. **Evidence quality** — How strong is the support for each claim?
4. **Unread references** — Mark important ones for background reading

### Output: Content Analysis

```markdown
## Pass 2 Analysis

### Main Argument Structure
[Outline the paper's logical flow: problem → approach → evaluation → conclusions]

### Key Figures & Results
| Figure/Table | What it shows | Supports claim? | Notes |
|--------------|---------------|-----------------|-------|
| Fig 1        | ...           | Yes/Partially/No| ...   |
| Table 2      | ...           | ...             | ...   |

### Key Architectures & Models
For each major component, identify:
- **Model/architecture used** (e.g., BERT variant, LLaMA, GNN)
- **How it was trained/adapted** (pre-trained + fine-tuned? from scratch? distilled?)
- **What it takes as input / produces as output**
- **Why this architecture was chosen over alternatives** (if stated)

If the paper combines multiple components into a pipeline, draw the data flow:
```
[input] → [component A: model X] → [intermediate] → [component B: model Y] → [output]
```

### Evidence Assessment
- **Strongest evidence:** [what is most convincing]
- **Weakest evidence:** [what is least convincing or missing]
- **Unstated assumptions:** [implicit assumptions in methodology]

### Technical Gaps (for me)
[List concepts, techniques, or background I'd need to fully understand this]

### Summary
[2-3 sentences: main thrust of paper with supporting evidence, suitable for explaining to someone else]

### Updated Recommendation
- **Worth a third pass?** [yes — if I need to reproduce/review/build on this; no — pass 2 sufficient]
- **Key references to read first:** [if third pass needed, what background is missing]
```

If depth = 2, stop here.

---

## Pass 3: Deep Understanding (4-5 hours equivalent for beginners)

The goal is to **virtually re-implement** the paper: make the same assumptions as the authors and mentally recreate the work.

Focus on:
1. **Challenge every assumption** — What if this assumption is wrong?
2. **Identify hidden assumptions** — What's implicit but not stated?
3. **Compare your approach** — How would you present this idea differently?
4. **Find innovations** — What's genuinely new vs. incremental?
5. **Find weaknesses** — Missing citations, flawed experiments, logical gaps
6. **Generate ideas** — What future work does this suggest?

### Output: Deep Analysis

```markdown
## Pass 3 Deep Analysis

### Virtual Re-implementation
If I were to recreate this work:
- **I would keep:** [aspects that are well-designed]
- **I would change:** [aspects I'd approach differently]
- **Key insight I gained:** [what clicked by thinking through the approach]

### Assumption Audit
| Assumption | Stated? | Valid? | Impact if wrong |
|------------|---------|--------|-----------------|
| ...        | Yes/No  | Yes/No/Unclear | High/Medium/Low |

### Innovations vs. Incremental
- **Genuinely novel:** [what's new]
- **Incremental/expected:** [what follows naturally from prior work]

### Weaknesses & Missing Elements
- **Missing citations:** [relevant work not cited]
- **Methodological issues:** [experimental or analytical problems]
- **Logical gaps:** [arguments that don't follow]
- **Threats to validity:** [internal and external]

### Key Statistical & Mathematical Terms
For each statistical or mathematical concept used in the paper, provide a plain-language definition:

| Term | Definition | Used for |
|------|------------|----------|
| [e.g., F1 score] | [Harmonic mean of precision and recall] | [Evaluating gap detection] |
| ... | ... | ... |

### Proof/Technique Inventory
[Techniques used that I can add to my repertoire]

### Future Work Ideas
[Ideas for follow-on research sparked by this paper]

### Final Assessment
- **Strong points:** [bullet list]
- **Weak points:** [bullet list]
- **Overall quality:** [excellent / good / adequate / poor]
- **Reproducible?** [yes / partially / no — and why]

### One-paragraph summary
[Comprehensive summary suitable for a literature review, covering problem, approach, key results, limitations]
```

---

## Literature Survey Mode

When the user asks to survey a field (not a single paper):

1. **Find seed papers**: Use WebSearch with well-chosen keywords to find 3-5 recent papers
2. **Pass 1 each**: Get a sense of the work, read their Related Work sections
3. **Look for surveys**: If a recent survey exists, read it and you're done
4. **Find key papers**: Identify shared citations and repeated author names in bibliographies
5. **Find top venues**: Check where key researchers publish to identify top conferences
6. **Scan proceedings**: Look through recent proceedings of top venues for related work
7. **Iterate**: If all papers cite something you missed, obtain and read it

### Output: Survey Summary

```markdown
## Literature Survey: [Topic]

### Key Papers (by influence)
1. [Paper] — [one-line contribution]
2. ...

### Key Researchers
- [Name] — [affiliation, focus area]

### Top Venues
- [Conference/Journal] — [why it's relevant]

### Research Themes
- **Theme 1:** [description, key papers]
- **Theme 2:** ...

### Open Problems
- [Problem 1]
- [Problem 2]

### Recommended Reading Order
1. [Start here — foundational]
2. [Then this — builds on #1]
3. ...
```
