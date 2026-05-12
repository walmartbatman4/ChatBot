# 📚 PDF RAG Chatbot using LangChain, ChromaDB, and Groq

## 🚀 Overview

This project implements a complete **Retrieval-Augmented Generation (RAG)** pipeline for interacting with PDF documents using Large Language Models (LLMs).

The system:

* Ingests PDF documents
* Extracts and preprocesses text
* Splits text into chunks
* Generates embeddings using Sentence Transformers
* Stores embeddings in a Chroma vector database
* Retrieves relevant chunks based on user queries
* Generates contextual responses using Groq-hosted LLMs

The chatbot supports conversational memory, enabling context-aware interactions across multiple queries.

---

# 🏗️ System Architecture

```text
                ┌──────────────────┐
                │   PDF Documents  │
                └────────┬─────────┘
                         │
                         ▼
              ┌─────────────────────┐
              │ PDF Text Extraction │
              │     (PyPDF)         │
              └────────┬────────────┘
                       │
                       ▼
          ┌──────────────────────────┐
          │ Text Chunking & Splitting│
          │ RecursiveCharacterSplitter│
          └────────┬─────────────────┘
                   │
                   ▼
          ┌──────────────────────────┐
          │ Embedding Generation     │
          │ all-MiniLM-L6-v2         │
          └────────┬─────────────────┘
                   │
                   ▼
          ┌──────────────────────────┐
          │ Chroma Vector Database   │
          └────────┬─────────────────┘
                   │
                   ▼
          ┌──────────────────────────┐
          │ Similarity Search        │
          │ Document Retrieval       │
          └────────┬─────────────────┘
                   │
                   ▼
          ┌──────────────────────────┐
          │ Groq LLM Response        │
          │ Conversational QA Chain  │
          └──────────────────────────┘
```

---

# ⚙️ Workflow Explanation

## 1️⃣ Document Ingestion

PDF documents stored inside the `PDFs/` directory are read using the `PyPDF` library.

### Key Features

* Reads multiple PDFs automatically
* Extracts text page-by-page
* Handles missing text safely

### Implementation

```python
reader = PdfReader(file_path)
```

---

## 2️⃣ Text Chunking and Preprocessing

The extracted text is split into smaller chunks using `RecursiveCharacterTextSplitter`.

### Why Chunking?

LLMs cannot process extremely large documents directly. Chunking:

* Reduces memory usage
* Improves retrieval quality
* Preserves semantic meaning

### Parameters Used

```python
chunk_size = 1000
chunk_overlap = 200
```

### Benefits of Overlap

Chunk overlap preserves context between adjacent text sections.

---

## 3️⃣ Embedding Generation

Each text chunk is converted into a high-dimensional vector embedding using:

```text
sentence-transformers/all-MiniLM-L6-v2
```

### Embedding Model

* Lightweight and fast
* Produces semantically meaningful embeddings
* Suitable for retrieval tasks

### Implementation

```python
embeddings = HuggingFaceEmbeddings(
    model_name="sentence-transformers/all-MiniLM-L6-v2"
)
```

---

## 4️⃣ Vector Database Creation

The embeddings are stored in a persistent Chroma vector database.

### Why Chroma?

* Fast similarity search
* Lightweight local database
* Persistent storage support
* Easy LangChain integration

### Implementation

```python
vector_db = Chroma.from_documents(
    docs,
    embeddings,
    persist_directory="./chroma_db"
)
```

---

## 5️⃣ Retrieval-Augmented Generation (RAG)

When a user asks a question:

1. The query is embedded
2. Similar chunks are retrieved from ChromaDB
3. Retrieved context is passed to the LLM
4. The LLM generates a contextual response

This improves factual accuracy and reduces hallucinations.

---

## 6️⃣ Conversational Memory

The chatbot uses:

```python
ConversationBufferMemory
```

This stores previous interactions and enables:

* Context-aware conversations
* Follow-up questions
* Multi-turn dialogue

---

## 7️⃣ Response Generation using Groq LLM

The project uses Groq-hosted LLMs through `ChatGroq`.

### Recommended Models

* `llama-3.3-70b-versatile`
* `mixtral-8x7b-32768`

### Implementation

```python
llm = ChatGroq(
    model_name="llama-3.3-70b-versatile",
    temperature=0
)
```

---

# 📂 Project Structure

```text
RAG_Chatbot/
│
├── PDFs/                     # Input PDF documents
├── chroma_db/                # Persistent Chroma vector database
├── key.txt                   # Groq API key
├── main.ipynb                # Main notebook implementation
├── requirements.txt          # Project dependencies
└── README.md
```

---

# 🛠️ Technologies Used

| Component       | Technology                        |
| --------------- | --------------------------------- |
| Language        | Python                            |
| Framework       | LangChain                         |
| PDF Processing  | PyPDF                             |
| Embeddings      | HuggingFace Sentence Transformers |
| Vector Database | ChromaDB                          |
| LLM Provider    | Groq                              |
| Model           | Llama 3.3 70B                     |

---

# 📦 Installation

## 1️⃣ Clone Repository

```bash
git clone <repository-url>
cd RAG_Chatbot
```

---

## 2️⃣ Create Virtual Environment

### Windows

```bash
python -m venv env
env\Scripts\activate
```

### Linux / Mac

```bash
python3 -m venv env
source env/bin/activate
```

---

## 3️⃣ Install Dependencies

```bash
pip install -r requirements.txt
```

---

# 🔑 API Key Setup

Create a file named:

```text
key.txt
```

Add your Groq API key inside it.

Example:

```text
gsk_xxxxxxxxxxxxxxxxx
```

---

# ▶️ Running the Project

## Start the Notebook

```bash
jupyter notebook main.ipynb
```

OR execute directly in Python if converted to a script.

---

# 💬 Example Usage

```text
You: What is Statistical mechanics?
Groq AI: Statistical mechanics is a branch of physics...
```

---

# ✅ Core Requirements Implemented

| Requirement                     | Status |
| ------------------------------- | ------ |
| Document ingestion pipeline     | ✅      |
| Text chunking and preprocessing | ✅      |
| Embedding generation            | ✅      |
| Vector database implementation  | ✅      |
| Query interface (CLI)           | ✅      |
| Response generation using LLM   | ✅      |
| Implemented conversational memory |✅      |

---

# 🔮 Future Improvements

* Streamlit or Gradio UI
* Support for DOCX and TXT files
* Hybrid search (BM25 + Vector Search)
* Citation-aware responses
* GPU acceleration
* Multi-user deployment

---

# 👨‍💻 Author

Alfien Jessurun

---

# ⭐ Acknowledgements

* LangChain
* HuggingFace
* ChromaDB
* Groq
* Sentence Transformers
