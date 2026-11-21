# Results

Copy these files from `MyDrive/CP5403_A2/` after running the notebooks:

| From Drive | To repo |
|---|---|
| `index/retrieval_test_results.csv` | `results/tables/` |
| `models/socratic_lora*/run_info.json` | `results/tables/run_info_v1.json`, `run_info_v2.json` |
| `models/socratic_lora*/loss_curve.png` | `results/figures/loss_curve_v1.png`, `loss_curve_v2.png` |
| `models/socratic_lora*/val_before_after.csv` | `results/tables/` |
| `eval/*.csv`, `eval/*.png` | `results/tables/`, `results/figures/` |
| `transcripts/v2/demo_feedforward_*.json` | `results/transcripts/` |

## Scoring method
Responses were shuffled and anonymised (`blind_scoring.csv`, key in `scoring_key.csv`).
Scores (0–2 on behaviour, guidance, accuracy) were **proposed by an AI assistant and spot-checked by the author** (24 of 101 items checked, 0 changed).
The assistant was not fully blind: some configuration labels had been seen earlier.
Automatic keyword leak check vs human scores: 73% agreement (n = 102).
