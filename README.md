# 🤖 Customer Support Agent — AI That Actually Acts

> A lightweight, working example of an AI agent built with Python and Claude.  
> Not just a chatbot. An agent that **does things** — and does them **safely**.

---

## What Is This?

This is a customer support agent that bridges the gap between *talking to an AI* and *using an AI*.

Most chatbots answer questions. This one takes **actions** — looking up accounts, fetching order details, explaining warehouse holds, and processing refunds — all by calling real tools behind the scenes.

But here is the part that makes it different from most examples you will find:

**The safety rules are enforced in code, not in prompts.**

If you tell an AI *"always verify identity before processing a refund"*, it might forget that rule mid-conversation. That is a probabilistic risk. In this project, the code literally cannot run a refund unless a verification flag is set to `True` in memory. No amount of clever prompting can bypass it. That is a deterministic guarantee.

---

## Why Does This Matter?

Most AI agent tutorials show you the happy path — the AI works, the tool runs, everyone is happy.

Real applications need to handle:

- A user who tries to skip identity verification
- An order ID that belongs to someone else
- A refund request with no verified session

This project shows you exactly how to handle all of those — with clean architecture that separates **what the AI wants to do** from **what the code actually allows**.

---

## 🌟 Key Features

| Feature | What It Means |
| :--- | :--- |
| **No database needed** | Uses realistic mock data — run it instantly |
| **Real tool logic** | Handles success, failure, and edge cases |
| **Session state gates** | Critical actions are blocked in code, not just in prompts |
| **Structured error recovery** | When a gate blocks an action, the agent understands why and corrects itself |
| **Clean architecture** | Data, tools, engine, and brain are all in separate files |
| **Production patterns** | `.env` for keys, structured error handling throughout |

---

## 🚀 Getting Started

### Prerequisites

Make sure you have **Python 3.9 or higher** installed.

Not sure? Check by running this in your terminal:

```bash
python --version
```

### Step 1 — Install Dependencies

Open your terminal in the project folder and run:

```bash
pip install anthropic python-dotenv
```

### Step 2 — Add Your API Key

Create a file named `.env` in the root folder and add this line:

```ini
ANTHROPIC_API_KEY=sk-ant-xxxxxxxxxxxx
```

> 🔒 **Security note:** The `.env` file is already in `.gitignore`. Never commit your API key to a public repository. Get a key at [console.anthropic.com](https://console.anthropic.com) if you do not have one yet.

### Step 3 — Run the Agent

```bash
python agent.py
```

That is it. The agent starts and waits for your input.

---

## 🧪 Try These Scenarios

The agent is designed to handle real situations, not just the easy ones. Here are five worth trying:

### ✅ The Happy Path — Order Lookup
```
"Where is my order ORD-8821?"
```
The agent finds the customer, the order, and explains the warehouse hold clearly.

---

### ⚠️ The Missing Order
```
"Where is my order ORD-1234?"
```
The agent finds the customer but notices the order ID does not exist, then guides the user politely instead of crashing.

---

### 📭 The Empty Account
```
"What orders do I have?"  (try this as James Okafor)
```
The account exists but has no orders. The agent explains this clearly without treating it as an error.

---

### 🚫 The Blocked Refund Attempt
```
"Refund order ORD-8821."
```
The agent tries to process the refund, but the code checks `session_state`. Since no identity has been verified, the tool returns a structured error. The agent politely asks for your name first — it cannot proceed any other way.

---

### ✅ The Full Refund Flow
```
"I'm Sarah Chen. Refund order ORD-8821."
```
This time it works:
1. Agent calls `get_customer` → finds Sarah
2. Writes verified ID to `session_state`
3. The gate opens
4. Refund processes

Try following this with `"Refund order ORD-9999"` — that order belongs to someone else, so even with Sarah verified, it gets blocked.

---

## 🏗️ How It Works

### Session State — The Memory That Counts

Instead of relying on the conversation history (which is just text), the agent maintains a Python dictionary called `session_state` that tracks what actually happened.

- **Conversation history says:** "Claude said it would verify the user."
- **Session state says:** `verified_customer_id = "C001"` — or `None`.

The code checks the state, not the chat log. If the state says unverified, the action is impossible.

---

### Programmatic Prerequisites — The Safety Gates

Every critical tool has a gate at the very top:

```python
def process_refund(order_id: str, session_state: dict) -> dict:
    # Gate: this check happens before anything else
    if not session_state.get("verified_customer_id"):
        return {"error": "Identity verification required before processing a refund."}

    # ... rest of the logic only runs if the gate passes
```

The AI can *decide* to call this function whenever it wants. But the Python function decides whether to actually run. That separation — AI handles intent, code handles enforcement — is the core pattern here.

---

### Structured Error Recovery — The Feedback Loop

When a gate blocks an action, the tool returns a clear structured error rather than crashing. The agent reads this, understands what is missing, and automatically corrects its approach.

```
Agent calls process_refund → Gate returns error: "verification required"
→ Agent reads error → Agent asks user for their name → User provides name
→ Agent calls get_customer → Writes verified ID to session_state
→ Agent calls process_refund again → Gate passes → Refund succeeds
```

This loop runs automatically. No hardcoded flow. The agent figures it out.

---

## 📂 Project Structure

```
project/
│
├── mock_data.py      # The "database" — customers, orders, and refund records
├── tools.py          # Definitions of what the agent can do
├── tool_runner.py    # The engine — executes tools and enforces state gates
├── agent.py          # The brain — manages the conversation loop and session state
└── .env              # Your API key (never commit this)
```

Each file has one job. This makes the code easy to read and even easier to extend.

---

## 💡 The Bigger Idea

This architecture reflects an important principle in production AI systems:

> **LLMs are probabilistic. Systems must be deterministic.**

Relying on prompts alone to enforce rules is a risk — especially for anything involving money, personal data, or security. The gates in this project are a direct implementation of that principle: the AI handles conversation and reasoning, while the code handles enforcement and trust.

The same pattern scales to any high-risk action — cancellations, data exports, privilege escalations, or anything where "the AI forgot the rule" is not an acceptable failure mode.

---

## 🔭 What to Build Next

This project is a foundation. Once you understand the patterns here, natural next steps include:

- **Add a `cancel_order` tool** with a gate that checks whether the order has already shipped
- **Add a timeout handler** for slow database responses
- **Add a frustration detector** that routes angry users to a human escalation path
- **Replace mock data** with a real database connection

The architecture is already set up for all of this. The files are separated, the patterns are clean, and the gates are in the right place.

---

## 🙏 Happy Building
