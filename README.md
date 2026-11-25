# Voice-Based Transaction Verification System

> **A real-time voice verification agent for confirming sensitive financial transactions.**

## Overview

The **Voice-Based Transaction Verification System** is an automated AI agent designed to operate over telephony to verify user identity and approve or cancel high-risk or sensitive transactions. It integrates multi-factor identity checks with real-time API polling, handling fraud prevention operations seamlessly during a standard phone call.

## Features

- **Multi-Factor Verification:** Validates users through multiple steps like phone number identification and specialized pass-phrases to prevent fraudulent activity.
- **Dynamic Liveness Checks:** Uses randomly generated, short phrases to thwart recorded voice replays.
- **Behavioral and Anomaly Detection Architecture:** Integrated risk-check scoring that can optionally pause payments and escalate calls to human specialists upon abnormal spikes.
- **Read-Back Functionality:** Dynamically retrieves exact payment amounts, payees, and transaction states before accepting an approval or cancellation.
- **Idempotency & Fraud Routing:** Detects if transactions have already been settled. Routes to an appropriate human agent if fraud flags are triggered.

## Architecture & Components

- **Voice Bridge (`main.py`):** WebSocket server that streams media.
- **Orchestration (`voice_verifier/graph.py`):** Utilizes LangGraph for reliable conversational state tracking and orchestrates the user interaction flow.
- **Agent Sandbox (`api/bank_sandbox.py`):** Provides the mocking layer to simulate a banking API.
- **Audit Logs (`voice_verifier/storage.py`):** Event and dialog logging utilizing SQLite for comprehensive historical auditing.

## Local Setup

### 0) Prerequisites

Ensure you have **Python 3.13+** installed on your system.
You will also need valid access keys for Open AI and Deepgram.

### 1) Configuration

Copy the example environment into your active configuration:

```bash
cp .env.example .env
```
Ensure you provide `DEEPGRAM_API_KEY` and `OPENAI_API_KEY`.

### 2) Run the API Server

Start the simulated external API layer:

```bash
python -m uvicorn api.bank_sandbox:app --host 0.0.0.0 --port 8000 --reload
```

### 3) Start the Agent Service

Run the voice bridge listener:

```bash
python main.py
```

### 4) Expose Telephony Service

You can leverage any standard media-stream gateway (such as Twilio) targeting your `main.py` WebSocket. Simply establish a local ingress tunnel or test via WebSocket directly.

## Tests

Execute the automated test suite through `pytest`:

```bash
pytest -q
```

Coverage natively tests:
- Model logic boundaries
- Conversational orchestration paths
- Data persistence mechanisms

## License

MIT
