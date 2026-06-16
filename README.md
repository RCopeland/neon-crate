# neon-crate

Compact workspace for running an `n8n` instance joined to a Tailscale tailnet.

Overview

- Services are defined in `compose.yaml` and include a Tailscale client plus `n8n`.
- A small tasks configuration (`ts-serve.json`) is provided for local dev convenience.

Prerequisites

- Docker & Docker Compose (to run the services directly)

Quickstart

1. From the project root (recommended via Docker Compose):

```powershell
docker compose up -d
```

2. To stop and remove containers:

```powershell
docker compose down
```

Files

- `compose.yaml` — service definitions (Tailscale + n8n)
- `ts-serve.json` — Tailscale serve configuration used by the Tailscale container

Tailscale + n8n notes

- The compose setup runs a Tailscale client (`tailscale`) and the `n8n` service in the same network namespace. The Tailscale client lets the `n8n` UI/webhooks be reachable over your tailnet.
- Required environment variables (set in your environment or in an env file):
  - `TAILSCALE_AUTH_KEY` (or `TAILSCALE_AUTHKEY`): Tailscale pre-auth key used to join the tailnet.
  - `N8N_WEBHOOK_URL`: optional webhook base URL if you need it set explicitly.
  - `N8N_BASIC_AUTH_USER` / `N8N_BASIC_AUTH_PASSWORD`: enable basic auth for the n8n UI.

How to inspect container Tailscale IP

```powershell
docker exec -it n8n-tailscale tailscale ip -4
```

Security

- Keep Tailscale auth keys secret and prefer ephemeral or scoped keys.
- Protect the `n8n` UI (enable basic auth) before exposing it to any network.

Contributing

- Update this README with project-specific environment variables, developer notes, or CI instructions as needed.
