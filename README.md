---
license: apache-2.0
library_name: sentence-transformers
pipeline_tag: feature-extraction
base_model: NeuML/bioclinical-modernbert-base-embeddings
tags:
- gguf
- llama.cpp
- embeddings
- modernbert
- biomedical
---

# BioClinical ModernBERT Embeddings GGUF

[Source model](https://huggingface.co/NeuML/bioclinical-modernbert-base-embeddings) · [Build hub](https://github.com/Dadhichi-Sarker-Shayon/BioClinical-ModernBERT-Embeddings-GGUF)

Pinned conversion of the Apache-2.0 BioClinical ModernBERT mean-pooling sentence embedding model. The native ModernBERT graph has 22 layers, 768 dimensions, local attention, and an 8,192-token native context window.

## Formats

| File | Purpose |
|---|---|
| `bioclinical-modernbert-base-embeddings-F16.gguf` | Reference embeddings |
| `bioclinical-modernbert-base-embeddings-Q8_0.gguf` | Higher-quality compact embeddings |
| `bioclinical-modernbert-base-embeddings-Q4_K_M.gguf` | Smallest release format |

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
