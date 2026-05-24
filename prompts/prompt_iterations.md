# Prompt Engineering Iterations

## Target Documents

The 3 worst-performing baseline documents were: eval_doc_04, eval_doc_06, eval_doc_05.
Selection criteria: lowest combined score of is_valid_json * has_all_required_keys * value_accuracy.

## Prompt Version 1 -- Zero-Shot Strict Instruction

**Rationale:** Adding explicit negative constraints ("do not use markdown") to test
whether the base model follows format instructions when stated directly.

**Prompt:**
You are a document extraction system. Extract all fields from the document below
and return ONLY a valid JSON object. Do not write any explanation, commentary,
or additional text. Do not use markdown formatting. Do not use code fences.
Do not prefix the JSON with any label. Return only the raw JSON object starting
with { and ending with }.

For invoices use these exact keys: vendor, invoice_number, date, due_date, currency,
subtotal, tax, total, line_items. For purchase orders use: buyer, supplier, po_number,
date, delivery_date, currency, total, items.

All dates must be YYYY-MM-DD. All monetary values must be numbers, not strings.
Use null for missing fields. line_items/items must be an array, never null.

Document:
{{DOCUMENT_TEXT}}

## Prompt Version 2 -- One-Shot Example

**Rationale:** Providing one complete worked example within the prompt to demonstrate
the expected output format, rather than describing it in words.

**Prompt:**
Extract structured data from the document below. Return only valid JSON.

Here is an example of correct extraction:

Document: "Acme Corp\nInvoice #1234\nDate: Jan 5, 2024\nDue: Feb 4, 2024\nWidget x10 at $25.00\nSubtotal: $250.00\nTax: $20.00\nTotal: $270.00"

Output: {"vendor": "Acme Corp", "invoice_number": "1234", "date": "2024-01-05", "due_date": "2024-02-04", "currency": "USD", "subtotal": 250.00, "tax": 20.00, "total": 270.00, "line_items": [{"description": "Widget", "quantity": 10, "unit_price": 25.00}]}

Now extract from this document. Return ONLY the JSON object, nothing else:

{{DOCUMENT_TEXT}}

## Prompt Version 3 -- Chain-of-Thought with Explicit Schema

**Rationale:** Asking the model to reason through each field before writing the JSON,
with the schema pasted directly into the prompt.

**Prompt:**
You are a document extraction assistant. Read the document below carefully.

First, mentally identify each of these fields in the document:
1. vendor (or buyer/supplier for POs)
2. invoice_number (or po_number)
3. date (convert to YYYY-MM-DD)
4. due_date or delivery_date (YYYY-MM-DD or null if absent)
5. currency (ISO 4217 code, infer from symbols)
6. subtotal (float, before tax)
7. tax (float or null)
8. total (float)
9. line_items or items (array of objects)

For any field not found in the document, use JSON null.
All monetary values must be numeric floats, not strings.
line_items must be an array (empty [] if no items found, never null).
Each line item needs: description (string), quantity (integer), unit_price (float).

After identifying all fields, return ONLY a single valid JSON object with no
additional text, no markdown, no code fences, no explanation.

Document:
{{DOCUMENT_TEXT}}
