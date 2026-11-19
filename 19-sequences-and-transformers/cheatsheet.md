# Module 19 Cheatsheet

## Language modelling

- A language model gives probabilities for the next token. Generating = sample a token, append it, repeat.
- Measure it with **cross-entropy** on unseen text (lower is better); **perplexity** = `exp(cross-entropy)`. Knowing nothing: `log(vocab_size)`.
- Training pairs: the target is the input shifted by one, so a window of `T` tokens gives `T` predictions.

```python
x = data[s:s + block_size]
y = data[s + 1:s + block_size + 1]
```

## Tokenisation

| tokens | vocabulary | sequences | unknown text |
|---|---|---|---|
| characters | tiny (65 here) | long | only new characters |
| words | huge | short | every new word |
| BPE subwords | chosen (50,000 to 200,000 in big models) | medium | never, when built on bytes |

**BPE training:** start from characters; repeatedly count neighbouring pairs (weighted by word counts), merge the most frequent pair into a new token. **Encoding:** apply the merges in the same order.

## Embeddings

```python
emb = nn.Embedding(vocab_size, d_model)     # a learnt table, one row per token
vectors = emb(ids)                          # same as emb.weight[ids]
```

- A `(V, V)` embedding table used as logits is a bigram model.
- Add a position embedding: `tok_emb(idx) + pos_emb(torch.arange(t))`.

## Attention

```python
scores = q @ k.transpose(-2, -1) / math.sqrt(d)           # (..., T, T)
future = torch.triu(torch.ones(T, T, dtype=torch.bool), diagonal=1)
weights = torch.softmax(scores.masked_fill(future, float("-inf")), dim=-1)
out = weights @ v

out = F.scaled_dot_product_attention(q, k, v, is_causal=True)   # the same, faster
```

- **Query**: what a position looks for. **Key**: what it contains. **Value**: what it passes on.
- Divide by `sqrt(d)` so the softmax doesn't saturate.
- The causal mask stops positions seeing the future. Without it (encoders, cross-attention) every position sees everything.
- Mask conventions differ: in `masked_fill` and `nn.MultiheadAttention`, `True` = blocked; in `F.scaled_dot_product_attention`'s boolean `attn_mask`, `True` = allowed.

## Multi-head attention

```python
q, k, v = self.qkv(x).split(d, dim=2)
q, k, v = (z.view(b, t, heads, d // heads).transpose(1, 2) for z in (q, k, v))   # (b, heads, t, head_size)
out = attention(q, k, v)
out = self.proj(out.transpose(1, 2).reshape(b, t, d))                          # join the heads
```

## Transformer block and GPT

```python
x = x + attn(ln1(x))          # communicate (pre-norm, residual)
x = x + mlp(ln2(x))           # compute: Linear(d, 4d), GELU, Linear(4d, d)

logits = head(ln(blocks(tok_emb(idx) + pos_emb(positions))))
loss = F.cross_entropy(logits.flatten(0, 1), targets.flatten())
```

- Layer norm normalises each position's vector, independent of the batch.
- Score only some targets by setting the others to `-100` (`ignore_index`).

## Generating

```python
@torch.no_grad()
def generate(model, idx, n_new, temperature=1.0, top_k=None):
    model.eval()
    for _ in range(n_new):
        logits = model(idx[:, -block_size:])[:, -1, :] / temperature   # crop context, last position only
        if top_k:
            logits[logits < logits.topk(top_k).values[:, -1:]] = float("-inf")
        idx = torch.cat([idx, torch.multinomial(logits.softmax(-1), 1)], dim=1)
    return idx
```

| setting | effect |
|---|---|
| temperature < 1 | safer, more repetitive |
| temperature > 1 | more surprising, more errors |
| greedy (`argmax`) | deterministic; good for exact answers, loops on open text |
| top-k / top-p | cut off the unlikely tail |

## Scale and practice

- Attention cost grows with the **square** of the context length.
- A **KV cache** stores earlier keys and values during generation.
- Pretraining (predict the next token on huge text) -> instruction tuning -> feedback training gives a chat assistant.
- Data format matters: writing numbers least significant digit first made addition far easier to learn; writing intermediate steps ("chain of thought") helps for the same reason.
- Position tables limit the length a model can handle; models don't automatically generalise to longer inputs than they were trained on.
