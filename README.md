
# 📖 PhishShield – Design Notes & Architecture Overview

## 🧭 Project Vision

**PhishShield** is an open-source, modular, and scalable platform for detecting phishing across multiple modalities — **text, audio, image, video** — using **Model Context Protocol (MCP)** and **LLM-based inference backends**.

> **Core Idea:** Start with real-time phishing detection using a client-server architecture powered by an LLM and MCP, while progressively integrating a training layer for improving detection accuracy over time using real-world submissions.

## ⚙️ High-Level Architecture

### 🧠 1. **Separation of Responsibilities**

| Layer | Description |
|-------|-------------|
| **PhishShield Inference Layer** | Real-time detection using LLM (vLLM or Ollama) and Model Context Protocol (MCP). Handles user queries, inference, and metadata tagging. |
| **Learning/Training Backend** | Offline and asynchronous ML training layer that receives flagged content (audio, video, etc.) and uses it to train/improve detection models. |

## 🧱 Component Breakdown

### 🔐 PhishShield Inference Server
- Main LLM server exposing MCP-compatible endpoints.
- Backend: vLLM or Ollama.
- Starts with a phishing detection model.
- REST + JSON-RPC interface.

### 🌉 Client Gateway
- SDK/REST client for mobile/web apps.
- Sends/receives data using MCP format.

### 🧪 Training Submission Layer (Future Phase)
- Stores flagged content.
- Feeds learning backend.

### 🧠 Learning/Training Pipeline
- Model retraining (text/audio/video).
- Tools: MLflow, PyTorch, HuggingFace.

## 🔁 Workflow Overview

### ✅ Phase 1: Real-Time Detection (POC)
1. Client submits message.
2. Server responds with phishing risk verdict.
3. Optional: Flag for training.

### 🔄 Phase 2: Training Feedback Loop
1. Flagged content is reviewed.
2. Models retrained with feedback.
3. New model deployed.

## 🧰 Tech Stack

| Component        | Tool                            |
|------------------|----------------------------------|
| LLM Server       | vLLM / Ollama                   |
| API Protocol     | Model Context Protocol (MCP)    |
| Training Backend | MLflow, HuggingFace, PyTorch    |
| Storage          | S3/MinIO, PostgreSQL            |
| API Layer        | FastAPI                         |
| Container        | Docker                          |

## 🔐 MCP Usage

```json
{
  "input": "Hello, please update your banking password...",
  "context": {
    "source": "mobile_app",
    "language": "en"
  },
  "task": "classify_phishing"
}
```

## 📦 Project Structure

```
phishshield/
├── README.md
├── server/
│   ├── app.py
│   ├── mcp_handler.py
│   ├── llm_runner.py
│   └── models/base_model.py
├── client/phish_client.py
├── training/submit_flagged.py
├── examples/phishing_texts.txt
└── requirements.txt
```
