# PI ‑ II House Price – Progress Report (2 Jun 2025)

| Milestone | Notebook(s) | Key Ideas & Changes | Validation Winkler | Public LB |
|-----------|-------------|---------------------|--------------------|-----------|
| Starter baseline | `pi-ii-demo-qrf.ipynb` | Kaggle Quantile‑RF demo, no FE, no calibration | ~420 k | – |
| LightGBM quantile baseline | `LightGBM-Quantile-baseline.ipynb` | Basic FE, two LightGBM quantiles (0.05/0.95) | ≈400 k | – |
| Constant conformal pad | `Ensemble.ipynb` | Global CQR (q̂ ≈ 8.4 k) → 90 % cov | **337 219** | – |
| Weight search | `Ensemble_updated_wsearch.ipynb` | Grid blend LGB : Cat (Cat ordinal) | 337 219 | – |
| CatBoost‑raw + adaptive pad | `Ensemble_optA_B.ipynb` | CatBoost on raw categoricals + LGB bag ×5 + 10‑bin adaptive pad | **316 184** | **459 802** (rank 55) |

---

## Achievements
- **Data pipeline** with log-area, distance‑to‑CBD, cleaned `sale_warning`, etc.  
- **Model set**: bagged LightGBM + CatBoost quantile with native cats.  
- **Calibration**: tested constant CQR and adaptive bin‑wise padding.  
- Entered leaderboard.

## Why LB score jumped to 459 k
Adaptive pad under‑covered hidden test ⇒ coverage < 90 % ⇒ Winkler penalty exploded.

## Recovery Plan
1. **Hybrid pad floor**: `pad = max(bin_q, 0.5 × global_q)` to guarantee ≥ ~6–8 k extra width.  
2. **Full OOF constant CQR** for more robust q̂.  
3. Optionally retrain tails at 0.04/0.96, then small pad.

### Expected gains
| Step | Target Winkler |
|------|----------------|
| Hybrid pad | 330 k–360 k |
| OOF CQR | −5 % |
| Tail tweak + stack | < 300 k |

## Next‑session tasks
- Implement hybrid pad, validate, submit.  
- Evaluate coverage gap; fine‑tune pad factor.  
- Move on to OOF calibration and stacking.

