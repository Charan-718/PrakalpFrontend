# Critique & Improve - Project Context

## 1. Project Overview

Project Name: Critique & Improve

Repository:
https://github.com/Kouhsik33/critique-and-improve

Local Path:
D:\Downloads\critique-and-improve

The project is an AI-powered multi-agent platform for improving ideas.

The core concept is called "IDEA ARENA".

A user submits an idea, and multiple specialized AI agents analyze, challenge, transform, and improve it. The system visualizes the agents' activities in real time rather than behaving like a traditional chatbot.

The objective is to create an interactive AI environment where an idea evolves through multiple perspectives.

---

# 2. Core Workflow

The main workflow is:

User Idea
    ↓
Creator
    ↓
Critic
    ↓
Radical
    ↓
Synthesizer
    ↓
Judge
    ↓
Final Improved Idea

Each agent has a specialized role.

## Creator

Purpose:
- Expand the user's initial idea.
- Identify possibilities.
- Generate a structured version of the idea.
- Add useful features or directions.

## Critic

Purpose:
- Identify weaknesses.
- Detect assumptions.
- Find technical or practical problems.
- Identify risks and missing components.
- Challenge the Creator's output.

## Radical

Purpose:
- Think beyond conventional solutions.
- Generate unconventional alternatives.
- Challenge assumptions.
- Explore completely different approaches.

## Synthesizer

Purpose:
- Combine useful information from Creator, Critic, and Radical.
- Remove unnecessary ideas.
- Construct a coherent improved solution.
- Produce a stronger version of the original idea.

## Judge

Purpose:
- Evaluate the synthesized idea.
- Assess the final solution.
- Provide structured evaluation.
- Produce the final result.

---

# 3. Core Architecture

High-level architecture:

Frontend
    ↓
FastAPI Backend
    ↓
Execution Service
    ↓
Workflow / Graph
    ↓
Multi-Agent System
    ↓
Creator / Critic / Radical / Synthesizer / Judge
    ↓
Final Result

Supporting systems:

PostgreSQL
    → persistent application data

FAISS Vector Store
    → contextual retrieval / memory

Redis
    → real-time streaming

WebSocket
    → live communication between backend and frontend

---

# 4. Frontend

Technology:

- React
- Vite
- JavaScript / JSX
- CSS
- Framer Motion
- WebSocket

The frontend is designed as an interactive visual "IDEA ARENA", not a conventional chatbot.

Important frontend files:

frontend/src/App.jsx

frontend/src/components/
- AgentOutputs.jsx
- AgentStatus.jsx
- AgentTimeline.jsx
- FeedbackPanel.jsx
- IdeaGraph.jsx
- MetricsPanel.jsx
- TokenPanel.jsx

frontend/src/context/
- AppContext.jsx
- ThemeContext.jsx

frontend/src/hooks/
- useAgentStream.js
- useDemoStream.js

frontend/src/services/
- api.js
- websocket.js

frontend/src/styles/
- animations.css
- base.css
- global.css
- panel.css
- shell.css
- tokens.css

frontend/src/styles/components/
- arena-stage.css
- feedback.css
- graph.css
- metrics.css
- status.css
- timeline.css
- token-bars.css

The frontend also contains agent visual assets:

- blue-agent.png
- gold-agent.png
- green-agent.png
- red-agent.png
- violet-agent.png

---

# 5. Backend

Technology:

- Python
- FastAPI
- PostgreSQL
- Redis
- FAISS
- WebSocket
- Docker

Backend structure:

backend/
│
├── app/
│   ├── agents/
│   │   ├── base_agent.py
│   │   ├── creator.py
│   │   ├── critic.py
│   │   ├── judge.py
│   │   ├── radical.py
│   │   └── synthesizer.py
│   │
│   ├── config/
│   │   └── llm_config.py
│   │
│   ├── db/
│   │   └── postgres.py
│   │
│   ├── graph/
│   │   └── workflow.py
│   │
│   ├── memory/
│   │   ├── state_manager.py
│   │   └── vector_store.py
│   │
│   ├── metrics/
│   │   └── metrics_engine.py
│   │
│   ├── schemas/
│   │   └── state_schema.py
│   │
│   ├── services/
│   │   └── execution_service.py
│   │
│   └── streaming/
│       ├── redis_streaming.py
│       └── websocket.py
│
├── Dockerfile
├── docker-compose.yml
├── requirements.txt
└── ...

---

# 6. Agent Architecture

Agents use a shared base abstraction:

backend/app/agents/base_agent.py

Individual agents:

- creator.py
- critic.py
- radical.py
- synthesizer.py
- judge.py

The workflow is orchestrated by:

backend/app/graph/workflow.py

Execution is handled through:

backend/app/services/execution_service.py

The system should keep agents modular so that additional agents can be added later without rewriting the entire workflow.

---

# 7. State Management

Workflow state is defined in:

backend/app/schemas/state_schema.py

State management is handled by:

backend/app/memory/state_manager.py

The state should carry information between agents.

Conceptually:

initial_idea
    ↓
creator_output
    ↓
critic_output
    ↓
radical_output
    ↓
synthesis_output
    ↓
judge_output
    ↓
final_result

The system should avoid treating each LLM call as an isolated request.

---

# 8. Memory and Retrieval

The backend includes a FAISS vector store:

backend/app/memory/vector_store.py

Data:

backend/data/vector_store/
- index/index.faiss
- index/index.pkl
- metadata.json

Purpose:

- Store contextual information.
- Retrieve relevant information.
- Support future RAG capabilities.
- Give agents access to useful historical/contextual information.

The vector store should remain separate from PostgreSQL.

PostgreSQL should handle structured/persistent application data, while FAISS handles vector similarity retrieval.

---

# 9. Database

PostgreSQL integration:

backend/app/db/postgres.py

Potential persistent information includes:

- ideas
- sessions
- agent executions
- agent outputs
- metrics
- feedback
- execution history

Do not assume additional database fields or tables unless they exist in the actual implementation.

---

# 10. Metrics

Metrics system:

backend/app/metrics/metrics_engine.py

Frontend visualization:

- MetricsPanel.jsx
- TokenPanel.jsx
- FeedbackPanel.jsx

The system is intended to measure execution characteristics such as:

- token usage
- agent execution information
- performance metrics
- user feedback

Metrics should help demonstrate how the multi-agent workflow performs.

---

# 11. Real-Time Streaming

Backend:

backend/app/streaming/websocket.py
backend/app/streaming/redis_streaming.py

Frontend:

frontend/src/hooks/useAgentStream.js
frontend/src/services/websocket.js

Architecture:

Agent execution
    ↓
Redis streaming
    ↓
WebSocket
    ↓
React frontend

The frontend should be able to show agents executing in real time.

The goal is to avoid making users wait for the complete workflow before seeing anything.

---

# 12. IDEA ARENA UI

The UI should communicate that multiple AI agents are collaborating.

Major visual concepts:

- Agent activity
- Agent status
- Agent timeline
- Idea graph
- Agent outputs
- Token usage
- Metrics
- Feedback
- Final synthesized idea

The interface should feel like an AI workspace rather than a simple chat application.

---

# 13. LLM Layer

LLM configuration is handled through:

backend/app/config/llm_config.py

The system should maintain a provider/model abstraction rather than hardcoding provider-specific logic throughout the agents.

Environment variables should be used for API credentials.

Never commit real API keys.

Use:

.env.example

instead of committing:

.env

---

# 14. Security Rules

Never commit:

.env
frontend/.env

These must remain ignored by Git.

Safe files:

backend/.env.example
frontend/.env.example

Example:

GROQ_API_KEY=your_api_key_here

Real API keys must be stored only in local environment variables or a secure deployment secret manager.

IMPORTANT:
A Groq API key was previously accidentally committed during development. The key must be considered compromised and should be revoked/rotated.

---

# 15. Docker

Docker is part of the project architecture.

Backend contains:

backend/Dockerfile
backend/docker-compose.yml

Docker should be used for reproducible development/deployment.

---

# 16. Current Technology Stack

Frontend:
- React
- Vite
- JavaScript / JSX
- Framer Motion
- CSS
- WebSocket

Backend:
- Python
- FastAPI

AI:
- Multiple specialized AI agents
- Configurable LLM provider layer

Workflow:
- Graph-based orchestration

Database:
- PostgreSQL

Vector Retrieval:
- FAISS

Streaming:
- Redis
- WebSocket

Deployment:
- Docker

---

# 17. Product Differentiation

The project should not be presented as simply:

"An application that uses multiple LLMs."

The main differentiators are:

1. Multi-agent collaboration
2. Specialized agent roles
3. Structured workflow
4. Real-time visualization
5. Idea evolution
6. Persistent/contextual memory
7. Metrics and token tracking
8. Interactive IDEA ARENA interface

The central product idea:

"An AI-powered collaborative environment where ideas are created, challenged, transformed, synthesized, and evaluated."

---

# 18. Future Advanced Routing / Optimization

A potential future extension is intelligent model routing.

Instead of sending every request to the same LLM, the system can select models dynamically based on:

- prompt complexity
- task type
- token requirements
- latency
- cost
- expected response quality
- provider availability
- historical performance

Possible architecture:

Prompt
    ↓
Prompt Analyzer
    ↓
Routing Engine
    ↓
Model Selection
    ↓
LLM Provider
    ↓
Response
    ↓
Quality / Cost / Latency Evaluation
    ↓
Feedback
    ↓
Routing Model Improvement

Potential ML approaches:

- XGBoost for tabular routing prediction
- Transformer/small language model for prompt classification
- Reinforcement Learning for adaptive routing
- LLM-as-a-Judge for response-quality evaluation

RL concept:

State:
- prompt features
- task type
- token estimate
- latency history
- cost
- user priority
- provider/model performance

Action:
- select a model/provider

Reward:
- response quality
- low cost
- low latency
- successful completion

The RL system should learn from historical outcomes rather than rely entirely on manually defined rules.

Rule-based routing can remain as:
- fallback
- safety mechanism
- cold-start strategy

Do not make RL the first implementation. Establish reliable metrics and historical data first.

---

# 19. Hackathon Goal

The project is intended to be strong enough for a national-level hackathon.

Important qualities:

- Real working system
- Clear problem statement
- Measurable impact
- Scalable architecture
- Modular backend
- Real-time frontend
- AI/ML component
- Strong visualization
- Demonstrable metrics
- Dockerized deployment
- Secure API-key handling

Avoid adding technologies purely for appearance.

Every technology should solve a demonstrated problem.

---

# 20. Development Principles

When modifying the project:

1. Preserve the existing architecture unless there is a strong reason to change it.
2. Keep agents modular.
3. Keep frontend components reusable.
4. Avoid hardcoding API keys.
5. Use environment variables for secrets.
6. Maintain WebSocket compatibility.
7. Avoid breaking the existing agent workflow.
8. Prefer incremental changes.
9. Test backend and frontend independently.
10. Keep Docker compatibility.
11. Record important execution metrics.
12. Keep the system understandable enough to explain to hackathon judges.

---

# 21. Git State / Repository Handling

Repository:

https://github.com/Kouhsik33/critique-and-improve

Primary branch:

main

The local repository was reinitialized after GitHub Push Protection detected a Groq API key inside `.env`.

The repository must remain free of secrets.

Before pushing changes:

git status

Verify tracked environment files:

git ls-files | findstr .env

Expected result:

No actual .env files should appear.

Only example files such as:

backend/.env.example
frontend/.env.example

may be tracked.

---

# 22. Important Current Project Mental Model

Think of the system as:

                    USER
                      │
                      ▼
                 IDEA ARENA
                      │
                      ▼
              FASTAPI BACKEND
                      │
                      ▼
             EXECUTION SERVICE
                      │
                      ▼
               WORKFLOW GRAPH
                      │
       ┌──────────────┼──────────────┐
       ▼              ▼              ▼
    CREATOR         CRITIC         RADICAL
       │              │              │
       └──────────────┼──────────────┘
                      ▼
                 SYNTHESIZER
                      │
                      ▼
                    JUDGE
                      │
                      ▼
               FINAL IDEA
                      │
          ┌───────────┴───────────┐
          ▼                       ▼
       METRICS                 MEMORY
          │                       │
          ▼                       ▼
      PostgreSQL                FAISS
          │
          ▼
     React Dashboard

Real-time path:

Agent Execution
      ↓
Redis
      ↓
WebSocket
      ↓
IDEA ARENA UI

---

# 23. AI Coding Agent Instructions

When working on this project:

- First understand the existing implementation before rewriting it.
- Do not replace working architecture unnecessarily.
- Do not install dependencies without explicit approval.
- Do not modify environment variables containing secrets.
- Do not commit `.env`.
- Preserve existing frontend design language.
- Preserve agent names and responsibilities unless explicitly requested.
- Keep API contracts compatible when possible.
- Explain breaking architectural changes before making them.
- Prefer small, testable changes.
- When adding features, integrate them into the existing architecture rather than creating disconnected prototypes.
- When debugging, identify the root cause before changing multiple files.
- Run appropriate tests/build checks after significant modifications.

---

# 24. One-Sentence Project Definition

Critique & Improve is a real-time, multi-agent AI platform that transforms user ideas through creation, criticism, radical exploration, synthesis, and evaluation inside an interactive visual IDEA ARENA.
