# Roblox Guard 1.0: Advancing Safety for LLMs with Robust Guardrails

Roblox Guard 1.0 is an instruction fine-tuned moderation model for classifying prompt-level and response-level safety. It can evaluate whether user inputs or model outputs violate a supplied safety taxonomy.

The model is fine-tuned from Llama 3.1 8B Instruct. Llama 3.1 is licensed under the Llama 3.1 Community License, Copyright Meta Platforms, Inc. All Rights Reserved.

## Links

- Model: https://huggingface.co/Roblox/Llama-3.1-8B-Instruct-RobloxGuard-1.0
- Evaluation dataset: https://huggingface.co/datasets/Roblox/RobloxGuard-Eval
- Blog: https://corp.roblox.com/newsroom/2025/07/roblox-guard-advancing-safety-for-llms-with-robust-guardrails
- Paper: https://arxiv.org/abs/2512.05339

## Installation

Use Python 3.10 through 3.13. The pinned `torch==2.7.1` dependency does not publish Python 3.14 wheels.

```bash
python -m venv venv_robloxguard
source venv_robloxguard/bin/activate
pip install -r requirements.txt
```

On Windows PowerShell:

```powershell
python -m venv venv_robloxguard
.\venv_robloxguard\Scripts\Activate.ps1
pip install -r requirements.txt
```

## Inference

Run an evaluation from the repository root:

```bash
python inference.py --config configs/RobloxGuardEval.json
```

Generation is deterministic by default with `generation_temperature` set to `0.0` in the bundled configs. To override it for exploratory runs:

```bash
python inference.py --config configs/RobloxGuardEval.json --temperature 0.2
```

Use `--verbose` to print raw model outputs and per-example records while debugging parse failures.

## Configuration

Each config file is JSON and should follow this shape:

```json
{
  "name": "RobloxGuardEval",
  "model_path": "Roblox/Llama-3.1-8B-Instruct-RobloxGuard-1.0",
  "base_model": "meta-llama/Meta-Llama-3.1-8B-Instruct",
  "max_output_tokens": 100,
  "generation_temperature": 0.0,
  "eval_prompt": "prompts/RobloxGuardEval.txt",
  "llm_output_field": "Response Safety",
  "llm_flagged_value": "unsafe",
  "eval_dataset": "Roblox/RobloxGuard-Eval",
  "eval_label_field": "violation",
  "eval_flagged_value": "true",
  "output_file": "outputs/RobloxGuardEval.csv"
}
```

Relative paths in configs are resolved from the project root, so the script can be launched from outside the repository directory.

## Output Files

- Evaluation CSV: one row per example with the input prompt, input response, prediction, ground-truth label when available, and correctness.
- Summary CSV: final confusion-matrix counts plus precision, recall, F1, and false-positive rate when labels are available.

CSV fields are sanitized to avoid spreadsheet formula execution when results are opened in office tools.

## Directory Structure

```text
.
|-- configs/          Evaluation configs for different datasets
|-- prompts/          Prompt templates with {prompt} and {response} placeholders
|-- inference.py      Evaluation runner
|-- models.py         Dataset/model name constants
|-- requirements.txt  Python dependencies
`-- README.md
```

## Citation

```bibtex
@article{nandwana2025taxonomy,
  title={Taxonomy-Adaptive Moderation Model with Robust Guardrails for Large Language Models},
  author={Nandwana, Mahesh Kumar and Lim, Youngwan and Liu, Joseph and Yang, Alex and Notibala, Varun and Khanna, Nishchaie},
  journal={arXiv preprint arXiv:2512.05339},
  year={2025}
}
```
