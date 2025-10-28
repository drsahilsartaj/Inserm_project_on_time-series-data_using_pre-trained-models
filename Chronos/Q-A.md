**Q.1 Why use dtype=torch.float32 and not bfloat16 because in the hugging face website its bfloat16?(https://huggingface.co/amazon/chronos-t5-large)**

Answer: <br>
- `torch.float32` is standard and fully supported on all hardware (CPU/GPU).

- `bfloat16` is more efficient only on specialized hardware (e.g., TPUs, high-end GPUs).

- In VS Code on CPU: stick with float32 for full compatibility.

- The Hugging Face model was trained with bfloat16 for efficiency, but you don’t need to use that for inference.

**Use float32 unless you’re running on a TPU/GPU that benefits from bfloat16.**

---

**Q.2 How do I know Chronos expects a 1D tensor?**

Answer: <br>
References: 
1. https://huggingface.co/amazon/chronos-t5-small
2. https://github.com/amazon-science/chronos-forecasting

```
context = torch.tensor([...])
forecast = pipeline.predict(context, prediction_length=4)

```
---
**Q.3 Why is forecast.shape = (1, 20, 4)? What is 20 (num_samples)?**

Answer:<br>
```
forecast = pipeline.predict(context, prediction_length) 
# shape [num_series, num_samples, prediction_length]
```
- `1` --> we have single epidemic time series.
- `4` --> next 4 weeks to forecast.
- `20` --> **20 sample trajectories** --> Chronos doesn’t output one fixed forecast but many possible futures.
**(In simple: It’s like generating 20 possible weather forecasts --> then you summarize using quantiles (e.g., median).)**

Default is `num_samples=20`, we can changed it to 100 like:
<br> Example: `pipeline.predict(context, prediction_length=4, num_samples=100)`

---
**Q.4 What does `forecast[0]` mean (output values)?**

Answer:<br>
- `forecast[0]` is a **(20, 4)** tensor --> 20 different forecast samples, each with 4 weeks.

Output Snippet row 1: <br>
`[14.847, 3.71, 14.847, 3.71]` <br>
means, one possible future = <br>
- week 1: ~15 cases
- week 2: ~3.7 cases
- ...
<br> **Note:** Use `np.quantile(forecast[0].numpy(), 0.5, axis=0)` to summarize across all 20.

---
**Q.5 Why `df["cases"].values` instead of `df["cases"]`?**

Answer: <br>
- `df["cases"]` -> returns a **Pandas Series**
- `df["cases"].values` -> returns a **NumPy array** (which needed for *torch.tensor()*)

Use `.values` to cleanly convert to a tensor without error.

---
**Q.6 Why do Chronos docs mention “tokens,” but we didn’t tokenize anything?**

Answer:<br>

**In short: <mark>Chronos tokenizes internally. No need for AutoTokenizer**<br>
- Internally, **Chronos converts numerical time series values into tokens**, kind of like words in a sentence.
- These tokens are created using `quantization + scaling`, not text tokenizers like in GPT or BERT.
- So when Hugging Face mentions “tokenization,” it refers to **numeric encoding**, not `AutoTokenizer-style`.

So, Why we didn't do it ourself?<br>
When we run:
```
pipeline = ChronosPipeline.from_pretrained(...)
forecast = pipeline.predict(...)
```
The pipeline **handles the tokenization/quantization automatically inside**.

**Note:** <mark> We don't need to tokenize manually, it's abstracted away by **ChronosPipeline**

---

**Q.7 Chronos (as a foundation model) makes probabilistic forecasts, what Does “Probabilistic Forecast” Mean?**

Answer:<br>
- Most traditional models give one fixed number per time step (called a **point forecast**).
- But **probabilistic forecasting** gives a **distribution of possible future outcomes**.
- Example (Probabilistic Forecast for 1 Week Ahead):

Instead of predicting: <br>
                `Cases = 120`

Chronos might say:<br>
80% chance it's between 100 and 140 (gives us the **uncertainty, confidence intervals, and better risk planning**.) <br>
`Median = 120`

**Note:** <mark> Chronos outputs 20 sample forecasts per time step **-->** then summarize them (median, percentiles) for evaluation or decision-making.
