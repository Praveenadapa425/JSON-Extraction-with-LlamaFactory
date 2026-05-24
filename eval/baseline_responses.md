# Baseline Model Responses

## Setup

Model: meta-llama/Llama-3.2-3B-Instruct (base -- no fine-tuning applied)
Inference tool: LlamaFactory inference tab
Temperature: 0.0
Date of evaluation: March 25, 2024

## Prompt Used for All 20 Documents

You are a document extraction system. Your task is to extract all fields from the document below and return ONLY a valid JSON object conforming to the schema. Do not write any explanation. Do not use markdown. Do not use code fences. Do not add any text before or after the JSON. Return only the raw JSON object starting with { and ending with }.

For invoices, the required fields are: vendor, invoice_number, date (YYYY-MM-DD), due_date (YYYY-MM-DD or null), currency (ISO 4217), subtotal (float), tax (float or null), total (float), line_items (array of objects with description, quantity, unit_price).

For purchase orders, the required fields are: buyer, supplier, po_number, date (YYYY-MM-DD), delivery_date (YYYY-MM-DD or null), currency (ISO 4217), total (float), items (array of objects with item_name, quantity, unit_price).

Use null for any field not found in the document. All monetary values must be floats. All dates must be YYYY-MM-DD strings.

Document:
{{DOCUMENT_TEXT}}

---

## eval_doc_01 Response
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

---
## eval_doc_02 Response
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

---
## eval_doc_03 Response
Sure! Here is the JSON:
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

---
## eval_doc_04 Response
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

---
## eval_doc_05 Response
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
  ],
  "CGST": "Rs. 16,650.00",
  "SGST": "Rs. 16,650.00"
}
```

---
## eval_doc_06 Response
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
  "Ref #": "BDW-7721",
  "Billed On": "2024-01-15",
  "Pay By": "2024-02-14",
  "Taxable Amount": 5680.0,
  "Amount Due": 6077.6
}

---
## eval_doc_07 Response
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

---
## eval_doc_08 Response
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

---
## eval_doc_09 Response
Sure! Here is the JSON:
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

---
## eval_doc_10 Response
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

---
## eval_doc_11 Response
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

---
## eval_doc_12 Response
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

---
## eval_doc_13 Response
{
  "buyer": "Indian Oil Corporation Ltd.",
  "supplier": "Thermax Ltd.",
  "po_number": "IOCL/PO/2024/REF/0334",
  "date": "2024-02-05",
  "delivery_date": "2024-05-05",
  "currency": "INR",
  "total": "Rs. 4,55,00,000.00 (including applicable taxes)",
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

---
## eval_doc_14 Response
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

---
## eval_doc_15 Response
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

---
## eval_doc_16 Response
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

---
## eval_doc_17 Response
```json
{
  "vendor": "Great Basin Environmental Consulting",
  "invoice_number": "GBEC-2024-0223",
  "date": "2024-03-05",
  "due_date": "2024-04-19",
  "currency": "USD",
  "subtotal": 15800.0,
  "tax": "Professional services exempt",
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

---
## eval_doc_18 Response
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

---
## eval_doc_19 Response
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

---
## eval_doc_20 Response
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

---
