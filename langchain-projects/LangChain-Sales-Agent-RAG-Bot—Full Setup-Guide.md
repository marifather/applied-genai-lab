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

## 💬 Ask Questions from Documentation with added memory support

## The memory portion in the code
```
# Memory setup
store: Dict[str, BaseChatMessageHistory] = {}

def get_session_history(session_id: str) -> BaseChatMessageHistory:
    if session_id not in store:
        store[session_id] = ChatMessageHistory()
    return store[session_id]

# Final memory-enhanced chain
memory_chain = RunnableWithMessageHistory(
    contextualize_question.assign(
        context=RunnableLambda(lambda x: retriever.invoke(
            x["question"].content if hasattr(x["question"], "content") else x["question"]
        ))
    ) | rag_chain,
    get_session_history,
    input_messages_key="question",
    history_messages_key="chat_history"
)
```

```bash
python ask.py
```

Sample interactions:

```
🔍 Ask your question (or type 'exit'): what is kensium pos
🧠 Answer: Kensium POS is a point-of-sale system designed to integrate seamlessly with Acumatica, a cloud-based ERP system. It is developed by Kensium to provide a comprehensive solution for retail businesses, enabling them to manage sales transactions, inventory, customer data, and more, all within a unified platform. Kensium POS is designed to enhance the efficiency and accuracy of retail operations by offering features such as real-time inventory updates, customer relationship management, and detailed sales reporting. Its integration with Acumatica ensures that data flows smoothly between the POS system and the ERP, providing businesses with a holistic view of their operations.
🔍 Ask your question (or type 'exit'): for what reason the customers can use this
🧠 Answer: Customers can use Kensium POS for several reasons, particularly if they are looking to streamline and enhance their retail operations. Here are some key reasons why businesses might choose Kensium POS:

1. **Seamless Integration with Acumatica**: Kensium POS is designed to integrate seamlessly with Acumatica ERP, allowing for real-time data synchronization. This integration ensures that sales, inventory, and customer information are consistently updated across systems, reducing errors and improving efficiency.
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
