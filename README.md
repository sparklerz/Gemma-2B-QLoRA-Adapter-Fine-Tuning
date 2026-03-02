# Gemma-2B QLoRA Adapter Fine-Tuning on Dolly-15k

Fine-tune **Google’s Gemma-2B** with **QLoRA (4-bit NF4)** + **LoRA adapters** on the **Databricks Dolly 15k** instruction-following dataset.  
This repo is notebook-first (Colab friendly) and produces a lightweight **PEFT adapter** you can load on top of the base model.

---

## Quick links

- **W&B runs:** https://wandb.ai/kannansarat9/gemma-qlora
- **Base model (gated):** https://huggingface.co/google/gemma-2b
- **Dataset:** https://huggingface.co/datasets/databricks/databricks-dolly-15k
- **Published adapter:** https://huggingface.co/ash001/gemma-2b-dolly-qlora-adapter
- **Training notebook (Colab):** https://colab.research.google.com/drive/1xKDREzjPWhXVwCSDQueVqQDpAf3PeSEB
- **Base vs tuned comparison notebook (Colab):** https://colab.research.google.com/drive/116A84xPf2crkR4MJ0hZZtsag4U24kbsP

---

## What you get

- A **memory-efficient fine-tuning pipeline** using:
  - `transformers` + `trl` (`SFTTrainer`)
  - `peft` (LoRA)
  - `bitsandbytes` (4-bit quantization)
- Saved artifacts:
  - `adapter_model.safetensors` + PEFT config
  - tokenizer files
  - `run_metadata.json` containing:
    - training/eval metrics (incl. eval loss + perplexity)
    - a small “before vs after” prompt suite outputs

---

## Requirements

### Recommended: Google Colab GPU
- Works well on common Colab GPUs (T4), depending on chosen settings.
- QLoRA (4-bit) keeps VRAM low compared to full fine-tuning.

### Secrets / tokens
You’ll need:
- `HF_TOKEN` — Hugging Face access token **with permission to the gated Gemma repo**
- `WANDB_API_KEY` — optional (for experiment tracking)

In Colab:
- **Colab → Secrets (🔑)** → add `HF_TOKEN` and (optionally) `WANDB_API_KEY`

> Note: Gemma on HF is gated. Make sure you’ve accepted the license terms on Hugging Face for `google/gemma-2b`.

---

## Run training (Colab)

1. Open **Training notebook**  
   https://colab.research.google.com/drive/1xKDREzjPWhXVwCSDQueVqQDpAf3PeSEB
2. Set runtime to **GPU**
3. Add `HF_TOKEN` (and optionally `WANDB_API_KEY`) in **Colab Secrets**
4. Run all cells

### Default training behavior (notebook defaults)
To keep Colab runs quick:
- Filters Dolly examples to **no-context** rows
- Splits train/val with `test_size=0.05`
- Subsamples to:
  - `max_train_samples=2000`
  - `max_eval_samples=200`

---

## Compare base vs fine-tuned outputs (Colab)

1. Open **Response comparison notebook**  
   https://colab.research.google.com/drive/116A84xPf2crkR4MJ0hZZtsag4U24kbsP
2. Add `HF_TOKEN` in **Colab Secrets**
3. Run all cells

This notebook:
- Loads `google/gemma-2b` (base) and `ash001/gemma-2b-dolly-qlora-adapter` (adapter)
- Runs a prompt suite and saves:
  - `outputs_compare/outputs_comparison.json`

---

## Training details

### Prompt format
The notebook uses a simple instruction template:

```text
Instruction:
{instruction}

Response:
{response}
````

During training, each example is ended with the tokenizer EOS token.

### QLoRA (4-bit) config

* 4-bit quantization: **NF4**
* double quantization enabled
* compute dtype: **bf16 if supported**, else fp16

### LoRA config (defaults)

Targets common attention + MLP projection layers:

* `q_proj`, `k_proj`, `v_proj`, `o_proj`
* `gate_proj`, `up_proj`, `down_proj`

---

## Default hyperparameters

| Setting                     |            Value |
| --------------------------- | ---------------: |
| max_seq_length              |              512 |
| epochs                      |              1.0 |
| per_device_train_batch_size |                1 |
| grad_accumulation           |                4 |
| learning_rate               |             2e-4 |
| warmup_steps                |               10 |
| optimizer                   | paged_adamw_8bit |
| LoRA r                      |                8 |
| LoRA alpha                  |               16 |
| LoRA dropout                |             0.05 |

---

## Inference: load base + adapter

```python
import os
import torch
from transformers import AutoTokenizer, AutoModelForCausalLM, BitsAndBytesConfig
from peft import PeftModel

BASE_MODEL_ID = "google/gemma-2b"
ADAPTER_ID = "ash001/gemma-2b-dolly-qlora-adapter"
HF_TOKEN = os.environ.get("HF_TOKEN")  # or set directly

bnb_config = BitsAndBytesConfig(
    load_in_4bit=True,
    bnb_4bit_quant_type="nf4",
    bnb_4bit_compute_dtype=torch.bfloat16 if torch.cuda.is_available() else torch.float32,
    bnb_4bit_use_double_quant=True,
)

tokenizer = AutoTokenizer.from_pretrained(BASE_MODEL_ID, token=HF_TOKEN)
if tokenizer.pad_token is None:
    tokenizer.pad_token = tokenizer.eos_token

base = AutoModelForCausalLM.from_pretrained(
    BASE_MODEL_ID,
    quantization_config=bnb_config if torch.cuda.is_available() else None,
    device_map="auto" if torch.cuda.is_available() else None,
    token=HF_TOKEN,
)

model = PeftModel.from_pretrained(base, ADAPTER_ID)
model.eval()

prompt = "Instruction:\nGive me 5 tips to grow a plant.\n\nResponse:\n"
inputs = tokenizer(prompt, return_tensors="pt").to(model.device)

with torch.no_grad():
    out = model.generate(**inputs, max_new_tokens=160, temperature=0.7, top_p=0.9)

print(tokenizer.decode(out[0], skip_special_tokens=True))
```

---
