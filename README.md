# Voice-Enabled Multimodal Enterprise Knowledge Assistant with Retrieval-Augmented Generation and Safety Guardrails

### Project Description

I developed a **Voice-Enabled Multimodal Retrieval-Augmented Generation (RAG) Assistant** that enables users to interact with enterprise knowledge through natural voice conversations while maintaining strong safety and reliability guarantees.

The system allows users to upload **PDF documents and images**, automatically extracts textual knowledge using document parsing and OCR, converts the extracted information into dense semantic embeddings using NVIDIA embedding models, and indexes them in a FAISS vector database for efficient semantic retrieval.

When a user submits a voice or text query, the system performs semantic retrieval over the indexed knowledge base, reranks the retrieved evidence, dynamically constructs a context window, and generates grounded responses using a large language model. Rather than relying solely on the model's internal knowledge, responses are generated from retrieved enterprise documents, significantly reducing hallucinations.

Unlike conventional chatbot implementations, this project treats **AI safety as a core architectural component** rather than an optional feature.

---

# AI Safety Guardrails (Core Focus)

The system implements a **multi-layer safety pipeline** that operates throughout the inference process.

## 1. Input Safety Validation

Before retrieval begins, every user query is analyzed by a safety classifier to identify potentially harmful, malicious, or policy-violating requests.

Examples include:

* Prompt injection attempts
* Harmful instructions
* Jailbreak attempts
* Toxic or abusive prompts
* Requests for unsafe information

Unsafe requests can be rejected, sanitized, or routed to a safe fallback response before reaching the language model.

---

## 2. Retrieval Grounding

Instead of allowing the LLM to freely generate answers, the system retrieves only relevant knowledge from trusted uploaded documents.

This constrains the generation process to verified information and reduces hallucination.

Pipeline:

```
User Query
      ↓
Semantic Retrieval
      ↓
Relevant Evidence
      ↓
Grounded LLM Generation
```

Grounding is itself an important AI safety mechanism because it minimizes unsupported claims.

---

## 3. Reranking for Evidence Quality

After FAISS retrieves candidate document chunks, a semantic reranker selects the most relevant evidence before generation.

This reduces:

* irrelevant context
* noisy retrieval
* misleading evidence

and improves factual accuracy.

---

## 4. On-Demand Context Construction

Instead of sending the entire document to the LLM, the system dynamically constructs a context window using only the highest-confidence retrieved evidence while respecting token limits.

Benefits include:

* lower latency
* reduced irrelevant information
* more interpretable retrieval
* decreased hallucination risk

---

## 5. Output Safety Filtering

After generation, the response is passed through a second safety validation stage.

The generated response is checked for:

* unsafe recommendations
* harmful content
* policy violations
* toxic language
* confidential information leakage

Unsafe responses can be blocked or replaced with a safe explanation before reaching the user.

---

## 6. Transparent Retrieval

The assistant is designed to answer using retrieved evidence rather than hidden model knowledge.

This improves:

* explainability
* traceability
* user trust
* enterprise compliance

---

# System Pipeline

```
Voice / Text Query
          │
          ▼
Speech Recognition
          │
          ▼
Input Safety Validation
          │
          ▼
Embedding Generation
          │
          ▼
FAISS Semantic Retrieval
          │
          ▼
Neural Reranker
          │
          ▼
On-Demand Context Builder
          │
          ▼
Large Language Model
          │
          ▼
Output Safety Validation
          │
          ▼
Safe Enterprise Response
```

---

# Technologies Used

### Frontend

* React
* TypeScript
* Vite
* Tailwind CSS
* Web Speech API
* Lucide Icons

### Backend

* FastAPI
* Python
* FAISS
* PyPDF
* Tesseract OCR
* NumPy

### AI Models

* NVIDIA Embedding Model
* NVIDIA-hosted Instruction-Tuned LLM
* Neural Reranker
* Safety / Moderation Model

---

# AI Safety Contributions

This project explores several practical AI safety principles:

* Retrieval grounding to reduce hallucinations
* Defense against prompt injection through input validation
* Multi-stage safety filtering
* Evidence-based response generation
* Transparent retrieval pipeline
* Reduced over-reliance on parametric model knowledge
* Modular guardrail architecture that can be extended with stronger safety models

---

# Why this aligns with AI Safety

Rather than treating safety as content moderation added after deployment, the system incorporates safety mechanisms throughout the inference pipeline—from input validation and retrieval quality control to grounded generation and post-generation safety checks. The objective is not only to improve answer quality but also to increase reliability, robustness, and user trust in enterprise AI systems.

---

> **Developed a voice-enabled multimodal enterprise RAG assistant that integrates semantic retrieval, reranking, grounded generation, and multi-layer AI safety guardrails—including input moderation, retrieval grounding, prompt-injection resistance, and output safety validation—to produce reliable, evidence-based responses from user-provided knowledge sources.**
