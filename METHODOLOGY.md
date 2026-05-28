# Methodology

## How This Index Was Generated

This structured index was produced using a quotes-only extraction prompt applied to official Microsoft Build 2026 blog posts. The process ensures every statement in the index traces back to exact wording in the source blogs.

## Generation Process

1. **Input**: Official blog post URLs published at [news.microsoft.com/build-2026](https://news.microsoft.com/build-2026)
2. **Batching**: Blogs are processed in tiered batches to reduce context pressure and maximize accuracy. Batch boundaries follow the natural tier structure (keynote summary → hero blogs → product blogs).
3. **Prompt**: A constrained extraction prompt that requires exact quoted wording only — no paraphrasing, no synthesis, no opinion ([see prompt](prompts/generation.md))
4. **Merge**: Batch outputs are concatenated in tier order to form the complete index

## Why Batching?

Testing showed that processing all blogs in a single prompt produces cross-sentence synthesis errors — the model assembles real words from different sentences into statements that don't exist in the source. Batching into smaller tier-aligned groups eliminates this failure mode entirely:

| Strategy | Traceability | Drift type |
|----------|-------------|------------|
| All at once | 99.3% | Cross-sentence synthesis |
| **Batched** | **99.6%** | Single word typo only |

Batching is strictly better: more detail extracted AND higher accuracy.

## Verification Pipeline

### Gate 1: Prompt Guardrails
The generation prompt constrains output to quoted wording only. Product names must appear exactly as written. No synthesis across blogs.

### Gate 2: Multi-Model AI Verification
A second AI model (different model family than the generator) checks every statement against the source blog. Statements are scored as:
- ✅ **EXACT**: Appears verbatim in the source
- ⚠️ **CLOSE**: Minor rewording (truncation, verb tense)
- ❌ **DRIFT**: Paraphrased or words not in source
- 🚫 **HALLUCINATED**: Not present in the source at all

**Target**: Traceability (EXACT + CLOSE) ≥ 99%, Drift = 0%, Hallucination = 0%

### Gate 3: Human PR Review
Each blog's PR owner reviews only their section of the index. Sign-off required before publish.

### Gate 4: Transparency
This methodology and the generation prompt are published so the process is reproducible.

## Testing

Prompt variants were tested against Build 2025 blog content (21 posts). Results are available at [mikki_microsoft/build-news-index-test](https://github.com/mikki_microsoft/build-news-index-test).

### Key Findings

1. **Quotes-only instruction (Variant B)** outperforms forced citations (Variant D): 99.3% vs 98.0%
2. **Batching eliminates synthesis drift**: 99.6% batched vs 99.3% all-at-once
3. **Multi-model verification is essential**: Claude checking Claude's work missed all drifts. GPT 5.5 caught them.
4. **Cross-sentence synthesis is the failure mode**: The model grabs real words from multiple sentences and assembles them into a statement that doesn't exist. Batching prevents this by reducing context pressure.

## Accuracy Target

- **Traceability Score** ≥ 99% (EXACT + CLOSE)
- **Drift**: 0%
- **Hallucination**: 0%
