# 🤖 Customer Support Agent
> *An AI that doesn't just talk — it acts. And acts safely.*

---

## What Is This?

A customer support agent built with **Python + Claude** that takes real actions — looking up accounts, fetching orders, processing refunds — by calling tools behind the scenes.

But here's what makes it different:

**Safety rules are enforced in code, not in prompts.**

Most agents rely on telling the AI *"remember to verify identity first."* That's a probabilistic hope. In this project, the refund function *literally cannot run* unless a verification flag is set in memory. No prompt trick can bypass it. That's a deterministic guarantee.

---

## The Problem It Solves

| Challenge | How It's Handled |
| :--- | :--- |
| AI picks the wrong tool | Precise, unambiguous tool descriptions |
| Vague error messages | Structured errors with category + recovery guidance |
| Safety rules forgotten mid-chat | Code-enforced gates — bypassing is impossible |
| Tools tied to one app | Stateless MCP server for reuse anywhere |

---

## How It Works

```
User Message
     ↓
 agent.py          ← Manages conversation + session state
     ↓
 tool_runner.py    ← Executes tools + enforces safety gates
     ↓
 tools.py          ← Defines what the agent can do
     ↓
 mcp_server.py     ← Exposes tools to any external app
```

### The Safety Gate (Core Idea)

```python
def process_refund(order_id: str, session_state: dict) -> dict:
    if not session_state.get("verified_customer_id"):
        return {"error": "Identity verification required before processing a refund."}
    # Only runs if the gate passes ↑
```

The AI decides *when* to call the function. The function decides *whether* to actually run. That separation is everything.

---

## Quick Start

**1. Install dependencies**
```bash
pip install anthropic python-dotenv
```

**2. Add your API key**

Create a `.env` file in the root folder:
```ini
ANTHROPIC_API_KEY=sk-ant-xxxxxxxxxxxx
```
> Get a key at [console.anthropic.com](https://console.anthropic.com) — and never commit this file.

**3. Run the agent**
```bash
python agent.py
```

---

## Try These 5 Scenarios

| # | What to Type | What You'll See |
| :- | :--- | :--- |
| ✅ | `"Where is my order ORD-8821?"` | Smooth lookup + warehouse hold explained |
| ⚠️ | `"Where is my order ORD-1234?"` | Order not found — handled gracefully |
| 📭 | `"What orders do I have?"` *(as James Okafor)* | Account exists, no orders — no crash |
| 🚫 | `"Refund order ORD-8821."` | Gate blocks it — asks for identity first |
| ✅ | `"I'm Sarah Chen. Refund order ORD-8821."` | Full verified flow — refund succeeds |

> 💡 After the last one, try `"Refund order ORD-9999"` — that order belongs to someone else, so even with Sarah verified, it gets blocked.

---

## Project Structure

```
project/
├── agent.py          # Conversation loop + session state
├── tool_runner.py    # Tool execution + safety gates
├── tools.py          # Tool definitions
├── mock_data.py      # In-memory "database" (no setup needed)
├── mcp_server.py     # Stateless MCP interface
└── .env              # Your API key — never commit this
```

---

## The Bigger Idea

> **LLMs are probabilistic. Systems must be deterministic.**

Prompts guide. Code enforces. This project keeps those two responsibilities cleanly separated — so "the AI forgot the rule" is never an acceptable failure mode.

The same pattern applies anywhere the stakes are real: refunds, cancellations, data exports, privilege changes.

---

## What to Build Next

Once you're comfortable with the patterns here, natural extensions include:

- `cancel_order` tool with a gate that checks shipment status
- A frustration detector that routes upset users to a human
- Real database connection replacing the mock data
- Timeout handling for slow external services

The architecture is already set up for all of it.

---

*Happy building.* 🌟