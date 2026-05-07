# 🤖 The Ultimate Simple Guide to AI Evaluation

Think of this guide as a "translation" from high-tech computer talk to everyday English. We'll explain how we test our AI, what the fancy words mean, and how we use a "Super AI" to grade our "Regular AI."

---

## 👨‍⚖️ The "Super AI" Teacher (The LLM Judge)

Before we look at the tests, you need to know about the **LLM Judge**. This is the secret weapon of our evaluation.

*   **What it is**: We use a very powerful AI model called **Llama 3.3 70B** (hosted on Groq). Think of this as the "Teacher" or the "Grader."
*   **Why we use it**: Regular code is good at checking math (2+2=4), but it's bad at checking conversation. Only an AI can understand if a bot is being polite, helpful, or confusing.
*   **How it works**:
    1. The "Regular AI" (the one we are testing) finishes its conversation.
    2. We send the **whole transcript** of that talk to the "Super AI" (The Judge).
    3. We give the Judge a **Rubric** (a checklist of what a good answer looks like).
    4. The Judge reads both and gives a score from 1 to 5.
*   **Where is it in the code?**:
    *   The logic for the Judge is in: `evals/utils/llm_judge.py`.
    *   The settings (which model to use) are in: `evals/.env`.

---

## 📋 The Five Evaluation Categories ("Suites")

### 1. The "Good Talk" Test (Suite 1: Conversational)
*   **Metric: Task Completion Rate**
    *   *Simple Translation*: "Did the user actually get what they wanted?" If you asked for a light to be turned on, did it happen?
*   **Metric: Policy Adherence**
    *   *Simple Translation*: "Did the bot follow the rules?" If a user asks for your password or something dangerous, did the bot say "No"?
*   **How it's used**: We run 12 scripted dialogues from `evals/data/conversations/dialogues.json`. The Judge reads each one and decides if the bot passed.

### 2. The "Library" Test (Suite 2: RAG)
*   **Term: RAG (Retrieval-Augmented Generation)**
    *   *Simple Translation*: "Searching the library." Instead of guessing, the bot looks up info in a document.
*   **Metric: Recall @ 5**
    *   *Simple Translation*: "Finding the needle in the haystack." We give the bot a haystack of 5 chunks of info. If the "needle" (the correct info) is in those 5, it's a success!
*   **Metric: Faithfulness**
    *   *Simple Translation*: "The No-Lies Rule." We check if the bot's answer is *actually* based on the documents it found. If it found a book about cats but started talking about dogs, it fails.

### 3. The "Memory" Test (Suite 3: CRM)
*   **Term: CRM (Customer Relationship Management)**
    *   *Simple Translation*: "The Bot's Address Book."
*   **Metric: Identity Consistency**
    *   *Simple Translation*: "Who are you again?" We test if the bot remembers your name, your favorite lights, or your schedule across multiple messages.
*   **How it's used**: We simulate a user logging in and giving info, then we check if the bot's brain (the database) actually stored it.

### 4. The "Toolbox" Test (Suite 4: Tools)
*   **Metric: Selection Accuracy**
    *   *Simple Translation*: "Picking the right tool." If you ask for math, does it pick the calculator? If you ask for news, does it pick the web search?
*   **Metric: Argument Extraction**
    *   *Simple Translation*: "Typing correctly." If it picks the calculator, does it type `2+2` or does it accidentally type `banana`?
*   **Where is it?**: Test cases are in `evals/data/tools/tool_test_cases.json`.

### 5. The "Speed" Test (Suite 5: Latency)
*   **Metric: TTFT (Time to First Token)**
    *   *Simple Translation*: "Reaction Time." The time from when you click 'Send' to when the first letter of the bot's reply appears. We want this to be faster than a blink!
*   **Metric: E2E (End-to-End)**
    *   *Simple Translation*: "Total Talk Time." How long it takes for the bot to finish its whole sentence.
*   **Why it matters**: A smart bot that takes 5 minutes to answer is a useless bot. We want it fast.

---

## 🚦 Summary of Metrics (The "Report Card")

| Fancy Word | Simple Meaning | Why we care |
| :--- | :--- | :--- |
| **Recall** | Search success | So the bot knows the facts. |
| **Faithfulness** | Honesty | So the bot doesn't lie to you. |
| **Latency (TTFT)** | Snappiness | So you don't get bored waiting. |
| **Selection** | Smart Choice | So the bot knows when it needs help. |

---

## 🚀 Ready to Run?
Just open your terminal and type:
```powershell
pytest evals/
```
Your computer will now act as the **Teacher**, the **Student**, and the **Library** all at once to give the bot its final grade!
