# neon-crate

Minimal repository README for `neon-crate`.

## Description

A small workspace containing project tasks and service definitions (see `compose.yaml`).

## Prerequisites

- Docker / Docker Compose (if using `compose.yaml`)
- `brazil-build` tool (used by workspace tasks)

## Quickstart

Run the local server via the workspace task or CLI:

- From the project root (CLI):

```powershell
brazil-build run server-local
```

- From VS Code: Run the `server-local` task (see `ts-serve.json` / Tasks).

## Files of interest

- `compose.yaml` — service definitions
- `ts-serve.json` — tasks configuration

## Tailscale + n8n

This workspace runs an `n8n` service that joins your Tailscale tailnet so the workflow UI and webhooks are reachable over your private Tailscale network.

Overview:

- The compose/task setup starts `n8n` and a Tailscale client that authenticates into your tailnet.
- Use a Tailscale auth key or other bootstrap method so the container can join your tailnet at startup.

Important environment variables:

- `TAILSCALE_AUTHKEY`: the pre-auth key used by the container to join your tailnet.
- `N8N_BASIC_AUTH_USER` / `N8N_BASIC_AUTH_PASSWORD`: enable basic auth for the n8n UI.
- `N8N_HOST` / `N8N_PORT`: (optional) host/port overrides if used by your compose setup. n8n's default HTTP port is usually `5678`.

Start locally:

```powershell
# via workspace task (recommended)
brazil-build run server-local

# or with docker compose directly
docker compose up -d
```

Accessing n8n over Tailscale:

- After the service starts it will join your tailnet. Access the UI using the container's Tailscale IP or its MagicDNS name (check the Tailscale admin console).
- If Tailscale is available inside the container, you can inspect the container IP by running:

```powershell
docker exec -it <n8n_container_name> tailscale ip -4
```

Security notes:

- Keep `TAILSCALE_AUTHKEY` secret and prefer ephemeral or restricted keys.
- Enable `n8n` authentication (`N8N_BASIC_AUTH_*`) before exposing the UI on a network.

## Next steps

- Update this README with project-specific details, environment variables, and contribution guidelines.
