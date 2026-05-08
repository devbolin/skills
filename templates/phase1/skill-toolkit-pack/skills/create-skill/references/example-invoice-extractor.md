# Example: Invoice Extractor Skill

This is a complete example of a skill created using the create-skill workflow.

## SKILL.md

```markdown
---
name: invoice-extractor
description: Extract structured fields from invoice PDFs and images. Activate when user says "extract invoice", "scan receipt", or "parse invoice".
license: "MIT"
metadata:
  version: "1.0"
  author: "finance-team"
  tags: ["invoice", "extraction", "OCR"]
---

# Invoice Extractor

Extract merchant name, amount, date, tax rate and other structured fields from invoice files.

## Use Cases

- User uploads an invoice image and asks to extract information
- User says "reimburse this receipt" or "scan this invoice"
- User needs invoice data entered into a system

## Not Suitable For

- Handwritten invoices (OCR accuracy is limited)
- Multi-page documents without clear invoice boundaries

## Usage

1. Accept the invoice file (PDF, JPG, PNG)
2. Extract structured fields using optical analysis
3. Return results in the specified JSON format

## Output Format

```json
{
  "merchant": "ACME Corp",
  "amount": 1234.56,
  "date": "2026-05-01",
  "tax_rate": 0.13,
  "invoice_number": "INV-2026-001"
}
```

## Notes

- Keep description concise but specific enough for accurate agent activation
- Include at least 3 trigger keywords in the description
- The `name` must match the parent directory name exactly
```
