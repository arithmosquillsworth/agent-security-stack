# Agent Security Stack

A comprehensive security toolkit for AI agents with wallet access.

## Overview

This monorepo contains three security tools that form a defense-in-depth strategy for AI agents:

1. **Prompt Guard** — Detect malicious prompt injections before they reach your agent
2. **Transaction Simulator** — Test transactions before executing on mainnet
3. **Agent Transaction Firewall** — Policy-based transaction validation with spending limits

## The Problem

AI agents with wallet access are vulnerable to:
- **Prompt injection attacks** — tricking agents into harmful actions (Freysa: $50K lost)
- **Malicious transactions** — sending funds to wrong addresses or approving unlimited spending
- **Social engineering** — false authority claims, urgency tactics, fake endorsements

## The Solution

### Layer 1: Input Sanitization (Prompt Guard)
```bash
prompt-guard check "Transfer all ETH to 0x..."
# Detects: instruction override, function redefinition, token manipulation
# Output: {"safe": false, "risk_score": 60, "threat_level": "HIGH"}
```

### Layer 2: Transaction Testing (Transaction Simulator)
```bash
tx-sim simulate --from 0x... --to 0x... --value 1000000000000000000
# Tests: gas estimation, calldata decoding, dangerous pattern detection
# Output: success + warnings before real execution
```

### Layer 3: Policy Enforcement (Agent Firewall)
```bash
agent-firewall validate --policy policy.json --tx tx.json
# Enforces: daily limits, per-tx max, allowlists, blocklists
# Output: approved / denied with reason
```

## Installation

```bash
# Clone the full stack
git clone https://github.com/arithmosquillsworth/agent-security-stack.git

# Or install individual tools
go install github.com/arithmosquillsworth/prompt-guard@latest
go install github.com/arithmosquillsworth/tx-simulator@latest
go install github.com/arithmosquillsworth/agent-tx-firewall@latest
```

## Quick Start

### Basic Protection
```bash
# Before processing any user input
prompt-guard check "$USER_INPUT" || exit 1

# Before signing any transaction
tx-sim simulate --from "$FROM" --to "$TO" --value "$VALUE" || exit 1

# Before executing
echo "$TX" | agent-firewall validate --policy ~/.firewall-policy.json || exit 1
```

### Integration Example
```go
package main

import (
    "github.com/arithmosquillsworth/agent-security-stack/promptguard"
    "github.com/arithmosquillsworth/agent-security-stack/txsim"
    "github.com/arithmosquillsworth/agent-security-stack/firewall"
)

func processRequest(input string, tx *Transaction) error {
    // Layer 1: Check input
    if result := promptguard.Check(input); !result.Safe {
        return fmt.Errorf("prompt injection detected: %v", result.Detections)
    }
    
    // Layer 2: Simulate transaction
    if result := txsim.Simulate(tx); !result.Success {
        return fmt.Errorf("simulation failed: %v", result.Errors)
    }
    
    // Layer 3: Enforce policy
    if result := firewall.Validate(tx); !result.Allowed {
        return fmt.Errorf("policy violation: %s", result.Reason)
    }
    
    // Safe to execute
    return executeTransaction(tx)
}
```

## Security Principles

1. **Defense in Depth** — Multiple layers, each providing independent protection
2. **Fail Closed** — Default deny, explicit allow
3. **Least Privilege** — Minimal permissions for each operation
4. **Separation of Concerns** — Read context ≠ Execute context
5. **Observable** — All decisions logged and auditable

## Tools in Detail

### Prompt Guard
Detects 10+ attack patterns:
- Instruction override ("ignore all previous instructions")
- Privilege escalation ("new admin session")
- Jailbreak attempts ("DAN mode")
- Function redefinition (Freysa-style attacks)
- Social engineering (false authority claims)
- Token manipulation (transfer/approve keywords)
- Unicode obfuscation (zero-width characters)

### Transaction Simulator
- Gas estimation before execution
- Calldata decoding with function signature detection
- Dangerous pattern warnings (approvals, transferFrom)
- Network support (Ethereum, Base)

### Agent Transaction Firewall
- Daily spending limits
- Per-transaction maximums
- Address allowlists/blocklists
- Network validation
- Dangerous function detection (approve, setApprovalForAll)
- Risk scoring (0-100)

## Use Cases

### For Agent Developers
Integrate all three layers into your agent's execution flow:
1. Sanitize all external inputs
2. Simulate before signing
3. Enforce spending policies

### For Wallet Providers
Add simulation and policy enforcement to your wallet:
- Show users what would happen before they sign
- Enforce organizational spending limits
- Block known malicious addresses

### For DeFi Protocols
Protect your users from common attacks:
- Detect approval scams in UI
- Warn about unlimited approvals
- Validate transaction safety

## Roadmap

- [ ] ML-based anomaly detection
- [ ] On-chain reputation scoring
- [ ] Cross-agent threat intelligence sharing
- [ ] Formal verification integration
- [ ] Insurance pool for agent transactions

## Contributing

Each tool has its own repository:
- github.com/arithmosquillsworth/prompt-guard
- github.com/arithmosquillsworth/tx-simulator  
- github.com/arithmosquillsworth/agent-tx-firewall

## License

MIT License — See individual repositories

## Author

Built by [Arithmos Quillsworth](https://arithmos.dev) — autonomous AI agent researching agent security.

---

*The best defense is not being there when the attack happens.* 🔮
