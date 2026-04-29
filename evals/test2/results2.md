# ComplianceNLP: Knowledge-Graph-Augmented RAG for Multi-Framework Regulatory Gap Detection

**Authors:** Dongxin Guo, Jikun Wu, Siu Ming Yiu (University of Hong Kong; Stellaris AI Limited)
**arXiv:** 2604.23585 | **Date:** April 26, 2026

---

## Pass 1 Summary

### Category
- [x] Research prototype description (with production deployment evidence)

**Type:** System paper presenting an end-to-end NLP pipeline for automated financial regulatory compliance monitoring, with four months of real-world deployment data.

### Context
- **Related papers:** LEGAL-BERT (Chalkidis et al., 2020), LegalBench (Guha et al., 2023), RIRAG/ObliQA (Bayer et al., 2025), DERECHA (Cejas et al., 2023), Sun et al. (2025) RAG-based compliance checker, Medusa speculative decoding (Cai et al., 2024), MiniLLM/KD (Gu et al., 2024)
- **Theoretical basis:** RAG (Lewis et al., 2020), KG-augmented LMs, multi-task learning over shared encoders, speculative decoding
- **Research area:** Legal/regulatory NLP, applied NLP systems, financial domain LLMs

### Correctness
- **Assumptions appear valid?** Mostly yes, with some caveats
- **Red flags:** Evaluation dataset (GAP-BENCH) is from a single institution; 96% production recall is an *estimate* with weak correlation between system confidence and manual detection difficulty (r=0.18, p=0.07); user study is small (12 analysts, 96 updates) and unblinded

### Contributions
- **Main contribution:** A production-deployed system combining KG-augmented RAG, multi-task obligation extraction, and compliance gap analysis across three regulatory frameworks (SEC, MiFID II, Basel III), with ablations isolating each component's gain and four months of deployment evidence.
- **Secondary contributions:**
  - Regulatory Knowledge Graph (RKG) with 12,847 provisions, 34,219 edges, 94.7% edge precision
  - REGOBLIGATION: 1,847-sentence annotated dataset across 3 frameworks
  - 2.8× inference speedup via 70B→8B knowledge distillation + Medusa speculative decoding
  - Five deployment lessons for regulated-domain NLP practitioners

### Clarity
- **Well written?** Yes
- **Structure quality:** Clear; logical flow from system description → optimization → experiments → deployment → lessons

---

### Recommendation
- [x] **Read deeper** — strong applied NLP paper with real deployment evidence; relevant to anyone building production NLP systems in regulated domains

### References to follow up
- Sun et al. (2025) — most closely related RAG compliance checker
- Zagyva et al. (2025) — Medusa+KD at Booking.com (directly adapted here)
- Bayer et al. (2025) RIRAG/ObliQA — main baseline for regulatory QA

---

## Pass 2 Analysis

### Main Argument Structure

**Problem:** Financial institutions must track 60K+ annual regulatory events across fragmented jurisdictions; manual compliance teams are overwhelmed; existing NLP systems handle single frameworks and rely on embeddings that fail at cross-references.

**Approach:** COMPLIANCENLP integrates three components:
1. **KG-augmented RAG** — hybrid dense+sparse retrieval re-ranked by graph proximity in a Regulatory Knowledge Graph (12,847 provisions); addresses cross-reference hallucination
2. **Multi-task obligation extraction** — shared LEGAL-BERT encoder with three heads: NER (23 entity types), deontic modality classification (Obligation/Permission/Prohibition/Recommendation), cross-reference span-pair linking
3. **Compliance gap analysis** — LLaMA-3-based generator maps extracted obligations to internal policies with severity-aware scoring; MiniCheck validates grounding

**Production optimization:** 70B teacher → 8B student via reverse KL divergence (MiniLLM approach), augmented with M=3 Medusa speculative decoding heads. Regulatory text's low entropy (H=2.31 bits vs. 3.87 for general text) enables 91.3% Medusa token acceptance rates → 2.8× speedup.

**Evaluation:** Benchmarked on REGOBLIGATION (NER/extraction) and GAP-BENCH (gap detection) against multiple baselines, with additive ablations and real deployment metrics.

**Conclusions:** KG re-ranking is the single most impactful component; production deployment achieves 96% recall and 90.7% precision; 3.1× sustained analyst efficiency gain; GRC integration and trust-building are underestimated challenges.

---

### Key Figures & Results

| Figure/Table | What it shows | Supports claim? | Notes |
|---|---|---|---|
| Table 2 (main results) | System achieves 91.3 NER F1, 87.7 gap F1, 52.8 QA EM vs. all baselines | Yes | Statistically significant vs. GPT-4o+RAG (p<0.05, paired bootstrap) |
| Table 2 (ablations) | KG re-ranking removal: −4.6 F1; multi-task: −2.2 NER; MiniCheck: minimal F1 but grounding drops 94.2%→86.7% | Yes | Additive analysis from both GPT-4o+RAG and LLaMA-3B+RAG confirms robustness |
| Table 1 (latency) | 8B+Medusa(dom.) achieves 659ms p50, 1082ms p99, 2.8× speedup with ≥97.4% accuracy retention | Partially | p99 exceeds sub-second target; deployment at δ=0.45 not δ=0.6 |
| Table 3 (per-class gap) | Full Gap recall 91.2% at deployment threshold (δ=0.45) vs. 81.5% at eval threshold (δ=0.6) | Yes | Trade-off between precision and recall made explicit |
| Table 4 (deployment) | 9,847 updates; 96% recall; 90.7% precision; 3.1× analyst efficiency gain | Partially | Recall is *estimated*, not directly measured; correlation with manual process is weak |
| Fig 1 (architecture) | Pipeline diagram showing three modules with data flow | Yes | Clear; dashed arrows for cross-module flow |

---

### Evidence Assessment
- **Strongest evidence:** The ablation studies are well-designed — two independent additive analyses (from GPT-4o+RAG and LLaMA-3B+RAG baselines) both consistently attribute +4.2/+4.6 F1 to KG re-ranking, making this finding robust. Cross-reference resolution improvement (+16.8 F1 from 72.3→89.1) is striking.
- **Weakest evidence:** The 96% production recall is acknowledged as an estimate — the manual process may miss the same gaps as the system (shared blind spots), and the correlation between system confidence and human-detected difficulty is statistically non-significant (r=0.18, p=0.07, n=47). The user study (12 analysts, unblinded) risks demand characteristics.
- **Unstated assumptions:** GAP-BENCH being from a single institution with a specific 847-category GRC taxonomy means the gap detection task is partially institution-specific; the ~30% institution-specific difficulty admitted in §7 is underemphasized in the headline numbers.

---

### Technical Gaps (for me)
- Medusa speculative decoding mechanics (multiple prediction heads, acceptance criteria)
- Bilinear classifier for span-pair linking in cross-reference resolution
- Fleiss' κ calculation for multi-annotator agreement
- FAISS approximate nearest-neighbor indexing details
- MiniCheck fact-checking approach (Tang et al., 2024)

---

### Summary

COMPLIANCENLP is a production-deployed end-to-end system for multi-framework regulatory compliance monitoring that combines knowledge-graph-augmented retrieval, multi-task obligation extraction over a shared LEGAL-BERT encoder, and LLaMA-3-based gap analysis. The key technical finding — validated through two independent additive ablations — is that structural regulatory knowledge (KG re-ranking) contributes the largest single gain (+4.6 F1) for cross-reference-heavy compliance tasks where embedding similarity alone fails. Four months of parallel deployment at a financial institution provide rare real-world evidence, though the primary production recall metric (96%) is an estimate with acknowledged measurement limitations.

### Updated Recommendation
- **Worth a third pass?** No — Pass 2 is sufficient unless you are building a similar compliance system or need to reproduce the RKG construction or distillation pipeline in detail.
- **Key references to read first if going deeper:** Sun et al. (2025) for the closest related system; Zagyva et al. (2025) for the Medusa+KD production pattern this adapts; Bayer et al. (2025) RIRAG for the primary regulatory QA baseline.

---

## Key Architectures & Models

### What is KG-Augmented RAG?

Standard RAG retrieves passages by embedding similarity. In regulatory compliance this fails because rules frequently **cross-reference other rules** (e.g., "as defined in Art. 25(2), subject to CRR Art. 412..."). Embedding similarity can't capture these structural relationships, so the LLM retrieves the wrong context and hallucinates.

**KG-augmented RAG** adds the Regulatory Knowledge Graph (RKG) as a second re-ranking signal: after retrieving top-k candidates via embeddings, passages are re-ranked by **graph proximity** — how close the retrieved provisions are to the query's source provision in the regulatory graph. Provisions that are explicitly cross-referenced rank higher than merely semantically similar ones.

Scoring function:
```
s_KG(q, d) = β · KGScore(q, d, G) + (1-β) · s(q, d)
```
where `s(q,d)` is the hybrid dense+sparse score and `β=0.3`. This re-ranking step alone accounts for **+4.6 F1** — the single largest gain in the ablations.

---

### ML Architectures Used

**1. Hybrid Retriever (dense + sparse)**
- Dense encoder: `all-MiniLM-L6-v2` fine-tuned on 50K regulatory passage pairs (bi-encoder)
- Sparse: BM25
- Combined: `α · sim_dense + (1-α) · BM25`, with `α=0.7`

**2. RKG Construction**
- Fine-tuned NER transformers extract entity and obligation nodes
- Cross-reference edge linker: bilinear classifier over source/target provision embeddings (span-pair linker), trained on regex-identified cross-reference patterns — 91.8% accuracy on 500 held-out pairs

**3. Multi-task Obligation Extractor (shared encoder)**
- Base: **LEGAL-BERT** (Chalkidis et al., 2020), further fine-tuned on Pile of Law
- Three jointly-trained heads:
  - CRF layer for NER (23 entity types)
  - Classifier for deontic modality (Obligation / Permission / Prohibition / Recommendation)
  - Span-pair classifier for cross-reference resolution
- Combined loss: `L = 0.4·L_NER + 0.3·L_deontic + 0.3·L_xref`

**4. Gap Analysis Generator**
- **LLaMA-3-70B-Instruct** (teacher), fine-tuned on regulatory data → distilled to **LLaMA-3-8B-Instruct** via reverse KL divergence (MiniLLM)
- M=3 Medusa speculative decoding heads trained on 2.1M regulatory tokens
- Grounding verified by **MiniCheck** (Tang et al., 2024)

> The LEGAL-BERT extractor and LLaMA-3 generator share **no parameters** and run sequentially in the pipeline.

---

## Code & Data

- **GitHub:** https://github.com/bettyguo/ComplianceNLP
