# Advanced Financial RAG Ingestion Engine (DocFinQA)

An optimized, GPU-accelerated document ingestion and preprocessing pipeline designed specifically to tackle the complex, unstructured tabular layouts of SEC financial statements. 

This repository contains the data-indexing architecture developed for a Master of Science dissertation evaluating structural layout resilience in Retrieval-Augmented Generation (RAG) systems. It processes the [DocFinQA](https://github.com/dki-lab/DocFinQA) dataset, semantically chunks multi-column tables, resolves temporal context leaks, and indexes the resulting vectors into ChromaDB for high-precision hybrid retrieval.

## 🚀 Key Architectural Features

Financial documents are notoriously difficult for standard RAG systems due to implied column headers, merged markdown cells, and hidden formatting pollution. This engine implements several advanced defenses:

* **Table-Dominant Year Mode Resolution:** Prevents macro-document temporal data (e.g., the overarching fiscal year) from leaking into localized variance/reconciliation tables by dynamically calculating the statistical mode year of the isolated table block.
* **Row-Atomic Semantic Decomposition:** Automatically fragments dense markdown grids into explicit, standalone declarative statements using a local Llama 3 engine. 
* **Whitespace & Tab Deflation:** Implements strict regex filters to purge hidden formatting artifacts (`\t`) and SEC boilerplate noise that traditionally corrupt exact-string metadata matching in vector databases.
* **Hierarchical Metadata Heuristics:** Automatically classifies the financial scope of extracted chunks into cascading tiers (`CONSOLIDATED`, `SEGMENT`, `ENTITY`, `GENERAL`) for highly granular retrieval filtering.
* **Dual-Pipeline Benchmarking:** Simultaneously indexes data into two isolated vector manifolds:
  * **Pipeline B:** Clean, raw Markdown table chunks.
  * **Pipeline C:** Fully unrolled, LLM-synthesized semantic narratives and row-atomic nodes.

## 🛠️ Technology Stack

* **Vector Database:** [ChromaDB](https://www.trychroma.com/) (Persistent Local Storage)
* **Embedding Model:** `BAAI/bge-large-en-v1.5` (via HuggingFace, CUDA-accelerated)
* **Semantic Synthesizer:** [Ollama](https://ollama.com/) running `llama3:latest` (8B) locally at `temperature=0.0`
* **Framework:** `llama-index`, `PyTorch`

## ⚙️ Prerequisites & Installation

1. **Clone the repository:**
   
   git clone [https://github.com/swapnilrdx/LJMU-Theis-FinanicalRAG](https://github.com/swapnilrdx/LJMU-Theis-FinanicalRAG.git)
   cd LJMU-Theis-FinanicalRAG

2. **Install Python dependencies:**
pip install llama-index llama-index-embeddings-huggingface llama-index-llms-ollama chromadb torch tqdm

3. **Install and Configure Ollama:** 
ollama run llama3

4. **Dataset Setup:**
This script is configured to parse the test.json file from the DocFinQA dataset.

Ensure the JSON file contains the standard Context, Question, and Program keys.

Update the TEST_JSON_PATH variable in the run_integrated_master_loop() function to point to your local dataset path.

Update the SANDBOX_DB_PATH to your desired ChromaDB storage directory.

**Academic Context**
This ingestion engine was built to prove that financial RAG systems require deterministic, structure-aware parsing rather than pure generative autonomy. By forcing tabular data into row-atomic narrative statements before embedding, the system mitigates the spatial context smearing inherent to dense vector models when processing multi-column matrices.

Developed as part of a Master of Science AI & ML Thesis.
