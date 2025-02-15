
# Introducing SmolLlama - A Smaller Language Model 

- So, I trained a Llama a 130M architecture I coded from ground up to try and behave like an instruct model, going through the below-mentioned stages from scratch 

The three main stages are:

1) Pretraining
2) SFT (Instruction Tuning)
3) Reward Tuning for human-like responses (DPO)


 ### 1) Pretraining

#### Dataset

 - I used the [FineWeb](https://huggingface.co/datasets/HuggingFaceFW/fineweb?row=0) dataset from HuggingFace (10BT checkpoint) consisting of roughly 15M texts.

  1) Train dataset - 12 M texts
  2) Val dataset - 3M texts

- After tokenization (GPT2), a total of 186k batches were formed for a batch size of 64 across 4x 4090s with 46k batches for each node.

---

####  ModelArgs (Hyperparameters)

| Parameter              | Value         | Description                                                                 |
|------------------------|---------------|-----------------------------------------------------------------------------|
| `block_size`           | 128           | The size of each block.                                                     |
| `batch_size`           | 64            | The number of samples processed before the model is updated.                |
| `embeddings_dims`      | 768           | The dimensionality of the embeddings.                                       |
| `attn_dropout`         | 0.1           | Dropout rate for attention layers.                                          |
| `no_of_heads`          | 6             | Number of attention heads (needs thorough calculation).                     |
| `dropout`              | 0.1           | Dropout rate for the model.                                                 |
| `max_lr`               | 1e-4          | Maximum learning rate.                                                      |
| `no_of_decoder_layers` | 6             | Number of decoder layers (needs thorough calculation).                      |
| `weight_decay_optim`   | 0.1           | Weight decay for the optimizer.                                             |
| `beta_1`               | 0.9           | Exponential decay rate for the first moment estimates in the optimizer.     |
| `beta_2`               | 0.95          | Exponential decay rate for the second moment estimates in the optimizer.    |
| `vocab_size`           | 50258         | Vocab size                                                                  |
| `device`               | 'cuda'      | The device to run the model on (e.g., 'cuda:0' for GPU).                    |
| `no_kv_heads`          | 2             | Number of key-value heads.                                                 
---
#### Hardware Setup

 - Used DPP using Pytorch torchrun consisting of 4x GeForce RTX 4090s (24gb VRAM each) rented on runpod.io
---

#### Frameworks:
**Pytorch**


--- 

#### Epochs/Steps
- Iterations (train) = 45k

- Val iterations = every 1k
---

#### Losses
- Train loss - Nill

- Val loss - Nill

---

#### Screenshots of the loss curves


---

### Local setup


### Requirements

```python
git [clone the repo](https://github.com/YuvrajSingh-mist/SmolLlama.git)
cd SmolLlama
bash ./install.sh

```

---

### Running

- If you want to use your dataset, please take a look at the dataset link provided.
- If you have one, move your dataset to the data/ folder and then change the following line to point to your dataset in the data/ (currently only .txt is supported) in the llama_multi_gpu_train.py
- Also please change 'device' to any of your available cuda gpus.

```python
'data/input.txt' -> 'data/{YPU_FILE_NAME_HERE}' line  66

```
To run:

```python
torchrun --standalone --nproc_per_node=gpu llama.py
```
--standalone - if all the gpu are on one server
--npro_per_node - number of gpus available and use the keyword gpu to use all
