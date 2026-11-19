# Data

## Riverside Community Library handbook (lesson)

`library_handbook.md` is a short member handbook for **Riverside Community Library**, a library invented for this course. It has fourteen sections (opening hours, borrowing, renewals, late returns, lost cards, printing, study rooms, events, accessibility and so on), each a few sentences long.

It's made up on purpose: because the library doesn't exist, no language model can know these details from its training data, so any correct answer about it must come from the handbook itself. That makes it a fair test of retrieval-augmented generation. Its email address uses the reserved `.example` domain, and its phone number is made up.

Written for this course and released with it.

## Course files (mini project)

The mini project reads this course's own `README.md` files, cheatsheets and glossary, either from the neighbouring folders (if you're running inside a copy of the course) or from the course's GitHub repository.

## Models

Both are downloaded from the Hugging Face Hub the first time a notebook runs, and cached in your home folder (`~/.cache/huggingface`), not here.

| model | used for | size | licence |
|---|---|---|---|
| [HuggingFaceTB/SmolLM2-135M-Instruct](https://huggingface.co/HuggingFaceTB/SmolLM2-135M-Instruct) | the lesson's chat model and LoRA fine-tuning | about 270 MB | Apache 2.0 |
| [HuggingFaceTB/SmolLM2-360M-Instruct](https://huggingface.co/HuggingFaceTB/SmolLM2-360M-Instruct) | the mini project's chat model | about 720 MB | Apache 2.0 |
| [sentence-transformers/all-MiniLM-L6-v2](https://huggingface.co/sentence-transformers/all-MiniLM-L6-v2) | sentence embeddings | about 90 MB | Apache 2.0 |

SmolLM2 was released by Hugging Face in 2024 (Allal and colleagues, *SmolLM2: When Smol Goes Big*, 2025). all-MiniLM-L6-v2 is part of the Sentence Transformers project (Reimers and Gurevych, *Sentence-BERT*, 2019).
