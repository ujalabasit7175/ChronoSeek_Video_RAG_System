# ChronoSeek: Temporal-Aware Video RAG System

ChronoSeek is an intelligent **Temporal-Aware Retrieval-Augmented Generation (RAG)** system designed for video intelligence. It converts YouTube videos into a searchable knowledge base by extracting transcripts, creating timestamp-aware semantic chunks, and using LLM-powered retrieval to answer questions with accurate timestamp references.

Instead of treating a video transcript as plain text, ChronoSeek preserves the temporal structure of the video, allowing the system to provide answers linked to the exact moment where information was discussed.

---

## 🚀 Features

### ⏱️ Temporal-Aware Retrieval
- Preserves exact timestamps during transcript processing.
- Provides source citations with responses (e.g., `[142.50s]`).
- Maintains the connection between retrieved information and its original position in the video.

### ⚡ Fast LLM Inference
- Powered by **Groq LPU inference** for extremely fast response generation.
- Optimized for interactive video question-answering.

### 📺 Automatic YouTube Ingestion
- Accepts any YouTube video URL.
- Automatically extracts transcripts using `youtube-transcript-api`.
- Converts video content into a searchable vector database.

### 🔎 Semantic Search with Local Vector Database
- Uses HuggingFace embeddings:
  - `sentence-transformers/all-MiniLM-L6-v2`
- Stores embeddings locally using:
  - `FAISS (Facebook AI Similarity Search)`
- Enables fast and lightweight semantic retrieval.

### 🛡️ Anti-Hallucination Guardrails
- Uses strict prompting to ensure answers are generated only from retrieved video context.
- Forces the model to provide timestamps.
- Instructs the model to acknowledge when information is unavailable.

---

# 🧠 Architecture Decision: Why Groq Instead of Gemini?

For ChronoSeek, **Groq (`llama-3.3-70b-versatile`)** was selected as the inference engine because the current system focuses on **transcript-based video understanding rather than visual video analysis**.

### 1. High-Speed Text Generation
Groq uses custom **Language Processing Units (LPUs)** optimized for fast sequential token generation. This makes it highly suitable for RAG applications where quick retrieval and response generation are important.

### 2. Suitable for Transcript-Based Video RAG
Gemini provides powerful multimodal capabilities and can analyze video frames directly. However, ChronoSeek currently works on extracted transcripts, meaning the additional multimodal processing is unnecessary for this workflow.

### 3. Efficient Architecture
Using Groq keeps the pipeline lightweight while providing:
- Fast inference
- Accurate responses
- Low latency conversational interaction

The model choice was based on matching the inference engine with the system requirements.

---

# 🏗️ System Architecture

```
YouTube URL
      |
      ↓
Transcript Extraction
(youtube-transcript-api)
      |
      ↓
Timestamp-Aware Semantic Chunking
      |
      ↓
Text Embedding Generation
(all-MiniLM-L6-v2)
      |
      ↓
FAISS Vector Database
      |
      ↓
Semantic Retrieval
      |
      ↓
Groq LLM
      |
      ↓
Timestamped Answer Generation
```

---

# 🛠️ Tech Stack

### Transcript Processing
- `youtube-transcript-api`

### Embeddings
- `sentence-transformers`
- `all-MiniLM-L6-v2`

### Vector Database
- `FAISS`

### LLM Inference
- `Groq`
- `llama-3.3-70b-versatile`

### Framework
- `LangChain`

---

# 📦 Installation

### 1. Clone Repository

```bash
git clone https://github.com/ujalabasit7175/ChronoSeek_Video_RAG_System.git

cd ChronoSeek_Video_RAG_System
```

### 2. Install Dependencies

```bash
pip install -q --upgrade \
youtube-transcript-api \
langchain \
langchain-community \
langchain-huggingface \
langchain-groq \
sentence-transformers \
faiss-cpu
```

### 3. Configure Groq API Key

Set your Groq API key:

```bash
export GROQ_API_KEY="your_api_key_here"
```

---

# 💻 Usage

Run the Python script or execute the notebook cells sequentially.

1. Enter a YouTube video URL.
2. The system extracts the transcript.
3. Transcript segments are converted into timestamp-aware chunks.
4. FAISS builds a searchable vector database.
5. Ask questions about the video content.

Example:

```
Enter a YouTube URL:
https://www.youtube.com/watch?v=example

Ask a question:
What is machine learning?
```

Response:

```
Machine learning is described as a method where computers learn patterns from data.
[Timestamp: 45.20s]
```

---

# 🧩 Temporal Chunking Strategy

Traditional RAG pipelines often lose important metadata during document splitting. ChronoSeek introduces a custom timestamp-aware chunking strategy.

The `create_timestamped_chunks()` function:

- Groups smaller transcript segments into meaningful semantic chunks.
- Maintains the original video start time.
- Stores timestamps as metadata inside LangChain Documents.
- Creates searchable chunks while preserving chronological information.

Example chunk metadata:

```python
{
    "start_time": 142.50,
    "content": "Explanation of neural networks..."
}
```

This allows the retrieval system to provide not only **what** was discussed, but also **when** it happened in the video.

---
# 👩‍💻 Author

**Ujala Basit**  
Computer Science Undergraduate | AI & Computer Vision Enthusiast
