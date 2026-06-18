# Module 4: Skill Patterns — Templates for Every Documentation Task
## 🟡 Intermediate Level

**Duration:** 2 hours | **Prerequisite:** Modules 1–3 | **Tech Writer's Tribe**

---

## What This Level Means

**Intermediate** means you've built skills and they work. Now you're leveling up: instead of building every skill from scratch, you'll learn four foundational patterns that cover 90% of documentation tasks. Experienced practitioners recognize patterns — you now become one.

### Intermediate Learning Objectives

By the end of Module 4, you will be able to:
- Identify which of the four patterns best fits a given documentation task
- Build a Reviewer, Generator, Transformer, and Analyzer skill
- Combine pattern templates with the RTCCO framework
- Evaluate a skill's quality using the peer review rubric

---

## Recap and Homework Share (15 min)

> "Show us the skill you built! What task does it automate? How did the output look?"

3–4 volunteers share their skill and demonstrate it live. Class discusses:
- What worked well?
- What would you improve?
- Which RTCCO element made the biggest difference?

---

## The Four Skill Patterns (30 min)

Most documentation tasks fit one of four patterns. Learning these patterns gives you a template for almost any skill you'll ever need.

---

### Pattern 1: The Reviewer

**Purpose:** Check content against criteria and report findings.

**When to use:** PR reviews, doc audits, style guide checks, completeness checks, accessibility reviews.

**Template:**
```markdown
You are a [ROLE] reviewing [TARGET] for [PURPOSE].

Review: $ARGUMENTS

## Checklist
1. [Criterion 1] — [What PASS looks like]
2. [Criterion 2] — [What PASS looks like]
3. [Criterion 3] — [What PASS looks like]

## Constraints
- Do NOT make edits to the document
- Report findings only; do not rewrite

## Output
For each criterion:
| Criterion | Status | Finding | Action |
|-----------|--------|---------|--------|

Status options: ✅ PASS | ⚠️ NEEDS WORK | ❌ MISSING

## Summary
- Score: X of Y criteria met
- Top 3 improvements (ranked by impact on the reader)
```

**Documentation examples:**
- Check an API endpoint doc for required sections → `/check-api-doc`
- Review a README against team standards → `/check-readme`
- Audit a doc set for consistent terminology → `/term-audit`

---

### Pattern 2: The Generator

**Purpose:** Create new content from source material.

**When to use:** README generation, API reference scaffolding, changelog creation, template filling, release notes.

**Template:**
```markdown
Read the source material at $ARGUMENTS and generate [OUTPUT TYPE].

## Rules
- [Content rule 1 — what must be included]
- [Content rule 2 — what must be excluded]
- [Formatting rule]

## Output
Output ONLY the [content type].
No preamble, no commentary, no explanations.
Begin with the first line of actual content.
```

**Documentation examples:**
- Generate a changelog from git history → `/release-notes`
- Create API doc scaffolding from an OpenAPI spec → `/scaffold-api-doc`
- Generate a glossary from project source code → `/build-glossary`

---

### Pattern 3: The Transformer

**Purpose:** Convert content from one format or style to another.

**When to use:** Style rewrites, format conversions, audience-level adjustments, translation prep.

**Template:**
```markdown
Transform the content at $ARGUMENTS.

## From
[Describe the current format/style/level]

## To
[Describe the target format/style/level]

## Preserve (do NOT change)
- [What must remain identical: code samples, proper nouns, product names]
- [Any structural elements that must not change]

## Change
- [What must be transformed]
- [Specific style rules to apply]

## Output
The transformed content only. No comparison, no markup showing changes.
```

**Documentation examples:**
- Rewrite passive voice to active voice → `/active-voice`
- Convert a wall of text into a structured reference doc → `/to-reference`
- Simplify expert content for a beginner audience → `/simplify`

---

### Pattern 4: The Analyzer

**Purpose:** Extract insights, patterns, or structure from raw material.

**When to use:** Coverage analysis, gap identification, dependency mapping, metrics extraction.

**Template:**
```markdown
Analyze the content at $ARGUMENTS for [ANALYSIS PURPOSE].

## What to Analyze
- [Dimension 1 — what to look for]
- [Dimension 2 — what to look for]
- [Dimension 3 — what to look for]

## What NOT to Analyze
- [Out-of-scope items — be explicit]

## Output
[SECTION 1: Summary Statistics]
[SECTION 2: Detailed Findings — organized by category]
[SECTION 3: Priority Recommendations — ranked by impact]

Keep each recommendation actionable. Start with a verb.
```

**Documentation examples:**
- Surface undocumented code functions → `/doc-coverage`
- Identify jargon a new reader wouldn't know → `/jargon-audit`
- Find inconsistent product naming across a doc set → `/naming-audit`

---

## Pattern Selection Guide (10 min)

Use this decision tree when you don't know which pattern to use:

```
What is your primary goal?
│
├── Evaluate existing content → REVIEWER
│     "Does this meet the standard?"
│
├── Create new content → GENERATOR
│     "Make this exist."
│
├── Change the form of existing content → TRANSFORMER
│     "Keep the substance; change how it's expressed."
│
└── Extract information from content → ANALYZER
      "Tell me what's in here / what's missing."
```

---

## Workshop: Build One of Each (30 min)

Work in pairs or alone. Build ONE skill using each pattern template. Use your own workflow context.

**Suggested assignments:**
- `/check-style` — Reviewer pattern (checks style guide compliance)
- `/release-notes` — Generator pattern (generates changelog entries)
- `/simplify-prose` — Transformer pattern (rewrites for a beginner audience)
- `/doc-coverage` — Analyzer pattern (finds undocumented functions)

For each skill:
1. Choose the right pattern
2. Fill in the template with your specific Role, Context, Checklist/Rules, Constraints, Output
3. Test against a sample document
4. Note one thing you'd improve

---

## Discussion and Wrap-Up (10 min)

### Reflection Questions

1. Which pattern covers the most tasks in your daily work?
2. Can you think of a task that doesn't fit any of the four patterns?
3. What's the risk of using a Generator pattern without a human review step?

---

## Homework (Before Module 5)

1. **Build** a `/doc-coverage` skill using the Analyzer pattern
2. **Build** one additional custom skill using any pattern — something specific to your workflow
3. **Read** the `peer-review-rubric.md` in the `resources/` folder — you'll use it in Module 5

---

## 🟡 Intermediate Checklist

Before moving to Module 5, confirm you can:

- [ ] Name the four skill patterns from memory
- [ ] Match a documentation task to its correct pattern without the decision tree
- [ ] Build a Reviewer skill with a complete, scored checklist
- [ ] Build a Generator skill that produces clean output with no preamble
- [ ] Explain when a Transformer is different from a Generator
- [ ] Apply the Analyzer pattern to surface undocumented gaps
