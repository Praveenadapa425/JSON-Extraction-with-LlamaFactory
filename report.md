# Structured Output Fine-Tuning -- Project Report

## Prompting vs Fine-Tuning

Prompt engineering for structured extraction works best when the base model already produces outputs close to the desired format and the primary issue is surface-level formatting noise. In the baseline evaluation, Llama 3.2 3B demonstrated strong field identification capabilities -- it could locate vendor names, dates, and monetary values with reasonable accuracy even without fine-tuning. The failure points were consistent: wrapping JSON in markdown code fences, prepending explanatory prose before the actual object, using non-standard key names like "invoice_date" instead of "date", and returning monetary values as formatted strings ("$1,450.00") instead of bare floats (1450.00). These are formatting failures, not comprehension failures. The model knew where to find the data; it did not know how to present it.

Adding a one-shot example to the prompt (Version 2 in the prompt experiments) improved format compliance more than adding explicit negative constraints (Version 1). This matches established findings in few-shot prompting research: demonstration by example is more effective than instruction by prohibition. The chain-of-thought prompt (Version 3) produced mixed results -- while it occasionally improved field identification for complex documents, it also increased the likelihood of the model including its reasoning text in the output, contaminating the JSON. In practice, chain-of-thought prompting is counterproductive for tasks where the sole output requirement is a structured object with no surrounding text.

Fine-tuning eliminated the formatting problems almost entirely. The fine-tuned model learned to output raw JSON without code fences, without preambles, and with the exact key names specified in the schema. This is the strongest argument for fine-tuning over prompt engineering in production contexts: consistency. A prompt-engineered solution will produce correct output most of the time, but the failure rate on edge cases means downstream parsing code must include extensive error handling for markdown stripping, key name normalization, and type coercion. A fine-tuned model reduces this error handling burden significantly because the output format is baked into the weights rather than suggested by the prompt.

The appropriate choice between prompting and fine-tuning depends on operational context. For prototyping or infrequent use where schema changes are common, prompt engineering offers faster iteration cycles and zero training cost. For production pipelines processing thousands of documents daily with a fixed schema, fine-tuning is justified by the reduction in parse failures and the corresponding reduction in manual review overhead. The break-even point in our experiment was roughly the point where the cost of curating 80 training examples and running a 15-minute training job was less than the accumulated cost of handling format-related parse failures at scale.

The failure analysis revealed that the remaining errors after fine-tuning were data problems, not format problems. The fine-tuned model consistently produced valid JSON with correct keys. Where it failed was in value accuracy: extracting the wrong amount from ambiguously formatted numbers, misidentifying which field a label referred to, or defaulting to null when a value was present but labeled unusually. These are problems that additional training examples addressing specific label variants and number formats would resolve. The format consistency problem was solved by fine-tuning; the remaining challenge is coverage of document variability in the training data.

## Key Findings

The baseline model achieved a parse success rate of 50.0% on the 20 held-out
documents when scored strictly -- json.loads() applied to the raw, unmodified
response string. The fine-tuned model achieved 100.0%, representing a 50 percentage
point improvement. The most significant qualitative change was the elimination of
markdown code fences from model outputs. Eight of the ten baseline failures occurred
because the model wrapped its response in triple-backtick code blocks, which
json.loads() cannot parse without preprocessing. The fine-tuned model returned raw
parseable JSON in all 20 cases.

## Limitations

This experiment operated under several constraints that limit the generalizability of its conclusions. The test set of 20 documents provides limited statistical power for drawing definitive conclusions about parse success rates -- a difference of even 2-3 documents between runs could shift the success rate by 10-15 percentage points. The 8GB RAM constraint restricted the model size to 3B parameters with 4-bit quantization, which may not represent the behavior of larger models where baseline formatting compliance is already higher. The training data of 80 examples, while carefully curated for diversity, is entirely synthetic -- real enterprise documents exhibit layout variability, scan quality degradation, and OCR error patterns that synthetic text cannot replicate. A production deployment would require training on actual scanned document outputs to achieve reliable accuracy. Additionally, the single-prompt evaluation methodology does not account for the variance reduction that majority voting or multi-sample strategies could provide to both the baseline and fine-tuned models.
