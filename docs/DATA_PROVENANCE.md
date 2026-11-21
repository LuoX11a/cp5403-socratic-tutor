# Data provenance

## 1. Course material (not included in this repository)
| Source | Use | Processing |
|---|---|---|
| CP5403 lecture + supplementary slides, Weeks 1–9 (17 decks) | RAG knowledge base | PPTX → PDF, page-level text, header/footer removal, short pages merged, reference/quiz/non-teaching pages excluded at retrieval |
| CP5403 practical companion guides, Weeks 2–7 (6 documents) | RAG knowledge base | One chunk per section; only instructor-written text kept (objectives, expected-output notes, definitions). Student answers, personal details and code removed |
| Course overview chunk (hand-written) | Logistics questions ("which week covers X?") | Week titles only |

The material is used only for this course assessment and is not redistributed. To rebuild the index, place the PDFs in `MyDrive/CP5403_A2/slides/` and run `01_preprocess_slides.ipynb`.

## 2. Fine-tuning data (included)
| File | Content |
|---|---|
| `data/socratic_train.jsonl` | 54 examples (v1) |
| `data/socratic_val.jsonl` | 5 validation examples |
| `data/socratic_train_v2_augmented.jsonl` | 62 examples (v2): v1 + 8 integrity/out-of-scope examples, context = 2 retrieved chunks + original snippet |
| `data/socratic_val_v2_augmented.jsonl | 5 validation examples with retrieved context (v2) |

Written by the author with AI assistance, then reviewed against the slides (two review rounds: question count, answer leakage, Week 7 coverage, grounding).
Eight categories: opening, misconception follow-up, confirm and extend, direct-answer request, stuck, out of scope, logistics, integrity.

## 3. Evaluation data (included)
`data/eval_questions_v2.json`: 15 held-out questions (checked: none appear in training data), with type, reference note and leak keywords.

## 4. Privacy
No personal data is included. Student names, IDs and answers were removed from practical guides before indexing.
