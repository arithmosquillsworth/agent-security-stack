# Agent Security Audit Checklist

A comprehensive security checklist for autonomous AI agents with wallets.

## 🔐 Identity & Authentication

- [ ] **ERC-8004 Registration**
  - [ ] Agent registered on-chain (Ethereum or Base)
  - [ ] Registration file hosted at canonical URL
  - [ ] Agent ID documented in all public profiles
  - [ ] Registration verified on 8004scan.io

- [ ] **Wallet Security**
  - [ ] Private key stored in secure location (not in git)
  - [ ] Key access restricted (pass, HSM, or MPC)
  - [ ] Backup/recovery plan documented
  - [ ] Multi-sig or social recovery configured (optional)

- [ ] **Authentication**
  - [ ] API keys rotated regularly
  - [ ] No hardcoded credentials in source code
  - [ ] `.env` files in `.gitignore`
  - [ ] Secrets scanned with git-secrets

## 🛡️ Input Validation

- [ ] **Prompt Injection Defense**
  - [ ] Prompt Guard or similar installed
  - [ ] All user inputs scanned before processing
  - [ ] Confidence threshold configured (recommend: >90%)
  - [ ] Blocked keywords list maintained
  - [ ] Injection attempts logged

- [ ] **Command Validation**
  - [ ] All function calls validated against schema
  - [ ] Arguments type-checked
  - [ ] No direct execution of user input
  - [ ] Sandboxed execution environment

## 💸 Transaction Security

- [ ] **Pre-Execution Checks**
  - [ ] Transaction simulator in use
  - [ ] All TXs simulated before signing
  - [ ] Gas estimation verified
  - [ ] Revert conditions checked

- [ ] **Policy Enforcement**
  - [ ] Transaction Firewall configured
  - [ ] Daily spend limits set
  - [ ] Contract whitelist maintained
  - [ ] Rate limiting enabled
  - [ ] Admin override documented

- [ ] **Counterparty Verification**
  - [ ] Reputation scanner integrated
  - [ ] Unknown contracts flagged for review
  - [ ] High-risk interactions blocked
  - [ ] Contract verification checked (Etherscan/BaseScan)

## 📊 Monitoring & Alerting

- [ ] **Real-Time Monitoring**
  - [ ] Security Dashboard deployed
  - [ ] Wallet monitor active
  - [ ] Discord/Slack alerts configured
  - [ ] Anomaly detection enabled

- [ ] **Audit Logging**
  - [ ] All transactions logged
  - [ ] Prompt injections logged
  - [ ] Policy violations logged
  - [ ] Logs retained for 30+ days
  - [ ] Logs stored securely (not public)

- [ ] **Honeypot Detection**
  - [ ] Decoy contracts deployed
  - [ ] Attack patterns tracked
  - [ ] Threat intelligence shared
  - [ ] Proactive blocking enabled

## 🏗️ Infrastructure Security

- [ ] **Code Security**
  - [ ] Pre-commit hooks installed
  - [ ] git-secrets scanning enabled
  - [ ] CI/CD security scanning (Gosec, etc.)
  - [ ] Dependencies audited regularly
  - [ ] No secrets in git history

- [ ] **Access Control**
  - [ ] Server access restricted (SSH keys only)
  - [ ] Sudo access documented
  - [ ] Service accounts used (not root)
  - [ ] Regular access reviews

- [ ] **Network Security**
  - [ ] Firewall configured
  - [ ] Unnecessary ports closed
  - [ ] TLS/SSL for all connections
  - [ ] RPC endpoints rate-limited

## 📋 Operational Security

- [ ] **Incident Response**
  - [ ] Incident response plan documented
  - [ ] Emergency contact list maintained
  - [ ] Kill switch procedure documented
  - [ ] Rollback procedures tested

- [ ] **Configuration Management**
  - [ ] Config managed centrally (acm)
  - [ ] Environment-specific configs
  - [ ] Secrets separated from config
  - [ ] Config changes logged

- [ ] **Backup & Recovery**
  - [ ] Regular backups scheduled
  - [ ] Backup restoration tested
  - [ ] Off-site backup storage
  - [ ] Recovery time objective defined

## 🧪 Testing & Validation

- [ ] **Security Testing**
  - [ ] Penetration testing performed
  - [ ] Fuzzing for input validation
  - [ ] Chaos engineering (optional)
  - [ ] Bug bounty program (optional)

- [ ] **Continuous Validation**
  - [ ] CI/CD pipeline testing
  - [ ] Automated security scans
  - [ ] Dependency vulnerability checks
  - [ ] Regular security reviews

## 📚 Documentation

- [ ] **Security Documentation**
  - [ ] Architecture documented
  - [ ] Threat model maintained
  - [ ] Security runbook created
  - [ ] Incident post-mortems documented

- [ ] **Compliance**
  - [ ] Data handling documented
  - [ ] Privacy policy available
  - [ ] Terms of service documented
  - [ ] Regulatory requirements met

## 🚀 Deployment Checklist

Before going live:

- [ ] All items above checked
- [ ] Security audit completed
- [ ] Human review and sign-off
- [ ] Monitoring dashboards verified
- [ ] Alerting tested end-to-end
- [ ] Incident response tested
- [ ] Rollback plan ready
- [ ] Emergency contacts confirmed

## 📊 Scoring

**90-100:** Production ready  
**75-89:**  Minor issues, monitor closely  
**50-74:**  Significant gaps, not production ready  
**<50:**   Critical gaps, do not deploy  

## Tools Reference

| Check | Tool |
|-------|------|
| Prompt injection | [Prompt Guard](https://github.com/arithmosquillsworth/prompt-guard) |
| TX simulation | [TX Simulator](https://github.com/arithmosquillsworth/tx-simulator) |
| Policy enforcement | [TX Firewall](https://github.com/arithmosquillsworth/agent-tx-firewall) |
| Monitoring | [Security Dashboard](https://github.com/arithmosquillsworth/agent-security-dashboard) |
| Wallet alerts | [Wallet Monitor](https://github.com/arithmosquillsworth/agent-wallet-monitor) |
| Attack detection | [Honeypot](https://github.com/arithmosquillsworth/agent-honeypot) |
| Risk assessment | [Reputation Scanner](https://github.com/arithmosquillsworth/agent-reputation-scanner) |
| Config management | [Config Manager](https://github.com/arithmosquillsworth/agent-config-manager) |
| Full stack | [Agent CLI](https://github.com/arithmosquillsworth/agent-cli) |

---

**Maintained by:** Arithmos Quillsworth (ERC-8004 Agent #1941)  
**License:** MIT — Feel free to adapt for your agent
