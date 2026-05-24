# Failure Analysis -- eval_doc_04

## Source Document

```
Manchester Piano Tuning
Date: 2 March 2024

Client: Bridgewater Hall Concert Venue

Piano tuning and regulation service - Steinway Model B

Charge: 120.00 GBP

Payment on completion.
```

## Expected JSON Output

```json
{
  "vendor": "Manchester Piano Tuning",
  "invoice_number": null,
  "date": "2024-03-02",
  "due_date": null,
  "currency": "GBP",
  "subtotal": 120.00,
  "tax": null,
  "total": 120.00,
  "line_items": [
    {"description": "Piano tuning and regulation service - Steinway Model B", "quantity": 1, "unit_price": 120.00}
  ]
}
```

## Model's Actual Output

```json
{"vendor": "Manchester Piano Tuning", "date": "2024-03-02", "currency": "GBP", "charge": 120.00, "line_items": [{"description": "Piano tuning and regulation service - Steinway Model B", "quantity": 1, "unit_price": 120.00}]}
```

## What Went Wrong

The model wrapped the result in code fences, hallucinated the key `charge` instead of `total`, and entirely missed evaluating `due_date`, `subtotal`, and `tax` to null. As expected with this
document: the sparse format with no explicit invoice number, no subtotal label, and
the label "Charge" instead of "Total" or "Amount Due" may cause the model to
misidentify fields or add extra keys.]

## Why It Likely Failed

This document uses an extremely sparse format with only five lines of content. The
label "Charge" rather than "Total" or "Amount Due" is not present in any of the 80
training examples. Additionally, the absence of an explicit invoice number header
means the model must correctly output null rather than fabricating a value. Only 3
of the 80 training examples have null invoice_number fields, making this pattern
underrepresented. The document also lacks any subtotal or tax labels, requiring the
model to infer that the charge amount is the total and that subtotal equals total
with null tax.

## Proposed Data Fix

Add 4 training examples where the total is labeled with non-standard terminology:
"Charge", "Fee", "Service Cost", and "Price". Each should use a sparse format with
fewer than 6 lines of content. At least 2 of the 4 should have null invoice_number
to reinforce proper null handling for that field.
