# Before vs After Fine-Tuning Comparison

## Methodology

Both evaluations used the same 20 held-out documents defined in
eval/held_out_documents.md. The same extraction prompt was used for
both the baseline and fine-tuned model. Temperature was set to 0.0 in both
cases to ensure deterministic, reproducible output. The only variable that
changed between the two evaluation runs was the model weights -- base
Llama 3.2 3B-Instruct for the baseline, and the same model with the trained
LoRA adapter for the post-fine-tuning evaluation.

is_valid_json is scored by running Python's json.loads() on the raw, unmodified
model response with no preprocessing. A response wrapped in markdown code fences
will fail this test, which is the correct production definition -- a downstream
parser receives the raw string and cannot call json.loads() on it without
additional stripping logic.

## Results Table

| Metric | Baseline (base model) | Post Fine-Tuning | Change |
|--------|-----------------------|-----------------|--------|
| Parse success rate (valid JSON + all keys) | 10/20 = 50.0% | 20/20 = 100.0% | +50.0pp |
| Average key accuracy | 0.54 | 1.00 | +0.46 |
| Average value accuracy | 0.55 | 1.00 | +0.45 |
| Responses wrapped in markdown code fences | 8 | 0 | -8 |
| Responses with prose preamble before JSON | 2 | 0 | -2 |
| Responses with wrong or extra schema keys | 2 | 0 | -2 |
| Responses returning invalid JSON entirely | 0 | 0 | 0 |

## Analysis

The results table reveals that the base model's failure mode was almost entirely
a formatting problem rather than a comprehension problem. In 8 of the 10 failing
baseline responses, the model correctly identified the fields and extracted accurate
values -- it simply wrapped the output in markdown code fences, which causes any
downstream json.loads() call to fail with a JSONDecodeError. The model was never
taught that structured output requests require raw JSON; its pretraining and
instruction-tuning instilled the habit of using code fences whenever code-like
content is produced, and it could not distinguish between "show me code" and
"return machine-parseable data."

The fine-tuned model eliminated this failure mode entirely. Across all 20 evaluation
documents, it returned raw JSON objects with no fences, no prose preambles, and no
schema key deviations. This improvement -- from 50% to 100% parse success -- happened
without any regression in extraction accuracy. The fine-tuned model achieved 1.00
average key accuracy and 1.00 average value accuracy, compared to 0.54 and 0.55 for
the baseline when scored on raw responses.

The documents where the baseline succeeded without fine-tuning (eval_doc_01, 07, 10,
12, 13, 14, 16, 18, 19, 20) share a common characteristic: the model happened to
return a clean JSON response without fences for these particular inputs. This
inconsistency is the central production reliability problem. A pipeline cannot rely
on a model that produces correctly formatted output 50% of the time -- the other
50% requires error handling, fence stripping, and key normalization, which defeats
the purpose of automated extraction. Fine-tuning removed this inconsistency by
making raw JSON output the model's default response pattern rather than an
occasional behaviour.
