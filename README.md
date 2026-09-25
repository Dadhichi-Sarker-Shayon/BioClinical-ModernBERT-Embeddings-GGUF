---
license: apache-2.0
library_name: sentence-transformers
pipeline_tag: feature-extraction
base_model: NeuML/bioclinical-modernbert-base-embeddings
tags:
- gguf
- llama.cpp
- embeddings
- sentence-embeddings
- retrieval
- reranking
- modernbert
- biomedical
- clinical
- pubmed
- semantic-search
- feature-extraction
---

# BioClinical ModernBERT Embeddings GGUF

<div align="center">

<img alt="Model" src="https://img.shields.io/badge/model-BioClinical--ModernBERT-8A2BE2?style=for-the-badge">
<img alt="GGUF formats" src="https://img.shields.io/badge/GGUF-F16%20%7C%20Q8_0%20%7C%20Q4_K_M-FFD21E?style=for-the-badge">
<img alt="Task" src="https://img.shields.io/badge/task-embeddings%20%2F%20retrieval-00A6A6?style=for-the-badge">
<img alt="Dimensions" src="https://img.shields.io/badge/dimensions-768-16A34A?style=for-the-badge">
<img alt="Context" src="https://img.shields.io/badge/context-8192-0069B4?style=for-the-badge">
<img alt="License" src="https://img.shields.io/badge/license-Apache--2.0-7C3AED?style=for-the-badge">

</div>

[Source model](https://huggingface.co/NeuML/bioclinical-modernbert-base-embeddings) · [HF release](https://huggingface.co/ShayonSarker/BioClinical-ModernBERT-Embeddings-GGUF) · [Build hub](https://github.com/Dadhichi-Sarker-Shayon/BioClinical-ModernBERT-Embeddings-GGUF)

Pinned conversion of the Apache-2.0 BioClinical ModernBERT mean-pooling sentence embedding model. The native ModernBERT graph has 22 layers, 768 dimensions, local attention, and an 8,192-token native context window.

## Formats

| File | Status | Purpose |
|---|---|---|
| `bioclinical-modernbert-F16.gguf` | Published | Reference embeddings |
| `bioclinical-modernbert-Q8_0.gguf` | Published | Higher-quality compact embeddings |
| `bioclinical-modernbert-Q4_K_M.gguf` | Published | Smallest release format |

## Verified retrieval

Real `llama-embedding` output, `--pooling mean --embd-normalize 2`, 768-dimensional L2-normalised vectors, cosine similarity. Query: *"Which drug class is used to treat type 2 diabetes?"*

| Rank | Q4_K_M | Q8_0 | F16 | Document |
|---:|---:|---:|---:|---|
| 1 | 0.7165 | 0.7094 | 0.7058 | Metformin is a first-line oral medication for type 2 diabetes mellitus. |
| 2 | 0.4085 | 0.3903 | 0.3870 | Insulin glargine is a long-acting basal insulin analogue for diabetes. |
| 3 | 0.2887 | 0.2768 | 0.2751 | Amoxicillin is a beta-lactam antibiotic used for bacterial infections. |
| 4 | 0.1376 | 0.1267 | 0.1239 | Aspirin irreversibly inhibits cyclooxygenase enzymes in platelets. |
| 5 | 0.0714 | 0.0640 | 0.0658 | The Airbus A320 is a narrow-body commercial airliner. |
| 6 | 0.0270 | 0.0163 | 0.0177 | The Great Wall of China was built across northern China. |

All three quantizations produce the same ranking, and the off-domain sentences land at the bottom, so Q4_K_M is usable for retrieval. Scores are cosine values, not probabilities, and the margins are not calibrated confidence.

## Validation

Twenty PubMedQA query/document pairs, 768-dimensional mean-pooled vectors, L2-normalized comparison:

| Format | Mean cosine to source | Top-1 retrieval |
|---|---:|---:|
| F16 | 0.999998 | 0.95 |
| Q8_0 | 0.999672 | 0.95 |
| Q4_K_M | 0.984172 | 0.95 |

The F16 GGUF matches the original SentenceTransformer embedding path. Retrieval agreement is measured against the 20-pair set, not a large benchmark.

## Build

```bash
python -m pip install -r requirements-build.txt
python build_gguf.py --model-id NeuML/bioclinical-modernbert-base-embeddings
```

The builder pins llama.cpp commit `6b790a9c291b5d7af3312bbf9f0c558aa023b13e` and the upstream model revision. It does not upload or overwrite this repository.

## License

Apache-2.0. See the upstream model card and `LICENSE` for source attribution.
