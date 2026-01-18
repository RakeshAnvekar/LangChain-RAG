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

---

## Summary

- Parametric knowledge is powerful but limited
- Prompting works only when knowledge already exists in the model
- Fine-tuning and RAG help LLMs work with **new, private, and large-scale data**
