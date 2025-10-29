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

By fine-tuning I mean, right now, Hugging Face lets us use the Chronos model, but in the future, they might let us **retrain or fine-tune** the model on our own custom data to make it even more accurate. In short: it would turn into a **fine-tuned model** (better performance for your specific use case).

**Direct Links of the files:**<br>
[amazon/chronos-t5-large Model](chronos-t5-large%20model/chronos-t5.ipynb)

[amazon/chronos-t5-base Model](chronos-t5-base%20model/chronos-t5.ipynb)

[amazon/chronos-t5-small Model](chronos-t5-small%20model/chronos-t5.ipynb)


**Some good references:**<br>
1. https://github.com/amazon-science/chronos-forecasting
2. https://github.com/amazon-science/chronos-forecasting/blob/main/notebooks/chronos-2-quickstart.ipynb
3. https://huggingface.co/amazon/chronos-t5-small
4. https://www.youtube.com/watch?v=jyrOmIiI2Bc
5. [Amazon Chronos Architecture Explanation](https://www.youtube.com/watch?v=_gFycwbfS0g)
6. [Research Paper: Chronos: Learning the Language of Time Series](https://arxiv.org/abs/2403.07815)
7. [Seminar Video where the creator explain about **Chronos**](https://www.youtube.com/watch?v=6eDVkNBxURo)
8. [Extra: Forecast Anything with Transformers with Chronos or PatchTST](https://www.youtube.com/watch?v=P5T-ytvbj50)
