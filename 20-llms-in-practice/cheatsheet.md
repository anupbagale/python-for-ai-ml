# Module 20 Cheatsheet

## Loading a model

```python
from transformers import AutoModelForCausalLM, AutoTokenizer

model_id = "HuggingFaceTB/SmolLM2-135M-Instruct"
tokenizer = AutoTokenizer.from_pretrained(model_id)
model = AutoModelForCausalLM.from_pretrained(model_id).to(device).eval()
```

- Read the **model card** and the **licence** before using a model.
- **Base** models continue text; **instruct** models follow instructions.
- Memory for the weights: parameters x 4 bytes (32-bit), x 2 (16-bit), x 0.5 (4-bit), plus extra to run.

## Chatting

```python
messages = [{"role": "system", "content": "You are concise."},
            {"role": "user", "content": "Explain RAG in one sentence."}]
prompt = tokenizer.apply_chat_template(messages, tokenize=False, add_generation_prompt=True)
inputs = tokenizer(prompt, return_tensors="pt").to(device)
output = model.generate(input_ids=inputs["input_ids"], attention_mask=inputs["attention_mask"],
                        max_new_tokens=100, do_sample=False, pad_token_id=tokenizer.eos_token_id)
reply = tokenizer.decode(output[0, inputs["input_ids"].shape[1]:], skip_special_tokens=True)
```

- Always use the model's **chat template**; never write the format by hand.
- The model has no memory: send the whole conversation every time.

| setting | use |
|---|---|
| `do_sample=False` | one right answer (extraction, classification) |
| `do_sample=True, temperature=0.7, top_p=0.9` | open-ended writing |
| `max_new_tokens` | always set it |
| `repetition_penalty=1.1` | if a small model loops |

## Prompting

- Be specific: task, audience, length, format.
- Instructions in the system message; material in the user message.
- Show examples as earlier conversation turns (few-shot).
- One task per prompt; split complex jobs into steps.
- Models hallucinate: fluent answers about things they can't know.

## Structured output

```python
match = re.search(r"\{.*?\}", reply, flags=re.DOTALL)     # simple; fails on nested objects (see Exercise 8)
data = json.loads(match.group(0)) if match else None
```

- Parse, **validate** (keys, types, allowed values), retry with sampling, then fall back or ask a person.
- Check the content too: valid JSON can hold a wrong value.

## Embeddings and vector search

```python
from sentence_transformers import SentenceTransformer

embedder = SentenceTransformer("sentence-transformers/all-MiniLM-L6-v2")
doc_vectors = embedder.encode(chunks, normalize_embeddings=True)       # (n, 384), unit length
q = embedder.encode([question], normalize_embeddings=True)[0]
scores = doc_vectors @ q                                               # cosine similarity
top = np.argsort(-scores, kind="stable")[:k]
```

| search | good at | weak at |
|---|---|---|
| keyword (TF-IDF, BM25) | exact names, numbers, rare words | different wording |
| embeddings | meaning, paraphrases | specific names and codes |
| hybrid (e.g. reciprocal rank fusion: sum of `1 / (60 + rank)`) | both | a little more work |

- Chunk by headings for structured documents; otherwise a few hundred words with 10 to 20% overlap.
- Large collections: FAISS, Chroma, Qdrant, pgvector (approximate nearest neighbours).

## RAG

1. Retrieve the top `k` chunks for the question.
2. Put them in the prompt, numbered, with "answer only from these sources; otherwise say you don't know".
3. Generate; show the sources to the reader.

- Refuse when the best similarity is below a threshold set on test questions.
- Keep a budget for the retrieved text (the context window).
- **Prompt injection:** retrieved text can contain instructions. Treat it as untrusted and keep the model's permissions small.

## LoRA

```python
from peft import LoraConfig, get_peft_model

config = LoraConfig(r=8, lora_alpha=16, lora_dropout=0.05, target_modules=["q_proj", "v_proj"], task_type="CAUSAL_LM")
model = get_peft_model(model, config)
model.print_trainable_parameters()
# train as usual, with labels = -100 on the prompt tokens so only the answer is learnt
model.save_pretrained("adapter")         # a few megabytes
model = model.merge_and_unload()         # or merge into the base weights
```

- `W' = W + (alpha / r) B A`, with `B` (`d_out x r`) starting at zero and `A` (`r x d_in`) random.
- QLoRA: LoRA on a 4-bit quantised base model.
- Try prompting, then RAG, then fine-tuning.

## Evaluating

| what | how |
|---|---|
| retrieval | recall@k, MRR on questions with known relevant documents |
| short answers | exact match, token F1, key-fact checks |
| long answers | people; or an LLM judge, checked against people |
| also | latency, cost, behaviour on hostile inputs |

- Separate failures: retrieval missed, or the model misread the right text.
- Read the answers, not just the scores.
- A few dozen questions shows how a system behaves; trust needs many more, written like real questions.

## Running models

- Quantisation (8-bit, 4-bit) shrinks models 2 to 8 times; llama.cpp and Ollama run them on laptops.
- Open models: free, private, controllable, you run them. APIs: strongest models, pay per token, data leaves your machine.
- Start with the smallest model that passes your tests.
