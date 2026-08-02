---
name: cloud-simulation
title: "Cloud World Model API"
description: "Multi-cloud infrastructure simulation with RL, chaos, and AI analysis endpoints; x402 pay-per-call on Solana mainnet USDC and Base, no signup required."
use_case: |
  Call this API when an agent needs to train autoscaling RL policies across AWS, GCP,
  Azure, OCI, or DigitalOcean without provisioning real cloud resources; when running
  chaos scenarios (instance kills, AZ outages, database overloads, network partitions)
  and scoring resilience; when comparing equivalent multi-cloud architectures on cost
  and latency; or when requesting AI-generated architecture analysis, optimization
  recommendations, or bottleneck diagnostics. All calls are pay-per-use — no account
  or API key required; the agent pays in USDC on Solana mainnet or Base via x402.
category: devtools
service_url: https://www.cloudworldmodel.ai
openapi:
  path: openapi.json
---

# Cloud World Model API

Cloud World Model is a multi-cloud infrastructure simulation platform. Developers,
architects, and AI agents can model AWS, GCP, Azure, OCI, and DigitalOcean deployments
without provisioning real resources or incurring cloud bills.

## What agents can do

- **RL training** — step through simulated cloud environments with a Gym-compatible
  observe → act → reward loop (`rl_step`, `rl_batch_step`, `rl_eval_episodes`).
- **Chaos engineering** — inject failures (instance kill, AZ outage, database crash,
  network latency, CPU stress) and receive resilience scores (`chaos_run`, `chaos_batch`).
- **Multi-cloud cost optimization** — compare equivalent workloads across all five
  providers in a single call (`multicloud_explore`).
- **Predictive scaling validation** — validate autoscaling thresholds against traffic
  forecasts (`prediction_validate`, `prediction_optimize_thresholds`).
- **AI architecture analysis** — request natural-language explanations, optimization
  plans, troubleshooting guides, and bottleneck analyses (`ai_explain`, `ai_optimize`,
  `ai_troubleshoot`, `ai_bottleneck`).
- **Hybrid simulation** — run the blended deterministic + ML simulation engine
  (`simulation_step_hybrid`).

## x402 payment flow

Every metered endpoint returns `HTTP 402` with a JSON challenge body when no payment
header is present. The challenge includes two `accepts` entries — one for Base mainnet
USDC (`eip155:8453`) and one for Solana mainnet USDC
(`solana:5eykt4UsFv8P8NJdTREpY1vzqKqZKvdp`). The agent signs a payment, attaches it
as `X-PAYMENT` (Base) or `PAYMENT-SIGNATURE` (Solana), and retries. The facilitator
at `https://facilitator.payai.network` settles both chains.

Fetch live prices before your first call:

```bash
curl https://www.cloudworldmodel.ai/api/billing/x402/config
```

## Pricing

| Call type | USDC (Solana / Base) |
|---|---|
| `rl_step`, `rl_batch_step`, `rl_eval_episodes` | $0.0010 |
| `simulation_step_hybrid` | $0.0010 |
| `ai_explain`, `ai_optimize`, `ai_troubleshoot`, `ai_bottleneck` | $0.0010 |
| `chaos_run`, `chaos_batch` | $0.0050 |
| `multicloud_explore` | $0.0050 |
| `prediction_validate`, `prediction_optimize_thresholds` | $0.0050 |

USDC uses 6 decimal places on both chains; 1 credit = 1 000 atomic units = $0.001.

## Spend-aware usage notes

- Fetch `/api/billing/x402/config` once per session to read the live `payTo` wallet
  address and exact atomic-unit amounts per call type — do not hardcode them.
- Prefer `rl_batch_step` (1 credit for up to 30 steps) over looping `rl_step` to
  reduce round-trips.
- High-cost calls (`chaos_run`, `multicloud_explore`) each cost 5× a step call; use
  them after a warm-up phase to avoid wasting budget on uninitialised environments.
- The `simulation_step_hybrid` endpoint auto-claims a wallet-owned simulation on the
  first call; the simulation is retained for 90 rolling days without an explicit
  `POST /api/simulations`.

## Discovery & compliance

| Resource | URL |
|---|---|
| Discovery endpoint | `GET https://www.cloudworldmodel.ai/api/billing/x402/config` |
| x402scan listing | https://www.x402scan.com/server/1a8052f0-c764-420a-96a5-f50d3c696795 |
| RL environments reference | https://www.cloudworldmodel.ai/rl/environments |
| x402 blog post | https://www.cloudworldmodel.ai/blog/x402-cloud-simulation-api |
| Facilitator | https://facilitator.payai.network |
