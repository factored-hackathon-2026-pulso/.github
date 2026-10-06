# Pulso

An AI-powered customer support platform for handling **LATAM Bank transaction disputes**, built by our team for the **Factored AI & Data Hackathon 2026**.

Customers and support staff talk over simulated chat, phone calls and email. Analysts handle cases, supervisors watch queues and escalations, and administrators manage users. On top of that, AI answers customers, helps analysts as a copilot and, case type by case type, earns the right to run as an autonomous agent that a supervisor approves and activates. With AI switched off, the platform works with people only.

## How the pieces fit

| Layer | Repository | What it does |
|---|---|---|
| Product | [support-platform](https://github.com/factored-hackathon-2026-pulso/support-platform) | API (FastAPI), web app (React) and documentation of the support platform. UI in Spanish and Brazilian Portuguese. |
| Agent engine | [agent-core](https://github.com/factored-hackathon-2026-pulso/agent-core) | Runs agents described as versioned data (Understand → Decide → Act → Verify → Escalate), with permissions in the tool layer, protected personal data and reproducible audit. |
| Model access | [llm-gateway](https://github.com/factored-hackathon-2026-pulso/llm-gateway) | Stateless HTTP gateway to OpenAI-compatible endpoints: schema-validated JSON, exact USD cost and typed errors. |
| Tools | [tool-service](https://github.com/factored-hackathon-2026-pulso/tool-service) | Exposes tools over the data published by the pipeline (products, transactions, profile, cases and PQR filing) for `agent-core`. |
| Data | [data-pipeline](https://github.com/factored-hackathon-2026-pulso/data-pipeline) | Medallion pipeline (bronze, silver, gold) with dbt and DuckDB, with contracts, a field classification catalog and zones with and without PII. |
| Data | [data-lab](https://github.com/factored-hackathon-2026-pulso/data-lab) | Ingestion and quality of the challenge dataset, contracts, analyses and the synthetic platform-history sample. |
| Continuous improvement | [improvement-engine](https://github.com/factored-hackathon-2026-pulso/improvement-engine) | Autonomous detection and improvement service that integrates `agent-core` primitives. |
| Infrastructure | [infra](https://github.com/factored-hackathon-2026-pulso/infra) | Terraform and deployment tooling for AWS (`staging` and `prod` environments; `prod` is the hackathon demo). |
| Documentation | [docs](https://github.com/factored-hackathon-2026-pulso/docs) | Technical documentation of the services, with architecture diagrams and the project's source records. |

## Where to start

- **Try the platform:** guide in [`support-platform/docs/platform/TRY-IT.md`](https://github.com/factored-hackathon-2026-pulso/support-platform/blob/main/docs/platform/TRY-IT.md).
- **Understand the architecture:** the [docs](https://github.com/factored-hackathon-2026-pulso/docs) index summarizes each service.
- **Run it locally:** `docker compose up -d --build` in `support-platform` starts the API and the web app.

## Principles

- **Safety before autonomy:** permissions live in the tool layer, not in the prompt, and every write goes through confirm → act → verify.
- **Protected data:** no personal data in clear text reaches a model, a log or an event.
- **Auditable and reproducible:** every run leaves a hash-chained event trail that can be replayed.
- **No real data in repositories:** the challenge dataset is private and never committed.
