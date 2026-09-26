<div align="center">

<img src="https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExNWc1aXl6MXI4MmNndXVjbnBpeGJucnFja2VvMGJ2M3cwazJraTZhNyZlcD12MV9naWZzX3NlYXJjaCZjdD1n/AU4qDdCl9Vncjvt43G/giphy.gif" width="80%" alt="AI Customer Service Agent">

# 🤖 AI Customer Service Agent — n8n

**An AI-powered customer service automation agent built with n8n, OpenAI, and API-based tool calling.**

</div>

---

## 📌 Project Name

**AI Customer Service Agent**

### Workflow Name

```text
AI Customer Service Agent
```

### Platform

**n8n**

### AI Model

**OpenAI GPT-4o-mini**

### Architecture

```text
Chat Trigger
     ↓
AI Agent
     ↓
AI Tool Calling
     ↓
External Order API
     ↓
AI-generated Customer Response
```

---

# 🎯 Why I Created This Project

I created this project to understand and demonstrate how **AI Agents can interact with external tools and real business data** instead of simply generating responses from their language-model knowledge.

A traditional chatbot might respond directly to a user:

```text
User
 ↓
LLM
 ↓
Response
```

The problem is that customer-service applications often need information that changes dynamically, such as:

* Customer details
* Orders
* Order prices
* Quantities
* Product categories
* Assigned employees
* Order statuses

An AI model should not guess this information.

Instead, the AI Agent can use a tool to retrieve the actual data:

```text
User
 ↓
AI Agent
 ↓
Determine required information
 ↓
Call Tool
 ↓
External API
 ↓
Real Business Data
 ↓
AI Agent
 ↓
Customer Response
```

This project was therefore created as a practical demonstration of **AI agents, tool calling, workflow automation, and API integration**.

---

# 🧠 Core Concept

The key difference between a normal chatbot and this project is **tool usage**.

### Traditional Chatbot

```text
User
  ↓
LLM
  ↓
Generated Response
```

### AI Customer Service Agent

```text
User
  ↓
AI Agent
  ↓
Understand Intent
  ↓
Does the question require business data?
  ↓
      YES
       ↓
Call appropriate tool
       ↓
External API
       ↓
Retrieve data
       ↓
AI Agent
       ↓
Generate response
```

The AI Agent acts as an **orchestrator** between the user and external business systems.

---

# 🏗️ Architecture

```text
                         ┌───────────────────────┐
                         │         User          │
                         │  Customer Question    │
                         └───────────┬───────────┘
                                     │
                                     ▼
                         ┌───────────────────────┐
                         │     Chat Trigger      │
                         │       n8n Chat        │
                         └───────────┬───────────┘
                                     │
                                     ▼
                     ┌──────────────────────────────┐
                     │           AI Agent            │
                     │                              │
                     │  Understand request          │
                     │  Check allowed scope        │
                     │  Decide whether tool needed │
                     └──────────┬───────────┬───────┘
                                │           │
                         AI Model│           │Tool Call
                                │           │
                                ▼           ▼
                     ┌───────────────┐  ┌────────────────┐
                     │ OpenAI        │  │ GetOrderData   │
                     │ GPT-4o-mini   │  │ HTTP Tool      │
                     └───────────────┘  └───────┬────────┘
                                                │
                                                ▼
                                      ┌──────────────────┐
                                      │   Order API      │
                                      │                  │
                                      │ Orders           │
                                      │ Prices           │
                                      │ Quantities       │
                                      │ Categories       │
                                      │ Employees        │
                                      │ Status           │
                                      └────────┬─────────┘
                                               │
                                               ▼
                                      ┌──────────────────┐
                                      │    AI Agent      │
                                      │ Interpret Data   │
                                      └────────┬─────────┘
                                               │
                                               ▼
                                      ┌──────────────────┐
                                      │ Customer Answer  │
                                      └──────────────────┘
```

---

# 🔒 Agent Scope

This is **not intended to be a general-purpose AI assistant**.

The agent is specifically designed for:

```text
Customer Service
       │
       ├── Customer information
       ├── Order information
       ├── Order prices
       ├── Order quantities
       ├── Product categories
       ├── Assigned employees
       └── Order statuses
```

If the user asks something outside this scope, the agent refuses to answer.

For example:

```text
User:
Write a Python program for me.
```

The agent responds:

```text
I am a Customer AI Agent and I am only meant to answer customer-related queries.
```

This prevents the workflow from becoming an unrestricted general-purpose chatbot.

---

# 🧾 System Prompt

The AI Agent uses the following system prompt:

```text
You are a customer service AI agent. Your purpose is strictly limited to handling customer-related queries using the customer and order data available through your tools.

You have access to a customer database and an order database.

When asked about customers, use the GetCustomers tool to look up their information.
When asked about orders, prices, employees, quantities, product categories, or order statuses, use the GetOrderData tool.

Only answer questions that are directly related to customer service, customers, orders, or the business data available through your tools.

Do not answer questions that are unrelated to your customer-service purpose, including general knowledge, programming, coding, entertainment, personal advice, politics, mathematics, or other unrelated topics.

If a user asks something outside your intended use case, respond exactly with:

"I am a Customer AI Agent and I am only meant to answer customer-related queries."

Do not attempt to answer, explain, or provide information about unrelated requests.

Be concise and helpful when handling valid customer-related queries.

If the available data does not contain the information the user is asking for, say so. Do not make up information.

Never fabricate customer, order, employee, price, quantity, category, or status information.
```

---

# 🔄 Workflow Components

## 1. Chat Trigger

### Node

```text
When chat message received
```

The Chat Trigger acts as the entry point for the workflow.

Example:

```text
Who is the employee assigned to order 5?
```

The user's message is passed to the AI Agent.

---

# 2. AI Agent

### Node

```text
AI Agent
```

The AI Agent is responsible for:

* Understanding the user's request
* Determining whether the request is within scope
* Deciding whether a tool is required
* Calling the appropriate tool
* Interpreting returned data
* Generating the final response
* Refusing unrelated requests
* Avoiding fabricated information

The AI Agent uses **GPT-4o-mini** as its language model.

---

# 3. OpenAI Chat Model

### Node

```text
OpenAI Chat Model
```

### Model

```text
gpt-4o-mini
```

The model provides the language-understanding and response-generation capabilities required by the AI Agent.

---

# 4. GetOrderData Tool

### Node

```text
HTTP Request
```

### Node Type

```text
n8n-nodes-base.httpRequestTool
```

The HTTP Request Tool allows the AI Agent to retrieve order information from the external API.

### Tool Description

```text
Retrieve order details including order prices, quantities, product categories, employee assignments, and order statuses for all customers
```

The description tells the AI Agent what information the tool can provide.

---

# 📊 Data Retrieved

The order API provides information such as:

| Field       | Description                        |
| ----------- | ---------------------------------- |
| Order ID    | Unique order identifier            |
| Customer ID | Customer associated with the order |
| Employee    | Employee assigned to the order     |
| Price       | Price of the order                 |
| Quantity    | Quantity associated with the order |
| Category    | Product category                   |
| Status      | Current order status               |

---

# 🔌 Tool Connection

The important n8n connection is:

```text
HTTP Request
      │
      │ ai_tool
      ▼
AI Agent
```

This is different from a normal sequential workflow connection.

The HTTP Request node is configured as an **AI Tool**, which allows the AI Agent to decide when to invoke it.

---

# 🧪 Example: Tool Calling

Consider the question:

```text
Who is the employee assigned to order 5?
```

The workflow operates approximately as follows:

```text
1. User sends question
        ↓
2. Chat Trigger receives question
        ↓
3. AI Agent analyzes request
        ↓
4. Agent determines order data is required
        ↓
5. Agent calls GetOrderData
        ↓
6. API returns order information
        ↓
7. Agent finds Order 5
        ↓
8. Agent identifies employee
        ↓
9. Agent generates response
```

The response is based on the retrieved API data rather than an invented answer.

---

# 💬 Example Queries

## Customer Questions

```text
What region is customer 10 in?
```

```text
Tell me about customer 3.
```

## Order Questions

```text
How many orders does customer 3 have?
```

```text
Who is the employee assigned to order 5?
```

```text
What is the status of order 10?
```

## Product Questions

```text
What is the total order value for all electronics orders?
```

```text
How many electronics orders are there?
```

## Multi-step Questions

```text
What orders belong to customers in Europe?
```

---

# 🚫 Out-of-Scope Questions

The agent intentionally refuses unrelated questions.

### Example

```text
User:
Write a Java program to sort an array.
```

### Response

```text
I am a Customer AI Agent and I am only meant to answer customer-related queries.
```

Another example:

```text
User:
Who won yesterday's cricket match?
```

Response:

```text
I am a Customer AI Agent and I am only meant to answer customer-related queries.
```

The purpose of this restriction is to keep the agent focused on its intended business function.

---

# 🛡️ Data Reliability

The system prompt explicitly instructs the AI Agent:

```text
If the available data does not contain the information the user is asking for, say so.

Do not make up information.

Never fabricate customer, order, employee, price, quantity, category, or status information.
```

This is important for customer-service systems because incorrect business information can result in misleading responses.

---

# ⚙️ Workflow Configuration

| Component       | Configuration             |
| --------------- | ------------------------- |
| Workflow Name   | AI Customer Service Agent |
| Platform        | n8n                       |
| Trigger         | Chat Trigger              |
| AI Agent        | n8n AI Agent              |
| AI Model        | OpenAI GPT-4o-mini        |
| Tool            | HTTP Request Tool         |
| Data Source     | External Order API        |
| Interaction     | Natural Language          |
| Agent Scope     | Customer Service          |
| Workflow Status | Active                    |

---

# 🔐 Credentials and Security

The workflow requires authentication to access the external API.

The exported workflow should **never contain real credentials**.

Use placeholders such as:

```text
{{YOUR_N8N_QUICKSTART_API_KEY}}
```

and:

```text
{{YOUR_X_ASSESSMENT_ID}}
```

Real API keys should never be committed to GitHub.

For production implementations, credentials should be stored using secure mechanisms such as:

* n8n Credentials
* Environment Variables
* Secret Managers
* Cloud Secret Storage

---

# 🚀 How to Run

## Step 1 — Import the Workflow

Import the workflow JSON into n8n.

Recommended repository structure:

```text
ai-customer-service-agent/
│
├── README.md
│
└── workflow/
    └── ai-customer-service-agent.json
```

## Step 2 — Configure OpenAI

Configure the OpenAI credential used by:

```text
OpenAI Chat Model
```

## Step 3 — Configure API Credentials

Configure the HTTP Request Tool with the required authentication.

Do not use credentials committed to the repository.

## Step 4 — Activate the Workflow

The workflow needs to be active when using the production Chat Trigger URL.

```text
Workflow
   ↓
Active = ON
```

## Step 5 — Test the Agent

Open the Chat interface and ask:

```text
Who is the employee assigned to order 5?
```

The agent should retrieve the order information using the tool and return the corresponding employee.

---

# 🧩 Why n8n?

This project could be implemented as a traditional software application using a frontend, backend, database, REST APIs, AI integration, and custom business logic.

However, the primary problem being demonstrated here is **workflow orchestration and AI-tool integration**.

n8n makes these integrations visual and allows the workflow to be assembled quickly.

Instead of:

```text
Frontend
   ↓
Backend
   ↓
Controller
   ↓
Service
   ↓
API Client
   ↓
External API
   ↓
AI Integration
```

the prototype can be represented as:

```text
Chat
 ↓
AI Agent
 ↓
Tool
 ↓
API
```

This makes n8n particularly useful for **rapid AI prototyping and automation**.

---

# ⚡ Advantages of the Workflow Approach

## Faster Prototyping

A working AI agent can be assembled without building an entire backend application.

This is useful when the objective is to validate an idea before investing significant development time.

## Visual Workflow

The architecture can be understood directly from the workflow:

```text
Trigger → Agent → Tool → API
```

## Easy API Integration

The HTTP Request Tool allows the agent to interact with an external API without requiring a complete custom API-client implementation.

## Easy to Extend

Additional tools can be connected to the AI Agent.

```text
                         ┌── Customer Database
                         │
                         ├── Order API
                         │
AI Customer Agent ───────┼── Product API
                         │
                         ├── Email
                         │
                         ├── CRM
                         │
                         └── Support Tickets
```

---

# ⚖️ n8n vs Traditional Application

This project does **not** suggest that n8n is always better than a traditional application.

The appropriate architecture depends on the requirements.

| Requirement                      | n8n Workflow               | Traditional Application            |
| -------------------------------- | -------------------------- | ---------------------------------- |
| Rapid prototyping                | Very suitable              | More development required          |
| Workflow automation              | Strong                     | Requires implementation            |
| AI tool orchestration            | Built into workflow        | Usually custom implementation      |
| API integration                  | Easy to configure          | Requires development               |
| Visual workflow                  | Strong                     | Usually requires separate diagrams |
| Complex business logic           | Can become difficult       | Strong                             |
| Custom algorithms                | Limited compared with code | Strong                             |
| Fine-grained application control | Lower                      | Higher                             |
| Large custom applications        | Depends on requirements    | Usually more suitable              |
| Rapid experimentation            | Strong                     | More effort                        |
| Multiple SaaS/API integrations   | Strong                     | Requires integration code          |

The key idea is:

> **Use n8n when workflow orchestration, automation, integrations, and AI-agent coordination are central to the problem.**

A traditional application may be more appropriate when the system requires extensive custom business logic, complex domain modeling, highly customized user interfaces, or fine-grained application-level control.

---

# 💡 Why This Can Be Better Than Building an Application for This Use Case

For this particular prototype, the main objective is not to create a complete customer-management platform.

The objective is to demonstrate:

```text
Can an AI Agent understand a customer question,
identify the required business information,
retrieve that information from an external system,
and return a useful answer?
```

Building a complete application first would introduce additional layers that are not necessary for validating that idea.

n8n allows the core hypothesis to be tested quickly:

```text
Customer Question
       ↓
AI Agent
       ↓
Tool Selection
       ↓
API
       ↓
Business Data
       ↓
Answer
```

Once the workflow proves useful, it can later be integrated into a larger application.

Therefore, n8n can act as a **rapid AI orchestration and prototyping layer** rather than necessarily replacing traditional application development.

---

# 🔮 Future Improvements

This prototype can be extended into a complete customer-service automation platform.

## Customer Database Tool

Add:

```text
GetCustomers
```

to retrieve customer information.

## Multiple Tools

```text
AI Agent
 ├── GetCustomers
 ├── GetOrderData
 ├── GetProducts
 ├── CRM
 ├── Email
 └── Support Tickets
```

## Persistent Conversation Memory

Store relevant conversation context so that users can ask follow-up questions without repeating information.

## Human Escalation

```text
Customer
   ↓
AI Agent
   ↓
Cannot Resolve
   ↓
Create Support Ticket
   ↓
Human Support Agent
```

## Production Database

Replace the Quickstart API with a real business data source such as:

```text
MySQL
PostgreSQL
REST API
GraphQL API
CRM
ERP
```

## Monitoring and Evaluation

A production implementation could include:

* Tool-call monitoring
* Execution logs
* Error handling
* Response evaluation
* Human feedback
* Analytics
* Authentication
* Authorization
* Rate limiting

---

# 📚 What This Project Demonstrates

This project demonstrates practical experience with:

* n8n
* Workflow automation
* AI Agents
* OpenAI models
* GPT-4o-mini
* Tool calling
* HTTP APIs
* API authentication
* Prompt engineering
* AI guardrails
* Natural-language interfaces
* External data retrieval
* AI workflow orchestration
* Rapid AI prototyping
* Domain-restricted AI agents

---

# 🏁 Final Result

The completed workflow provides a focused AI customer-service agent.

```text
Receive Customer Question
          ↓
Understand Intent
          ↓
Check Agent Scope
          ↓
Determine Required Data
          ↓
Call External Tool
          ↓
Retrieve Business Data
          ↓
Generate Data-Based Response
```

### Valid Request

```text
User:
Who is the employee assigned to order 5?

        ↓

AI Agent
        ↓
GetOrderData
        ↓
Order API
        ↓
Order 5
        ↓
Employee: Amelie
        ↓

AI Agent:
The employee assigned to order 5 is Amelie.
```

### Invalid Request

```text
User:
Write a Python program.

        ↓

AI Agent:

I am a Customer AI Agent and I am only meant
to answer customer-related queries.
```

The project demonstrates the transition from a simple **LLM chatbot** to a **domain-specific AI agent capable of using external tools**.

---

# 📁 Repository Structure

```text
ai-customer-service-agent/
│
├── README.md
│
├── workflow/
│   └── ai-customer-service-agent.json
│
├── screenshots/
│   ├── workflow.png
│   ├── tool-call.png
│   └── example-response.png
│
└── docs/
    └── architecture.md
```

---

# 📜 License

This project is primarily intended as a learning, experimentation, and demonstration project.

The external Quickstart API and associated course resources belong to their respective owners.

---

<div align="center">

<img src="https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExNWc1aXl6MXI4MmNndXVjbnFpeGJucnFja2VvMGJ2M3cwazJraTZhNyZlcD12MV9naWZzX3NlYXJjaCZjdD1n/WJNzMUrXZefmePgdVr/giphy.gif" width="40%" alt="AI Customer Service Agent Footer">

**AI Customer Service Agent · Built with n8n + OpenAI**

</div>
