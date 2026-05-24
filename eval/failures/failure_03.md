# Failure Analysis -- eval_doc_05

## Source Document

```
=== SELLER ===
Chennai Auto Parts Distributors
No. 88, Anna Salai, Guindy
Chennai - 600032, Tamil Nadu
GSTIN: 33AABCC1234D1Z7

=== TAX INVOICE ===
Invoice: CAPD/2024/CHN/0567
Date: 22/02/2024
Payment Due: 22/03/2024

Buyer: Ashok Leyland Ltd., Ennore

=== ITEMS ===
Brake Drum Assembly (Heavy Vehicle)       50    2200.00    110000.00
Clutch Plate 14 inch                      30    1500.00     45000.00
Oil Filter Element (bulk pack of 10)      40     750.00     30000.00

=== SUMMARY ===
Subtotal: Rs. 1,85,000.00
CGST (9%): Rs. 16,650.00
SGST (9%): Rs. 16,650.00
Grand Total: Rs. 2,18,300.00
```

## Expected JSON Output

```json
{
  "vendor": "Chennai Auto Parts Distributors",
  "invoice_number": "CAPD/2024/CHN/0567",
  "date": "2024-02-22",
  "due_date": "2024-03-22",
  "currency": "INR",
  "subtotal": 185000.00,
  "tax": 33300.00,
  "total": 218300.00,
  "line_items": [
    {"description": "Brake Drum Assembly (Heavy Vehicle)", "quantity": 50, "unit_price": 2200.00},
    {"description": "Clutch Plate 14 inch", "quantity": 30, "unit_price": 1500.00},
    {"description": "Oil Filter Element (bulk pack of 10)", "quantity": 40, "unit_price": 750.00}
  ]
}
```

## Model's Actual Output

Here is the extracted information:
```json
{"vendor": "Chennai Auto Parts Distributors", "invoice_number": "CAPD/2024/CHN/0567", "date": "2024-02-22", "due_date": "2024-03-22", "currency": "INR", "subtotal": "Rs. 1,85,000.00", "CGST": "Rs. 16,650.00", "SGST": "Rs. 16,650.00", "Grand Total": "Rs. 2,18,300.00"}
```

## What Went Wrong

The model failed float coercion due to the lakh numerical commas, converting all monetary values to explicit strings like `"Rs. 1,85,000.00"`. It also failed to aggregate tax, creating illegal schema keys `CGST` and `SGST`. Expected issues: the Indian
number format (1,85,000.00 uses lakh grouping instead of thousand grouping) and the
need to sum CGST + SGST into a single tax value are likely failure points.]

## Why It Likely Failed

Indian number formatting uses a different comma grouping system than Western formats.
The value "1,85,000.00" means 185,000.00 but a model unfamiliar with the Indian
numbering system may parse it incorrectly or return it as a string. Similarly, the
model must sum CGST and SGST into a single tax value -- this is a computation step
rather than simple extraction, and while the training data includes examples of this
pattern (ex_006, ex_018), there are only 5 INR examples total in the training set.
The "Rs." currency symbol may also cause confusion if the model does not map it to INR.

## Proposed Data Fix

Add 5 additional INR-denominated training examples using the Indian lakh/crore number
format (e.g., 12,45,000.00 for twelve lakh forty-five thousand). At least 3 should
include separate CGST/SGST breakdown requiring summation. Include examples where the
amounts cross lakh boundaries (e.g., 9,99,999.00) to test edge cases in number parsing.
