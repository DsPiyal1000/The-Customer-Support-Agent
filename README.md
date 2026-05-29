# 🤖 The Customer Support Agent
### *A Gentle Introduction to Building AI Tools with Claude*

> **What is this?**  
> This is a lightweight, working example of an AI customer support agent. It doesn't just "chat"—it **acts**. It can look up customer accounts, find order details, and explain complex statuses (like warehouse holds) to a user, all by using tools behind the scenes.

> **What problem does it solve?**  
> It bridges the gap between *talking* to an AI and *using* an AI. It demonstrates how to:
> *   Connect an AI model (Claude) to real (or mock) data.
> *   Handle the "loop" where the AI decides to use a tool, gets data, and then responds.
> *   Gracefully manage errors (like a wrong order ID) without breaking the conversation.

---

## 🌟 Why Run This?
Whether you are an architect, a developer, or just curious about AI agents, this project shows you the **exact mechanics** of a functional agent.

*   ✅ **No Database Needed**: Uses realistic mock data so you can run it instantly.
*   ✅ **Real Tool Logic**: See how the AI handles success, failure, and edge cases.
*   ✅ **Clean Architecture**: Separated into data, tools, and the agent loop for easy learning.
*   ✅ **Production-Ready Patterns**: Uses best practices like `.env` for keys and structured error handling.

---

## 🚀 Getting Started in 3 Steps

### 1. Prerequisites
Make sure you have **Python 3.9** or higher installed.
*(If you aren't sure, type `python --version` in your terminal.)*

### 2. Install Dependencies
Open your terminal in the project folder and run:
```bash
pip install anthropic python-dotenv

```

### 3. Set Up Your API Key
We keep things secure by never hardcoding keys.

Create a file named .env in the root folder.
Add your Anthropic API key inside:
env

Copy
ANTHROPIC_API_KEY=sk-ant-xxxxxxxxxxxx
(Get a key at console.anthropic.com if you don't have one.)
🔒 Security Note: The .env file is already ignored by Git. Never commit your API key to a public repository!

--- 

## 🧑‍💻 🎮 How to Run & Test
Start the agent with a single command:

bash

Copy
python agent.py
Once running, try these three scenarios to see the agent in action:

✨ The Happy Path
"Where is my order ORD-8821?"
The agent finds the customer, the order, and explains the warehouse hold clearly.
⚠️ The Missing Order
"Where is my order ORD-1234?"
The agent finds the customer but notices the order is missing, then guides the user politely.
📭 The Empty Account
"What orders do I have?" (as James Okafor)
The agent sees the account exists but has no orders, explaining this without treating it as an error.

---    

## 📚 🏗️ Under the Hood
The project is organized to be easy to read:

📂 mock_data.py: The "database" of customers and orders.
🛠️ tools.py: Defines what the agent can do (lookups).
⚙️ tool_runner.py: The engine that executes the tools.
🔄 agent.py: The brain. It manages the conversation history and loops until the job is done.

---

## 💡 What's Next?
This agent works perfectly under ideal conditions. In a real production environment, you'd want to handle:

Slow database timeouts.
Frustrated users or off-topic questions.
Complex workflows like processing refunds.
This project is the foundation. The patterns here are exactly what need to scale up to those harder challenges.

🙏 Enjoy Building!

---



