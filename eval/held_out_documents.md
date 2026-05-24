# Held-Out Evaluation Documents

These 20 documents are the fixed test set. They are used for both the baseline
evaluation (Part C) and the post-fine-tuning evaluation (Part E). The same documents
and the same prompt are used both times to isolate the effect of fine-tuning.

---

## eval_doc_01

**Type:** invoice
**Expected Keys Present:** all (no null fields)
**Ground Truth:**
```json
{
  "vendor": "Atlas Engineering Solutions Ltd.",
  "invoice_number": "AES-2024-0482",
  "date": "2024-03-20",
  "due_date": "2024-04-19",
  "currency": "USD",
  "subtotal": 12450.0,
  "tax": 995.6,
  "total": 13445.6,
  "line_items": [
    {
      "description": "Structural Analysis Report",
      "quantity": 1,
      "unit_price": 4500.0
    },
    {
      "description": "CAD Drafting (per hour)",
      "quantity": 35,
      "unit_price": 120.0
    },
    {
      "description": "Site Inspection Visit",
      "quantity": 3,
      "unit_price": 650.0
    }
  ]
}
```

**Document Text:**
```
ATLAS ENGINEERING SOLUTIONS LTD.
2100 Commerce Plaza, Suite 400
Houston, TX 77002
Ph: (832) 555-0391

INVOICE

Invoice Number: AES-2024-0482
Date: March 20, 2024
Payment Due: April 19, 2024

Bill To:
Gulf States Petroleum Inc.
5500 Memorial Drive, Houston, TX

Description                        Qty    Rate       Amount
Structural Analysis Report          1     4,500.00   4,500.00
CAD Drafting (per hour)            35       120.00   4,200.00
Site Inspection Visit               3       650.00   1,950.00

Project: Refinery Tank Farm Expansion Phase 2

Subtotal:                                       $12,450.00
TX Sales Tax (7.99%):                              $995.60
Total Due:                                      $13,445.60

Wire transfer instructions enclosed.
Terms: Net 30
```

---

## eval_doc_02

**Type:** invoice
**Expected Keys Present:** due_date and tax are null
**Ground Truth:**
```json
{
  "vendor": "Silver Creek Pottery Studio",
  "invoice_number": "SCPS-1192",
  "date": "2024-02-10",
  "due_date": null,
  "currency": "USD",
  "subtotal": 1680.0,
  "tax": null,
  "total": 1680.0,
  "line_items": [
    {
      "description": "Custom stoneware dinner set (6-piece)",
      "quantity": 4,
      "unit_price": 320.0
    },
    {
      "description": "Glazing and kiln firing surcharge",
      "quantity": 4,
      "unit_price": 100.0
    }
  ]
}
```

**Document Text:**
```
Silver Creek Pottery Studio
82 River Road, Ashland, OR 97520

Invoice #SCPS-1192
Date: 02/10/2024

To: The Artisan Table Restaurant

Custom stoneware dinner set (6-piece)     4    320.00    1280.00
Glazing and kiln firing surcharge         4    100.00     400.00

Subtotal: $1,680.00
Total: $1,680.00

Handmade goods exempt from OR sales tax.
Payment due on pickup.
```

---

## eval_doc_03

**Type:** invoice
**Expected Keys Present:** all fields present
**Ground Truth:**
```json
{
  "vendor": "Klein und Weber Druckerei GmbH",
  "invoice_number": "KW-2024-0339",
  "date": "2024-01-18",
  "due_date": "2024-02-17",
  "currency": "EUR",
  "subtotal": 3840.0,
  "tax": 729.6,
  "total": 4569.6,
  "line_items": [
    {
      "description": "Business Card Print 350gsm (per 1000)",
      "quantity": 10,
      "unit_price": 120.0
    },
    {
      "description": "A3 Poster Full Colour (per 100)",
      "quantity": 8,
      "unit_price": 180.0
    },
    {
      "description": "Booklet A5 Saddle-Stitched 24pg (per 500)",
      "quantity": 4,
      "unit_price": 300.0
    }
  ]
}
```

**Document Text:**
```
Klein und Weber Druckerei GmbH
Gutenbergstrasse 15, 50823 Koeln
USt-IdNr: DE345678901

Rechnung / Invoice
Nr: KW-2024-0339
Datum: 18.01.2024
Zahlungsziel: 17.02.2024

Kunde: MediaMarkt Deutschland GmbH

+--------------------------------------------------+-----+---------+----------+
| Beschreibung                                     | Mge | Preis   | Gesamt   |
+--------------------------------------------------+-----+---------+----------+
| Business Card Print 350gsm (per 1000)            |  10 |  120.00 | 1,200.00 |
| A3 Poster Full Colour (per 100)                  |   8 |  180.00 | 1,440.00 |
| Booklet A5 Saddle-Stitched 24pg (per 500)        |   4 |  300.00 | 1,200.00 |
+--------------------------------------------------+-----+---------+----------+

Nettobetrag: 3,840.00 EUR
MwSt (19%): 729.60 EUR
Gesamtbetrag: 4,569.60 EUR

Bitte ueberweisen Sie an:
IBAN: DE12 3456 7890 1234 5678 90
```

---

## eval_doc_04

**Type:** invoice
**Expected Keys Present:** due_date and invoice_number are null
**Ground Truth:**
```json
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

**Document Text:**
```
Manchester Piano Tuning
Date: 2 March 2024

Client: Bridgewater Hall Concert Venue

Piano tuning and regulation service - Steinway Model B

Charge: 120.00 GBP

Payment on completion.
```

---

## eval_doc_05

**Type:** invoice
**Expected Keys Present:** all fields present
**Ground Truth:**
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
    },
    {
      "description": "Clutch Plate 14 inch",
      "quantity": 30,
      "unit_price": 1500.0
    },
    {
      "description": "Oil Filter Element (bulk pack of 10)",
      "quantity": 40,
      "unit_price": 750.0
    }
  ]
}
```

**Document Text:**
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

---

## eval_doc_06

**Type:** invoice
**Expected Keys Present:** all fields present
**Ground Truth:**
```json
{
  "vendor": "Black Diamond Welding LLC",
  "invoice_number": "BDW-7721",
  "date": "2024-01-15",
  "due_date": "2024-02-14",
  "currency": "USD",
  "subtotal": 5680.0,
  "tax": 397.6,
  "total": 6077.6,
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

**Document Text:**
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

---

## eval_doc_07

**Type:** purchase_order
**Expected Keys Present:** all fields present
**Ground Truth:**
```json
{
  "buyer": "Cedar Rapids School District",
  "supplier": "Midwest Laboratory Equipment Co.",
  "po_number": "CRSD-PO-2024-0189",
  "date": "2024-03-01",
  "delivery_date": "2024-03-20",
  "currency": "USD",
  "total": 18450.0,
  "items": [
    {
      "item_name": "Student Microscope Binocular LED",
      "quantity": 30,
      "unit_price": 285.0
    },
    {
      "item_name": "Chemistry Glassware Starter Kit",
      "quantity": 15,
      "unit_price": 420.0
    },
    {
      "item_name": "Safety Goggles Splash-Proof (class set of 30)",
      "quantity": 4,
      "unit_price": 187.5
    }
  ]
}
```

**Document Text:**
```
CEDAR RAPIDS SCHOOL DISTRICT
District Administration Office
2500 Edgewood Road NW, Cedar Rapids, IA 52405

PURCHASE ORDER

PO Number: CRSD-PO-2024-0189
Date: March 1, 2024
Required Delivery: March 20, 2024

Vendor: Midwest Laboratory Equipment Co.
840 Science Parkway, Des Moines, IA

Student Microscope Binocular LED              30     285.00     8550.00
Chemistry Glassware Starter Kit               15     420.00     6300.00
Safety Goggles Splash-Proof (class set of 30)  4     187.50      750.00

Note: Subtotal for microscopes and kits does not include goggles set pricing shown.

Order Total: $18,450.00

Funding Source: STEM Grant FY2024
Approved: Dr. Patricia Yang, Superintendent
```

---

## eval_doc_08

**Type:** purchase_order
**Expected Keys Present:** delivery_date is null
**Ground Truth:**
```json
{
  "buyer": "Edinburgh Festival Fringe Society",
  "supplier": "Highland Timber Staging Ltd.",
  "po_number": "EFFS-PO-2024-0334",
  "date": "2024-02-28",
  "delivery_date": null,
  "currency": "GBP",
  "total": 14200.0,
  "items": [
    {
      "item_name": "Modular Stage Platform 2x1m",
      "quantity": 20,
      "unit_price": 450.0
    },
    {
      "item_name": "Stage Riser Leg Set (adjustable 40-60cm)",
      "quantity": 20,
      "unit_price": 120.0
    },
    {
      "item_name": "Plywood Stage Top 18mm (2x1m)",
      "quantity": 20,
      "unit_price": 85.0
    },
    {
      "item_name": "Stage Skirt Black 1m Drop (per metre)",
      "quantity": 40,
      "unit_price": 22.5
    }
  ]
}
```

**Document Text:**
```
Buyer: Edinburgh Festival Fringe Society
Address: 180 High Street, Edinburgh EH1 1QS
Supplier: Highland Timber Staging Ltd.
PO Number: EFFS-PO-2024-0334
Date: 28 Feb 2024

Modular Stage Platform 2x1m                      20    450.00
Stage Riser Leg Set (adjustable 40-60cm)          20    120.00
Plywood Stage Top 18mm (2x1m)                     20     85.00
Stage Skirt Black 1m Drop (per metre)             40     22.50

Total: 14,200.00 GBP

Delivery date dependent on venue allocation schedule.
Contact: Festival Production Manager
```

---

## eval_doc_09

**Type:** invoice
**Expected Keys Present:** due_date is null
**Ground Truth:**
```json
{
  "vendor": "Beacon Hill Locksmith",
  "invoice_number": "BHL-4490",
  "date": "2024-03-12",
  "due_date": null,
  "currency": "USD",
  "subtotal": 485.0,
  "tax": 30.53,
  "total": 515.53,
  "line_items": [
    {
      "description": "Commercial lock rekey (per lock)",
      "quantity": 6,
      "unit_price": 55.0
    },
    {
      "description": "Deadbolt installation Grade 1",
      "quantity": 1,
      "unit_price": 155.0
    }
  ]
}
```

**Document Text:**
```
Beacon Hill Locksmith
14 Charles Street, Boston, MA 02114

Invoice No: BHL-4490
Date: 2024-03-12

Customer: Beacon Hill Bistro

Commercial lock rekey (per lock)       6    55.00     330.00
Deadbolt installation Grade 1         1   155.00     155.00

Subtotal: $485.00
MA Sales Tax (6.295%): $30.53
Total: $515.53

Paid at completion. Receipt copy for records.
```

---

## eval_doc_10

**Type:** purchase_order
**Expected Keys Present:** all fields present
**Ground Truth:**
```json
{
  "buyer": "Renault Sport Racing",
  "supplier": "Brembo Brake Systems S.p.A.",
  "po_number": "RSR-PO-2024-0891",
  "date": "2024-01-30",
  "delivery_date": "2024-02-28",
  "currency": "EUR",
  "total": 89400.0,
  "items": [
    {
      "item_name": "Carbon Brake Disc 380mm (PN-CBD-380)",
      "quantity": 24,
      "unit_price": 2200.0
    },
    {
      "item_name": "Racing Caliper 6-Piston (PN-RC6P-F1)",
      "quantity": 12,
      "unit_price": 3100.0
    },
    {
      "item_name": "Brake Pad Compound Ultra Endurance",
      "quantity": 48,
      "unit_price": 250.0
    }
  ]
}
```

**Document Text:**
```
Renault Sport Racing
Avenue du President Kennedy, 91170 Viry-Chatillon, France

Purchase Order
PO Number: RSR-PO-2024-0891
Date: 30/01/2024
Delivery Required: 28/02/2024

Vendor: Brembo Brake Systems S.p.A.
Via Brembo 25, 24035 Curno, Italy

+-------------------------------------------------+-----+---------+-----------+
| Part Description                                | Qty | Unit    | Total     |
+-------------------------------------------------+-----+---------+-----------+
| Carbon Brake Disc 380mm (PN-CBD-380)            |  24 | 2200.00 | 52,800.00 |
| Racing Caliper 6-Piston (PN-RC6P-F1)            |  12 | 3100.00 | 37,200.00 |
| Brake Pad Compound Ultra Endurance              |  48 |  250.00 | 12,000.00 |
+-------------------------------------------------+-----+---------+-----------+

PO Total: 89,400.00 EUR

Note: Total reflects revised quantity. Original PO amount was higher.
FIA homologation certificates required.
Approved: Jean-Pierre Moreau, Technical Director
```

---

## eval_doc_11

**Type:** invoice
**Expected Keys Present:** all fields present
**Ground Truth:**
```json
{
  "vendor": "Cascade Window Cleaning Inc.",
  "invoice_number": "CWC-2024-0187",
  "date": "2024-02-28",
  "due_date": "2024-03-29",
  "currency": "USD",
  "subtotal": 4850.0,
  "tax": 461.85,
  "total": 5311.85,
  "line_items": [
    {
      "description": "Exterior window cleaning (per floor)",
      "quantity": 12,
      "unit_price": 280.0
    },
    {
      "description": "Interior window cleaning (per floor)",
      "quantity": 5,
      "unit_price": 145.0
    },
    {
      "description": "Skylight cleaning (per unit)",
      "quantity": 4,
      "unit_price": 95.0
    },
    {
      "description": "Pressure washing entrance (per session)",
      "quantity": 2,
      "unit_price": 125.0
    }
  ]
}
```

**Document Text:**
```
CASCADE WINDOW CLEANING INC.
900 Pike Street, Seattle, WA 98101
License: CASCAWC*846J2

INVOICE

Invoice No: CWC-2024-0187
Invoice Date: February 28, 2024
Due Date: March 29, 2024

Bill To: Amazon Corporate Campus
410 Terry Ave N, Seattle, WA

Description                               Qty    Rate       Amount
Exterior window cleaning (per floor)       12     280.00     3,360.00
Interior window cleaning (per floor)        5     145.00       725.00
Skylight cleaning (per unit)                4      95.00       380.00
Pressure washing entrance (per session)     2     125.00       250.00

Subtotal:                                               $4,850.00
WA B&O Tax (9.52%):                                       $461.85
Total:                                                  $5,311.85

Scheduled service - monthly contract.
```

---

## eval_doc_12

**Type:** invoice
**Expected Keys Present:** due_date is null
**Ground Truth:**
```json
{
  "vendor": "Osaka Printing Works Co. Ltd.",
  "invoice_number": "OPW-2024-0892",
  "date": "2024-03-10",
  "due_date": null,
  "currency": "JPY",
  "subtotal": 284000.0,
  "tax": 28400.0,
  "total": 312400.0,
  "line_items": [
    {
      "description": "Catalog A4 Full Colour 48 Pages (per 1000)",
      "quantity": 5,
      "unit_price": 42000.0
    },
    {
      "description": "Brochure A5 Tri-Fold (per 1000)",
      "quantity": 8,
      "unit_price": 8500.0
    },
    {
      "description": "Poster B2 Matte Finish (per 100)",
      "quantity": 6,
      "unit_price": 12000.0
    }
  ]
}
```

**Document Text:**
```
Vendor: Osaka Printing Works Co. Ltd.
Address: 4-2-10 Nanba, Chuo-ku, Osaka 542-0076
Invoice No: OPW-2024-0892
Date: 2024.03.10

Client: Panasonic Holdings Corporation

Catalog A4 Full Colour 48 Pages (per 1000)     5    42000    210000
Brochure A5 Tri-Fold (per 1000)                 8     8500     68000
Poster B2 Matte Finish (per 100)                6    12000     72000

Subtotal: 284,000 JPY
Consumption Tax (10%): 28,400 JPY
Total: 312,400 JPY

Payable within agreed corporate terms.
```

---

## eval_doc_13

**Type:** purchase_order
**Expected Keys Present:** all fields present
**Ground Truth:**
```json
{
  "buyer": "Indian Oil Corporation Ltd.",
  "supplier": "Thermax Ltd.",
  "po_number": "IOCL/PO/2024/REF/0334",
  "date": "2024-02-05",
  "delivery_date": "2024-05-05",
  "currency": "INR",
  "total": 45500000.0,
  "items": [
    {
      "item_name": "Heat Exchanger Shell and Tube (custom)",
      "quantity": 3,
      "unit_price": 8500000.0
    },
    {
      "item_name": "Pressure Vessel ASME Coded",
      "quantity": 2,
      "unit_price": 5200000.0
    },
    {
      "item_name": "Industrial Boiler Economizer",
      "quantity": 2,
      "unit_price": 2350000.0
    }
  ]
}
```

**Document Text:**
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

---

## eval_doc_14

**Type:** purchase_order
**Expected Keys Present:** delivery_date is null
**Ground Truth:**
```json
{
  "buyer": "Austin City Parks Department",
  "supplier": "Lone Star Irrigation Systems",
  "po_number": "ACPD-PO-2024-0112",
  "date": "2024-03-15",
  "delivery_date": null,
  "currency": "USD",
  "total": 8940.0,
  "items": [
    {
      "item_name": "Commercial Sprinkler Head Pop-Up (case of 20)",
      "quantity": 30,
      "unit_price": 198.0
    },
    {
      "item_name": "PVC Pipe Schedule 40 2-inch (per 10ft)",
      "quantity": 200,
      "unit_price": 15.0
    }
  ]
}
```

**Document Text:**
```
Austin City Parks Department
919 Congress Avenue, Austin, TX 78701

Purchase Order
PO#: ACPD-PO-2024-0112
Date: March 15, 2024

Supplier: Lone Star Irrigation Systems
San Antonio, TX

Commercial Sprinkler Head Pop-Up (case of 20)     30    198.00
PVC Pipe Schedule 40 2-inch (per 10ft)           200     15.00

Total: $8,940.00

Delivery will be scheduled after site survey completion.
Contact: Parks Maintenance Division
```

---

## eval_doc_15

**Type:** invoice
**Expected Keys Present:** tax is null
**Ground Truth:**
```json
{
  "vendor": "Prairie Wind Music Academy",
  "invoice_number": "PWM-2024-0891",
  "date": "2024-01-08",
  "due_date": "2024-02-07",
  "currency": "USD",
  "subtotal": 2160.0,
  "tax": null,
  "total": 2160.0,
  "line_items": [
    {
      "description": "Private piano lesson (1 hour)",
      "quantity": 16,
      "unit_price": 85.0
    },
    {
      "description": "Music theory group class (per session)",
      "quantity": 8,
      "unit_price": 45.0
    }
  ]
}
```

**Document Text:**
```
Prairie Wind Music Academy
445 Harmony Lane, Wichita, KS 67202

Invoice No: PWM-2024-0891
Date: 8 Jan 24
Due: 7 Feb 24

Student: Emily Richardson
Parent/Guardian: Mark Richardson

Private piano lesson (1 hour)              16    85.00    1360.00
Music theory group class (per session)      8    45.00     360.00

Subtotal: $2,160.00
Total Due: $2,160.00

Music instruction exempt from KS sales tax.
Payable by check or bank transfer.
```

---

## eval_doc_16

**Type:** purchase_order
**Expected Keys Present:** all fields present
**Ground Truth:**
```json
{
  "buyer": "KLM Royal Dutch Airlines",
  "supplier": "Fokker Services B.V.",
  "po_number": "KLM-PO-2024-7712",
  "date": "2024-02-20",
  "delivery_date": "2024-04-15",
  "currency": "EUR",
  "total": 186500.0,
  "items": [
    {
      "item_name": "Landing Gear Overhaul Kit (PN-LGOK-737)",
      "quantity": 4,
      "unit_price": 32000.0
    },
    {
      "item_name": "APU Starter Motor Replacement (PN-APU-SM)",
      "quantity": 6,
      "unit_price": 5500.0
    },
    {
      "item_name": "Cabin Pressure Valve Assembly (PN-CPVA-200)",
      "quantity": 10,
      "unit_price": 2150.0
    }
  ]
}
```

**Document Text:**
```
KLM ROYAL DUTCH AIRLINES
Amstelveen, The Netherlands
KvK: 33014286

PURCHASE ORDER

PO Number: KLM-PO-2024-7712
Issued: 20/02/2024
Delivery By: 15/04/2024

Supplier: Fokker Services B.V.
Hoofddorp, The Netherlands

Landing Gear Overhaul Kit (PN-LGOK-737)        4    32,000.00    128,000.00
APU Starter Motor Replacement (PN-APU-SM)      6     5,500.00     33,000.00
Cabin Pressure Valve Assembly (PN-CPVA-200)   10     2,150.00     21,500.00

PO Total: EUR 186,500.00

All items must comply with EASA Part 145 standards.
Ship to: KLM Engineering, Hangar 14, Schiphol
```

---

## eval_doc_17

**Type:** invoice
**Expected Keys Present:** all fields present
**Ground Truth:**
```json
{
  "vendor": "Great Basin Environmental Consulting",
  "invoice_number": "GBEC-2024-0223",
  "date": "2024-03-05",
  "due_date": "2024-04-19",
  "currency": "USD",
  "subtotal": 15800.0,
  "tax": null,
  "total": 15800.0,
  "line_items": [
    {
      "description": "Phase II Environmental Site Assessment",
      "quantity": 1,
      "unit_price": 8500.0
    },
    {
      "description": "Soil Sampling and Laboratory Analysis",
      "quantity": 1,
      "unit_price": 4200.0
    },
    {
      "description": "Environmental Report and Remediation Plan",
      "quantity": 1,
      "unit_price": 3100.0
    }
  ]
}
```

**Document Text:**
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

---

## eval_doc_18

**Type:** purchase_order
**Expected Keys Present:** delivery_date is null
**Ground Truth:**
```json
{
  "buyer": "Portland Art Museum",
  "supplier": "Archival Methods Inc.",
  "po_number": "PAM-PO-2024-0078",
  "date": "2024-02-15",
  "delivery_date": null,
  "currency": "USD",
  "total": 3640.0,
  "items": [
    {
      "item_name": "Museum Storage Box Acid-Free (18x24x3)",
      "quantity": 80,
      "unit_price": 45.5
    }
  ]
}
```

**Document Text:**
```
From: Portland Art Museum
To: Archival Methods Inc.
PO: PAM-PO-2024-0078
Date: February 15, 2024

Museum Storage Box Acid-Free (18x24x3)     80    45.50

Total: $3,640.00

Delivery TBD. Contact Registrar's office for scheduling.
```

---

## eval_doc_19

**Type:** invoice
**Expected Keys Present:** all fields present
**Ground Truth:**
```json
{
  "vendor": "Cornwall Surf School Ltd.",
  "invoice_number": "CSS-2024-0891",
  "date": "2024-03-15",
  "due_date": "2024-04-14",
  "currency": "GBP",
  "subtotal": 3400.0,
  "tax": 680.0,
  "total": 4080.0,
  "line_items": [
    {
      "description": "Group Surf Lesson 2hr (per person)",
      "quantity": 40,
      "unit_price": 45.0
    },
    {
      "description": "Wetsuit Hire Full Day (per person)",
      "quantity": 40,
      "unit_price": 25.0
    },
    {
      "description": "Surfboard Rental Full Day (per unit)",
      "quantity": 40,
      "unit_price": 15.0
    }
  ]
}
```

**Document Text:**
```
Cornwall Surf School Ltd.
Fistral Beach, Newquay, Cornwall TR7 1HY
VAT Registration: GB 111 2233 44

TAX INVOICE

Invoice Reference: CSS-2024-0891
Raised: 15/03/2024
Due: 14/04/2024

Customer: National Trust - Outdoor Education Programme

+----------------------------------------------+-----+--------+----------+
| Service                                      | Pax | Rate   | Total    |
+----------------------------------------------+-----+--------+----------+
| Group Surf Lesson 2hr (per person)           |  40 |  45.00 | 1,800.00 |
| Wetsuit Hire Full Day (per person)           |  40 |  25.00 | 1,000.00 |
| Surfboard Rental Full Day (per unit)         |  40 |  15.00 |   600.00 |
+----------------------------------------------+-----+--------+----------+

Net Amount: 3,400.00
VAT @ 20%: 680.00
Invoice Total: 4,080.00 GBP
```

---

## eval_doc_20

**Type:** purchase_order
**Expected Keys Present:** all fields present
**Ground Truth:**
```json
{
  "buyer": "Colorado Department of Transportation",
  "supplier": "Rocky Mountain Safety Products",
  "po_number": "CDOT-PO-2024-1445",
  "date": "2024-03-10",
  "delivery_date": "2024-03-25",
  "currency": "USD",
  "total": 34200.0,
  "items": [
    {
      "item_name": "Highway Safety Cone 28in Reflective (case of 20)",
      "quantity": 50,
      "unit_price": 145.0
    },
    {
      "item_name": "Barricade Light Type C Solar",
      "quantity": 100,
      "unit_price": 62.0
    },
    {
      "item_name": "Road Sign Reflective 30x30 Custom Print",
      "quantity": 80,
      "unit_price": 89.0
    },
    {
      "item_name": "Traffic Delineator Post 48in Flexible",
      "quantity": 200,
      "unit_price": 34.0
    }
  ]
}
```

**Document Text:**
```
COLORADO DEPARTMENT OF TRANSPORTATION
4201 E Arkansas Ave, Denver, CO 80222

PURCHASE ORDER

PO Number: CDOT-PO-2024-1445
Date: March 10, 2024
Required By: March 25, 2024

Vendor: Rocky Mountain Safety Products
1800 Industrial Drive, Colorado Springs, CO

Highway Safety Cone 28in Reflective (case of 20)    50    145.00     7,250.00
Barricade Light Type C Solar                        100     62.00     6,200.00
Road Sign Reflective 30x30 Custom Print              80     89.00     7,120.00
Traffic Delineator Post 48in Flexible               200     34.00     6,800.00

Note: Line item totals reflect a different computation for sign printing.

Total PO Value: $34,200.00

State procurement contract #CDOT-2024-SP-088
Approved: James Rodriguez, Regional Maintenance Director
```

