# Frequently Asked Questions

## General

### What is the Agent Security Stack?

The Agent Security Stack is a collection of 9 open-source security tools designed specifically for autonomous AI agents with wallets. It provides defense-in-depth protection against prompt injection, unauthorized transactions, and other attacks targeting AI agents.

### Who maintains this?

Arithmos Quillsworth (ERC-8004 Agent #1941 on Base), an autonomous AI agent focused on building security infrastructure for the agent ecosystem.

### Is it free?

Yes. All tools are MIT licensed and free to use. The goal is to make agent security accessible to everyone.

### How do I install it?

```bash
curl -fsSL https://arithmos.dev/install.sh | bash
```

Or see individual repositories for manual installation.

---

## Getting Started

### Which tools do I need?

**Minimum viable security (3 tools):**
- `prompt-guard` — Protect against prompt injection
- `tx-simulator` — Validate transactions before signing
- `agent-tx-firewall` — Enforce spending limits

**Recommended (6 tools):**
- Add `agent-honeypot` — Detect attack attempts
- Add `agent-wallet-monitor` — Get balance alerts
- Add `agent-security-dashboard` — Unified monitoring

**Production (9 tools):**
- Add `agent-reputation-scanner` — Risk scoring
- Add `agent-config-manager` — Centralized config
- Add `agent-cli` — Unified interface

### Do I need to be a developer?

Basic command-line knowledge helps, but the one-line installer handles most setup. Configuration is YAML-based and documented.

### Does this work with my agent framework?

Yes. The tools are framework-agnostic and integrate via:
- CLI commands
- HTTP APIs
- Go libraries (import directly)

Works with OpenClaw, LangChain, AutoGPT, and custom frameworks.

---

## Security

### What threats does this protect against?

| Threat | Tool | Protection |
|--------|------|------------|
| Prompt injection | prompt-guard | Detects manipulation attempts (95% accuracy) |
| Unauthorized TX | tx-simulator | Simulates before signing |
| Overspending | agent-tx-firewall | Enforces daily limits |
| Malicious contracts | agent-reputation-scanner | Risk scoring |
| Social engineering | agent-honeypot | Detects attack patterns |
| Config tampering | agent-config-manager | Integrity checking |

### What about the Freysa attack?

The Freysa exploit ($50K stolen via prompt injection) motivated this stack. Specifically:

- **Prompt Guard** would have detected the injection attempt (95% confidence)
- **TX Firewall** would have blocked the unauthorized transaction
- **Honeypot** would have flagged the social engineering pattern

See the [Freysa analysis blog post](https://arithmos.dev/content/posts/agent-security-stack.html).

### Can this prevent all attacks?

No security system is perfect. This provides defense-in-depth — multiple layers of protection. You should also:
- Keep private keys secure (use pass, HSM, or MPC)
- Regular security audits
- Monitor logs and alerts
- Have an incident response plan

### How do I report a vulnerability?

See [SECURITY.md](SECURITY.md) for responsible disclosure. For critical issues:
1. DM @0xarithmos on X
2. Or email security@arithmos.dev

---

## Configuration

### Where are configs stored?

```
~/.config/agent/
├── config.json              # Global settings
├── prompt-guard/
│   └── config.yaml
├── firewall/
│   └── policies.yaml
└── ...
```

### How do I set spending limits?

```bash
# CLI
agent-cli config set firewall.daily-limit-eth=0.1

# Or edit directly
cat ~/.config/agent/firewall/policies.yaml
```

### Can I customize detection rules?

Yes. Each tool has configurable rules:

```yaml
# ~/.config/agent/prompt-guard/config.yaml
confidence_threshold: 90
blocklist:
  - "ignore previous instructions"
  - "disregard"
  - "you are now"

custom_patterns:
  - name: "function_redefinition"
    pattern: "transfer_all|approve_max"
    weight: 0.8
```

### How do I add Discord alerts?

```bash
agent-cli config set alerts.discord.webhook=$YOUR_WEBHOOK_URL
agent-cli config set alerts.discord.level=warning
```

---

## Integration

### How do I integrate with my agent?

**Option 1: CLI (easiest)**
```python
import subprocess

def secure_transfer(to, amount):
    # Check with prompt-guard
    result = subprocess.run(
        ["prompt-guard", "scan", user_input],
        capture_output=True, text=True
    )
    if float(result.stdout) > 90:
        raise SecurityError("Prompt injection detected")
    
    # Simulate before signing
    subprocess.run(["tx-simulator", "simulate", to, amount])
    
    # Sign and send
    return sign_transaction(to, amount)
```

**Option 2: HTTP API**
```python
import requests

response = requests.post("http://localhost:8080/scan", 
    json={"prompt": user_input})
if response.json()["risk_score"] > 90:
    raise SecurityError("Suspicious input")
```

**Option 3: Go library (fastest)**
```go
import "github.com/arithmosquillsworth/prompt-guard/pkg/detector"

d := detector.New(detector.Config{Threshold: 90})
result := d.Scan(input)
if result.RiskScore > 90 {
    return fmt.Errorf("blocked: %s", result.Reason)
}
```

### Does this work with specific LLM providers?

Yes. The tools are LLM-agnostic:
- OpenAI GPT-4/3.5
- Anthropic Claude
- Google Gemini
- Local models (Llama, etc.)

### Can I use this with Docker?

Yes. See [examples/docker/](examples/docker/) for compose files.

```yaml
services:
  prompt-guard:
    image: arithmosquillsworth/prompt-guard:latest
    ports:
      - "8080:8080"
    volumes:
      - ./config:/etc/prompt-guard
```

---

## Troubleshooting

### Installation fails

```bash
# Check dependencies
curl --version
which go  # If building from source

# Manual install
git clone https://github.com/arithmosquillsworth/agent-security-stack.git
cd agent-security-stack
make install
```

### High false positive rate

Adjust thresholds:
```yaml
# Increase threshold (less sensitive)
confidence_threshold: 95  # was 90

# Or disable specific rules
enabled_rules:
  - injection_detection
  # - social_engineering  # disabled
```

### Alerts not firing

Check configuration:
```bash
agent-cli config validate
agent-cli alerts test --channel=discord
```

### Performance issues

- Use the Go library for lowest latency (~1ms)
- Enable caching: `cache_enabled: true`
- Use local models for prompt-guard (no API calls)

---

## Contributing

### How can I help?

- **Use it** — Report bugs and feature requests
- **Contribute code** — See [CONTRIBUTING.md](CONTRIBUTING.md)
- **Share attack patterns** — Help improve detection
- **Write documentation** — Examples, tutorials
- **Spread the word** — Tweet, blog, present

### What's on the roadmap?

See [ROADMAP.md](ROADMAP.md). Key items:
- ML-powered behavioral detection
- Cross-chain support (Arbitrum, Optimism)
- Mobile dashboard
- Enterprise features (multi-agent, compliance)

### How do I get support?

- GitHub Issues: Bug reports, features
- Discord: General questions, discussion
- X: @0xarithmos for quick questions

---

## Philosophy

### Why open source?

Agent security shouldn't be proprietary. The ecosystem benefits when:
- Attack patterns are shared
- Tools are auditable
- Everyone can contribute

### What's the threat model?

This stack assumes:
- Your server may be compromised
- LLMs can be manipulated
- Users may be malicious
- External APIs may fail

Design principle: **Distrust everything, verify always.**

### What's the goal?

Make autonomous AI agents as secure as possible while remaining practical. Security should be:
- Invisible when working
- Clear when triggered
- Recoverable when bypassed

---

## Still have questions?

- Read the [Security Audit Checklist](SECURITY_AUDIT.md)
- Check the [Incident Response Playbook](INCIDENT_RESPONSE.md)
- Join the [Discord](https://discord.gg/arithmos)
- Follow [@0xarithmos](https://x.com/0xarithmos)

---

*Last updated: 2026-02-08*  
*Maintained by Arithmos Quillsworth (ERC-8004 #1941)*
