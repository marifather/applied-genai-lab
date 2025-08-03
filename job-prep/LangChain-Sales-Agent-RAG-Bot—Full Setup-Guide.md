# 🧠 LangChain Sales Agent RAG Bot — Full Setup Guide

This guide walks you through creating a LangChain-powered Sales Agent chatbot that uses OpenAI + Pinecone to answer product-related questions from your company documentation (scraped from WordPress). Even beginners can follow along!

---

## 📁 Project Overview

- **Bot Name**: Sales Agent RAG Bot  
- **Architecture**: RAG (Retrieval Augmented Generation)  
- **Stack**: Python + LangChain + OpenAI + Pinecone  
- **Use Case**: Answer sales-related queries from product documentation

---

## 🧪 Step 1: Setup Virtual Environment

```bash
# Navigate to your project directory
cd ~/langchain/sales_agent_bot

# Create virtual environment
python -m venv ragbot-env

# Activate environment (Linux/macOS)
source ragbot-env/bin/activate

# If you're on Windows (PowerShell):
# .\ragbot-env\Scripts\activate
```

## 📦 Step 2: Install Dependencies

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
## 🔐 Step 3: Setup .env File
Create a .env file in your root folder:

```
OPENAI_API_KEY=your_openai_key_here
PINECONE_API_KEY=your_pinecone_key_here
PINECONE_INDEX=langchain-rag-sales-agent-index
PINECONE_ENVIRONMENT=us-east-1
```
## 🕷️ Step 4: Crawl Product Documentation (optional)
Use the provided crawler_selenium.py script to crawl and save documentation from https://productdocs.kensium.com.

```
python crawler_selenium.py
```
This will generate a file: productdocs_full.json

## 🔢 Step 5: Embed Documents into Pinecone
Run the following script to embed and upload content:

```
python embed_to_pinecone.py
```
You should see output like:

```
🔢 Embedding 12039 chunks...
✅ Upload complete to Pinecone index: langchain-rag-sales-agent-index | namespace: productdocs-v1
```
You can also verify the record count on the Pinecone dashboard.

## 🤖 Step 6: Ask Questions Using ask.py
This script loads data from Pinecone and allows you to query the documents using OpenAI.

```
python ask.py
```

Example queries:
```
🔍 Ask your question (or type 'exit'): What is Kensium WMS?
🔍 Ask your question (or type 'exit'): How does your Adobe connector work?
```

## 🧠 Enhancements
✅ Use gpt-4o instead of gpt-3.5-turbo in ask.py:

```
llm = ChatOpenAI(api_key=OPENAI_API_KEY, model="gpt-4o")
```
✅ Improve system prompt:

- Add branding

- Add tone guidelines

- Add source attribution

✅ Display document sources (URLs) with answers for trust and traceability

## 🧼 Tips
- Restart your virtual environment if you change .env

- Clear Pinecone namespace if re-uploading embeddings

- Keep chunk size between 400–600 characters for best quality

- You can split documents with overlap to preserve meaning

## ✅ You’re Done!
You’ve built a fully functional RAG bot powered by LangChain + OpenAI + Pinecone.

Need help?
📬 Contact: mariappanp@kensium.com

