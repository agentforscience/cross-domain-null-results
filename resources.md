# Resources and research process

The resource-finder stage was skipped for this run (`.neurico/pipeline_state.json`: `"skipped": true`), so no
`literature_review.md`, `papers/`, `code/` or pre-downloaded `datasets/` existed. Everything below was gathered
during the experiment session. The literature summary is in REPORT.md §3 and is written from the author's
knowledge of the cited papers, not from a systematic search.

## Datasets
| Dataset | Used for | Access |
|---|---|---|
| JailbreakBench `judge_comparison` (Chao et al., 2024) | overt-safety anchors/controls with human labels | HF `JailbreakBench/JBB-Behaviors` |
| sycophancy-eval `answer.jsonl` (Sharma et al., 2023) | prompts in which the user asserts a wrong answer | HF `meg-tong/sycophancy-eval` |
| GSM8K test (Cobbe et al., 2021) | math problems with gold answers | HF `gsm8k` |

## Models (all open weights, run locally in fp16 on one RTX A6000)
| Model | Role |
|---|---|
| `Qwen/Qwen2.5-7B-Instruct` | anchor/control generator; second judge |
| `unsloth/Meta-Llama-3.1-8B-Instruct` (ungated mirror of Llama-3.1-8B-Instruct) | primary LLM judge |
| `ibm-granite/granite-guardian-3.1-2b` | guard model, default "harm" risk |
| `unitary/toxic-bert` | toxicity classifier |

Not available: Llama Guard 3 and ShieldGemma (gated; no access with the machine's HF account); any API model
(no `OPENROUTER_KEY` or other provider key in the environment).

## Process log
1. Found workspace without resources or API key → designed a local-model study.
2. Mis-read the GPU check as "no GPU"; piloted llama.cpp on CPU; machine became heavily loaded by other jobs.
3. Re-checked, found 4× RTX A6000, moved to GPU; discarded CPU pilots.
4. Built and verified anchors (two prompt revisions, one filter revision — all before running detectors).
5. Ran detectors, analysis, one post-hoc rubric variant; wrote the report.
