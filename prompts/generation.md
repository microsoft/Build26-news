# Generation Prompt

<!-- This is the exact prompt used to generate news.md. Published for transparency. -->

You are generating a structured index of Microsoft Build 2026 announcements from official blog posts. This index will be used by an AI agent to answer developer questions about "what's new."

## CRITICAL CONSTRAINT: Quotes Only

You MUST follow these rules absolutely:

1. **Use only exact wording from the source blogs.** Every product name, feature name, and descriptive statement in your output must appear verbatim in the source blog text. Do not paraphrase, synthesize, rephrase, or add commentary.

2. **Product names must be written exactly as they appear in the source.** Do not abbreviate, expand, or rebrand any product name. Use only the exact name as written in the blog.

3. **No synthesis across blogs.** If two blogs mention the same feature, index each mention separately under its respective blog. Do not combine or merge information from different sources.

4. **No opinions or editorializing.** Do not add phrases like "importantly," "notably," "a major update," or any language that doesn't appear in the source.

5. **Customer stories**: Include only if the blog contains them. Use the exact wording from the blog to describe the customer and their use case.

6. **If uncertain whether wording is exact**, err on the side of quoting a shorter, verifiable phrase rather than a longer paraphrased one.

## Output Format

For each blog post provided, produce an entry in this format:

```
### [Section Number]. [Product/Feature Area — exact name from blog]
**Products**: [comma-separated product names, exactly as written in blog]
**Blog**: [URL]

1. **[Feature name — exact from blog]**: [One-sentence description using exact blog wording]
2. **[Feature name — exact from blog]**: [One-sentence description using exact blog wording]
...
```

## Ordering

Order sections to match the tiered structure:
- Tier 1: Official Microsoft Blog / Keynote summary
- Tier 2: Hero blogs (major cross-product themes)
- Tier 3: Individual product announcement blogs

## Input

You will receive the full text of multiple blog posts. Process each one and produce the structured index entries.

**IMPORTANT: Process ALL blogs provided. Do not skip any.**
