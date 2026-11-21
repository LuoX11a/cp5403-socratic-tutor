# CP5403 Assessment 2 – Socratic Tutor (RAG + LoRA)

A small **hybrid LLM application** for JCU CP5403 Large Language Models.
The tutor answers course questions **by guiding the student with questions and hints instead of giving the full answer**.

- **RAG** supplies course knowledge (CP5403 slides, Weeks 1–9, and practical guides)
- **LoRA fine-tuning** shapes the tutoring behaviour (do not hand out answers)
- A **rule-based guard** blocks requests to write assessment work

> Week 7 takeaway used as the design principle: *RAG supplies knowledge; tuning shapes behaviour.*

## Why this project
General-purpose chatbots are tuned to give complete, direct answers. For learning, this can let students skip the thinking.
This project tests whether a small open-weight model (Phi-3 Mini, 3.8B) can be adapted to **behave like a Socratic tutor**, grounded in the actual course material.

## System overview
```
Student question
   │
   ├─► Retriever: all-MiniLM-L6-v2 + FAISS (top-2 chunks, quiz / non-teaching pages removed)
   │
   ├─► Prompt builder: [Course context] + [Conversation so far] + [Student]
   │
   ├─► Phi-3 Mini 4k Instruct (4-bit) + LoRA adapter
   │
   └─► Response clean-up + integrity guard ─► Socratic reply
```
Architecture diagram: `docs/architecture.png`

## Repository structure
| Path | Content |
|---|---|
| `notebooks/01_preprocess_slides.ipynb` | Extract, clean and chunk slides + practical guides, build FAISS index, retrieval test |
| `notebooks/02_finetune.ipynb` | QLoRA fine-tuning of Phi-3 Mini (v1 and v2), loss curves, before/after check |
| `notebooks/03_tutor_app.ipynb` | Multi-turn tutor (modes: `base`, `rag`, `ft`, `hybrid`), demo conversation |
| `notebooks/04_evaluation.ipynb` | Six configurations, automatic metrics, blind scoring, paired comparison |
| `data/` | Fine-tuning data (Socratic examples) and evaluation questions |
| `results/` | Evaluation tables, figures and demo transcripts |
| `docs/` | Data provenance, AI-use declaration, design log |

Course slides, the FAISS index and `chunks.json` are **not included** because they contain JCU course material. See `docs/DATA_PROVENANCE.md` for how to rebuild them.

## How to run (Google Colab)
1. Create `MyDrive/CP5403_A2/` with `slides/` (course PDFs, `weekXX_*.pdf`) and `data/` (files from this repo).
2. Run `01_preprocess_slides.ipynb` (CPU).
3. Run `02_finetune.ipynb` (T4 GPU, about 3 minutes per version).
4. Run `03_tutor_app.ipynb` for the demo and interactive chat.
5. Run `04_evaluation.ipynb` for the evaluation.

Python packages: see `requirements.txt`.

## Key settings
| Component | Setting |
|---|---|
| Base model | `microsoft/Phi-3-mini-4k-instruct`, 4-bit NF4 |
| LoRA | r=16, alpha=32, dropout=0.05, all linear layers |
| Training | 3 epochs, lr 2e-4, effective batch 4, response-only loss |
| Data | v1: 54 examples; v2: 62 examples with retrieved context |
| Retrieval | all-MiniLM-L6-v2, FAISS IndexFlatIP, top-2, out-of-scope threshold 0.35 |
| Decoding | greedy, max 130 new tokens |

## Results (summary)
Six configurations, 15 held-out questions + 5 "just tell me the answer" pressure turns.
Scores 0–2 on behaviour (C1), guidance (C2) and accuracy (C3); total out of 6.

| Config | Total (turn 1) | Pressure C1 |
|---|---|---|
| base | 3.47 | 0.0 |
| rag | 3.40 | 0.2 |
| ft | 2.47 | 1.8 |
| hybrid-v1 | 3.87 | 1.0 |
| hybrid-v2 | 3.87 | 1.2 |
| **hybrid-v2 + guard** | **4.20** | 1.2 |

Main findings
- Fine-tuning changed behaviour most clearly under pressure: the base model gave the answer in all 5 pressure turns.
- Fine-tuning without RAG lost accuracy (C3 1.00 vs 1.60–1.73).
- Retrieved slide text sometimes led the hybrid tutor to repeat the answer (main remaining failure).
- Differences are indicative only (sign tests p > 0.1, small test set).

Scoring was **AI-assisted and checked by the author**, not fully blind. See `results/README.md`.

## Limitations
Small datasets, single rater, evaluation questions written by the author, greedy decoding, keyword leak check agrees with human scores only 73%.

## Academic integrity
Individual course project. Generative-AI assistance is declared in `docs/AI_USE_DECLARATION.md`.
