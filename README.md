# Parametric Knowledge in LLMs

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

---

### 2️⃣ RAG (Retrieval-Augmented Generation)

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
