# Failure Analysis -- eval_doc_17

## Source Document

```
--- FIRM ---
Great Basin Environmental Consulting
1400 Research Way, Suite 200
Reno, NV 89502
Nevada License: ENV-2019-4412

--- INVOICE ---
Invoice Number: GBEC-2024-0223
Date of Issue: 5 March 2024
Payment Terms: Net 45
Payment Due: 19 April 2024

--- CLIENT ---
Nevada Gold Mines LLC
3000 Mining Road, Elko, NV

--- SERVICES ---
Phase II Environmental Site Assessment           1    8500.00    8500.00
Soil Sampling and Laboratory Analysis            1    4200.00    4200.00
Environmental Report and Remediation Plan        1    3100.00    3100.00

--- TOTAL ---
Subtotal: $15,800.00
Tax: Professional services exempt
Total Due: $15,800.00

Project Reference: Carlin Trend Site 14-B
```

## Expected JSON Output

```json
{
  "vendor": "Great Basin Environmental Consulting",
  "invoice_number": "GBEC-2024-0223",
  "date": "2024-03-05",
  "due_date": "2024-04-19",
  "currency": "USD",
  "subtotal": 15800.00,
  "tax": null,
  "total": 15800.00,
  "line_items": [
    {"description": "Phase II Environmental Site Assessment", "quantity": 1, "unit_price": 8500.00},
    {"description": "Soil Sampling and Laboratory Analysis", "quantity": 1, "unit_price": 4200.00},
    {"description": "Environmental Report and Remediation Plan", "quantity": 1, "unit_price": 3100.00}
  ]
}
```

## Model's Actual Output

```json
{"vendor": "Great Basin Environmental Consulting", "invoice_number": "GBEC-2024-0223", "subtotal": 15800.00, "tax": "Professional services exempt", "total": 15800.00}
```

## What Went Wrong

The model failed the null type safety requirement for text-based non-numeric taxes, injecting the string explicitly. Expected issues: the tax line
reads "Professional services exempt" rather than a number or N/A. The model may
output the string "Professional services exempt" instead of null, or it may output
0.0 instead of null. Additionally "Date of Issue" is an unusual label variant.]

## Why It Likely Failed

The tax field contains explanatory text ("Professional services exempt") rather than
a numeric value, an empty field, or a standard marker like "N/A" or "--". The training
data includes examples where tax is absent (using null), and examples where tax is
explicitly marked as "N/A" or "--", but no training examples include a full-sentence
explanation in the tax field. The model must recognize that any non-numeric content
in the tax field should map to null. The label "Date of Issue" instead of "Date" or
"Invoice Date" adds a minor additional complexity layer. The section separator style
(--- HEADER ---) matches the multi-section format in training but the specific
label variations are new.

## Proposed Data Fix

Add 3 training examples where the tax field contains explanatory text rather than
a numeric value: "Exempt per state regulation", "Not applicable - professional
services", and "Zero rated supply". In all cases, the output should set tax to null.
This teaches the model that any non-numeric tax field content maps to null regardless
of the specific wording.
