# 🚀 Heading-Aware Markdown & RAG Data Preparation Actor

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1RLKkKUuq6vC6oRSG3i0dT6XWqWfQKGgh?usp=sharing)

Convert any webpage or complex documentation into clean, structured Markdown optimized specifically for **Retrieval-Augmented Generation (RAG)** pipelines and Large Language Models (LLMs).

Unlike standard HTML-to-Markdown converters that output noisy text, this Actor respects document hierarchy, preserves crucial metadata, enforces strict token/length constraints, and formats structured elements like tables and code blocks for optimal vector embedding.

---

## 🔥 Key Features

* **Hierarchy-Aware Parsing:** Respects `<h1>` to `<h6>` heading boundaries to preserve semantic context for chunking.
* **Token-Friendly Output:** Prevents context window overflows by applying strict limits without cutting off mid-sentence.
* **Metadata & Hashing Preservation:** Extracts titles, canonical URLs, publication dates, and unique content hashes for vector DB deduplication.
* **Table & Code Block Integrity:** Keeps Markdown tables readable and isolates code blocks cleanly.
* **Noise Removal:** Automatically strips navigation bars, footers, popups, scripts, and sidebars.

---

## 📊 Comparison: Standard Converter vs. This Actor

| Feature | Standard HTML Converter | Our RAG-Optimized Actor |
| :--- | :--- | :--- |
| **Noise Filtering** | Low (includes header/footer links) | High (pure content only) |
| **Heading Boundaries** | Ignored / Arbitrary | Heading-Aware Chunking |
| **Vector DB Ready** | No (needs manual post-processing) | Yes (clean Markdown + Metadata) |
| **Token Safety** | Risks exceeding context limits | Enforced token length bounds |

---

## ⚡ Quick Start (Apify API / Python)

### Python Integration

```python
from apify_client import ApifyClient

# Initialize client with your API token
client = ApifyClient("YOUR_APIFY_TOKEN")

# Prepare input
run_input = {
    "startUrls": [{"url": "[https://docs.python.org/3/](https://docs.python.org/3/)"}],
    "maxDepth": 1,
    "maxTokensPerChunk": 1000,
    "includeMetadata": True,
}

# Run the Actor and wait for completion
run = client.actor("your-username/markdown-rag-crawler").call(
    run_input=run_input
)

# Fetch results from dataset
for item in client.dataset(run["defaultDatasetId"]).iterate_items():
    print(f"Title: {item.get('title')}")
    print(f"Content Sample:\n{item.get('markdown')[:200]}...\n")
