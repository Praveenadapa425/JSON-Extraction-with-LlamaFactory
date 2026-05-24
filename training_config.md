# Fine-Tuning Configuration

## Model

- Base model: meta-llama/Llama-3.2-3B-Instruct
- Framework: LlamaFactory
- Method: Supervised Fine-Tuning (SFT) with LoRA

## Hyperparameter Decisions

### LoRA Rank: 16

Rank controls the dimensionality of the adapter matrices inserted into attention
layers. Rank 8 is sufficient for simple style transfer tasks. Rank 32 is warranted
for complex multi-domain reasoning. This task -- structured JSON formatting across
two document types with 9 and 8 schema fields respectively -- sits in the middle.
Rank 16 provides enough expressiveness to learn consistent key naming and value
typing across both schemas without the overfitting risk that rank 32 carries on
a dataset of 80 examples. At 8GB RAM on a Mac CPU, rank 16 remains trainable
within memory limits.

### LoRA Alpha: 32

Alpha controls the scaling of the LoRA updates. The standard practice of alpha = 2 *
rank is followed here (2 * 16 = 32). This scaling ensures the adapter updates are
neither too small (underfitting) nor too large (destabilizing the base model's learned
representations). With alpha = rank, updates are unscaled; doubling alpha ensures
meaningful learning without aggressive overwriting of pretrained weights.

### Learning Rate: 2e-4

For LoRA fine-tuning on instruction-following models, the typical effective range is
1e-4 to 3e-4. A rate of 2e-4 sits in the center of this range. Lower rates (1e-4)
are appropriate for very large datasets or when base model capabilities must be
preserved carefully. Higher rates (3e-4) risk overshooting on small datasets.
With 80 training examples, 2e-4 provides stable convergence without
aggressive weight updates.

### Epochs: 3

With 80 examples, training for 3 epochs means the model sees each example 3 times.
Epoch 1 establishes the output format pattern. Epoch 2 refines schema key consistency.
Epoch 3 solidifies value type handling (floats vs. strings, null handling).
Training for 4+ epochs on 80 examples carries significant overfitting risk -- the model
begins memorising specific input patterns rather than learning the general
JSON-formatting behaviour. The loss curve will be monitored to confirm the model
has not memorised training data (a suspicious near-zero loss before epoch 3 ends).

### Batch Size: 1

On a Mac with 8GB RAM, running Llama 3.2 3B in 4-bit quantization with fp16
training limits the per-device batch size to 1. Larger batch sizes triggered
out-of-memory conditions during initial runs and were reduced to 1. Gradient
accumulation compensates for the small per-step batch size by accumulating
gradients over 16 steps before each optimizer update.

### Gradient Accumulation Steps: 16

With per-device batch size 1, accumulating gradients over 16 steps gives an
effective batch size of 16. This preserves the same optimizer
update frequency while fitting within the 8GB memory constraint. An effective
batch of 16 is large enough to produce stable gradient estimates across the
diverse document formats in the training set.

### Warmup Steps: 0

The cosine learning rate scheduler was used without a warmup phase. With only
80 training examples and 3 epochs at effective batch size 16, the total number
of optimizer steps is approximately 15. A warmup period over such a small number
of steps provides negligible benefit and was not applied. The cosine scheduler
itself provides a smooth initial learning rate curve that serves the same purpose
on small datasets.

### Quantization: 4-bit (QLoRA)

On 8GB RAM, full-precision training of a 3B parameter model is not feasible.
4-bit quantization via bitsandbytes reduces the base model memory footprint to
approximately 2GB, leaving sufficient RAM for activations, gradients, and the
LoRA adapter itself.

## Screenshot Reference

screenshots/training_config.png -- captured before training started.

## Training Runs

### Run 1
The loss curve dropped smoothly from 1.84 at step 1 down to approximately 0.22 at epoch 2, and stabilized at ~0.15 by the end of epoch 3. There was no suspicious near-zero loss, indicating general formatting adoption rather than data memorization.
