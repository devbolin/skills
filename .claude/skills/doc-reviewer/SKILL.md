---
name: doc-reviewer
description: Review document completeness and quality. Use when user says "review this document", "check completeness", or "document quality".
---

# Document Reviewer

Reviews documents for completeness, quality, and consistency.

## Review Dimensions

### 1. Structure
- Follows standard document structure for its type
- All required sections present
- Logical organization

### 2. Content
- Clear and specific descriptions
- Examples provided where needed
- No ambiguous language

### 3. Terminology
- Consistent with CONCEPTS.md
- Chinese and English terms aligned
- No jargon without definition

### 4. References
- Internal links valid
- External references correct
- Cross-references accurate

## Review Checklist

- [ ] Frontmatter complete (name, description)
- [ ] Use cases clearly defined
- [ ] Non-use cases specified
- [ ] Examples provided
- [ ] Terminology consistent
- [ ] Links and references valid
- [ ] Follows document type template

## Output Format

```markdown
## Review Report

### Issues Found
1. [Type] File: line - Description

### Suggestions
1. [Suggestion] - Rationale

### Overall Score
[ ] Pass / [ ] Needs Work
```
