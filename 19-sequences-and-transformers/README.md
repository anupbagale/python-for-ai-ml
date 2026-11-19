# Module 19: Sequences and Transformers

Large language models such as ChatGPT, Gemini and Llama are built from one design, the transformer, and trained to do one thing: predict the next token. This module builds that design from the ground up. You'll tokenise text with your own byte-pair encoder, start from a model that simply counts, write attention in a few lines, assemble multi-head attention and transformer blocks, and train a small GPT that writes in the style of Shakespeare. The mini project then teaches a transformer to add, and shows how much the way data is written decides what a model finds easy.

Everything runs on an ordinary CPU, in Colab or on your own computer.

## Notebooks

| # | Notebook | What's in it | Open |
|---|----------|--------------|------|
| 1 | Lesson | Next-token prediction, tokenisation and BPE from scratch, a bigram model, cross-entropy and perplexity, embeddings, attention from scratch, queries, keys and values, multi-head attention, positions, the transformer block, training a small GPT, temperature and top-k, attention maps, from here to ChatGPT | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/19-sequences-and-transformers/01_lesson.ipynb) |
| 2 | Exercises | 12 exercises, from the causal mask to a BPE trainer, multi-head attention and a transformer block checked against PyTorch's own | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/19-sequences-and-transformers/02_exercises.ipynb) |
| 3 | Solutions | Worked solutions, with the idea behind each and the mistake each check catches | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/19-sequences-and-transformers/03_solutions.ipynb) |
| 4 | Mini project | A transformer that learns to add 5-digit numbers: normal against reversed answers, unseen test problems, where the mistakes come from, and what the attention heads learnt | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/19-sequences-and-transformers/04_mini_project.ipynb) |

There's also a [cheatsheet](cheatsheet.md) for quick revision. The data is described in [data/README.md](data/README.md).

## What you'll learn

- **Language models.** Predicting the next token, generating text one token at a time, and measuring a model with cross-entropy and perplexity.
- **Tokenisation.** Characters, words and subwords, and byte-pair encoding written from scratch.
- **A counting model.** The bigram model, and the same model learnt by gradient descent.
- **Embeddings.** Learnt vectors for tokens and for positions.
- **Attention.** From averaging the past with a triangular matrix to queries, keys and values, the causal mask, and why scores are divided by `sqrt(d)`.
- **Multi-head attention** and the **transformer block**: attention and an MLP, with residual connections and layer normalisation.
- **A small GPT.** About 800,000 parameters, trained on Tiny Shakespeare in a couple of minutes on a CPU.
- **Generation.** Temperature, top-k and top-p.
- **Interpretability.** Reading what the attention heads have learnt.
- **Scale.** What separates this model from GPT-2 and today's large models, and how a text predictor becomes a chat assistant.

## Exercises

| Level | Exercises |
|-------|-----------|
| Easy | `encode_chars`, `causal_mask`, `next_token_pairs`, `temperature_top_k` |
| Medium | `bigram_probabilities`, `merge_pair`, `scaled_dot_product_attention`, `sinusoidal_positions`, `generate_greedy` |
| Hard | `bpe_train`, `MultiHeadAttention`, `TransformerBlock` |

The checks compare your code with PyTorch wherever possible, and catch the classic mistakes:

- a mask that lets positions see the future, or hides their own token
- counting repeated pairs only once with fancy indexing
- merging overlapping pairs twice
- `k.T` instead of `k.transpose(-2, -1)` once there's a batch
- reading the prediction from the wrong position when generating
- splitting heads with `view` alone, without the transpose
- post-norm instead of pre-norm, or a missing residual connection

## Mini project

**A transformer that learns to add.** The lesson's GPT, trained on random sums of two 5-digit numbers.

- **Two formats, one difference.** Identical models learn to write the answer in the usual order or backwards (least significant digit first).
- **Honest testing.** 5,000 test problems never used in training, out of 10 billion possible, so neither model can rely on memory.
- **The result.** The reversed model is perfect on the test set; the normal-order model is good but still makes mistakes.
- **Why.** Its mistakes come from runs of columns that add up to 9, where it has to look several steps ahead to know whether a carry is coming. Its accuracy falls from 100% with no such run to under 40% with runs of four or five.
- **Inside.** The reversed model's attention heads line up each answer digit with the matching digits of the two numbers, or with the column the carry comes from.

## Prerequisites

Modules [1](../01-python-foundations) to [18](../18-computer-vision). Module 17 (PyTorch, training loops, layer and batch normalisation) is used throughout; residual connections come from Module 18.

## Next

Module 20: LLMs in Practice (planned).
