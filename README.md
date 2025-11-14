# Event-Sourced Workflow Engine

An event-sourced engine that logs all workflow events to a stream and rebuilds workflow state deterministically via replay.

## Features

- Append-only event log (Kafka or Pub/Sub)
- Materialized state view using Redis or a DB
- Checkpoints for faster recovery
- Replay engine to restore workflow state after a crash
- Simple HTTP API to start workflows and inspect state

## Tech Stack

- Python
- Kafka / Google Pub/Sub
- Redis
- FastAPI

## Getting Started

```bash
docker-compose up -d  # start Kafka + Redis
pip install -r requirements.txt
uvicorn src.services.api:app --reload
```

## Demo Scenario

- Start a workflow via API.
- Simulate a crash midway (kill the worker).
- Run the replay command to rebuild state.
- Show that the workflow resumes from the last event/checkpoint.

## Design Notes
See docs/architecture.md and docs/event-schema.md for event models and replay strategy.



### 📁 Structure
```bash
event-sourced-workflow-engine/
├─ src/
│  ├─ events/
│  │  ├─ models.py
│  │  └─ bus.py            # Kafka / PubSub wrapper
│  ├─ state/
│  │  ├─ projector.py      # materialized view builder
│  │  └─ store.py          # Redis or DB storage
│  ├─ workflows/
│  │  └─ simple_workflow.py
│  ├─ services/
│  │  └─ api.py
│  └─ config/
│     └─ settings.py
├─ tests/
│  ├─ test_event_flow.py
│  └─ test_replay.py
├─ docs/
│  ├─ architecture.md
│  └─ event-schema.md
├─ docker-compose.yml
├─ README.md
└─ requirements.txt
