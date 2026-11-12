# 🩺 MEDIC ASTRO: Zero-Trust Multi-Agent Hospital Assistant

![Python](https://img.shields.io/badge/Python-3.10%2B-blue?logo=python&logoColor=white)
![LangGraph](https://img.shields.io/badge/Framework-LangGraph-orange)
![Gradio](https://img.shields.io/badge/UI-Gradio-ff69b4)
![License](https://img.shields.io/badge/License-MIT-green)

**MEDIC ASTRO** is an enterprise-grade, secure multi-agent AI system designed for hospital operations. It moves beyond simple RAG chatbots by utilizing a stateful, multi-agent graph that routes queries to specialized databases, masks PHI, and enforces strict Zero-Trust Role-Based Access Control (RBAC) with Human-in-the-Loop (HITL) approvals.

Built for healthcare, where **hallucinations and data leaks are not an option.**

---

## 🏗️ System Architecture

This system uses **LangGraph** to orchestrate a complex network of specialized agents. It ensures that no single LLM call has absolute control over sensitive data.

```text
[User Query] 
     │
     ▼
[🛡️ Input Guardrail] ──(Blocks Prompt Injections)
     │
     ▼
[🧭 MEDIC ASTRO (Supervisor)] ──(Routes Intent)
     │
     ├─▶ [📄 Policy Retriever] ────▶ Pinecone (Vector) + Reranker + Sanitizer
     ├─▶ [👥 Staff Agent] ─────────▶ Turso DB (SQL)
     ├─▶ [💼 HR Salary Agent] ─────▶ Turso DB (SQL) + Clearance Check
     ├─▶ [🏥 Patient Agent] ───────▶ Turso Patients DB (SQL) + PHI Masking
     └─▶ [🌐 Web Search Agent] ────▶ Tavily API + Content Sanitizer
     │
     ▼
[🧠 MEDIC NOVA (Synthesizer)] ──(Streams final answer)
     │
     ▼
[🔍 MEDIC REVIEWER (DeepSeek)] ─(Independent AI Audit)
     │
     ▼
[🧑‍⚕️ Human-in-the-Loop] ───────(Pauses for Salary/Patient queries)
     │
     ▼
[🛡️ Output Guardrail] ────────(Scans for API Key/PHI Leaks)
     │
     ▼
[💻 Gradio UI (Text + Voice)]
