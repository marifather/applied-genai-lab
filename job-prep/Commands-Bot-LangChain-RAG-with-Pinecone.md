---
title: "Commands Bot - LangChain RAG with Pinecone"
description: "A Retrieval-Augmented Generation (RAG) chatbot to answer Linux and DevOps commands using LangChain, OpenAI, and Pinecone."
created: 2025-07-28
---

# 🤖 Commands Bot (LangChain RAG)

This project demonstrates how to build a **RAG-based AI Assistant** that answers questions related to Linux and DevOps commands using your own `.txt` file as the knowledge base. It's built using:

- 🧠 **LangChain**
- 📦 **OpenAI** for embeddings and responses
- 📘 **Pinecone** for vector storage
- 🐍 Python and `.env` for secure API handling

---

## 🗂 Folder Structure

```
commands_bot/
├── commands.txt           # Your knowledge base
├── ingest.py              # Script to load and embed documents into Pinecone
├── ask.py                 # Query interface to get answers
├── .env                   # API credentials (kept secret)
```

---

## 🔧 Setup Instructions

### 1. Create a Virtual Environment

```bash
conda create -n myproject python=3.11 -y
conda activate myproject
```

### 2. Install Required Packages

```bash
pip install langchain openai pinecone-client tiktoken python-dotenv
```

### 3. Prepare the `.env` File

Create a file named `.env` in your project directory:

```env
OPENAI_API_KEY=sk-...              # Keep this secret
PINECONE_API_KEY=...               # Keep this secret
PINECONE_ENVIRONMENT=us-east-1
PINECONE_INDEX=langchain-rag-commands-search-index
```

---

## 📥 Ingesting Your Data

The `ingest.py` script:
- Loads the `commands.txt` file
- Splits text into chunks
- Embeds using OpenAI
- Stores vectors in Pinecone

Run:

```bash
python ingest.py
```

✅ You should see: `Embedding and upload complete.`

---

## 💬 Asking Questions

The `ask.py` script:
- Loads the Pinecone vector store
- Accepts user input
- Retrieves relevant chunks
- Sends prompt to OpenAI and prints the response

Run:

```bash
python ask.py
```

Sample Output:

```
🔍 Ask your question (or type 'exit'): what is the fastly vcl / fiddle site

🧠 Answer: The Fastly Fiddle is a tool provided by Fastly that allows users to generate and test VCL (Varnish Configuration Language) configurations in a live environment...
```

---

## 🔒 Notes

- Keep your `.env` secrets **out of GitHub** by using `.gitignore`.
- This uses `OpenAIEmbeddings` + `ChatOpenAI`. You can plug in other models too.

---

## ✅ Next Steps

- Add Streamlit/Gradio UI
- Log responses and sources
- Add feedback buttons
- Deploy with Docker or FastAPI

---

## 🧠 Inspired By

- LangChain Documentation: https://docs.langchain.com/
- Pinecone Docs: https://docs.pinecone.io/

---

_Contributed by [@marifather](https://github.com/marifather) | Part of the [applied-genai-lab](https://github.com/marifather/applied-genai-lab)_
