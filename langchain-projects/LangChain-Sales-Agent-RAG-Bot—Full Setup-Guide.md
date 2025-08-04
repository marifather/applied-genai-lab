---
title: "Sales Agent Bot - LangChain RAG with Pinecone"
description: "A Retrieval-Augmented Generation (RAG) chatbot for sales documentation queries using LangChain, OpenAI, and Pinecone."
created: 2025-08-03
---

# 💼 Sales Agent RAG Bot (LangChain + Pinecone)

This project demonstrates how to build a **RAG-based Sales Assistant** that answers product and sales-related queries using your internal documentation (scraped from WordPress). It uses:

- 🧠 **LangChain**
- 🧾 **OpenAI** for embeddings and chat
- 🧱 **Pinecone** for semantic vector search
- 🐍 Python + `.env` for configuration

---

## 🗂 Folder Structure

```
sales_agent_bot/
├── productdocs_full.json   # Crawled documentation content
├── crawler_selenium.py     # Scraper for productdocs.kensium.com
├── embed_to_pinecone.py    # Embeds chunks into Pinecone
├── ask.py                  # Ask questions from the bot
├── .env                    # Secure credentials
```

---

## ⚙️ Setup Instructions

### 1. Create a Virtual Environment

```bash
python -m venv ragbot-env
source ragbot-env/bin/activate
```

### 2. Install Dependencies

```bash
pip install langchain==0.3.27 \
            langchain-community==0.3.27 \
            langchain-core==0.3.72 \
            langchain-openai==0.3.28 \
            langchain-text-splitters==0.3.9 \
            openai==1.98.0 \
            pinecone-client==3.0.0 \
            python-dotenv
```

### 3. Create Your `.env` File

```env
OPENAI_API_KEY=sk-...              # Your OpenAI Key
PINECONE_API_KEY=pcsk-...          # Your Pinecone Key
PINECONE_ENVIRONMENT=us-east-1
PINECONE_INDEX=langchain-rag-sales-agent-index
```

---

## 🕸️ Crawl Product Documentation

```bash
python crawler_selenium.py
```

This will create `productdocs_full.json` by crawling all valid internal links from `https://productdocs.kensium.com`.

---

## 🧠 Embed Documents to Pinecone

```bash
python embed_to_pinecone.py
```

You should see output like:

```
🔢 Embedding 12039 chunks...
✅ Upload complete to Pinecone index: langchain-rag-sales-agent-index | namespace: productdocs-v1
```

---

## 💬 Ask Questions from Documentation

```bash
python ask.py
```

Sample interactions:

```
🔍 Ask your question (or type 'exit'): What is Kensium WMS?

🧠 Answer: Kensium WMS is an advanced inventory management system...

🔍 Ask your question (or type 'exit'): What is Kensium Adobe connector?

🧠 Answer: The Kensium Adobe Connector helps integrate Adobe software...
```

---

## 🌟 Optional Enhancements

- ✅ Use `gpt-4o` for better reasoning:
  ```python
  llm = ChatOpenAI(api_key=OPENAI_API_KEY, model="gpt-4o")
  ```

- ✅ Improve system prompt tone and format
- ✅ Display source URLs from Pinecone metadata
- ✅ Add streaming + frontend (Streamlit, Gradio, Webflow)

---

## 🚫 Security Notes

- Do not commit `.env` files to Git
- Always use `.gitignore` for secrets

---

## 📌 Resources

- [LangChain Documentation](https://docs.langchain.com/)
- [Pinecone Docs](https://docs.pinecone.io/)
- [OpenAI Platform](https://platform.openai.com/)

---

_Contributed by [@marifather](https://github.com/marifather) | Part of the [applied-genai-lab](https://github.com/marifather/applied-genai-lab)_
