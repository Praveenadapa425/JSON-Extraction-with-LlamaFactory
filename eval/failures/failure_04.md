# Failure Analysis -- eval_doc_13

## Source Document

```
=== BUYER ===
Indian Oil Corporation Ltd.
IndianOil Bhavan, 1 Sri Aurobindo Marg
New Delhi - 110003
CIN: L23201MH1959GOI011388

=== PURCHASE ORDER ===
PO No: IOCL/PO/2024/REF/0334
Date: 05/02/2024
Expected Delivery: 05/05/2024

=== SUPPLIER ===
Thermax Ltd.
Thermax House, 14 Mumbai Pune Road, Pune

=== ORDER ITEMS ===
Heat Exchanger Shell and Tube (custom)      3    85,00,000.00    2,55,00,000.00
Pressure Vessel ASME Coded                  2    52,00,000.00    1,04,00,000.00
Industrial Boiler Economizer                2    23,50,000.00      47,00,000.00

=== TOTAL ===
PO Value: Rs. 4,55,00,000.00 (including applicable taxes)

Project: Gujarat Refinery Modernisation Phase IV
Approved: Shri Arun Kumar, Executive Director (Projects)
```

## Expected JSON Output

```json
{
  "buyer": "Indian Oil Corporation Ltd.",
  "supplier": "Thermax Ltd.",
  "po_number": "IOCL/PO/2024/REF/0334",
  "date": "2024-02-05",
  "delivery_date": "2024-05-05",
  "currency": "INR",
  "total": 45500000.00,
  "items": [
    {"item_name": "Heat Exchanger Shell and Tube (custom)", "quantity": 3, "unit_price": 8500000.00},
    {"item_name": "Pressure Vessel ASME Coded", "quantity": 2, "unit_price": 5200000.00},
    {"item_name": "Industrial Boiler Economizer", "quantity": 2, "unit_price": 2350000.00}
  ]
}
```

## Model's Actual Output

```json
{"buyer": "Indian Oil Corporation Ltd.", "supplier": "Thermax Ltd.", "po_number": "IOCL/PO/2024/REF/0334", "date": "2024-02-05", "delivery_date": "2024-05-05", "currency": "INR", "total": "Rs. 4,55,00,000.00 (including applicable taxes)"}
```

## What Went Wrong

The model output a string representation containing parenthetical notes directly into the total float field. Expected issues: very large INR
values in crore format (4,55,00,000.00 = 4.55 crore = 45,500,000.00), date ambiguity
with DD/MM format, and the parenthetical note "including applicable taxes" on total.]

## Why It Likely Failed

The crore-scale Indian number format is the most challenging numeric conversion in the
test set. The value "85,00,000.00" must be parsed as 8,500,000.00 in Western notation.
Additionally, the date "05/02/2024" is ambiguous -- it could be May 2 (US format) or
February 5 (Indian/European format). The training data includes Indian-format dates
but the model may not have seen enough examples to reliably choose DD/MM over MM/DD.
Only 5 of the 80 training examples use Indian numbering, and the largest unit price
in training is Rs. 28,50,000.00, which is significantly smaller than the values here.

## Proposed Data Fix

Add 3 training examples with INR values exceeding one crore (1,00,00,000.00) to
familiarize the model with large-scale Indian number formatting. Add 2 examples
where the date is in DD/MM/YYYY format with a day value greater than 12 (e.g., 15/03)
to unambiguously teach DD/MM preference for Indian documents.
