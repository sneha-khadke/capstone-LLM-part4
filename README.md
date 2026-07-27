LLM-Powered Structured Insight & Retrieval Feature

Project Overview

This project implements two complete LLM-powered pipelines:

1. Structured Information Extraction with Schema Validation
2. Retrieval-Augmented Generation (RAG) Mini Pipeline

The project demonstrates how Large Language Models can extract structured data from unstructured text while ensuring schema validation, and how Retrieval-Augmented Generation can answer questions using a document collection instead of relying only on the model's internal knowledge.

---

Technologies Used


| Component                  | Tool Used                                              | Reason for Selection                                                                 |
|----------------------------|--------------------------------------------------------|--------------------------------------------------------------------------------------|
| Large Language Model (LLM) | Google Gemini 2.5 Flash (Google AI Studio API)         | Free API, fast inference, reliable structured JSON generation, and strong instruction following. |
| Schema Validation          | Pydantic v2                                            | Provides strict schema validation, type safety, enum support, and automatic error handling. |
| Embedding Model            | sentence-transformers (all-MiniLM-L6-v2)               | Free, lightweight, and generates high-quality semantic embeddings for retrieval tasks. |
| Vector Store               | FAISS (Facebook AI Similarity Search)                  | Enables fast and efficient similarity search for semantic document retrieval. |
| Programming Language       | Python                                                 | Rich ecosystem for AI/ML development, data processing, and library integration. |
| Development Environment    | Google Colab                                           | Cloud-based environment with no local setup, free GPU access, and easy package management. |

---

Project Structure

```

│── README.md
│── requirements.txt
│── Part4.ipynb
│── customer_messages.csv
│── validated_results.csv
│── validated_results.json
│── rag_demo_output.csv
│── rag_demo_output.json
│── documents/
│     ├── doc1.txt
│     ├── doc2.txt
│     ├── doc3.txt
│     ├── doc4.txt
│     └── doc5.txt
```

---

Pipeline 1 – Structured Information Extraction

Objective

Convert unstructured customer support messages into structured JSON using Gemini.

Each message is processed to extract:

- Category
- Urgency
- Sentiment
- One-line Summary

---

Prompt Design

The prompt includes:

- Clear instruction
- Defined AI role (Customer Support Analyst)
- Explicit JSON output format
- Allowed values for enum fields

---

Schema Validation

Validation is performed using Pydantic.

Fields

| Field | Type |
|--------|------|
| category | Enum |
| urgency | Enum |
| sentiment | String |
| summary | String |

Allowed Categories

- Delivery
- Payment
- Refund
- Exchange
- Complaint
- Praise
- Cancellation
- Other

Allowed Urgency

- Low
- Medium
- High

---

Validation Failure Demonstration

The assignment requires at least one malformed output.

The following fixture was intentionally created:

```python
bad_output = {
    "category": "delivery",
    "urgency": "Very High",
    "sentiment": "Negative",
    "summary": "Package delayed",
    "extra_field": "Invalid"
}
```

Detected Problems

- Invalid category casing
- Invalid urgency value
- Unexpected extra field

Pydantic correctly rejected the response.

Handling Strategy

The pipeline automatically:

- Converts category to proper casing
- Maps "Very High" → "High"
- Removes unexpected fields
- Validates again

The corrected output successfully passes schema validation.

---

Structured Extraction Output

Processed:

- 15 customer support messages

Generated:

- validated_results.csv
- validated_results.json

---

Pipeline 2 – Retrieval-Augmented Generation (RAG)

Objective

Answer user questions using retrieved document chunks instead of relying only on model memory.

---

Documents

The corpus contains:

- 5 documents
- More than 2,000 words

Topics include:

- Refund Policy
- Shipping Policy
- Payment Policy
- Return Policy
- Customer Support Policy

---

RAG Pipeline

The pipeline contains separate implementation steps:

1. Document Chunking

Documents are divided into small text chunks.

2. Embedding Generation

Embeddings are generated using

all-MiniLM-L6-v2

---

3. Vector Store

Embeddings are stored in

FAISS

```

4. Similarity Search

Top-k most relevant chunks are retrieved.

5. Grounded Answer Generation

Retrieved chunks are supplied to Gemini Flash.

The model is instructed to answer only from retrieved context.

---

Example Queries

The project demonstrates five example questions.

Example:

Query

```
How many days does a refund take?
```

Retrieved chunk:

```
Refunds are processed within seven business days after warehouse inspection.
```

Generated answer:

```
Refunds are processed within seven business days after inspection.
```

Similar demonstrations are included for all five queries.

---

Results

Generated files:

- validated_results.csv
- validated_results.json
- rag_demo_output.csv
- rag_demo_output.json

---

API key

The API key should never be committed.

 Google Colab:


GEMINI_API_KEY = "API_KEY"


---

Installation

Install dependencies:

```bash

pip install -r requirements.txt

```

---

Run

Open

```
final-llm.ipynb

```

Run all cells sequentially.

The notebook will:

- Extract structured information
- Validate responses
- Demonstrate validation failure
- Build embeddings
- Create FAISS index
- Execute RAG queries
- Save all outputs

---

Conclusion

This project demonstrates:

- Prompt Engineering
- Structured Data Extraction
- Pydantic Validation
- Error Handling
- Semantic Embeddings
- Vector Search
- Retrieval-Augmented Generation
- Grounded LLM Responses

Author

Sneha Khadke

LLM-Powered Structured Insight & Retrieval Feature