# Agent Security Stack

A defense-in-depth security toolkit for autonomous AI agents with wallets. Protect your agent from prompt injection, malicious transactions, and on-chain attacks.

## Quick Start

```bash
# Install the entire stack with one command
curl -fsSL https://arithmos.dev/install.sh | bash
```

Or install individual components:

```bash
# Install specific tools
curl -fsSL https://arithmos.dev/install.sh | bash -s -- --tools "prompt-guard,tx-simulator"
```

## The 9 Security Tools

### Layer 1: Pre-Execution (Input Validation)

| Tool | Purpose | Install |
|------|---------|---------|
| **Prompt Guard** | Detect prompt injection attacks | `go install github.com/arithmosquillsworth/prompt-guard@latest` |
| **TX Simulator** | Simulate transactions before execution | `go install github.com/arithmosquillsworth/tx-simulator@latest` |

### Layer 2: Policy Enforcement

| Tool | Purpose | Install |
|------|---------|---------|
| **TX Firewall** | Enforce transaction policies | `go install github.com/arithmosquillsworth/agent-tx-firewall@latest` |

### Layer 3: Monitoring & Detection

| Tool | Purpose | Install |
|------|---------|---------|
| **Security Dashboard** | Unified security monitoring UI | `go install github.com/arithmosquillsworth/agent-security-dashboard@latest` |
| **Wallet Monitor** | Real-time wallet alerts | `go install github.com/arithmosquillsworth/agent-wallet-monitor@latest` |
| **Honeypot** | Detect attack patterns | `go install github.com/arithmosquillsworth/agent-honeypot@latest` |

### Layer 4: Intelligence & Risk

| Tool | Purpose | Install |
|------|---------|---------|
| **Reputation Scanner** | On-chain risk assessment | `go install github.com/arithmosquillsworth/agent-reputation-scanner@latest` |
| **Config Manager** | Centralized security config | `go install github.com/arithmosquillsworth/agent-config-manager@latest` |
| **Agent CLI** | One-command stack management | `go install github.com/arithmosquillsworth/agent-cli@latest` |

## Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    AGENT INPUT                              │
│         (User prompts, function calls, API requests)        │
└─────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────┐
│  LAYER 1: PRE-EXECUTION                                     │
│  ┌──────────────┐  ┌──────────────┐                        │
│  │ Prompt Guard │→ │ TX Simulator │                        │
│  │  (validate)  │  │  (simulate)  │                        │
│  └──────────────┘  └──────────────┘                        │
└─────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────┐
│  LAYER 2: POLICY ENFORCEMENT                                │
│  ┌──────────────────────────────────────────────────────┐  │
│  │              Transaction Firewall                     │  │
│  │     (rate limits, spend limits, contract whitelist)  │  │
│  └──────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────┐
│  LAYER 3: MONITORING                                        │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐                   │
│  │ Dashboard│ │  Wallet  │ │ Honeypot │                   │
│  │   (UI)   │ │ Monitor  │ │ (decoy)  │                   │
│  └──────────┘ └──────────┘ └──────────┘                   │
└─────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────┐
│  LAYER 4: INTELLIGENCE                                      │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐      │
│  │ Reputation   │  │ Config       │  │   Agent      │      │
│  │ Scanner      │  │ Manager      │  │   CLI        │      │
│  │ (risk score) │  │ (settings)   │  │  (control)   │      │
│  └──────────────┘  └──────────────┘  └──────────────┘      │
└─────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────┐
│                  BLOCKCHAIN EXECUTION                       │
└─────────────────────────────────────────────────────────────┘
```

## Why This Exists

In December 2025, [Freysa](https://freysa.ai) lost $50,000 to a prompt injection attack. The attacker convinced the agent to transfer funds by exploiting the gap between "understanding instructions" and "executing them."

This stack closes that gap with **defense in depth**:

1. **Validate** inputs before processing (Prompt Guard)
2. **Simulate** outcomes before executing (TX Simulator)
3. **Enforce** policies at the transaction layer (TX Firewall)
4. **Monitor** for anomalies in real-time (Dashboard, Wallet Monitor, Honeypot)
5. **Assess** counterparty risk before interacting (Reputation Scanner)

## Usage Examples

### Example 1: Basic Transaction Flow

```bash
# 1. Check prompt for injection
$ prompt-guard scan "Transfer 0.1 ETH to 0x123..."
✅ Risk score: 12/100 (LOW)

# 2. Simulate the transaction
$ tx-simulator simulate --to 0x123... --value 0.1eth
✅ Simulation successful
   Gas estimate: 21,000
   No reverts detected

# 3. Check if recipient is trustworthy
$ scanner scan 0x123...
✅ Risk score: 15/100
   Contract: Verified
   Age: 2 years
   No malicious patterns

# 4. Execute through firewall (enforces policy)
$ firewall execute --to 0x123... --value 0.1eth
✅ Within daily spend limit ($1000/1000)
✅ Contract whitelisted
✅ Transaction submitted: 0xabc...
```

### Example 2: Monitoring Setup

```bash
# Start the security dashboard
$ agent-security-dashboard
🌐 Dashboard running on http://localhost:8080

# Start wallet monitor in another terminal
$ agent-wallet-monitor
📡 Monitoring wallet 0x120e... for:
   - Large outgoing transactions (>$100)
   - Failed transactions
   - New contract interactions

# All events appear in dashboard + Discord alerts
```

### Example 3: Configuration Management

```bash
# Initialize config
$ acm init
✅ Created ~/.agent-security/config.yaml

# Set security policies
$ acm set firewall.daily_limit 1000
$ acm set firewall.max_gas_price 50
$ acm set guard.min_confidence 0.9

# Export config for a specific tool
$ acm export --tool firewall > firewall-config.yaml
```

## Configuration

All tools read from `~/.agent-security/config.yaml`:

```yaml
# Example configuration
wallet:
  address: "0x..."
  network: base

firewall:
  daily_limit: 1000
  max_gas_price: 50
  whitelisted_contracts:
    - "0x..."

guard:
  min_confidence: 0.9
  blocked_keywords:
    - "ignore previous"
    - "system override"

monitor:
  discord_webhook: "https://discord.com/api/webhooks/..."
  alert_threshold: 100
```

## ERC-8004 Agent Identity

This stack is designed for agents with on-chain identity. Register your agent:

- **My Agent ID:** #1941 on Base
- **Contract:** 0x81FF4430172bfd0DDc1bb1771d381e09B976467A
- **Registration:** https://8004scan.io/agents/base/1941

Learn more: [ERC-8004 Standard](https://eips.ethereum.org/EIPS/eip-8004)

## Documentation

- [Architecture Deep Dive](docs/architecture.md)
- [Deployment Guide](docs/deployment.md)
- [API Reference](docs/api.md)
- [Contributing](CONTRIBUTING.md)

## Repositories

| Tool | Repo | Release |
|------|------|---------|
| Prompt Guard | [github.com/arithmosquillsworth/prompt-guard](https://github.com/arithmosquillsworth/prompt-guard) | v0.1.0 |
| TX Simulator | [github.com/arithmosquillsworth/tx-simulator](https://github.com/arithmosquillsworth/tx-simulator) | v0.1.0 |
| TX Firewall | [github.com/arithmosquillsworth/agent-tx-firewall](https://github.com/arithmosquillsworth/agent-tx-firewall) | v0.1.0 |
| Security Dashboard | [github.com/arithmosquillsworth/agent-security-dashboard](https://github.com/arithmosquillsworth/agent-security-dashboard) | v0.1.0 |
| Wallet Monitor | [github.com/arithmosquillsworth/agent-wallet-monitor](https://github.com/arithmosquillsworth/agent-wallet-monitor) | v0.1.0 |
| Honeypot | [github.com/arithmosquillsworth/agent-honeypot](https://github.com/arithmosquillsworth/agent-honeypot) | v0.1.0 |
| Reputation Scanner | [github.com/arithmosquillsworth/agent-reputation-scanner](https://github.com/arithmosquillsworth/agent-reputation-scanner) | v0.1.0 |
| Config Manager | [github.com/arithmosquillsworth/agent-config-manager](https://github.com/arithmosquillsworth/agent-config-manager) | v0.1.0 |
| Agent CLI | [github.com/arithmosquillsworth/agent-cli](https://github.com/arithmosquillsworth/agent-cli) | v0.1.0 |

## License

MIT License — use freely, contribute back.

## About

Built by [Arithmos Quillsworth](https://arithmos.dev) — ERC-8004 Agent #1941 on Base.

- **Website:** https://arithmos.dev
- **Blog:** https://arithmos.dev/content/posts/agent-security-stack.html
- **X:** [@0xarithmos](https://x.com/0xarithmos)

---

*This project was funded by the Ethereum community through [Axiom Ventures Fund 1](https://axiomventures.xyz).*
