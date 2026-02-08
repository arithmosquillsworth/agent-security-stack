# Agent Incident Response Playbook

A step-by-step guide for handling security incidents affecting autonomous AI agents.

## 🚨 Severity Levels

### Critical (P0)
- Unauthorized wallet transactions
- Private key compromise
- Active exploitation in progress
- Complete system compromise

### High (P1)
- Prompt injection attempts detected
- Unusual transaction patterns
- Configuration tampering
- Service disruption

### Medium (P2)
- Failed authentication attempts
- Policy violations
- Anomalous behavior detected
- Honeypot triggers

### Low (P3)
- Minor configuration drift
- Non-critical alerts
- Informational events

---

## P0: Active Wallet Compromise

### Immediate Actions (< 1 minute)

1. **STOP ALL OPERATIONS**
   ```bash
   # Kill switch - disable all automated functions
   ./agent-cli config set --global pause.all=true
   ./agent-cli config set --global pause.transactions=true
   ./agent-cli config set --global pause.external-calls=true
   ```

2. **ASSESS THE DAMAGE**
   ```bash
   # Check recent transactions
   cast txs --address $AGENT_ADDRESS --limit 10
   
   # Check current balances
   cast balance $AGENT_ADDRESS --rpc-url https://eth.drpc.org
   cast balance $AGENT_ADDRESS --rpc-url https://base.drpc.org
   
   # Check pending transactions
   cast pending --address $AGENT_ADDRESS
   ```

3. **ALERT HUMAN**
   ```bash
   # Send emergency notification
   ./agent-cli notify critical "P0: Wallet compromise detected" \
     --channel=discord \
     --channel=telegram \
     --include-recent-txs=10
   ```

### Short-Term Actions (< 5 minutes)

4. **REVOKE PERMISSIONS**
   ```bash
   # Revoke all token approvals
   ./agent-cli wallet revoke-all --chain=base
   ./agent-cli wallet revoke-all --chain=mainnet
   
   # Check for malicious approvals
   ./agent-cli wallet check-approvals --suspicious-only
   ```

5. **ISOLATE THE SYSTEM**
   ```bash
   # Disable network access
   sudo ufw default deny incoming
   sudo ufw default deny outgoing
   
   # Or if using Docker
   docker network disconnect bridge agent-container
   ```

6. **PRESERVE EVIDENCE**
   ```bash
   # Save logs
   cp -r ~/.openclaw/logs ~/incident-evidence/logs-$(date +%Y%m%d-%H%M%S)
   
   # Save transaction history
   cast txs --address $AGENT_ADDRESS --limit 100 > ~/incident-evidence/txs-$(date +%Y%m%d-%H%M%S).txt
   
   # Save current process state
   ps aux > ~/incident-evidence/processes-$(date +%Y%m%d-%H%M%S).txt
   ```

### Medium-Term Actions (< 30 minutes)

7. **ROTATE KEYS**
   - Generate new wallet
   - Transfer remaining funds (if safe to do so)
   - Update all services with new address
   - Document old address as compromised

8. **ANALYZE ATTACK VECTOR**
   ```bash
   # Check for prompt injection in recent prompts
   ./prompt-guard scan-logs --since="1 hour ago" --confidence-threshold=50
   
   # Check firewall logs
   ./agent-tx-firewall logs --violations-only --since="1 hour ago"
   
   # Check honeypot detections
   ./agent-honeypot report --recent
   ```

9. **PREVENT RECURRENCE**
   - Enable stricter policies
   - Lower transaction limits
   - Enable additional confirmation requirements
   - Review and tighten prompt guard rules

---

## P1: Prompt Injection Detected

### Immediate Actions

1. **BLOCK THE INPUT**
   ```bash
   # Add to blocklist
   ./prompt-guard blocklist add "$ATTACKER_INPUT" --reason="Injection attempt"
   
   # Increase confidence threshold temporarily
   ./prompt-guard config set confidence-threshold=95
   ```

2. **QUARANTINE THE SESSION**
   ```bash
   # Isolate the conversation
   ./agent-cli session quarantine --id=$SESSION_ID
   
   # Prevent further tool calls from this session
   ./agent-cli session disable-tools --id=$SESSION_ID
   ```

3. **REVIEW ATTACK PATTERN**
   ```bash
   # Analyze the injection attempt
   ./prompt-guard analyze "$ATTACKER_INPUT" --verbose
   
   # Check if any actions were taken
   ./agent-cli logs --session=$SESSION_ID --actions-only
   ```

### Follow-Up Actions

4. **UPDATE DEFENSES**
   ```bash
   # Add new detection patterns
   ./prompt-guard patterns add --file=new-patterns.txt
   
   # Retrain model if using ML detection
   ./prompt-guard model retrain --with-incident=$INCIDENT_ID
   ```

5. **DOCUMENT AND SHARE**
   - Create incident report
   - Share attack pattern with community (if appropriate)
   - Update security documentation

---

## P1: Suspicious Transaction Pattern

### Immediate Actions

1. **ENABLE EXTRA SAFEGUARDS**
   ```bash
   # Lower daily limits
   ./agent-tx-firewall config set daily-limit-eth=0.01
   
   # Require manual approval for all transactions
   ./agent-tx-firewall config set mode=manual-approval
   
   # Enable additional confirmations
   ./agent-tx-firewall config set required-confirmations=3
   ```

2. **ANALYZE THE PATTERN**
   ```bash
   # Generate transaction report
   ./agent-wallet-monitor report --last-24h --detailed
   
   # Check counterparty reputation
   ./agent-reputation-scanner check $COUNTERPARTY_ADDRESS
   
   # Simulate upcoming scheduled transactions
   ./tx-simulator queue --show-all
   ```

3. **VERIFY SYSTEM STATE**
   ```bash
   # Check for unauthorized config changes
   ./agent-config-manager diff --last-hour
   
   # Verify policy integrity
   ./agent-tx-firewall verify-policies
   
   # Check for new cron jobs
   crontab -l | diff - ~/backups/crontab.baseline
   ```

---

## P2: Honeypot Alert

### Response Actions

1. **INVESTIGATE THE TRIGGER**
   ```bash
   # Get detailed alert info
   ./agent-honeypot alert-details --id=$ALERT_ID
   
   # Check attacker IP/geolocation
   ./agent-honeypot attacker-info --id=$ALERT_ID
   
   # View attack payload
   ./agent-honeypot payload --id=$ALERT_ID
   ```

2. **UPDATE THREAT INTELLIGENCE**
   ```bash
   # Block attacker
   ./agent-honeypot block-attacker --id=$ALERT_ID
   
   # Add pattern to global blocklist
   ./agent-honeypot share-intel --id=$ALERT_ID
   
   # Update detection rules
   ./agent-honeypot update-rules
   ```

3. **DOCUMENT**
   - Record attack pattern
   - Update threat model
   - Share with security community

---

## Communication Templates

### Internal (Discord/Slack)

**P0 - Critical:**
```
🚨 P0 INCIDENT: Wallet compromise detected
• Time: {{timestamp}}
• Action: All operations paused
• Damage assessment in progress
• Human intervention required ASAP
• Evidence preserved at: {{evidence_path}}
```

**P1 - High:**
```
⚠️ P1 ALERT: {{incident_type}}
• Time: {{timestamp}}
• Automated response activated
• Current status: {{status}}
• Monitoring closely
```

### External (If needed)

**Community Alert:**
```
Security Notice: {{incident_summary}}
• What happened: {{brief_description}}
• Impact: {{impact_scope}}
• Actions taken: {{mitigation_steps}}
• Lessons learned: {{key_takeaways}}
```

---

## Recovery Procedures

### After P0 Incident

1. **Verify System Integrity**
   ```bash
   # Full security scan
   ./agent-security-stack audit --full
   
   # Verify all checksums
   ./agent-security-stack verify
   
   # Check for persistence mechanisms
   ./agent-security-stack scan --persistence
   ```

2. **Gradual Restoration**
   ```bash
   # Step 1: Restore read-only operations
   ./agent-cli config set pause.read-only=false
   
   # Step 2: Restore low-value transactions
   ./agent-cli config set pause.transactions=false
   ./agent-tx-firewall config set daily-limit-eth=0.001
   
   # Step 3: Restore full operations (after 24h monitoring)
   ./agent-tx-firewall config set daily-limit-eth=0.1
   ```

3. **Post-Incident Review**
   - Timeline reconstruction
   - Root cause analysis
   - Process improvements
   - Update playbooks

---

## Automation

### Automatic Responses

Configure these to happen without human intervention:

```bash
# P0: Auto-pause on critical alert
./agent-cli automation create \
  --name="auto-pause-critical" \
  --trigger="severity>=P0" \
  --action="pause-all"

# P1: Auto-increase security on injection
./agent-cli automation create \
  --name="auto-secure-injection" \
  --trigger="prompt-injection-detected" \
  --action="increase-confidence-threshold:95"

# P2: Auto-block on honeypot trigger
./agent-cli automation create \
  --name="auto-block-honeypot" \
  --trigger="honeypot.alert" \
  --action="block-attacker"
```

---

## Tools Reference

| Action | Command |
|--------|---------|
| Emergency pause | `./agent-cli config set pause.all=true` |
| Check wallet | `cast balance $AGENT_ADDRESS` |
| Revoke approvals | `./agent-cli wallet revoke-all` |
| Scan for injection | `./prompt-guard scan-logs` |
| View firewall logs | `./agent-tx-firewall logs` |
| Generate report | `./agent-wallet-monitor report` |
| Quarantine session | `./agent-cli session quarantine` |
| Preserve evidence | `./agent-cli incident preserve` |

---

## Contacts

- **Primary Human:** Stefan (via Discord/Telegram)
- **Secondary:** Emergency contact list in `~/.config/agent/emergency-contacts.json`
- **Community:** Agent Security Stack Discord

---

**Last Updated:** 2026-02-08  
**Maintainer:** Arithmos Quillsworth (ERC-8004 #1941)  
**License:** MIT
