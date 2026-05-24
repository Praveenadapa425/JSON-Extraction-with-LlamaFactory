# Purchase Order Extraction Schema

## Overview

This document defines the canonical JSON schema for extracting structured data
from purchase order (PO) documents. Every training example and every model output
must conform to this schema exactly.

## Schema Definition

```json
{
  "buyer": "string",
  "supplier": "string",
  "po_number": "string",
  "date": "YYYY-MM-DD",
  "delivery_date": "YYYY-MM-DD or null",
  "currency": "ISO 4217 three-letter code",
  "total": "float",
  "items": [
    {
      "item_name": "string",
      "quantity": "integer",
      "unit_price": "float"
    }
  ]
}
```

## Field Specifications

### buyer
- Type: string
- Description: The organization issuing the purchase order, placing the order,
  and committing to payment upon delivery. Appears in "Bill To", "Issued By",
  or "Purchase Order From" sections.
- When absent: Use null.

### supplier
- Type: string
- Description: The organization receiving the purchase order and responsible for
  fulfilling it. Appears in "Vendor", "Supplier", "Ship From" sections.
- When absent: Use null.

### po_number
- Type: string
- Description: The unique purchase order number assigned by the buyer. Used to
  track the order through the procurement cycle.
- Format: Preserve exactly as shown (e.g., "PO-2024-0012", "4500123456").
- When absent: Use null.

### date
- Type: string
- Description: The date the purchase order was issued by the buyer.
- Format: YYYY-MM-DD. Convert from any source format.
- When absent: Use null.

### delivery_date
- Type: string or null
- Description: Expected or requested delivery date. Also labeled "Required By",
  "Ship By", "Expected Delivery", or "Delivery On".
- Format: YYYY-MM-DD.
- When absent: Use null.

### currency
- Type: string
- Description: The currency for all monetary values in the PO.
- Format: Three-letter ISO 4217 code. Apply same symbol-to-code inference
  as the invoice schema.
- When absent with no symbol: Use null.

### total
- Type: float
- Description: The total monetary commitment of this purchase order, typically
  labeled "PO Total", "Order Total", or "Total Value".
- Format: Numeric float.
- When absent: Compute from items if possible. Use null if unresolvable.

### items
- Type: array of objects
- Description: Each line item ordered. Each object contains:
  - item_name (string): Product name, part number, or description.
    Include part codes if present.
  - quantity (integer): Number of units ordered.
  - unit_price (float): Agreed price per unit.
- When absent: Use empty array []. Never null.

## Null Handling Convention

Same convention as the invoice schema. All keys must be present. Missing
fields use null. Empty item lists use [].

## Compliant Output Example

```json
{
  "buyer": "Orion Manufacturing Co.",
  "supplier": "Precision Parts International Ltd.",
  "po_number": "PO-2024-00412",
  "date": "2024-02-08",
  "delivery_date": "2024-03-01",
  "currency": "GBP",
  "total": 24800.00,
  "items": [
    {"item_name": "Stainless Steel Shaft (PN-SS-440C)", "quantity": 200, "unit_price": 62.00},
    {"item_name": "Bearing Housing Assembly", "quantity": 50, "unit_price": 148.00},
    {"item_name": "Sealing Ring Set (16-piece)", "quantity": 100, "unit_price": 32.00}
  ]
}
```

## Example With Missing Optional Fields

```json
{
  "buyer": "Harrington Logistics Group",
  "supplier": "Westfield Office Supplies",
  "po_number": "HL-PO-0887",
  "date": "2024-04-10",
  "delivery_date": null,
  "currency": "USD",
  "total": 1247.50,
  "items": [
    {"item_name": "A4 Copy Paper (Box of 5 reams)", "quantity": 10, "unit_price": 52.50},
    {"item_name": "Ballpoint Pen (Box of 50)", "quantity": 5, "unit_price": 24.50},
    {"item_name": "Stapler Heavy Duty", "quantity": 3, "unit_price": 38.00},
    {"item_name": "Manila File Folders (100-pack)", "quantity": 4, "unit_price": 18.50}
  ]
}
```
