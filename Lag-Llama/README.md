Step 1: To use the Lag-Llama model, **clone the Lag-Llama repository**<br>
```
#downloaded the model’s official code from GitHub into the local laptop.
git clone https://github.com/time-series-foundation-models/lag-llama.git

#go inside the lag-llama folder
cd lag-llama

#Install Repo Editable and Requirements (installed everything Lag-Llama needs to run like PyTorch, transformers, etc.)
pip install -e .
pip install -r requirements.txt
```

---

Step2: Login to Hugging Face
```
pip install huggingface-hub
(or)
#Connects the computer to Hugging Face so we can download the pretrained model.
huggingface-cli login
```
(Here, we need a **Hugging Face token**)

---

Step 3: Download Pre-trained Model Weights (the trained model weights (the model’s “brain”) and saves them locally.)
```
huggingface-cli download time-series-foundation-models/Lag-Llama lag-llama.ckpt --local-dir ./
```
- `huggingface-cli` $\rightarrow$ a small tool that connects to Hugging Face, where the model is hosted online.
- `download ... lag-llama.ckpt` $\rightarrow$ it tells it to download the trained weights (the model file).
- `--local-dir ...` $\rightarrow$ tells it where to save that file on your computer.
- Example: `huggingface-cli download time-series-foundation-models/Lag-Llama lag-llama.ckpt --local-dir ./checkpoints` $\rightarrow$ Moving file to **checkpoints/lag-llama.ckpt**

---

#### Now we have the weights for a pre-trained model that we can use in zero-shot forecasting and fine-tuning.

---

