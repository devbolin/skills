---
name: invoice-extractor
description: Extract structured invoice fields
---

# Invoice Extractor

## Use When
- User asks to parse invoice PDF to structured fields.

## Inputs
- `file`: invoice document
- `locale` (optional): locale hint

## Output
- Normalized JSON containing invoice metadata and line items.
