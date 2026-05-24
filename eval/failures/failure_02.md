# Failure Analysis -- eval_doc_06

## Source Document

```
BLACK DIAMOND WELDING LLC
Certified AWS D1.1 Structural Welding
3300 Foundry Street, Birmingham, AL 35203

BILLING STATEMENT

Ref #: BDW-7721
Billed On: January 15, 2024
Pay By: February 14, 2024

Account: Southern Steel Erectors Inc.

MIG Welding Service (per hour)          40     95.00    3800.00
Welding Rod ER309L (5lb box)            12     48.00     576.00
Metal Fabrication - Custom Bracket       8     65.00     520.00

Taxable Amount:                                       $5,680.00
AL State Tax (7%):                                      $397.60
Amount Due:                                           $6,077.60

Pay via ACH or check.
```

## Expected JSON Output

```json
{
  "vendor": "Black Diamond Welding LLC",
  "invoice_number": "BDW-7721",
  "date": "2024-01-15",
  "due_date": "2024-02-14",
  "currency": "USD",
  "subtotal": 5680.00,
  "tax": 397.60,
  "total": 6077.60,
  "line_items": [
    {"description": "MIG Welding Service (per hour)", "quantity": 40, "unit_price": 95.00},
    {"description": "Welding Rod ER309L (5lb box)", "quantity": 12, "unit_price": 48.00},
    {"description": "Metal Fabrication - Custom Bracket", "quantity": 8, "unit_price": 65.00}
  ]
}
```

## Model's Actual Output

```json
{"vendor": "Black Diamond Welding LLC", "Ref #": "BDW-7721", "Billed On": "2024-01-15", "Pay By": "2024-02-14", "currency": "USD", "Taxable Amount": 5680.00, "tax": 397.60, "Amount Due": 6077.60, "line_items": []}
```

## What Went Wrong

The baseline model fell back to verbatim source-text label matching instead of the requested schemas, naming its keys `Ref #` instead of `invoice_number`, and `Taxable Amount`. Expected issues: the document
uses "BILLING STATEMENT" instead of "INVOICE", "Ref #" instead of "Invoice Number",
"Billed On" instead of "Date", "Pay By" instead of "Due Date", and "Taxable Amount"
instead of "Subtotal". These non-standard labels may confuse field mapping.]

## Why It Likely Failed

The document uses entirely non-standard field labels that do not appear in the training
data. None of the 80 training examples use the term "Billing Statement" as a document
header -- all use "Invoice" or "Tax Invoice". The label "Taxable Amount" for subtotal
appears in zero training examples; all training data uses "Subtotal", "Net", or
"Nettobetrag". The "Ref #" label for invoice number is also absent from training data.
The model must perform semantic equivalence mapping across four non-standard labels
simultaneously, which exceeds the pattern recognition it has been trained on.

## Proposed Data Fix

Add 5 training examples using alternative field labels: "Billing Statement" as header,
"Reference" or "Ref #" for invoice number, "Billed On" or "Issued" for date, "Pay By"
or "Settle By" for due date, and "Taxable Amount" or "Net Amount" for subtotal. These
examples should span at least 3 different layout styles to avoid overfitting to a
single alternative-label pattern.
