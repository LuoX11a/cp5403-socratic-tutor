# Design log (iterations and decisions)

| # | Problem observed | Change | Effect |
|---|---|---|---|
| 1 | Idea "upload slides and ask questions" has no advantage over ChatGPT | Re-scoped to Socratic tutor (behaviour, not knowledge) | Clear reason for fine-tuning |
| 2 | Slide text extraction slow per query | Offline pre-processing + persisted FAISS index | Retrieval ≈ 0.02 s vs generation 4–6 s |
| 3 | Reference-only and "Thank you" pages retrieved | Filters in chunking | Cleaner context |
| 4 | First dataset: up to 4 questions per reply, one reply leaked the answer, little Week 7 data | Dataset review v2: max 2 questions, 5 new Week 7 + 2 logistics examples | 59 examples |
| 5 | `warmup_ratio` removed in new transformers | Use `warmup_steps`; filter unsupported args | Training runs on transformers 5.x |
| 6 | Tutor invented links, ReLU, wrong facts | Removed `repetition_penalty=1.1`; shorter context (top-2, 60 words) | Most invented content gone |
| 7 | Tutor looped on "which of the four options" | Exclude quiz slides from retrieval | Conversation follows the student |
| 8 | Odd out-of-scope replies; invented assessment requirements | v2 data: 8 new examples + retrieved context in training | Out-of-scope fixed; integrity still failed |
| 9 | New course material (Weeks 8–9, practical guides) | Added to index (568 chunks); new quiz/noise filters | Retrieval test 16/16 |
| 10 | Model still writes frameworks for assessment requests | Rule-based integrity guard, evaluated separately | Integrity item 6/6 |
