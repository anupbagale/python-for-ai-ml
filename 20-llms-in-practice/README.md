# Module 20: LLMs in Practice

Module 19 built a language model from scratch. Real applications start from a pretrained one and build around it. This module covers that practical toolkit with free, open models that run on an ordinary computer or a free Colab session: talking to a chat model properly, prompting and structured output, embeddings and vector search, retrieval-augmented generation (RAG), fine-tuning with LoRA, and, most importantly, evaluating whether an LLM application actually works.

The models come from the Hugging Face Hub: **SmolLM2** (135 and 360 million parameters, Apache 2.0) for generation, and **all-MiniLM-L6-v2** (Apache 2.0) for embeddings. No paid API or account is needed.

## Notebooks

| # | Notebook | What's in it | Open |
|---|----------|--------------|------|
| 1 | Lesson | Pretrained models and the Hub, chat templates, generation settings, prompting, structured JSON output with validation and retries, embeddings, keyword against semantic search, vector search and chunking, RAG, prompt injection, fine-tuning with LoRA, evaluating a RAG system, choosing and running models | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/20-llms-in-practice/01_lesson.ipynb) |
| 2 | Exercises | 12 exercises, from cosine similarity and chunking to BM25, a LoRA layer and 8-bit quantisation; no model downloads needed | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/20-llms-in-practice/02_exercises.ipynb) |
| 3 | Solutions | Worked solutions, with the idea behind each and the mistake each check catches | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/20-llms-in-practice/03_solutions.ipynb) |
| 4 | Mini project | Ask the Course: a RAG assistant over this course's own README files, with four search methods compared, answers scored, and a safety net for off-topic questions | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/20-llms-in-practice/04_mini_project.ipynb) |

There's also a [cheatsheet](cheatsheet.md) for quick revision. The data is described in [data/README.md](data/README.md).

**About the outputs.** The lesson and the mini project are saved without outputs, so you see the model's real answers when you run them rather than someone else's. Language models give slightly different text on different computers and library versions, so no two runs look exactly alike. The exercises and solutions are saved with their outputs, as in other modules.

## What you'll learn

- **Pretrained models.** The Hugging Face Hub, model cards and licences, base against instruct models, and how much memory a model needs.
- **Chat templates.** Why every model family has its own conversation format, and how to use it correctly.
- **Generation settings.** Greedy decoding, temperature, top-p and the length limit.
- **Prompting.** Specific instructions, examples (few-shot prompting), and the limits no prompt can fix, such as hallucination.
- **Structured output.** Asking for JSON, then parsing, validating and retrying, because a model's output is never guaranteed.
- **Embeddings.** Sentence embeddings, cosine similarity, and why meaning-based search finds what keyword search misses.
- **Vector search** and **chunking**.
- **RAG.** Answering from your own documents, refusing when the documents don't help, and the risk of prompt injection.
- **LoRA.** Fine-tuning a small low-rank correction, here to turn a chat model into a reliable message router.
- **Evaluation.** Test sets, retrieval and answer metrics, reading failures, and the caveats of using a model as a judge.
- **Choosing models.** Open models against commercial APIs, quantisation, and running models locally.

## Exercises

| Level | Exercises |
|-------|-----------|
| Easy | `cosine_similarity_matrix`, `top_k_indices`, `chunk_words`, `format_chatml` |
| Medium | `build_rag_prompt`, `retrieval_metrics`, `answer_f1`, `extract_json`, `top_p_filter` |
| Hard | `bm25_scores`, `LoRALinear`, `quantize_int8` |

The checks catch the classic mistakes:

- `nan` from zero vectors in a similarity search
- unstable sorting that reorders tied results
- duplicate chunks at the end of a document
- a prompt budget that cuts sources in the wrong place
- recall divided by `k` instead of by the number of relevant documents
- a JSON extractor that stops at the first closing brace
- dropping the token that crosses the top-p threshold
- a LoRA layer that doesn't start out identical to the original

## Mini project

**Ask the Course.** A question-answering assistant for this course, built from its README files, cheatsheets and glossary.

- **A test set first.** 24 questions, each with the module that answers it and the key facts a correct answer must contain.
- **Four search methods** (TF-IDF, BM25, embeddings and a hybrid with reciprocal rank fusion), compared with recall@3 and MRR.
- **Answers with sources**, from SmolLM2-360M-Instruct, scored automatically and compared with closed-book answers.
- **Failure analysis** that separates retrieval failures from reading failures.
- **A safety net** that refuses questions the course doesn't cover, with the threshold set by measurement.

## Prerequisites

Modules [1](../01-python-foundations) to [19](../19-sequences-and-transformers). Module 19 (tokens, attention, generation) is used throughout; cosine similarity comes from Module 16, and the training loop from Module 17.

## Next

[Module 21: Responsible and Production AI](../21-responsible-and-production-ai)
