---
{"dg-publish":true,"permalink":"/knowledge/02-tech-second-brain/ai-core/nlp/lo-ra-playbook-chinh-tham-so-va-sweep/","pinned":"false","tags":["type/howto","topic/peft-lora","topic/fine-tune","status/stable"]}
---


# LoRA — Playbook chỉnh tham số và sweep

> **Bộ ba note về LoRA.** Ba note dưới đây tách theo câu hỏi mà chúng trả lời:
>
> - [Siêu tham số r, alpha và target modules](LoRA - Siêu tham số r, alpha và target modules.md) — *các tham số nghĩa là gì, nên đặt bao nhiêu*
> - **Playbook chỉnh tham số và sweep** (đang đọc) — *làm sao dò ra giá trị tối ưu*
> - [LoRA cho Continual Learning](LoRA cho Continual Learning - O-LoRA, N-LoRA, MoLA, SLIM.md) — *nhiều task nối tiếp thì tổ chức adapter thế nào*
>
> Gộp ngày 2026-08-06 từ bốn note cũ vốn chồng nội dung nhau.

Giá trị khởi điểm của từng tham số nằm ở note [Siêu tham số r, alpha và target modules](LoRA - Siêu tham số r, alpha và target modules.md). Note này chỉ nói **cách dò** ra giá trị tốt hơn khởi điểm đó.

## Cấu hình khởi điểm

```python
# PEFT LoRA for Llama-2-7b SFT
# References:
# - Unsloth hyperparam guide (r/alpha/targets/dropout/LR): https://docs.unsloth.ai/get-started/fine-tuning-llms-guide/lora-hyperparameters-guide
# - QLoRA paper and LR guidance: https://arxiv.org/abs/2305.14314
# - NVIDIA NeMo QLoRA tips (LR 2e-4 small, 1e-4 big): https://docs.nvidia.com/nemo-framework/user-guide/24.12/sft_peft/qlora.html
from peft import LoraConfig
lora_config = LoraConfig(
    r=16,                     # try 16 → 32 if underfitting
    lora_alpha=16,            # try 32 if underfitting (alpha = 2r)
    lora_dropout=0.0,         # use 0.05–0.1 only if overfitting
    target_modules=["q_proj","k_proj","v_proj","o_proj","gate_proj","up_proj","down_proj"],
    bias="none",
    task_type="CAUSAL_LM",
)
# Trainer hints:
# learning_rate=2e-4, warmup_ratio=0.1, weight_decay=0.01, lr_scheduler_type="cosine"
# num_train_epochs=2–3 for ~4k examples; eval each epoch; early stop on plateaus.
```

## Trình tự chỉnh cho dataset nhỏ (~4k mẫu)

Tuning playbook for your 4k-example dialog summarization:

1. **Start**: `r=16, alpha=16, dropout=0.0, LR=2e-4, warmup=10%, cosine`, target all modules.
2. **If underfitting** (val loss high, ROUGE low, outputs generic): set **alpha=32**; if still flat, set **r=32**. Keep LR at **2e-4**. ([docs.unsloth.ai](https://docs.unsloth.ai/get-started/fine-tuning-llms-guide/lora-hyperparameters-guide "LoRA Hyperparameters Guide | Unsloth Documentation"))
3. **If overfitting** (train loss falls, val ROUGE drops): add **dropout=0.05–0.1**, enable small **weight_decay=0.01**, or reduce **epochs**. ([docs.unsloth.ai](https://docs.unsloth.ai/get-started/fine-tuning-llms-guide/lora-hyperparameters-guide "LoRA Hyperparameters Guide | Unsloth Documentation"))
4. **If unstable loss**: reduce **LR to 1e-4** or keep r fixed and lower alpha. ([NVIDIA Docs](https://docs.nvidia.com/nemo-framework/user-guide/24.12/sft_peft/qlora.html "NeMo QLoRA Guide"))

Extra sources that explain choices and trade-offs:

* **Unsloth LoRA Hyperparameters Guide**. Practical defaults and rationale for r, alpha, targets, dropout, LR. Updated Jul 29 2025. ([docs.unsloth.ai](https://docs.unsloth.ai/get-started/fine-tuning-llms-guide/lora-hyperparameters-guide "LoRA Hyperparameters Guide | Unsloth Documentation"))
* **QLoRA paper**. Uses **r=64** in several experiments and motivates PEFT on 4-bit backbones. May 2023. ([arXiv](https://arxiv.org/pdf/2305.14314 "QLORA: Efficient Finetuning of Quantized LLMs"))
* **NVIDIA NeMo QLoRA guide**. LR guidance: **2e-4 small models**, **1e-4 big models**. Dec 2024. ([NVIDIA Docs](https://docs.nvidia.com/nemo-framework/user-guide/24.12/sft_peft/qlora.html "NeMo QLoRA Guide"))
* **HF community findings**. Higher r is not always better; r=16 performed best in a PEFT discussion; alpha and LR are key. 2024. ([GitHub](https://github.com/huggingface/peft/discussions/2037 "Lora rank & alpha · huggingface peft · Discussion #2037"))
* **Empirical scaling study**. Larger **alpha/LR** improved results more than larger **r** in instruction tuning. 2023. ([arXiv](https://arxiv.org/pdf/2309.09958 "An Empirical Study of Scaling Instruction-Tuned Large ..."))
* **Module names cross-check**. Llama `q/k/v/o` in attention and `gate/up/down` in MLP in HF Llama code. 2024. ([Hugging Face](https://huggingface.co/zai-org/LongCite-llama3.1-8b/blob/main/modeling_llama.py "modeling_llama.py · zai-org/LongCite-llama3.1-8b at main"))

---

Use a small, disciplined sweep. Optimize learning rate and `lora_alpha` first. Hold `r` and `target_modules` fixed. Add complexity only if the metric stalls.

## Workflow

1. Lock a baseline

* Start from working LoRA defaults for Llama-2-7B (e.g., `r=16`, `alpha=16`, `dropout=0`, targets on `q,k,v,o,gate,up,down`). Then tune off this point. HF PEFT docs and Unsloth summarize these choices. ([Hugging Face](https://huggingface.co/docs/peft/en/package_reference/lora "LoRA"))

2. Pick robust metrics and a validation split

* Use ROUGE-L and BERTScore for summaries. Track validation loss too. Keep a fixed dev set. QLoRA and follow-ups emphasize small high-quality dev data and careful eval. ([arXiv](https://arxiv.org/abs/2305.14314 "QLoRA: Efficient Finetuning of Quantized LLMs"))

3. Bound the learning rate with an LR-range test

* Run a short “LR range test”: increase LR linearly over a few hundred steps, plot loss vs LR, choose LR just before loss blows up. This is standard from Smith’s CLR papers. Use that LR band in your sweep. ([arXiv](https://arxiv.org/abs/1506.01186 "Cyclical Learning Rates for Training Neural Networks"))

4. Run a budgeted Bayesian sweep

* Use Optuna TPE or Ray Tune with BOHB. Limit trials to ~20–40 to start. Optimize your main metric on the dev set. HF Trainer integrates with Optuna; Ray Tune integrates broadly. ([Hugging Face Forums](https://discuss.huggingface.co/t/running-optuna-on-two-huggingface-trainer-tasks/24023 "Running Optuna on Two HuggingFace Trainer Tasks"))

5. Triage hyperparameters in tiers

* Tier 1: **LR**, **`lora_alpha`**, **warmup ratio**.
* Tier 2: **`r`**, **dropout**, **weight decay**.
* Tier 3: **target modules** ablation (all vs attention-only).
  Empirical reports and community discussions show LR/alpha dominate quality shifts; larger `r` helps less and can even hurt. ([ACL Anthology](https://aclanthology.org/2024.emnlp-main.1168.pdf "ApiQ: Finetuning of 2-Bit Quantized Large Language Model"))

6. Use early stopping and learning-curve diagnostics

* Stop when dev metric stalls for 1–2 evals. Plot loss and metric over steps. W&B or Ray Tune dashboards help compare runs cleanly. ([Weights & Biases Documentation](https://docs.wandb.ai/guides/sweeps/define-sweep-configuration/ "Define a sweep configuration")) 

7. Control variance

* Fix seeds, shuffle deterministically, repeat best configs over 3 seeds; pick by mean and tie-break by variance. QLoRA ran many controlled runs to compare fairly. ([arXiv](https://arxiv.org/abs/2305.14314 "QLoRA: Efficient Finetuning of Quantized LLMs"))

8. Confirm generalization with a quick ablation

* Re-train best config with: a) attention-only targets, b) `r=32` with same alpha, c) `alpha=2r` at fixed `r`. Keep all else constant. Choose the smallest adapter that matches the best score. HF docs and Unsloth explain target naming and module coverage. ([Hugging Face](https://huggingface.co/docs/peft/en/package_reference/lora "LoRA"))

## Minimal, practical search spaces

Start tight. Widen only if results cluster at an edge.

* LR: from LR-range test knee, scan ×0.5…×1.5. Example: `1e-4…3e-4`. ([arXiv](https://arxiv.org/abs/1506.01186 "Cyclical Learning Rates for Training Neural Networks"))
* `lora_alpha`: `{r, 2r}` first.
* `r`: `{16, 32}` only if alpha sweep is inconclusive. Evidence: limited gains beyond 16–32 for many tasks. ([GitHub](https://github.com/huggingface/peft/discussions/2037 "Lora rank & alpha · huggingface peft · Discussion #2037"))
* Warmup ratio: `{0.05, 0.1}`.
* Dropout: `{0.0, 0.05}` only if overfitting appears.
* Target modules: start with all linear layers; ablate to attention-only if VRAM bound. Names per HF Llama modules. ([Hugging Face](https://huggingface.co/docs/peft/en/package_reference/lora "LoRA"))

## Example: Optuna over HF Trainer + PEFT

```python
# Optuna + HF Trainer + PEFT LoRA
# Docs:
# - HF PEFT LoRA: https://huggingface.co/docs/peft/en/package_reference/lora
# - HF Trainer hparam search threads: https://discuss.huggingface.co/t/using-hyperparameter-search-in-trainer/785
# - Optuna: https://optuna.org, Ray Tune alt: https://docs.ray.io/en/latest/tune/index.html
import optuna

def objective(trial):
    # TIER 1 + small TIER 2
    lr = trial.suggest_float("learning_rate", 1e-4, 3e-4, log=True)  # band from LR-range test
    warmup = trial.suggest_float("warmup_ratio", 0.05, 0.1)
    r = trial.suggest_categorical("lora_r", [16, 32])
    alpha = trial.suggest_categorical("lora_alpha", [r, 2*r])
    dropout = trial.suggest_categorical("lora_dropout", [0.0, 0.05])

    # build LoraConfig and Trainer, train for 1–2 epochs, evaluate on dev
    # return ROUGE-L (maximize) or 1/val_loss (minimize)
    return dev_metric_value
```

> If you prefer a config-file approach, use a W&B Sweep (grid/random/BOHB) with the same spaces and metric. ([Weights & Biases Documentation](https://docs.wandb.ai/guides/sweeps/define-sweep-configuration/ "Define a sweep configuration"))

## Learning-rate range test in one line of reasoning

* Run 300–1000 warmup steps with LR increasing linearly from 1e-6 to 1e-3.
* Plot loss vs LR. Pick LR near the largest value where loss is still decreasing smoothly. Use that as the center of your sweep band. This is straight from CLR/“LR range test.” ([arXiv](https://arxiv.org/abs/1506.01186 "Cyclical Learning Rates for Training Neural Networks"))

## What to log and watch

* Primary: ROUGE-L or BERTScore on dev.
* Secondary: validation loss, generation length, and a few qualitative samples.
* Record config, seed, tokens seen, effective batch size, and wall time per run. Use W&B or Ray Tune analysis utilities to compare. ([docs.ray.io](https://docs.ray.io/en/latest/tune/examples/tune_analyze_results.html "Analyzing Tune Experiment Results — Ray 2.49.2"))

## Why this works

* QLoRA and follow-ups show small high-quality datasets respond most to LR and scaling of adapters (`alpha`) rather than very large ranks. Start simple, then ablate. ([arXiv](https://arxiv.org/abs/2305.14314 "QLoRA: Efficient Finetuning of Quantized LLMs"))

## Short curated references

* QLoRA paper. Method and large-scale tuning protocol. May 2023. ([arXiv](https://arxiv.org/abs/2305.14314 "QLoRA: Efficient Finetuning of Quantized LLMs"))
* HF PEFT LoRA docs. Parameter meanings and module names. 2024–2025. ([Hugging Face](https://huggingface.co/docs/peft/en/package_reference/lora "LoRA"))
* Unsloth LoRA hyperparameters guide. Practical defaults and ranges. Jul 29 2025. ([docs.unsloth.ai](https://docs.unsloth.ai/get-started/fine-tuning-llms-guide/lora-hyperparameters-guide "LoRA Hyperparameters Guide | Unsloth Documentation"))
* HF forum threads on Trainer HPO with Optuna. Integration patterns. 2020–2023. ([Hugging Face Forums](https://discuss.huggingface.co/t/using-hyperparameter-search-in-trainer/785?page=2 "Using hyperparameter-search in Trainer - Page 2"))
* Ray Tune analysis and examples. Result comparison and search algorithms. 2024–2025. ([docs.ray.io](https://docs.ray.io/en/latest/tune/examples/tune_analyze_results.html "Analyzing Tune Experiment Results — Ray 2.49.2"))
* CLR and LR-range test papers by Leslie Smith. LR bounding procedure. 2015–2017. ([arXiv](https://arxiv.org/abs/1506.01186 "Cyclical Learning Rates for Training Neural Networks"))

---

Here’s a tight, high-signal reading list. Grouped. Each item states why it’s useful and the date.

# Canonical papers

* **QLoRA** (Dettmers et al., May 23 2023). Core method, default LRs, r choices, quantization tricks. Good for small, high-quality datasets. ([arXiv](https://arxiv.org/abs/2305.14314 "QLoRA: Efficient Finetuning of Quantized LLMs"))
* **Cyclical LR + LR-range test** (Smith, 2015–2018). Fast way to bracket a good LR before any sweep. Use once, then sweep narrowly. ([arXiv](https://arxiv.org/abs/1506.01186 "Cyclical Learning Rates for Training Neural Networks"))

# Official docs and model-specific notes

* **PEFT LoRA docs**. Parameter meanings, API, and rsLoRA toggle. Living reference. ([Hugging Face](https://huggingface.co/docs/peft/package_reference/lora "LoRA - Hugging Face"))
* **Transformers Llama docs**. Layer names (`q/k/v/o`, `gate/up/down`) used in `target_modules`. Prevents “target modules not found.” ([Hugging Face](https://huggingface.co/docs/transformers/en/model_doc/llama2 "Llama 2"))
* **Optimum tutorial: LoRA on Llama** (example uses `q_proj…down_proj`, `alpha=16`, `dropout=0.05`). Concrete, copyable config. ([Hugging Face](https://huggingface.co/docs/optimum-neuron/training_tutorials/sft_lora_finetune_llm "Fine-Tune Llama 3 8B with LoRA - Hugging Face"))

# Practical hyperparameter guides

* **Unsloth: LoRA Hyperparameters Guide** (updated ~Aug 2025). Clear defaults for r, alpha, dropout, LR; when to widen search. ([docs.unsloth.ai](https://docs.unsloth.ai/get-started/fine-tuning-guide/lora-hyperparameters-guide "LoRA Hyperparameters Guide | Unsloth Documentation"))
* **Unsloth: Fine-tuning LLMs Guide**. End-to-end best practices and what to tune first. ([docs.unsloth.ai](https://docs.unsloth.ai/get-started/fine-tuning-llms-guide "Fine-tuning LLMs Guide | Unsloth Documentation"))
* **Unsloth: GPT-OSS fine-tune tutorial** (Aug 2025). Concrete LoRA knobs with overfit warnings. ([docs.unsloth.ai](https://docs.unsloth.ai/basics/gpt-oss-how-to-run-and-fine-tune/tutorial-how-to-fine-tune-gpt-oss "Tutorial: How to Fine-tune gpt-oss - Unsloth Docs"))

# Variants and improvements

* **rsLoRA explainer** (HF blog, Apr 2024). When rank scaling helps and how to enable in PEFT. Useful if base LoRA saturates. ([Hugging Face](https://huggingface.co/blog/damjan-k/rslora "Rank-Stabilized LoRA: Unlocking the Potential of LoRA Fine-Tuning"))

# HPO tooling

* **HF Trainer + Optuna/Ray pointers**. Use Bayesian/TPE over a tight space after LR-range test. (HF forum threads and docs referenced in the earlier plan.) For concrete integration patterns, start here, then wire to your training script. ([Hugging Face Forums](https://discuss.huggingface.co/t/llama2-fine-tunning-with-peft-qlora-and-testing-the-model/50581 "Llama2 fine-tunning with PEFT QLora and testing the model"))

# Common pitfalls and “gotchas” to avoid

* **“Target modules not found”**. Mismatch between model layer names and `target_modules`. See real failures; fix by matching Llama names exactly. ([GitHub](https://github.com/tloen/alpaca-lora/issues/251 "ValueError: Target modules [q_proj,v_proj] not found in the ..."))
* **Gradient checkpointing with LoRA**. Known interactions that throw “does not require grad” or slowdowns. Verify flags before enabling. ([GitHub](https://github.com/huggingface/peft/issues/137 "RuntimeError: element 0 of tensors does not require grad ..."))
* **FlashAttention dtype constraints**. FA-2 requires fp16/bf16; errors if dtype mismatched or with some 4-bit setups. ([GitHub](https://github.com/huggingface/transformers/issues/26066 "FlashAttention only supports fp16 and bf16 data type” ..."))
* **bitsandbytes environment issues**. CUDA mismatch or CPU-only wheels on Windows cause slow or broken QLoRA. Check version support. ([GitHub](https://github.com/bitsandbytes-foundation/bitsandbytes/issues/1093 "Failed to import transformers.integrations.bitsandbytes ..."))
* **Merging adapters can alter outputs**. `merge_and_unload` sometimes shifts quality on quantized bases; re-evaluate after merge. ([GitHub](https://github.com/huggingface/peft/issues/1043 "Merge and unload changes inference a lot on quantized ..."))
* **Multi-GPU utilization quirks in SFTTrainer**. Watch that both GPUs are used; some reports and fixes exist. ([GitHub](https://github.com/huggingface/trl/issues/1303 "SFTTrainer not using both GPUs · Issue #1303"))

# Focused forum threads worth scanning

* **Overfitting signs and mitigations in Llama-2 LoRA** (HF forum, Apr 2024). Simple checks and regularization levers. ([Hugging Face Forums](https://discuss.huggingface.co/t/training-llama2-7b-chat-is-my-model-overfitting-i-think-my-model-is-not-learning-anything-how-to-better-train/81974 "Training llama2-7b-chat, is my model overfitting? i think ..."))
* **Script verification for SFT; dropout/weight-decay tips** (HF forum, Jun 23 2025). Quick sanity checks to reduce overfit. ([Hugging Face Forums](https://discuss.huggingface.co/t/verification-of-script-to-train-a-llm-on-supervised-data/160406 "Verification of script to train a LLM on supervised data"))
* **Module coverage debates and loader mismatches**. Track adapter-loader parity across runtimes (vLLM, SGLang). Names must align. ([GitHub](https://github.com/vllm-project/vllm/issues/10798 "LoRa adapter responses not matching peft/transformers ..."))

# Minimal playbook distilled from these sources

* Pick LR via **LR-range test**. Center sweep around the knee. ([arXiv](https://arxiv.org/pdf/1803.09820 "arXiv:1803.09820v2 [cs.LG] 24 Apr 2018"))
* Start with **r=16, alpha=16**, **targets = all linear** (`q,k,v,o,gate,up,down`), **dropout=0**; warmup 5–10%. Expand only if metrics stall. ([docs.unsloth.ai](https://docs.unsloth.ai/get-started/fine-tuning-guide/lora-hyperparameters-guide "LoRA Hyperparameters Guide | Unsloth Documentation"))
* If underfitting, try **alpha=2r** before raising rank. If unstable, reduce LR to **1e-4**. ([docs.unsloth.ai](https://docs.unsloth.ai/get-started/fine-tuning-guide/lora-hyperparameters-guide "LoRA Hyperparameters Guide | Unsloth Documentation"))
* Validate after any **merge** and after any **dtype/FA-2** change. Many regressions come from these toggles. ([GitHub](https://github.com/huggingface/peft/issues/1043 "Merge and unload changes inference a lot on quantized ..."))
