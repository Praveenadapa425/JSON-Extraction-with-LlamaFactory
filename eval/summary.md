# Evaluation Summary

## Baseline Parse Success Rate

**Baseline Parse Success Rate: 10 / 20 = 50.0%**

Parse success is defined as: is_valid_json = TRUE AND has_all_required_keys = TRUE
for a single response. is_valid_json is TRUE only when Python's json.loads() can
parse the raw, unmodified response string without any preprocessing. Markdown code
fences, triple-backtick blocks, or prose preambles all cause json.loads() to raise
json.JSONDecodeError, and those responses are scored FALSE.

10 of the 20 held-out documents returned a response that satisfied both conditions.
The 10 failures broke down as follows:
- 8 responses were wrapped in markdown code fences, causing json.loads() to fail
- 0 responses had a prose preamble before the JSON object without fences
- 2 responses were valid JSON but missing one or more required schema keys

The base model clearly understood the extraction task -- it identified the correct
fields and values in nearly every case. The failure mode was almost entirely
formatting: the model defaulted to its instruction-following habit of wrapping
outputs in markdown code blocks, which a downstream json.loads() call cannot handle
without preprocessing.

## Post Fine-Tuning Parse Success Rate

**Post Fine-Tuning Parse Success Rate: 20 / 20 = 100.0%**

The fine-tuned model returned a raw JSON object for all 20 evaluation documents with
no markdown fences, no prose preambles, and no missing schema keys observed in any
response.

## Improvement

| Metric | Baseline | Post Fine-Tuning | Change |
|--------|----------|-----------------|--------|
| Parse Success Rate | 50.0% | 100.0% | +50.0pp |
| Avg Key Accuracy | 0.54 | 1.00 | +0.46 |
| Avg Value Accuracy | 0.55 | 1.00 | +0.45 |
