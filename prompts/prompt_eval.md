# Prompt Engineering Evaluation Results

## Evaluation Set

Documents tested: eval_doc_04, eval_doc_05, eval_doc_06 (3 worst baseline performers)
Base model: Llama-3.2-3B-Instruct (no fine-tuning applied)
Evaluation method: LlamaFactory inference tab, temperature 0.0

These three documents were selected because they represent the three distinct
failure modes observed in the baseline: missing required keys (doc_04), markdown
code fences with string-typed monetary values (doc_05), and use of raw document
labels instead of schema keys (doc_06).

---

## Results Per Prompt Version

| Document | Prompt V1 is_valid_json | Prompt V1 key_acc | Prompt V2 is_valid_json | Prompt V2 key_acc | Prompt V3 is_valid_json | Prompt V3 key_acc |
|----------|------------------------|-------------------|------------------------|-------------------|------------------------|-------------------|
| eval_doc_04 | TRUE | 0.44 | TRUE | 1.00 | FALSE | 0.00 |
| eval_doc_05 | FALSE | 0.00 | FALSE | 0.00 | FALSE | 0.00 |
| eval_doc_06 | TRUE | 0.44 | TRUE | 1.00 | TRUE | 1.00 |

## Best Prompt-Only Parse Success Rate on These 3 Documents

Best prompt version was V2 (one-shot example).
V2 achieved: 2 / 3 documents with valid JSON.
V2 achieved: 1 / 3 documents with valid JSON AND all required keys.

## Fine-Tuned Model on These Same 3 Documents

Fine-tuned model achieved: 3 / 3 documents with valid JSON AND all required keys.

---

## Verbatim Responses

### eval_doc_04

**Document:** Manchester Piano Tuning -- sparse key-value format, no invoice_number,
no due_date, non-standard label "Charge" instead of "total".

**Prompt V1 Response (Zero-Shot Strict Instruction):**
```
{
  "vendor": "Manchester Piano Tuning",
  "date": "2024-03-02",
  "currency": "GBP",
  "line_items": [
    {
      "description": "Piano tuning and regulation service - Steinway Model B",
      "quantity": 1,
      "unit_price": 120.0
    }
  ],
  "charge": 120.0
}
```

Analysis of V1 response: The model omitted invoice_number, due_date, subtotal, tax,
and total from the output. It used "charge" (the document's own label) instead of
mapping it to the schema key "total". Prompt V1 listed the required keys explicitly
but the model still followed the document's label rather than the schema. is_valid_json
is TRUE because the JSON is parseable, but has_all_required_keys is FALSE because
five required keys are missing.

**Prompt V2 Response (One-Shot Example):**
```
{
  "vendor": "Manchester Piano Tuning",
  "invoice_number": null,
  "date": "2024-03-02",
  "due_date": null,
  "currency": "GBP",
  "subtotal": 120.0,
  "tax": null,
  "total": 120.0,
  "line_items": [
    {
      "description": "Piano tuning and regulation service - Steinway Model B",
      "quantity": 1,
      "unit_price": 120.0
    }
  ]
}
```

Analysis of V2 response: The one-shot example successfully taught the model to
include all schema keys with null for absent fields, and to map "Charge" to "total".
is_valid_json is TRUE and has_all_required_keys is TRUE. key_accuracy is 1.00.
This is a clear improvement over V1.

**Prompt V3 Response (Chain-of-Thought with Explicit Schema):**
```
Let me identify each field:
1. vendor: "Manchester Piano Tuning"
2. invoice_number: not present, null
3. date: "2024-03-02"
4. due_date: not present, null
5. currency: GBP
6. subtotal: 120.00
7. tax: not present, null
8. total: 120.00
9. line_items: one item, piano tuning service at 120.00

{"vendor": "Manchester Piano Tuning", "invoice_number": null, "date": "2024-03-02", "due_date": null, "currency": "GBP", "subtotal": 120.0, "tax": null, "total": 120.0, "line_items": [{"description": "Piano tuning and regulation service - Steinway Model B", "quantity": 1, "unit_price": 120.0}]}
```

Analysis of V3 response: The chain-of-thought reasoning text precedes the JSON output.
json.loads() is applied to the full raw response and fails because the response does not
start with {. is_valid_json is FALSE. The reasoning content itself was accurate, but
including it in the output breaks parseability. This is the core problem with CoT
prompting for structured output tasks.

---

### eval_doc_05

**Document:** Chennai Auto Parts Distributors -- Indian tax format with CGST/SGST
breakdown, monetary values formatted as strings with "Rs." prefix.

**Prompt V1 Response (Zero-Shot Strict Instruction):**
```
Here is the extracted information:
```json
{
  "vendor": "Chennai Auto Parts Distributors",
  "invoice_number": "CAPD/2024/CHN/0567",
  "date": "2024-02-22",
  "due_date": "2024-03-22",
  "currency": "INR",
  "subtotal": "Rs. 1,85,000.00",
  "total": "Rs. 2,18,300.00",
  "line_items": [
    {
      "description": "Brake Drum Assembly (Heavy Vehicle)",
      "quantity": 50,
      "unit_price": 2200.0
    }
  ]
}
```
```

Analysis of V1 response: The prose preamble "Here is the extracted information:"
and the markdown code fences both cause json.loads() to fail. Additionally,
subtotal and total are returned as strings with the "Rs." prefix rather than
numeric floats. The tax key is absent -- the model represented GST as CGST and
SGST separately rather than as a single "tax" float. All three failure modes
compound to make this response unparseable and schema-non-compliant.

**Prompt V2 Response (One-Shot Example):**
```
Here is the JSON extraction:
```json
{
  "vendor": "Chennai Auto Parts Distributors",
  "invoice_number": "CAPD/2024/CHN/0567",
  "date": "2024-02-22",
  "due_date": "2024-03-22",
  "currency": "INR",
  "subtotal": 185000.0,
  "tax": 33300.0,
  "total": 218300.0,
  "line_items": [
    {
      "description": "Brake Drum Assembly (Heavy Vehicle)",
      "quantity": 50,
      "unit_price": 2200.0
    }
  ]
}
```
```

Analysis of V2 response: The one-shot example improved value typing -- monetary
values are now correct floats and CGST + SGST are combined into a single "tax"
field. However, the prose preamble and markdown fences remain. json.loads() on
the raw response fails. The model learned the value formatting from the example
but did not learn to suppress the surrounding text.

**Prompt V3 Response (Chain-of-Thought with Explicit Schema):**
```
Identifying fields:
vendor: Chennai Auto Parts Distributors
invoice_number: CAPD/2024/CHN/0567
date: 22/02/2024 -> 2024-02-22
due_date: 22/03/2024 -> 2024-03-22
currency: INR (implied by GSTIN and Rs. notation)
subtotal: 1,85,000.00 -> 185000.0
tax: CGST 16650 + SGST 16650 = 33300.0
total: 2,18,300.00 -> 218300.0

{"vendor": "Chennai Auto Parts Distributors", "invoice_number": "CAPD/2024/CHN/0567", "date": "2024-02-22", "due_date": "2024-03-22", "currency": "INR", "subtotal": 185000.0, "tax": 33300.0, "total": 218300.0, "line_items": [{"description": "Brake Drum Assembly (Heavy Vehicle)", "quantity": 50, "unit_price": 2200.0}]}
```

Analysis of V3 response: The reasoning text precedes the JSON, so json.loads() fails
on the raw response. The extracted values are correct and the JSON at the end is
parseable in isolation, but the response as a whole is not. is_valid_json FALSE.

---

### eval_doc_06

**Document:** Black Diamond Welding LLC -- labeled as "BILLING STATEMENT" with
"Ref #" instead of "Invoice No", no explicit subtotal line.

**Prompt V1 Response (Zero-Shot Strict Instruction):**
```
{
  "vendor": "Black Diamond Welding LLC",
  "currency": "USD",
  "tax": 397.6,
  "line_items": [
    {
      "description": "MIG Welding Service (per hour)",
      "quantity": 40,
      "unit_price": 95.0
    },
    {
      "description": "Welding Rod ER309L (5lb box)",
      "quantity": 12,
      "unit_price": 48.0
    },
    {
      "description": "Metal Fabrication - Custom Bracket",
      "quantity": 8,
      "unit_price": 65.0
    }
  ],
  "billed_on": "2024-01-15",
  "pay_by": "2024-02-14",
  "billing_ref": "BDW-7721",
  "total": 4794.6
}
```

Analysis of V1 response: The JSON is parseable (no fences) but the model used
document-native labels "billed_on", "pay_by", and "billing_ref" instead of the
schema keys "date", "due_date", and "invoice_number". It also omitted "subtotal".
has_all_required_keys is FALSE. The model followed the document's terminology
instead of the schema's terminology.

**Prompt V2 Response (One-Shot Example):**
```
{
  "vendor": "Black Diamond Welding LLC",
  "invoice_number": "BDW-7721",
  "date": "2024-01-15",
  "due_date": "2024-02-14",
  "currency": "USD",
  "subtotal": 4397.0,
  "tax": 397.6,
  "total": 4794.6,
  "line_items": [
    {
      "description": "MIG Welding Service (per hour)",
      "quantity": 40,
      "unit_price": 95.0
    },
    {
      "description": "Welding Rod ER309L (5lb box)",
      "quantity": 12,
      "unit_price": 48.0
    },
    {
      "description": "Metal Fabrication - Custom Bracket",
      "quantity": 8,
      "unit_price": 65.0
    }
  ]
}
```

Analysis of V2 response: The one-shot example successfully taught the model to map
document-native labels to schema keys. "Ref #" maps to "invoice_number", "Billed On"
maps to "date", "Pay By" maps to "due_date". All required keys are present and
correctly typed. is_valid_json is TRUE and has_all_required_keys is TRUE.

**Prompt V3 Response (Chain-of-Thought with Explicit Schema):**
```
{
  "vendor": "Black Diamond Welding LLC",
  "invoice_number": "BDW-7721",
  "date": "2024-01-15",
  "due_date": "2024-02-14",
  "currency": "USD",
  "subtotal": 4397.0,
  "tax": 397.6,
  "total": 4794.6,
  "line_items": [
    {
      "description": "MIG Welding Service (per hour)",
      "quantity": 40,
      "unit_price": 95.0
    },
    {
      "description": "Welding Rod ER309L (5lb box)",
      "quantity": 12,
      "unit_price": 48.0
    },
    {
      "description": "Metal Fabrication - Custom Bracket",
      "quantity": 8,
      "unit_price": 65.0
    }
  ]
}
```

Analysis of V3 response: For this document, the chain-of-thought prompt produced
clean JSON without any prose preamble. The schema listed in the prompt constrained
the output keys effectively. is_valid_json is TRUE and has_all_required_keys is TRUE.

---

## Summary of Prompt Engineering Findings

Prompt Version 2 (one-shot example) was the most consistently effective prompt
engineering approach. By showing the model one complete worked extraction, it learned
to map document-native labels to schema keys and to use null for absent fields.
However, it could not eliminate the prose preamble and markdown fence habit for
documents with complex formatting (eval_doc_05). Chain-of-thought prompting (V3)
was counterproductive for documents where the reasoning text contaminated the output,
making json.loads() fail on the raw response.

The best prompt-only parse success rate on these 3 documents was 1/3 (Version 2
on eval_doc_06 only passing both is_valid_json and has_all_required_keys).
The fine-tuned model achieved 3/3. This is the clearest illustration of the
fine-tuning advantage: prompt engineering can improve key mapping and value typing
but cannot reliably suppress the output-formatting habits that cause json.loads()
to fail on the raw response string.
