# Process Automation Assistant — Sensor/Log Anomaly Agent

A LangGraph + Gemini agent that monitors industrial, building-automation, and traffic-management sensor streams, detects anomalies, grounds its reasoning in a small maintenance knowledge base (RAG), and recommends a concrete next action — with an LLM-as-judge evaluation layer and a live Gradio dashboard.

Built as a portfolio project reflecting the kind of work described in [evon](https://evon-automation.com/)'s *Software Development & AI* internship posting. evon's XAMControl platform is deployed across [industry](https://evon-automation.com/xamcontrol-industrie/), [traffic management](https://evon-automation.com/xamcontrol-verkehrstechnik/), and [building automation](https://evon-automation.com/xamcontrol-glt/) — so this project simulates sensor/log data across all three domains rather than picking just one.

## Pipeline

```
sensor/log stream → anomaly detection → RAG retrieval (maintenance KB) → LLM reasoning (Gemini) → recommended action → structured log
```

## Why this project

evon's internship mission has three parts — implement an AI use case end-to-end, evaluate a new technology, and optimize an internal process (knowledge management, technical support, automation, or assistance systems). This project touches all three:

- **Implements a use case**: a working agent that goes from raw sensor data to a structured, actionable recommendation.
- **Evaluates a technology**: LangGraph agent orchestration is compared against a simple rolling-statistics baseline, and every recommendation is scored by an independent LLM-as-judge pass.
- **Optimizes a process**: anomaly triage — turning raw signal drift into a prioritized, escalation-ready action — is exactly the kind of assistance system the posting describes, applied to the domains XAMControl actually serves.

## Stack

Google Colab · [`google-genai`](https://pypi.org/project/google-genai/) (Gemini) · [LangGraph](https://github.com/langchain-ai/langgraph) · [FAISS](https://github.com/facebookresearch/faiss) · [sentence-transformers](https://www.sbert.net/) · [Gradio](https://www.gradio.app/) · scikit-learn

## Run it

1. Open `evon_process_automation_assistant.ipynb` in Google Colab.
2. Get a free Gemini API key from [Google AI Studio](https://aistudio.google.com/apikey).
3. In Colab, add it as a secret named `GEMINI_API_KEY` (Runtime → Secrets), or just paste it when prompted.
4. Run all cells top to bottom. Data generation, anomaly detection, and retrieval work with no key; the reasoning, recommendation, and judge steps need the Gemini key.
5. The last cell launches a Gradio dashboard where you can pick a domain, inject an anomaly type, and watch the agent work through the pipeline live.

## Notes & limitations

- Sensor/log data is synthetic (three domains: industry, building automation, traffic), each with a normal operating band and a few anomaly types. Swapping in real historian/PLC exports only requires replacing the data-generation function — the rest of the pipeline is unchanged.
- Anomaly detection is a simple rolling z-score + IsolationForest, chosen to keep the focus on the agent/RAG/reasoning layer.
- See the notebook's final section for suggested extensions (conditional re-retrieval on low judge scores, human-approval routing for critical priority, real data source integration).

---
*Author: Sanusi — M.Sc. Data Science, University of Leoben. Built as a portfolio project for evon's Software Development & AI internship posting (St. Ruprecht an der Raab, Austria).*
