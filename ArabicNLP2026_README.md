# Rosetta at AlexandriaX-2026

**LoRA-Adapted NileChat for Context-Aware Dialectal Arabic Dialogue Translation**

Nada Esmaeil, Fathima Rena, Sibi Subhash, Osama Elgendy, Mina Naguib, Salma Omar, Muhammad Arif

📄 Paper: [`Rosetta_at_AlexandriaX.pdf`](Rosetta_at_AlexandriaX.pdf)
💻 Code, fine-tuning scripts and adapter weights: [NadaAdelMousa/Rosetta_at_AlexandriaX](https://github.com/NadaAdelMousa/Rosetta_at_AlexandriaX)

## Overview

Rosetta is our system for **Subtask 1 (Context-Aware English-to-Dialectal Arabic Dialogue Translation)** of the AlexandriaX-2026 shared task. Given an English dialogue turn plus its metadata (target country/dialect, domain, speaker/addressee gender, persona) and the preceding dialogue history, the system translates only the current turn into the target Arabic dialect. The test set covers 13 dialects.

## Method

- **Base model:** `UBC-NLP/NileChat-3B` (frozen)
- **Adapter:** LoRA, r = 32, α = 32, dropout = 0, on attention and MLP projections (`q/k/v/o_proj`, `gate/up/down_proj`)
- **Prompting:** structured system/user prompts that encode the target dialect, conversation metadata and dialogue history. At inference, the model's own earlier translations are fed back as history.
- **Training:** 1 epoch, max sequence length 1,024, batch size 8, gradient accumulation 4, learning rate 2 × 10⁻⁴ with a cosine schedule, single Tesla T4
- **Constrained track:** Alexandria data only (model ≤ 5B parameters)
- **Unconstrained track:** same architecture, with the adapter first pretrained on MADAR and PADIC

## Results (official test set)

| Track | spBLEU | chrF++ | Rank |
|---|---|---|---|
| Constrained | 26.10 | 41.79 | 4th |
| Unconstrained | 25.09 | 41.02 | 5th |

Performance is strongest on Levantine and Egyptian dialects and weakest on Mauritanian, Libyan, Sudanese and Moroccan. External pretraining on MADAR/PADIC helped only Libyan and Moroccan and slightly lowered the overall average, which suggests negative transfer. Per-dialect scores are in Table 3 of the paper.

## Citation

```bibtex
@misc{esmaeil2026rosetta,
  title  = {Rosetta at AlexandriaX-2026: LoRA-Adapted NileChat for Context-Aware Dialectal Arabic Dialogue Translation},
  author = {Esmaeil, Nada and Rena, Fathima and Subhash, Sibi and Elgendy, Osama and Naguib, Mina and Omar, Salma and Arif, Muhammad},
  year   = {2026},
  note   = {AlexandriaX-2026 Shared Task, Subtask 1}
}
```
