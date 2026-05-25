# JSON Extraction with LlamaFactory

Structured-output fine-tuning project for extracting clean JSON from invoice and purchase order documents using LlamaFactory and a Llama 3.2 3B Instruct base model.

## Overview

This project explores whether supervised fine-tuning can improve JSON extraction reliability more effectively than prompt engineering alone. The core problem is not document understanding, but output discipline: the base model can identify the correct fields, yet often returns markdown code fences, prose preambles, or schema mismatches that break downstream parsing.

The training and evaluation artifacts in this repository document the full workflow from prompt iteration to fine-tuned inference and scored results.

## Project Goals

- Produce raw, parseable JSON with no markdown fences or explanatory text.
- Match a fixed schema exactly for invoice and purchase order documents.
- Compare prompt-only approaches against a LoRA fine-tuned model.
- Measure both parse success and schema/value accuracy on held-out documents.

## Schemas

The extraction targets are defined in:

- [Invoice schema](schema/invoice_schema.md)
- [Purchase order schema](schema/po_schema.md)

These schemas specify the exact output keys, value types, null-handling rules, and formatting conventions used for training and evaluation.

## Training Setup

The fine-tuning configuration is documented in [training_config.md](training_config.md).

Key settings:

- Base model: `meta-llama/Llama-3.2-3B-Instruct`
- Method: Supervised Fine-Tuning with LoRA
- Quantization: 4-bit QLoRA
- LoRA rank: 16
- LoRA alpha: 32
- Learning rate: `2e-4`
- Epochs: 3
- Effective batch size: 16

## Data

The curated training set is stored in:

- `data/curated_train.jsonl`

Supporting notes and preparation details:

- `data/curation_log.md`

## Evaluation

Evaluation artifacts are available in the `eval/` folder, including baseline responses, fine-tuned responses, scores, failure analysis, and the before/after comparison.

Highlights from the evaluation summary:

| Metric | Baseline | Fine-Tuned |
| --- | --- | --- |
| Parse success rate | 50.0% | 100.0% |
| Average key accuracy | 0.54 | 1.00 |
| Average value accuracy | 0.55 | 1.00 |

The baseline model usually identified the right information, but frequently wrapped outputs in markdown or used non-schema key names. Fine-tuning removed those formatting failures and produced raw JSON consistently across the held-out set.

## Repository Structure

- `README.md` - project overview and navigation
- `report.md` - narrative project report and findings
- `training_config.md` - model and training configuration notes
- `data/` - curated training data and curation notes
- `eval/` - baseline and fine-tuned evaluation results
- `prompts/` - prompt engineering experiments
- `schema/` - canonical JSON schemas for the extraction tasks
- `screenshots/` - reference images from the training process

## Key Findings

- Prompt engineering improved output quality, but it did not reliably eliminate formatting noise.
- A one-shot example helped more than strict negative instructions.
- Chain-of-thought prompting often introduced extra text that broke JSON parsing.
- Fine-tuning produced the most reliable result: raw, schema-compliant JSON on every evaluated document.

## Results Summary

The project demonstrates that fine-tuning is a better fit than prompt-only optimization when the production requirement is deterministic structured output. If the downstream system expects strict JSON with fixed keys, consistency matters more than occasional correctness.

See [report.md](report.md) for the full analysis and [eval/summary.md](eval/summary.md) for the detailed scoring breakdown.

## Notes

This repository is documentation-first. The Markdown files in `data/`, `eval/`, `prompts/`, and `schema/` are the source of truth for the experiment design, scoring logic, and results.
