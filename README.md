# 📄 RAG Study Assistant

A **document-centric Retrieval-Augmented Generation (RAG) application** built with **Streamlit, FAISS, and Ollama** that lets you chat with your documents while keeping answers **strictly grounded in the uploaded content**.

This project evolved through hands-on engineering decisions: managing document ingestion, chunking strategies, vector database lifecycle, chat persistence, and UI state. The result is a **production-style learning system** that is robust, extensible, and suitable for demos or a portfolio.

---

## 🎯 What This Project Solves

Traditional chatbots answer from general knowledge and can hallucinate. This app instead:

- Retrieves **only the most relevant parts of your document**
- Uses those parts as **context for the LLM**
- Produces answers that are **document-scoped and reproducible**
- Keeps **separate chat sessions per document**, so contexts never mix

---

## 🧠 What is RAG (Retrieval-Augmented Generation)?

RAG combines **search** and **generation**:

1. Documents are split into chunks
2. Each chunk is converted into an embedding
3. Embeddings are stored in a vector database (FAISS)
4. For every user query:
   - Relevant chunks are retrieved via semantic similarity
   - Retrieved text is passed to the LLM as context
5. The LLM answers **using only that context**

This approach:
- Reduces hallucinations
- Works with private/local data
- Avoids fine-tuning
- Scales better than prompt-stuffing

---

## ✨ Key Features

- 📁 Upload documents (`PDF`, `DOCX`, `PPTX`, `TXT`)
- 💬 Chat-style UI (ChatGPT-like experience)
- 🧠 True Retrieval-Augmented Generation
- 🔍 FAISS-based semantic search
- 📚 Persistent chat history per document
- 🗂️ One chat per document (no cross-contamination)
- 🧭 Multi-page Streamlit application
- 🖥️ Fully local inference using Ollama (no API keys)
- ♻️ Controlled vector database lifecycle to avoid disk/RAM bloat

---

## 🏗️ System Architecture

```

User
│
▼
Streamlit UI (Multi-page)
│
├── New Chat / Upload Page
├── Chats List Page
├── Chat Page (per document)
│
▼
FAISS Vector Store (per document)
│
▼
Ollama Local LLM

```

### Design Principles

- **One document = one chat**
- **One document = one FAISS index**
- **Vector DBs are treated as cache, not source code**
- **Chat history is persistent and reloadable**
- **UI state is cleanly separated from data storage**

---

## 🗂️ Project Structure

```

rag-project/
│
├── loaders/              # Document loaders (PDF, DOCX, PPTX, TXT)
├── chunking/             # Text splitting & chunking logic
├── embeddings/           # Embedding model wrapper
├── vector_store/         # FAISS index creation & loading
├── llm/                  # Ollama LLM wrapper
│
├── pages/                # Streamlit multipage UI
│   ├── 1_Chats.py        # List & reopen previous chats
│   └── 2_Chat.py         # Chat interface
│
├── chat_store.json       # Persistent chat metadata
├── chat_utils.py         # Chat persistence utilities
├── ui.py                 # Main entry point (new chat / upload)
├── requirements.txt
├── .gitignore
└── README.md

````

---

## ⚙️ Installation & Setup

This application is designed to run **locally**.

It relies on:
- **Ollama** for local LLM inference
- **FAISS** for vector search
- Native document processing libraries

Because of these dependencies, it is **not suitable for direct deployment** on platforms like:
- Streamlit Cloud
- Vercel
- Netlify

### 1️⃣ Clone the Repository
```bash
git clone https://github.com/yashwanth-reddyy/rag-study-assistant.git
cd RAG-study-assistant
````

---

### 2️⃣ Create a Virtual Environment

```bash
python -m venv .venv
```

Activate it:

**Windows**

```bash
.venv\Scripts\activate
```

**Linux / macOS**

```bash
source .venv/bin/activate
```

---

### 3️⃣ Install Dependencies

```bash
pip install -r requirements.txt
```

The dependency list is intentionally **defensive**, covering all loaders and edge cases encountered during development.

---

## 🧠 Ollama Setup (Required)

This project uses **local LLM inference** via Ollama.

### Install Ollama

👉 [https://ollama.com](https://ollama.com)

### Pull Required Models

```bash
ollama pull phi3:mini
ollama pull nomic-embed-text
```

> ⚠️ For low-RAM systems, consider smaller models such as `phi` or `mistral`, since my system is low ram, the default is set to phi3:mini 
---

## 🚀 Running the Application

```bash
streamlit run ui.py
```

Open your browser at:

```
http://localhost:8501
```

---

## 🧪 How the Application Works (Step-by-Step)

1. User uploads a document
2. Document is parsed and cleaned
3. Text is split into overlapping, context-aware chunks
4. Chunks are embedded and stored in FAISS
5. User asks a question
6. FAISS retrieves the most relevant chunks
7. Retrieved context is sent to the LLM
8. The LLM generates a grounded response

The model **never answers without retrieved document context**.

---

## 🗂️ Chat System Design

* Each document has:

  * its own FAISS index
  * its own chat history
* Chat metadata is stored in `chat_store.json`
* Chats persist across:

  * page reloads
  * browser refreshes
  * application restarts
* Old chats can be reopened and continued seamlessly

---

## 🧹 Repository Hygiene

The following are **intentionally not committed**:

* `.venv/`
* FAISS index data
* Uploaded documents
* OS cache files

These are runtime artifacts and should not be version-controlled.

---

## 🛠️ Tech Stack

* **Python**
* **Streamlit**
* **LangChain**
* **FAISS**
* **Ollama**
* **Unstructured**
* **PyPDF**
* **python-docx**
* **python-pptx**

---
## 👤 Author

**Yashwanth Reddy**

## 📜 License

MIT License. See the [LICENSE](LICENSE) file for more information.
