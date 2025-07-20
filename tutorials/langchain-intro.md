# Introduction to Langchain

Langchain is a powerful open-source framework that helps developers build applications powered by large language models (LLMs). It abstracts away the complexity of chaining prompts, memory, tools, and data sources so you can focus on building real-world AI systems like chatbots, RAG pipelines, and agents.

---

## 🔧 Why Langchain?

Langchain makes it easy to:
- ✅ Connect to LLMs like OpenAI, Anthropic, Cohere, HuggingFace, etc.
- ✅ Add memory to conversations
- ✅ Chain multiple steps (e.g., user question → search → summarize → respond)
- ✅ Integrate external tools and APIs
- ✅ Build Retrieval-Augmented Generation (RAG) systems

---

## 🧱 Core Concepts

| Concept            | Description |
|--------------------|-------------|
| `PromptTemplate`   | A dynamic prompt with variables like `{product}` |
| `LLMChain`         | A basic unit combining a prompt + LLM call |
| `Tools`            | External functions the agent can call |
| `Agents`           | Decide what actions to take based on input |
| `Memory`           | Helps remember past interactions in a conversation |
| `Vector Stores`    | Stores embedded documents for similarity search |
| `Document Loaders` | Load PDFs, websites, WordPress, Notion, etc. |

---

## 🔑 Prerequisites

To run Langchain with OpenAI models like `gpt-3.5-turbo`, you need:

### 1. A Python environment
Install Langchain and OpenAI libraries:

```bash
pip install langchain openai

```

### 2. An OpenAI API Key
How to Get Your API Key:
  1. Go to https://platform.openai.com/
  2. Sign in or create an account
  3. Set up billing: Billing Dashboard
  4. Visit API Keys and click “Create new secret key”
  5. Copy the key securely (you won't see it again)

**How to Use It:**
Option 1: (Recommended - use environment variable)

```bash
export OPENAI_API_KEY="your-key-here"
```

Then in your Python script:
```bash
from dotenv import load_dotenv
load_dotenv()
```
# ✅ Explanation:
These lines load the environment variables from your .env file into your Python script.

from dotenv import load_dotenv: Imports the function that handles .env loading.

load_dotenv(): Reads the .env file (located in the same folder) and makes variables like OPENAI_API_KEY available via os.environ.

So later, you can safely access the key using:

```bash
import os
api_key = os.environ.get("OPENAI_API_KEY")
```
🧠 This keeps your code clean and secure — your sensitive keys are not hard-coded or exposed in your scripts.

💰 Check pricing for open api key: https://openai.com/pricing

# 🚀 First Example: Simple LLMChain

from langchain.llms import OpenAI
from langchain.prompts import PromptTemplate
from langchain.chains import LLMChain

### 1. Choose the language model you want to use
llm = OpenAI(model_name="gpt-3.5-turbo")

### 2. Create a reusable prompt template with a placeholder variable
prompt = PromptTemplate(
    input_variables=["product"],
    template="What is a good name for a company that makes {product}?"
)

### 3. Chain the prompt and LLM together
chain = LLMChain(llm=llm, prompt=prompt)

### 4. Run the chain by passing a value for 'product'
print(chain.run("solar panels"))

🧠 What’s Happening?
Line	Explanation
llm = OpenAI(...)	Initializes the LLM wrapper using GPT-3.5
PromptTemplate(...)	Defines a prompt that expects an input like {product}
LLMChain(...)	Combines your prompt and LLM into one callable unit
chain.run("solar panels")	Sends the filled prompt to the model and prints the result

**🧪 Example Output:**

“EcoSpark” or “SolarNova” — creative brand names from GPT!

# 📚 What’s Next?
**In the upcoming tutorials, we’ll explore:**
  1. Building a chatbot with Langchain + Pinecone
  2. RAG pipeline: fetch → clean → embed → query → respond
  3. Connecting Langchain to n8n or your website
