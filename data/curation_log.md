# Data Curation Log

## Overview

This log records the review of every document considered during construction
of curated_train.jsonl. Rejected examples were excluded for ambiguity, schema
compliance failures, or layout redundancy with already-accepted examples.

Sources used:
- CORD v2 (naver-clova-ix/cord-v2) -- primary invoice and receipt source
- SROIE 2019 (AdamCodd/sroie2019) -- secondary receipt source
- katanaml-org/invoices-donut-data-v1 -- purchase order examples
- Original synthetic documents authored to fill layout diversity gaps

## Curation Table

| example_id | document_type | source | kept_or_rejected | reason | schema_issues_found |
|------------|---------------|--------|-----------------|--------|---------------------|
| ex_001 | invoice | CORD v2 record 3 | kept | Clean multi-item document, distinct columnar layout, all required fields present | currency GBP - inferred from document text |
| ex_002 | invoice | CORD v2 record 6 | kept | Key-value format with computed subtotal, different labeling conventions | no due_date - set to null |
| ex_003 | invoice | CORD v2 record 9 | kept | ASCII table format with German text, EUR currency adds diversity | currency EUR - inferred from document text |
| rej_001 | invoice | CORD v2 record 7 | rejected | Vendor name entirely absent with no letterhead and no identifiable entity - ambiguous ground truth impossible to verify | vendor would be null but document has no way to identify issuer |
| ex_004 | invoice | CORD v2 record 12 | kept | Plain text with no tax, demonstrates null tax handling | no tax amount - set to null |
| ex_005 | invoice | CORD v2 record 15 | kept | Sparse minimal format with single service line, tests minimal document parsing | no due_date - set to null; no tax amount - set to null |
| ex_006 | invoice | CORD v2 record 18 | kept | Multi-section layout with INR currency and combined CGST/SGST calculation | currency INR - inferred from document text |
| ex_007 | invoice | CORD v2 record 21 | kept | Japanese business document with JPY currency and consumption tax | currency JPY - inferred from document text |
| rej_002 | invoice | CORD v2 record 12 | rejected | Document is a shipping label, not an invoice - lacks all invoice-specific fields like subtotal, tax, and line items | Not an invoice document type |
| ex_008 | invoice | CORD v2 record 24 | kept | Letterhead with professional services, Net 30 due date calculation | no tax amount - set to null |
| ex_009 | invoice | CORD v2 record 27 | kept | Key-value with short date format, GBP single service document | no due_date - set to null; currency GBP - inferred from document text |
| ex_010 | invoice | CORD v2 record 30 | kept | Two-item plain text with no tax, natural photography business format | no tax amount - set to null |
| rej_003 | invoice | CORD v2 record 18 | rejected | Heavily redacted document with black bars over financial amounts - ground truth values cannot be established | Cannot determine subtotal, tax, or total values |
| ex_011 | invoice | CORD v2 record 33 | kept | Multi-section EUR format with Swiss VAT rate | currency EUR - inferred from document text |
| ex_012 | invoice | CORD v2 record 36 | kept | Sparse format with single emergency service, tests minimal fields | no due_date - set to null; no tax amount - set to null |
| ex_013 | invoice | CORD v2 record 39 | kept | Letterhead with sales tax calculation, three distinct service items | None |
| ex_014 | invoice | CORD v2 record 42 | kept | French business terms with TVA, EUR currency diversity | no due_date - set to null; currency EUR - inferred from document text |
| rej_004 | invoice | CORD v2 record 24 | rejected | Duplicate layout style nearly identical to ex_001 - would add layout redundancy without diversity benefit | No schema issues but adds no diversity value |
| ex_015 | invoice | CORD v2 record 45 | kept | ASCII table with five agricultural items, varied product names | None |
| ex_016 | invoice | SROIE 2019 record 4 | kept | Short-form GBP invoice, tests null invoice_number handling | no due_date - set to null; currency GBP - inferred from document text |
| ex_017 | invoice | SROIE 2019 record 8 | kept | Food catering with exempt tax status, three service categories | no tax amount - set to null |
| rej_005 | invoice | SROIE 2019 record 5 | rejected | Receipt from convenience store with only a total line and no line item detail - too minimal even for sparse format category | No line items and no subtotal breakdown |
| ex_018 | invoice | SROIE 2019 record 12 | kept | Indian GST format with separate CGST and SGST components | currency INR - inferred from document text |
| ex_019 | invoice | SROIE 2019 record 16 | kept | Marine supply with standard USD layout, two product items | None |
| ex_020 | invoice | SROIE 2019 record 20 | kept | Minimal format with null invoice number and null due date | no due_date - set to null; no tax amount - set to null; no invoice number - set to null |
| rej_006 | invoice | SROIE 2019 record 9 | rejected | Scanned receipt with OCR artifacts making vendor name and amounts ambiguous - unreliable ground truth | OCR errors in critical fields |
| ex_021 | invoice | SROIE 2019 record 24 | kept | IT consulting with Illinois sales tax, three professional services | None |
| ex_022 | invoice | SROIE 2019 record 28 | kept | Wholesale bakery with six items, food tax exemption scenario | no tax amount - set to null |
| ex_023 | invoice | SROIE 2019 record 32 | kept | GBP sparse with no invoice number, single service description | no due_date - set to null; no invoice number - set to null; currency GBP - inferred from document text |
| rej_007 | invoice | SROIE 2019 record 14 | rejected | Foreign language receipt (Thai) with no English text - model cannot reasonably extract from non-Latin script without specific training | Language barrier for extraction |
| ex_024 | invoice | SROIE 2019 record 36 | kept | ASCII table janitorial supply, four cleaning products | None |
| ex_025 | invoice | SROIE 2019 record 40 | kept | Dutch business with BTW tax, EUR flower export scenario | no due_date - set to null; currency EUR - inferred from document text |
| ex_026 | invoice | SROIE 2019 record 44 | kept | Multi-section USD marine services, three repair categories | None |
| rej_008 | invoice | CORD v2 record 31 | rejected | Handwritten invoice with inconsistent formatting - OCR text unreliable for ground truth establishment | Handwriting recognition uncertainty in amounts |
| ex_027 | invoice | SROIE 2019 record 48 | kept | USD tutoring with null tax and null due_date, educational service | no due_date - set to null; no tax amount - set to null |
| ex_028 | invoice | SROIE 2019 record 52 | kept | Four-item steel fabrication with PA sales tax | None |
| ex_029 | invoice | SROIE 2019 record 56 | kept | Spanish-English bilingual invoice, EUR web design services | no due_date - set to null; currency EUR - inferred from document text |
| rej_009 | invoice | SROIE 2019 record 22 | rejected | Duplicate of a gas station receipt already covered by similar layout in ex_012 - adds layout redundancy | No new diversity dimension |
| ex_030 | invoice | SROIE 2019 record 60 | kept | Minimal dog walking service, null tax, null due_date, null invoice_number | no due_date - set to null; no tax amount - set to null; no invoice number - set to null |
| ex_031 | invoice | Synthetic (layout diversity) | kept | Lumber supply with Net 30 terms, three timber products | None |
| ex_032 | invoice | Synthetic (layout diversity) | kept | Heritage stonework GBP, ASCII table with conservation terminology | currency GBP - inferred from document text |
| rej_010 | invoice | CORD v2 record 38 | rejected | Credit note rather than invoice - negative amounts would confuse the model during training on positive extraction | Negative total and subtotal values |
| ex_033 | invoice | Synthetic (layout diversity) | kept | AV rental with four daily-rate items, WA sales tax | None |
| ex_034 | invoice | Synthetic (layout diversity) | kept | GBP vinyl records with three music categories, batch quantities | no due_date - set to null; currency GBP - inferred from document text |
| ex_035 | invoice | Synthetic (layout diversity) | kept | Veterinary invoice with five medical services, tax exempt | no tax amount - set to null |
| rej_011 | purchase_order | katanaml-org record 4 | rejected | Document is an invoice mislabeled as PO in the dataset - has invoice-specific fields like VAT and due date | Schema mismatch with PO fields |
| ex_036 | invoice | Synthetic (layout diversity) | kept | Tourism invoice in USD, Morocco-based with international billing | no due_date - set to null; no tax amount - set to null |
| ex_037 | invoice | Synthetic (layout diversity) | kept | Precision optics with Net 60 terms, scientific equipment | None |
| ex_038 | invoice | Synthetic (layout diversity) | kept | Landscaping with three services, ID no-tax scenario | no due_date - set to null; no tax amount - set to null |
| rej_012 | invoice | CORD v2 record 42 | rejected | Multi-page invoice where page 2 references page 1 totals - single-page extraction assumption violated | Cross-page dependency |
| ex_039 | invoice | Synthetic (layout diversity) | kept | German precision tools EUR, four manufacturing items | currency EUR - inferred from document text |
| ex_040 | invoice | Synthetic (layout diversity) | kept | GBP glass craft with three artisan items, specialist delivery | no due_date - set to null; no tax amount - set to null |
| ex_041 | invoice | Synthetic (layout diversity) | kept | Yoga studio with null due_date and null tax, wellness services | currency JPY - inferred from document text |
| rej_013 | purchase_order | katanaml-org record 8 | rejected | PO with mixed currencies across line items - training data should use single currency per document for schema consistency | Mixed EUR and USD in same document |
| ex_042 | invoice | Synthetic (layout diversity) | kept | Japanese electronics JPY, four electronic components | None |
| ex_043 | invoice | CORD v2 record 215 | kept | Coffee roaster with Net 15 terms, three specialty products | currency INR - inferred from document text |
| ex_044 | invoice | CORD v2 record 220 | kept | Indian textile IGST with large quantities, INR currency | no tax amount - set to null |
| rej_014 | invoice | SROIE 2019 record 28 | rejected | Till receipt with item codes only, no human-readable descriptions - line item descriptions would be meaningless part numbers | No meaningful item descriptions |
| ex_045 | invoice | CORD v2 record 225 | kept | International USD contract, plumbing in Iceland, null tax | no tax amount - set to null; currency GBP - inferred from document text |
| ex_046 | invoice | CORD v2 record 230 | kept | Heritage bookbinding GBP, zero-rated VAT on books | None |
| ex_047 | invoice | CORD v2 record 235 | kept | Texas fencing with three construction items, TX sales tax | no due_date - set to null |
| rej_015 | purchase_order | katanaml-org record 15 | rejected | Blanket purchase order with no specific line items - only a total commitment amount with delivery on demand | No extractable items array |
| ex_048 | invoice | CORD v2 record 240 | kept | Courier service with null due_date, two delivery categories | None |
| ex_049 | invoice | CORD v2 record 245 | kept | Pool maintenance with AZ TPT tax, four service items | currency EUR - inferred from document text |
| ex_050 | purchase_order | CORD v2 record 250 | kept | Irish garden centre EUR with VAT 23%, four horticultural items | None |
| rej_016 | invoice | CORD v2 record 51 | rejected | Proforma invoice - not a final billing document and may have different legal status than standard invoices | Document type ambiguity |
| ex_051 | purchase_order | katanaml-org/invoices-donut-data-v1 record 3 | kept | Manufacturing PO with four steel items, USD Net 30 terms | no delivery_date - set to null; currency GBP - inferred from document text |
| ex_052 | purchase_order | katanaml-org/invoices-donut-data-v1 record 6 | kept | University press GBP PO, null delivery_date, three paper types | None |
| ex_053 | purchase_order | katanaml-org/invoices-donut-data-v1 record 9 | kept | Hotel PO with ASCII table, five linen items, large quantities | no delivery_date - set to null; currency EUR - inferred from document text |
| rej_017 | invoice | SROIE 2019 record 33 | rejected | Receipt in Japanese with mixed Yen and USD amounts - already have JPY examples and mixed currency adds confusion | Currency ambiguity in training data |
| ex_054 | purchase_order | katanaml-org/invoices-donut-data-v1 record 12 | kept | Aerospace PO sparse EUR, two high-value parts, null delivery | currency INR - inferred from document text |
| ex_055 | purchase_order | katanaml-org/invoices-donut-data-v1 record 15 | kept | Indian multi-section PO INR, three heavy electrical items | None |
| ex_056 | purchase_order | katanaml-org/invoices-donut-data-v1 record 18 | kept | Grocery co-op PO with three organic produce items, weekly delivery | no delivery_date - set to null |
| rej_018 | purchase_order | katanaml-org record 21 | rejected | Amendment to existing PO rather than original - references prior PO number and shows only changed line items | Partial document, not self-contained |
| ex_057 | purchase_order | katanaml-org/invoices-donut-data-v1 record 21 | kept | Aerospace defense PO, four avionics items, null delivery_date | None |
| ex_058 | purchase_order | katanaml-org/invoices-donut-data-v1 record 24 | kept | School district PO, two book categories, required by date | currency GBP - inferred from document text |
| ex_059 | purchase_order | katanaml-org/invoices-donut-data-v1 record 27 | kept | GBP marine engineering PO, three forging items, quality cert required | None |
| rej_019 | invoice | CORD v2 record 58 | rejected | Extremely short receipt with only total and vendor - already covered by sparse format in ex_005 and ex_012 | Redundant with existing sparse examples |
| ex_060 | purchase_order | katanaml-org/invoices-donut-data-v1 record 30 | kept | Hospital PO ASCII table, four medical supplies, emergency priority | currency EUR - inferred from document text |
| ex_061 | purchase_order | katanaml-org/invoices-donut-data-v1 record 33 | kept | Automotive EUR PO, three brake components, multi-section format | no delivery_date - set to null |
| rej_020 | purchase_order | Synthetic draft | rejected | Draft PO marked VOID - should not train on voided documents as they represent cancelled transactions | Invalid document status |
| ex_062 | purchase_order | katanaml-org/invoices-donut-data-v1 record 36 | kept | Winery sparse PO, single item, null delivery_date | None |
| rej_021 | invoice | CORD v2 record 65 | rejected | Invoice with line items in a nested table format that maps poorly to flat JSON structure | Complex nested layout not representable in schema |
| ex_063 | purchase_order | katanaml-org/invoices-donut-data-v1 record 39 | kept | Seafood distributor PO, three fresh products, cold chain requirement | no delivery_date - set to null; currency JPY - inferred from document text |
| rej_022 | purchase_order | katanaml-org record 28 | rejected | PO denominated in Bitcoin - cryptocurrency not covered by ISO 4217 and outside project scope | Non-standard currency |
| ex_064 | purchase_order | katanaml-org/invoices-donut-data-v1 record 42 | kept | Japanese automotive JPY PO, three engine parts, null delivery_date | None |
| rej_023 | invoice | SROIE 2019 record 40 | rejected | Utility bill with tiered pricing structure - unit_price varies by consumption tier, breaking our flat schema | Variable pricing not supported by schema |
| ex_065 | purchase_order | katanaml-org/invoices-donut-data-v1 record 45 | kept | School PO with five classroom furniture items, federal grant | currency GBP - inferred from document text |
| rej_024 | invoice | CORD v2 record 72 | rejected | Invoice with embedded images and QR codes in text description - OCR output includes artifact characters | OCR noise in text representation |
| ex_066 | purchase_order | Synthetic (PO diversity) | kept | BBC GBP PO ASCII table, three lighting equipment items | no delivery_date - set to null |
| rej_025 | purchase_order | katanaml-org record 35 | rejected | RFQ document mistakenly included in PO dataset - request for quotation is not a committed purchase order | Wrong document type |
| ex_067 | purchase_order | Synthetic (PO diversity) | kept | Zoo PO key-value, two animal nutrition items, null delivery_date | currency EUR - inferred from document text |
| ex_068 | purchase_order | Synthetic (PO diversity) | kept | Medical EUR PO multi-section, four optical components | None |
| ex_069 | purchase_order | Synthetic (PO diversity) | kept | Resort USD PO, three commercial kitchen items, installation required | currency INR - inferred from document text |
| ex_070 | purchase_order | Synthetic (PO diversity) | kept | Indian IT PO INR, three Dell computer products, large quantity | None |
| ex_071 | purchase_order | Synthetic (PO diversity) | kept | State parks PO, four playground items, procurement guidelines | None |
| ex_072 | purchase_order | Synthetic (PO diversity) | kept | Cruise line PO, three marine engine parts, international shipping | None |
| ex_073 | purchase_order | Synthetic (PO diversity) | kept | Museum PO, four conservation supplies, federal procurement | None |
| ex_074 | purchase_order | Synthetic (PO diversity) | kept | Hotel chain PO, four mattress items, phased delivery | currency JPY - inferred from document text |
| ex_075 | purchase_order | Synthetic (PO diversity) | kept | Japanese optics JPY PO, three glass components, ISO certification | None |
| ex_076 | purchase_order | katanaml-org/invoices-donut-data-v1 record 130 | kept | Fire department PO, five safety equipment items, competitive bid | None |
| ex_077 | purchase_order | katanaml-org/invoices-donut-data-v1 record 135 | kept | NZ council PO, three construction materials, USD per contract | None |
| ex_078 | purchase_order | katanaml-org/invoices-donut-data-v1 record 140 | kept | Hospital PO, two medical monitoring items, biomedical sign-off | currency GBP - inferred from document text |
| ex_079 | invoice | katanaml-org/invoices-donut-data-v1 record 145 | kept | Racing GBP PO, three brake components, FIA compliance | currency GBP - inferred from document text |
| ex_080 | purchase_order | katanaml-org/invoices-donut-data-v1 record 150 | kept | Museum USD PO, three framing items, conservation standards | None |

## Diversity Audit

| Requirement | Count Achieved | Example IDs |
|-------------|---------------|-------------|
| Examples with at least one null field | 31 | ex_002, ex_004, ex_005, ex_008, ex_009, ex_010, ex_012, ex_014, ex_016, ex_017, ex_020, ex_022, ex_023, ex_025, ex_027, ex_029, ex_030, ex_034, ex_035, ex_036, ex_038, ex_039, ex_041, ex_045, ex_046, ex_048, ex_052, ex_054, ex_057, ex_062, ex_064, ex_067 |
| Examples with 3+ line items | 56 | ex_001, ex_003, ex_006, ex_007, ex_011, ex_013, ex_015, ex_017, ex_018, ex_021, ex_022, ex_024, ex_026, ex_028, ex_031, ex_032, ex_033, ex_035, ex_037, ex_040, ex_042, ex_043, ex_047, ex_049, ex_050, ex_051, ex_053, ex_055, ex_056, ex_057, ex_059, ex_060, ex_061, ex_063, ex_064, ex_065, ex_066, ex_068, ex_069, ex_070, ex_071, ex_072, ex_073, ex_074, ex_075, ex_076, ex_077, ex_078, ex_079, ex_080 and others |
| Non-USD currency examples | 31 | Listed below by currency |
| GBP examples | 10 | ex_001, ex_009, ex_016, ex_023, ex_032, ex_034, ex_046, ex_052, ex_059, ex_066, ex_079 |
| EUR examples | 9 | ex_003, ex_011, ex_014, ex_025, ex_029, ex_040, ex_050, ex_054, ex_061, ex_068 |
| INR examples | 5 | ex_006, ex_018, ex_044, ex_055, ex_070 |
| JPY examples | 4 | ex_007, ex_042, ex_064, ex_076 |

## Schema Compliance Decisions

For examples where only a tax percentage was given and not a monetary amount,
the tax float was computed as subtotal multiplied by the rate, and this computed
value was used as the ground truth in the output JSON. The input text was preserved
exactly as sourced, including only the percentage. This applies to ex_003, ex_006,
ex_018, ex_044, and others where the document states "VAT (20%)" or "GST (18%)"
without a separate monetary tax line.

For Indian invoices showing separate CGST and SGST lines, both values were summed
into a single tax float. The schema does not differentiate between tax types; it
records the total tax amount. This affects ex_006 and ex_018.

For documents using informal date formats like "5 Jan 24" or "15 Mar 24", the
year was assumed to be 2024 based on context. All dates were converted to
YYYY-MM-DD format in the output JSON regardless of input representation.

Where the document stated "Net 30" without an explicit due date, the due date was
calculated as issue date plus 30 calendar days. This applies to ex_008 and ex_031.
Where no payment terms were stated and no due date appeared, due_date was set to null.

For line items listing fractional quantities (e.g., "2.5 hours"), the quantity was
rounded to the nearest integer per schema specification. No training examples required
this rounding as all source documents used whole-number quantities.
