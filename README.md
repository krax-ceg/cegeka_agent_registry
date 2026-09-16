# Cegeka Agent Registry

Version-controlled registry of autonomous AI agent personas, roles, cryptographic identity policies, and skill authorizations used internally at Cegeka.

## Agent Role Directory

| Role | Primary Purpose | Default Lifecycle Stages | Authorized Skills |
|---|---|---|---|
| **kyb-investigator** | `corporate_due_diligence` | `DISCOVERY`, `EXECUTION` | `kyb-verification`, `web-search`, `web-crawl`, `okf` |
| **researcher** | `customer_enrichment` | `DISCOVERY`, `EXECUTION` | `web-search`, `web-crawl`, `okf` |
| **analyst** | `stakeholder_enrichment` | `DISCOVERY`, `EXECUTION` | `web-search`, `okf` |
| **sales-agent** | `transcript_extraction` | `EXTRACTION`, `EXECUTION` | `okf` |
| **developer** | `task_execution` | `INIT`, `EXECUTION` | `goose-doc-guide`, `web-search` |
| **auditor** | `custody_verification` | `VERIFY`, `AUDIT` | `goose-doc-guide` |

## Cryptographic Identity & Chain of Custody

All agents registered and spawned through the Cegeka Agent Platform (`services/agent-service`):
1. **Generate an Ed25519 Keypair**: Ephemeral or persistent cryptographic key generated at creation.
2. **Sign Genesis Certificates**: Cryptographically binds the agent's ID, role, purpose, stage, and public key.
3. **Maintain Chain-of-Custody**: Every prompt turn, task handover, and model inference generates a SHA-256 Merkle hash-chained record signed by the agent's private key.
4. **Audit Verification**: Every event is independently verifiable via `GET /api/v1/agents/{id}/custody/verify`.

## Microservice Integration

This registry serves as the authoritative source of truth for:
- **`services/agent-service`**: Enforces authorization policies, role definitions, and skill permissions.
- **`services/sales-intelligence`**: Coordinates `sales-agent`, `researcher`, and `kyb-investigator` personas to ingest call transcripts and publish OKF 0.2 knowledge bundles.
