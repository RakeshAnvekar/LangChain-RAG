## Overview
LLMs (Large Language Models) store knowledge inside their **parameters (weights)**.

- These parameters are **numerical values**
- Knowledge learned during training is embedded in these numbers
- This is called **parametric knowledge**

👉 As the **number of parameters increases**, the model generally becomes:
- Better at understanding language
- Better at reasoning
- More capable of handling complex tasks

---

## How Users Access This Knowledge

Since the knowledge is stored internally, users **cannot directly query the parameters**.

So how do we access it?

### ✅ Through Prompting

1. User gives a **prompt** (question/input)
2. The prompt reaches the LLM
3. The LLM:
   - Understands the intent of the prompt
   - Converts words into **embeddings (numbers)**
4. The model then:
   - Uses its **parametric knowledge**
   - Predicts the most likely **next word**, token by token
5. This continues until a full answer is generated

📌 **In short:**

```
Prompt → Understanding → Parametric Knowledge → Token-by-token Answer
```

---

## Why This Workflow Fails in Many Real Scenarios

Although powerful, parametric knowledge has limitations.

### 1. ❌ Knowledge is Frozen
- The model only knows what it was trained on
- It **cannot know new or updated information**

Examples:
- New company policy
- Latest API change
- Today’s stock price

---

### 2. ❌ Hallucinations
- If the answer is **not clearly stored** in parameters
- The model may **guess**

This leads to:
- Incorrect facts
- Confident but wrong answers

---

### 3. ❌ No Access to Private or Enterprise Data

LLM parameters do **not include**:
- Internal documents
- PDFs
- Databases
- Company wikis

---

### 4. ❌ Limited Context Size
- The model can only consider a **limited amount of text** at a time
- Large documents cannot be fully “remembered” via prompting alone

---

### 5. ❌ Poor Domain-Specific Accuracy

Highly specialized domains such as:
- Legal
- Medical
- Internal business rules

Often require **exact wording**, which parametric knowledge may not have.

---

## What Comes Next? (Natural Next Step)

There are **two main approaches** to overcome these limitations:

### 1️⃣ Fine-Tuning
- Take a **pre-trained LLM**
- Retrain it with **small domain-specific data**
- The LLM gains **knowledge about your specific domain**

### Disadvantages

1. ❌ **Computationally Expensive**  
   - Requires significant compute resources (GPUs/TPUs)
   - Training and retraining increase infrastructure cost

2. ❌ **Requires Strong Technical Expertise**  
   - Needs deep understanding of:
     - Model architecture
     - Training pipelines
     - Hyperparameter tuning
   - Debugging and evaluation are complex

3. ❌ **Repeated Tuning for Latest Data**  
   - Model must be retrained whenever:
     - New data is added
     - Information changes
   - This makes it hard to keep the model always up to date

---
### 2️⃣ In-Context Learning
   - In-Context Learning is a **core capability of Large Language Models** such as **GPT-3/4, Claude, and LLaMA**, where the model learns to solve a task **purely by observing examples provided in the prompt**, **without updating its weights or parameters**.
   - **Note** In lmm this future became internal part.(GPT-3)
### Simple In-Context Learning Prompt Example (Sentiment Classification)

**Prompt:**

Classify the sentiment of the text as **Positive** or **Negative**.

Text: I love this phone  
Sentiment: Positive  

Text: This product is terrible  
Sentiment: Negative  

Text: The battery life is amazing  
Sentiment:

### 3️⃣ RAG (Retrieval-Augmented Generation)

This exact limitation is why **RAG** exists:

- Instead of relying only on parametric knowledge
- We **retrieve external knowledge**
- Inject it into the prompt
- Then let the LLM generate the answer
## Retrieval-Augmented Generation (RAG)

RAG was developed by combining **two core concepts**:

### 1️⃣ Information Retrieval
- Fetches **relevant information** from external sources
- Sources can include:
  - Documents
  - PDFs
  - Databases
  - Knowledge bases
- Ensures the model has **up-to-date and accurate data**

### 2️⃣ Text Generation
- Uses a **Large Language Model (LLM)**
- Generates a **natural language response**
- The response is based on:
  - Retrieved information
  - User query
  - Model’s parametric knowledge

📌 **In RAG, retrieval happens first, generation happens second.**

---

## Summary

- Parametric knowledge is powerful but limited
- Prompting works only when knowledge already exists in the model
- Fine-tuning and RAG help LLMs work with **new, private, and large-scale data**

## RAG Application Workflow

A RAG (Retrieval-Augmented Generation) application typically has **four main steps**:

1. Indexing  
2. Retrieval  
3. Augmentation  
4. Generation

---

## 1️⃣ Indexing

**Indexing** is the process of preparing your **external knowledge base** so that it can be **efficiently searched** later during user queries.

In RAG, we do not store knowledge inside the LLM.  
Instead, we create an **external knowledge base**, which is done during indexing.

### Why Indexing?
- To fetch relevant context for a user query
- To send that context along with the query to the LLM
- This external knowledge base is used at runtime

### Indexing Steps

#### 1. Document Ingestion
Load source knowledge into memory.

**Examples of sources:**
- PDFs
- Websites
- YouTube transcripts
- Git repositories

**Tools:**
- LangChain Loaders  
  - `PyPDFLoader`
  - `WebBaseLoader`
  - `YoutubeLoader`
  - `GitLoader`

---

#### 2. Text Chunking
Break large documents into **small, semantically meaningful chunks**.

**Why chunking is required:**
- LLMs have a **context limit** (e.g., 4K–32K tokens)
- Smaller chunks are:
  - More focused
  - Better for semantic search
  - Easier to retrieve accurately

**Tools:**
- `RecursiveCharacterTextSplitter`
- `SemanticChunker`

---

#### 3. Embedding Generation
Convert each text chunk into a **dense vector (embedding)** that represents its meaning.

- Embeddings capture semantic similarity
- Similar text → similar vectors

**Tools:**
- OpenAI Embeddings
- SentenceTransformer Embeddings

---

#### 4. Store in Vector Database
Store embeddings along with:
- Chunk text
- Metadata (source, page number, etc.)

This vector database becomes your **external knowledge base**.

**Vector Stores:**
- **Local**: FAISS, Chroma  
- **Cloud**: Pinecone, Qdrant, Milvus

---

## 2️⃣ Retrieval

**Retrieval** is a **real-time process** where the system finds the most relevant information from the indexed knowledge base based on the user’s query.

Goal:
> From all the knowledge available, find the **top 3–4 chunks** that best answer the query.

### Retrieval Steps

1. Generate an **embedding for the user query**
   - Use the **same embedding model** used during indexing

2. Search the vector database
   - Find chunks with highest similarity

3. Rank the retrieved chunks
   - Based on relevance score

4. Select top results
   - Extract chunk text/content

## 3️⃣ Augmentation

Augmentation is the step where the system **combines the retrieved context with the user’s query** to form a single prompt.

- Retrieved chunks are inserted into the prompt as **context**
- The original user question is preserved
- This ensures the LLM answers **based on external knowledge**, not guesses

**Example:**
- Prompt = User Query + Retrieved Context
## 4️⃣ Generation

Generation is the **final step** of the RAG pipeline.

- The augmented prompt is sent to the LLM
- The LLM uses:
  - Retrieved context
  - Its parametric knowledge
- The model generates a **final natural language response**
