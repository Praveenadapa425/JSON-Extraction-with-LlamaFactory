# Invoice Extraction Schema

## Overview

This document defines the canonical JSON schema for extracting structured data
from invoice documents. Every training example and every model output must conform
to this schema exactly. No deviation in key names, data types, or null handling
conventions is acceptable.

## Schema Definition

```json
{
  "vendor": "string",
  "invoice_number": "string",
  "date": "YYYY-MM-DD",
  "due_date": "YYYY-MM-DD or null",
  "currency": "ISO 4217 three-letter code",
  "subtotal": "float",
  "tax": "float or null",
  "total": "float",
  "line_items": [
    {
      "description": "string",
      "quantity": "integer",
      "unit_price": "float"
    }
  ]
}
```

## Field Specifications

### vendor
- Type: string
- Description: The full legal name of the company or individual issuing the invoice.
  This is the seller or service provider, not the buyer.
- Format: Preserve the name exactly as it appears in the document header or letterhead,
  including abbreviations such as "Ltd.", "Inc.", "Pvt.".
- When absent: Use null. Do not invent or infer a vendor name.

### invoice_number
- Type: string
- Description: The unique identifier assigned to this invoice by the issuing vendor.
  May contain letters, digits, hyphens, and slashes.
- Format: Preserve exactly as shown (e.g., "INV-2024-0431", "00842", "SI/2024/MAR/001").
- When absent: Use null.

### date
- Type: string
- Description: The date the invoice was issued.
- Format: Must be converted to YYYY-MM-DD regardless of the source format.
  "March 15, 2024" becomes "2024-03-15". "15/03/24" becomes "2024-03-15".
- When absent: Use null.

### due_date
- Type: string or null
- Description: The payment due date. Also labeled "Payment Due", "Pay By", "Net 30" 
  (in which case calculate from issue date), or equivalent.
- Format: YYYY-MM-DD. Convert from any source format.
- When absent: Use null. Do not calculate from issue date unless "Net X days" is stated.

### currency
- Type: string
- Description: The currency in which all monetary values are denominated.
- Format: Three-letter ISO 4217 code. Infer from symbol when code not stated:
  $ -> USD, GBP -> GBP, EUR -> EUR, INR -> INR, JPY -> JPY.
  When bare $ appears with no country context, default to USD.
- When absent with no symbol: Use null.

### subtotal
- Type: float
- Description: The sum of all line item amounts before tax. If not separately labeled
  but line items are present, compute as sum of (quantity * unit_price) for all items.
- Format: Numeric float. No currency symbols, no commas.
- When absent and not computable: Use null.

### tax
- Type: float or null
- Description: The total tax amount charged. Covers VAT, GST, sales tax, expressed as
  a monetary amount, not a percentage. If a percentage is given but no amount, compute
  the amount as tax_rate * subtotal.
- Format: Numeric float.
- When absent with no rate to compute from: Use null. Never default to 0.

### total
- Type: float
- Description: The final amount due, including all taxes and fees. Labeled "Total",
  "Amount Due", "Grand Total", or equivalent.
- Format: Numeric float.
- When absent: Compute from subtotal + tax if both are available. Otherwise use null.

### line_items
- Type: array of objects
- Description: Each individual product or service billed. Each object contains:
  - description (string): Name or description of the item or service.
  - quantity (integer): Number of units. Round fractional units to nearest integer.
  - unit_price (float): Price per single unit before discount or tax.
- When absent: Use an empty array []. Never use null for line_items.
- When quantities are not listed: Use 1 as the default quantity.

## Null Handling Convention

Fields marked "or null" must use JSON null -- not the string "null", not empty
string "", not 0, not "N/A". All keys must be present in every output. A missing
key is a schema violation. null signals the field was examined but not found.

## Compliant Output Example

```json
{
  "vendor": "Meridian Supplies Pvt. Ltd.",
  "invoice_number": "INV-2024-00834",
  "date": "2024-03-15",
  "due_date": "2024-04-14",
  "currency": "INR",
  "subtotal": 87500.00,
  "tax": 15750.00,
  "total": 103250.00,
  "line_items": [
    {"description": "Industrial Valve Assembly", "quantity": 50, "unit_price": 1200.00},
    {"description": "Pipe Fitting Kit (16mm)", "quantity": 100, "unit_price": 350.00},
    {"description": "Installation Labour", "quantity": 2, "unit_price": 8750.00}
  ]
}
```

## Example With Missing Optional Fields

```json
{
  "vendor": "Coastal Print Works",
  "invoice_number": "CPW-0091",
  "date": "2024-01-22",
  "due_date": null,
  "currency": "USD",
  "subtotal": 540.00,
  "tax": null,
  "total": 540.00,
  "line_items": [
    {"description": "Business Card Print (500 units)", "quantity": 1, "unit_price": 540.00}
  ]
}
```
