### Results
| Model                | MAE    | RMSE   | R²        |
| -------------------- | ------ | ------ | --------- |
| **chronos-t5-large** | 266.11 | 303.96 | -112.12 ❌ |
| **chronos-t5-base**  | 53.90  | 57.11  | -2.99 ❌   |
| **chronos-t5-small** | 197.25 | 233.28 | -65.63 ❌  |

Simple Interpretation:<br>
| Metric   | Meaning                                                                             |
| -------- | ----------------------------------------------------------------------------------- |
| **MAE**  | Average size of prediction error (lower is better)                                  |
| **RMSE** | Same as MAE but penalizes large errors more                                         |
| **R²**   | 1.0 = perfect, 0 = no better than mean, **< 0 = worse than predicting the average** |

**Observations**:<br>
- All models have negative $R^2$,

This means: <mark>“The model performed worse than just predicting the average of the training values.”

- `t5-base` clearly outperforms small and large models:
<br> MAE and RMSE are lowest.

Still not “good” but better than others.

**Why we are getting this bad result and Future Work**:<br>
| Cause                                | Explanation                                                                                           |
| ------------------------------------ | ----------------------------------------------------------------------------------------------------- |
| 🔢 **Small dataset**                 | Chronos is pre-trained, but in our series data we have < 50 points which is not enough context for large models. |
| 💣 **T5-large overfits or diverges** | More parameters $\neq$ better when data is scarce.                                                         |
| ⚖️ **T5-base = balanced**            | Enough capacity without overfitting your small context.                                               |
| 🎯 **Zero-shot limitation**          | You're not training or fine-tuning — so model has to generalize blindly.                              |

Therefore, in future we can work on more data and do some fine-tuning.

**Direct Links of the files:**<br>
[amazon/chronos-t5-large Model](./Chronos/chronos-t5-large%20model/chronos-t5.ipynb)
[amazon/chronos-t5-base Model](./Chronos/chronos-t5-base%20model/chronos-t5.ipynb)
[amazon/chronos-t5-small Model](./Chronos/chronos-t5-small%20model/chronos-t5.ipynb)



