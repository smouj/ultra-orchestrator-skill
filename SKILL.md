---
name: Ultra Orchestrator
description: AI-powered security orchestration and automation platform for enterprise security operations
version: "2.1.0"
maintainer: Security Team <security@company.com>
tags:
  - security
  - ai
  - automation
  - orchestration
  - incident-response
platforms:
  - linux
  - darwin
  - windows
---

# Ultra Orchestrator

## Purpose

Ultra Orchestrator is an AI-powered security orchestration platform designed to automate and coordinate security operations across hybrid infrastructure. It integrates with enterprise security tools, performs intelligent threat analysis, and executes automated response workflows with human-in-the-loop approval for critical actions.

Real use cases:
- **Automated vulnerability management**: Schedule scans with Nessus/OpenVAS, AI-risk score findings, auto-create JIRA tickets, and trigger patch deployment through Ansible
- **Real-time incident response**: Detect anomalies via ML models, execute containment playbooks (isolate VLANs, block IPs, rotate credentials), and notify stakeholders automatically
- **Compliance automation**: Continuous compliance monitoring against CIS, PCI-DSS, HIPAA; generate evidence packages with immutable audit trails
- **Threat intelligence enrichment**: Automatically lookup IOCs in VirusTotal, AlienVault OTX, and internal threat feeds; correlate with SIEM alerts
- **Security posture dashboards**: Aggregate findings from 50+ tools into unified risk score with AI-generated executive summaries
- **Credential rotation orchestration**: Rotate 10,000+ database/service passwords on 30-day schedule using HashiCorp Vault + Ansible
- **Patch deployment coordination**: Intelligent patch batching based on CB dependency graph, maintenance windows, and rollback validation
- **Security configuration drift detection**: Compare current state against CIS benchmarks; auto-remediate non-compliant configurations

## Scope

Ultra Orchestrator operates within the security domain and provides these specific commands:

### Core Commands
- `ultra scan [target] [options]` - Run security scans with AI analysis
- `ultra analyze [data-source] [--ai]` - AI-powered security analysis
- `ultra playbook [execute|list|create|approve] [name]` - Incident response playbook management
- `ultra compliance [check|report|evidence] [framework]` - Compliance assessment and evidence collection
- `ultra threat-intel enrich [indicator] [--sources]` - Threat intelligence lookup and correlation
- `ultra remediate [issue-id] [--dry-run]` - Automated remediation with rollback capability
- `ultra monitor [start|stop|status|alerts]` - Continuous security monitoring and alerting
- `ultra report generate [type] [--format]` - Security reporting (executive, technical, compliance)
- `ultra credential rotate [service] [--emergency]` - Credential rotation management
- `ultra config [get|set|validate|rollback]` - Configuration management
- `ultra status [--detailed]` - System and component health checking
- `ultra ticket [create|update|escalate]` - Integration with ITSM (JIRA, ServiceNow)
- `ultra backup [create|verify|restore]` - Backup and restore operations

### Subcommands and Flags
```bash
# Network scanning with AI risk scoring
ultra scan network \
  --range 10.0.0.0/16 \
  --exclude 10.0.0.1,10.0.0.2 \
  --ports 22,80,443,3389 \
  --engine openvas \
  --ai-risk-score \
  --model risk-v2 \
  --credentials-file /etc/ultra/scan-creds.yaml \
  --output-format json \
  --report-path /var/reports/scan-$(date +%Y%m%d-%H%M%S).json \
  --compliance cis,hipaa \
  --ticket SEC-$(date +%Y%m%d)-001 \
  --notify slack:#security-alerts,email:secops@company.com

# Web application scanning with AI-powered attack path analysis
ultra scan web \
  --target https://app.example.com \
  --deep \
  --auth-file /etc/ultra/web-auth.json \
  --ai-attack-paths \
  --sso-integration okta \
  --rate-limit 10 \
  --user-agent "UltraOrchestrator/2.1"

# Log analysis with ML anomaly detection
ultra analyze logs \
  --source /var/log/auth.log,/var/log/secure \
  --ai-anomalies \
  --model anomaly-detection-v3 \
  --time-range "24h" \
  --threshold 0.85 \
  --baseline /var/lib/ultra/baselines/auth-baseline.json \
  --alert-slack https://hooks.slack.com/services/xxx \
  --output /reports/anomalies-$(date +%Y%m%d).json

# Incident response playbook execution
ultra playbook execute ransomware-response \
  --target-servers db01,app01,web01 \
  --containment immediate \
  --preserve-evidence \
  --forensic-mode \
  --notify legal-team,exec-team,soc-team \
  --ticket IR-2024-001 \
  --approval required:manager \
  --timeout 4h \
  --post-verification

# Compliance assessment with evidence collection
ultra compliance check pci-dss \
  --level 3 \
  --output html,json,csv \
  --evidence-dir /var/lib/ultra/evidence/pci-dss-20240301 \
  --sign-off required:compliance-officer \
  --auto-remediate false \
  --report-email compliance@company.com \
  --slack-notification #compliance

# Threat intelligence enrichment pipeline
ultra threat-intel enrich \
  --file /tmp/iocs.csv \
  --sources virus-total,alien-vault,internal-threats,mandiant \
  --ai-correlation \
  --confidence-threshold 0.7 \
  --output /var/lib/ultra/threat-intel/enriched-$(date +%Y%m%d).json \
  --auto-block \
  --ticket THREAT-$(date +%Y%m%d)-001 \
  --ttl 30d

# Automated remediation with rollback plan
ultra remediate CVE-2024-0001 \
  --targets web01,web02,db01 \
  --patch-current \
  --reboot-strategy staged \
  --rollback-snapshot /backups/ultra/pre-remediate-$(date +%Y%m%d)/ \
  --verify-services \
  --health-check-url https://healthchecks.io/ultra/xxx \
  --notify dev-team,secops \
  --maintenance-window "02:00-06:00"

# Continuous monitoring with AI anomaly detection
ultra monitor start \
  --name production-monitor \
  --sources /var/log/nginx/access.log,/var/log/mysql/error.log,/var/log/secure \
  --ai-model anomaly-detection-v3 \
  --alert-threshold high,medium \
  --slack-webhook https://hooks.slack.com/services/xxx \
  --email-alerts secops@company.com \
  --pagerduty-integration \
  --rate-limit 100 \
  --retention 90d \
  --baseline-update daily

# Security report generation
ultra report generate executive \
  --period 30d \
  --format pdf,html \
  --template executive-summary-v3 \
  --ai-summary \
  --include-charts \
  --recipients executive@company.com,board@company.com \
  --password-protect \
  --classification internal
```

## Detailed Work Process

### 1. System Initialization and Validation
```bash
# Initialize Ultra Orchestrator with full configuration validation
sudo ultra init \
  --config /etc/ultra-orchestrator/config.yaml \
  --secrets /etc/ultra-orchestrator/secrets/ \
  --ca-cert /etc/ultra/ssl/ca.pem \
  --force-checks \
  --dry-run

# Expected output:
# [INIT] Loading configuration from /etc/ultra-orchestrator/config.yaml
# [VALIDATE] Schema version: 2.1
# [CHECK] Python dependencies: OK (46/46)
# [CHECK] Docker daemon: running (version 24.0)
# [CHECK] External tools: OpenVAS (OK), Nessus (OK), Splunk (OK), Vault (OK)
# [AI] Loading risk model: risk-v2.1 (sha256: abc123...)
# [AI] Loading anomaly model: anomaly-detection-v3.0 (sha256: def456...)
# [DB] PostgreSQL connection: OK (version 14.5)
# [NETWORK] Required ports open: 8200, 9390, 8834, 8088, 9200
# [READY] Ultra Orchestrator initialized successfully
```

### 2. Vulnerability Management Workflow (Production)
```bash
# Step 1: Create scan target definition
ultra scan target create prod-web-servers \
  --hosts web01,web02,web03,web04 \
  --ports 80,443,8080,8443 \
  --credentials vault:secret/data/scan-creds/web \
  --exclude-ports 22,3389 \
  --rate-limit 50 \
  --parallel 4

# Step 2: Schedule compliance-aware scan
ultra scan schedule prod-weekly \
  --target prod-web-servers \
  --engine openvas \
  --schedule "0 2 * * 1" \  # Mondays 2AM
  --ai-risk-score \
  --compliance cis-level2,pci-dss,hipaa \
  --ticket-prefix SEC-WEEKLY \
  --maintenance-window "02:00-06:00" \
  --notify slack:#security-scans,email:secops@company.com

# Step 3: Manually trigger with emergency flag
ultra scan run prod-weekly \
  --now \
  --ticket SEC-EMERGENCY-20240301 \
  --priority critical \
  --notify pagerduty \
  --escalate-after 2h

# Step 4: AI analysis and prioritization
ultra analyze vulnerabilities \
  --scan-id scan-$(date +%Y%m%d)-001 \
  --ai-model risk-v2 \
  --cvss-threshold 7.0 \
  --exploitability-check \
  --asset-criticality-weight 2.0 \
  --output /var/reports/ai-analysis-$(date +%Y%m%d).json

# Step 5: Auto-generate remediation tickets with playbook linkage
ultra ticket create \
  --from-analysis /var/reports/ai-analysis-$(date +%Y%m%d).json \
  --template jira-security \
  --auto-assign team:web-patch \
  --due-date $(date -d "+7 days" +%Y-%m-%d) \
  --link-playbook patch-deployment-v2 \
  --escalation-rules \
  --slack-notification

# Step 6: Execute automated remediation (if approved)
ultra remediate batch-001 \
  --ticket SEC-WEEKLY-001 \
  --playbook patch-deployment-v2 \
  --pre-check health-check \
  --dry-run \
  --approval required:secops-manager \
  --window "02:00-04:00"

# If approved:
ultra remediate batch-001 --apply --watch
```

### 3. Incident Response Workflow (Data Breach Scenario)
```bash
# Step 1: Trigger from SIEM alert (automated)
# SIEM sends webhook to Ultra Orchestrator
ultra playbook execute data-breach \
  --target db-prod-01,app-prod-01 \
  --severity critical \
  --ticket IR-2024-001 \
  --evidence-dir /forensics/IR-2024-001 \
  --chain-of-custody \
  --notify legal,exec,cfo,pr \
  --slack-channels #incidents,#legal \
  --pagerduty-escalation \
  --law-enforcement notify

# Step 2: Playbook execution with AI guidance
[PLAYBOOK] Loading 'data-breach' v4.2 (certified)
[STEP 1/20] Isolate network segment (vlan 150) ✓ - 0m 12s
[STEP 2/20] Create forensic memory dumps (LiME) ✓ - 2m 34s
[STEP 3/20] Preserve logs (30-day retention) ✓ - 0m 08s
[STEP 4/20] Block IOCs at firewall ✓ - 0m 03s
[STEP 5/20] Rotate database credentials (Vault) ✓ - 1m 22s
[STEP 6/20] Enable application-level audit logging ✓ - 0m 15s
[AI ANALYSIS] Anomaly detection: Unusual data export pattern detected
[AI RECOMMENDATION] Consider: Check data exfiltration to external IP 185.220.101.45
[STEP 7/20] Capture network flow data (NetFlow) ✓ - 1m 45s
[...]
[STEP 20/20] Generate incident report ✓ - 3m 22s
[COMPLETE] Incident contained. Evidence preserved at /forensics/IR-2024-001
[NEXT] Forensic analysis phase recommended

# Step 3: Post-incident AI analysis
ultra analyze incident IR-2024-001 \
  --ai-root-cause \
  --timeline-reconstruction \
  --lessons-learned \
  --update-playbooks \
  --ticket IRR-2024-001

# Step 4: Generate compliance evidence package
ultra compliance evidence collect \
  --framework nist-800-61 \
  --incident IR-2024-001 \
  --audit-trail /var/log/ultra/audit.log \
  --chain-of-custody \
  --sign-off required:cio,legal \
  --output /evidence/IR-2024-001-compliance.zip
```

### 4. Continuous Compliance Monitoring
```bash
# Step 1: Define compliance framework baseline
ultra compliance framework import cis-ubuntu-22.04 \
  --source https://www.cisecurity.org/benchmark/ubuntu_linux \
  --version 2.0.0 \
  --levels 1,2 \
  --controls 1.1,2.3,4.5,6.2

# Step 2: Schedule daily automated checks
ultra compliance schedule cis-daily \
  --framework cis-ubuntu-22.04 \
  --targets all-linux-servers \
  --cron "0 3 * * *" \
  --ai-gap-analysis \
  --auto-remediate low,medium \
  --critical-approval required:compliance-manager \
  --evidence-capture \
  --report-format html,json

# Step 3: Execute compliance check on demand
ultra compliance check cis-ubuntu-22.04 \
  --servers web01,db01,app01 \
  --levels 1,2 \
  --controls-all \
  --evidence-dir /var/lib/ultra/evidence/cis-20240301 \
  --ssh-key /etc/ultra/ssh/compliance-key \
  --parallel 10 \
  --timeout 2h

# Step 4: Generate compliance report
ultra compliance report generate \
  --framework cis-ubuntu-22.04 \
  --period 30d \
  --format executive,technical,raw \
  --ai-summary \
  --charts compliance-trend,top-violations \
  --recipients compliance@company.com,audit@company.com \
  --password-protect \
  --send-signed

# Sample output:
# CIS Ubuntu 22.04 Level 1+2 Assessment Report
# Period: 2024-02-01 to 2024-03-01
# Total Controls: 295
# Compliant: 273 (92.5%)
# Non-Compliant: 22
#   - High: 3 (1.0%)
#   - Medium: 9 (3.1%)
#   - Low: 10 (3.4%)
# AI Summary: "Overall compliance improved 5% since last month. 
# Top issues: 1.5.1 (sshd config), 4.2 (auditd rules), 6.1.3 (syslog)."
```

### 5. Threat Intelligence Enrichment Pipeline
```bash
# Step 1: Ingest IOCs from multiple sources
ultra threat-intel ingest \
  --source alien-vault-otx \
  --pulse-id 5f8d1a2b3c4d5e6f \
  --update-frequency 15m \
  --output /var/lib/ultra/threat-intel/otx-pulse.json

# Step 2: Enrich existing IOCs with multiple feeds
ultra threat-intel enrich \
  --file /tmp/new-iocs.csv \
  --sources virus-total,alien-vault,mandiant,intel471,internal \
  --ai-correlation \
  --confidence-threshold 0.8 \
  --ai-reputation-score \
  --geolocation \
  --timestamp-analysis \
  --output /var/lib/ultra/threat-intel/enriched-$(date +%Y%m%d).json \
  --ttl 90d

# Step 3: Auto-block high-confidence malicious IOCs
ultra threat-intel block \
  --input /var/lib/ultra/threat-intel/enriched-$(date +%Y%m%d).json \
  --confidence >=0.9 \
  --targets firewall,proxy,siem \
  --rule-expiry 30d \
  --ticket THREAT-BLOCK-$(date +%Y%m%d)-001 \
  --dry-run \
  --approval required:secops-manager

# Step 4: Correlate with internal SIEM events
ultra threat-intel correlate \
  --enriched-iocs /var/lib/ultra/threat-intel/enriched-$(date +%Y%m%d).json \
  --siem-source splunk \
  --query "index=security earliest=-24h" \
  --ai-timeline \
  --affected-assets \
  --output /reports/correlations-$(date +%Y%m%d).json

# Step 5: Generate threat intelligence bulletin
ultra threat-intel bulletin create \
  --correlations /reports/correlations-$(date +%Y%m%d).json \
  --ai-summary \
  --affected-regions \
  --recommended-actions \
  --recipients security-team@company.com \
  --format html,pdf \
  --classification internal
```

### 6. AI-Powered Security Dashboard Generation
```bash
# Generate executive dashboard with AI insights
ultra dashboard create \
  --name "Q1-2024-Security-Posture" \
  --period 90d \
  --ai-insights \
  --metrics vulnerability-trend,incident-response,compliance,threat-intel \
  --charts heat-map-risk,top-vulnerable-assets,incident-timeline \
  --benchmark industry:financial \
  --forecast 90d \
  --output /var/www/dashboards/q1-2024.html \
  --email-executive-summary \
  --password-protect \
  --share-slack #security-dashboards
```

## Golden Rules

1. **Never store credentials in plaintext** - Use HashiCorp Vault exclusively; `ultra creds validate` must pass before any scan
2. **Always use dry-run for production changes** - Append `--dry-run` to any remediation, configuration, or deletion command
3. **Maintain immutable audit trail** - All actions logged to `ULTRA_AUDIT_LOG` with SHA256 signatures; retention 7 years
4. **AI decisions require human validation for critical actions** - Set `ULTRA_AI_REVIEW_REQUIRED=true` for P1/P2 incidents; never auto-approve credential rotations
5. **Segregate environments strictly** - Production commands require `--env production` flag; staging defaults to `--env staging`; `ultra doctor` must show green for prod
6. **Backup before any remediation** - Automatic snapshots created when `ULTRA_AUTO_BACKUP=true` (default); verify with `ultra backup verify`
7. **Rollback mandatory for all changes** - Every remediation ticket must include `--with-rollback` and `ultra remediate rollback --test`
8. **Compliance-first scanning** - All scans must include at least one compliance framework: `--compliance cis,hipaa,pci`
9. **Rate limiting enforcement** - Respect tool limits; configure `ULTRA_SCAN_RATE_LIMIT` (default: 10 req/min); use `--throttle` flag
10. **Change management integration** - Production changes require valid ticket ID: `--ticket ITIL-#####`; reject without `ultra ticket validate`
11. **Encrypt all data in transit and at rest** - TLS 1.3+ for all API calls; AES-256 encryption for stored reports
12. **Least privilege access** - Ultra service account uses `ultra-svc` with only required sudo permissions; no root login
13. **Security monitoring coverage** - All critical assets must have at least one active monitor: `ultra monitor list --critical-only`
14. **Evidence preservation** - Incident response playbooks must preserve memory dumps, logs, and network captures for 90 days

## Examples

### Example 1: Critical vulnerability scan and automated remediation
```bash
# Real user command
$ ultra scan run critical-cves --engine nessus --ai-risk-score --ticket SEC-2024-0789 --compliance pci-dss --dry-run

# Real system output (abbreviated)
[INFO] Ultra Orchestrator v2.1.0 (commit: a1b2c3d)
[SCAN] Initializing Nessus engine (host: scanner01.company.com:8834)
[AUTH] Validating credentials from Vault secret/data/nessus/creds ✓
[SCAN] Target: 47 production servers (range: 10.1.0.0/16)
[AI] Loading risk model: risk-v2.1 (confidence threshold: 0.85)
[SCAN] Starting vulnerability scan (policy: full-audit, max-hosts: 50)
[PROGRESS] 0%...15%...42%...67%...89%...100%
[SCAN] Completed: 234 vulnerabilities found
[AI] Risk scoring: 12 critical (CVSS ≥9.0), 34 high (7.0-8.9), 89 medium
[AI] Top findings: CVE-2024-0001 (9.8) - Remote Code Execution in nginx, CVE-2024-0002 (9.5) - Privilege Escalation
[COMPLIANCE] PCI-DSS mapping: 8/12 requirements impacted
[TICKET] Would create: JIRA tickets SEC-2024-0789-01 through SEC-2024-0789-12
[REMEDIATE] Would execute playbook: patch-deployment-v2 (4 targets, estimated 2h)
[ROLLBACK] Would create snapshot: /backups/ultra/pre-remediate-20240301-143022
[DRY-RUN] No changes applied. Remove '--dry-run' to execute.
```

### Example 2: Incident response orchestration - Ransomware Detection
```bash
# Real user command (triggered by SIEM alert)
$ ultra playbook execute ransomware-detection --target db-prod-01,app-prod-01 --severity critical --ticket IR-2024-001 --chain-of-custody --notify legal,exec,cfo,soc

# Real system output
[IR] Incident Response initialized: IR-2024-001
[PLAYBOOK] Loading 'ransomware-detection' v5.1 (certified: 2024-02-15)
[STEP 1/45] Validate alert source: Splunk alert "Possible ransomware: encrypt*.exe process" ✓
[STEP 2/45] Create forensic evidence directory: /forensics/IR-2024-001/ (immutable) ✓
[STEP 3/45] Isolate network: Adding servers to vlan 199 (quarantine) ✓
[STEP 4/45] Capture memory dumps via LiME kernel module ✓ /forensics/IR-2024-001/mem-db-prod-01.dump
[THREAT-INTEL] Querying internal threat feed for "encrypt*.exe" → Match: extortion gang "BlackCat" TTP
[AI ANALYSIS] Behavioral analysis: Process tree matches ransomware pattern (confidence: 94%)
[RECOMMENDATION] Check for: C:\Windows\Temp\random.tmp, network connections to 185.220.101.0/24
[STEP 5/45] Preserve logs: /var/log/secure (retention: 365d) ✓
[STEP 6/45] Block IOCs at perimeter firewall ✓ (4 IPs, 12 domains)
[STEP 7/45] Rotate database credentials using Vault ✓ (27 service accounts rotated)
[STEP 8/45] Enable forensic mode on database (read-only replicas) ✓
[STEP 9/45] Snapshot all affected VMs ✓
[STEP 10/45] Notify stakeholders: legal (email+Slack), exec (SMS), CFO (encrypted email) ✓
[...]
[STEP 45/45] Generate incident report (draft) ✓ /reports/IR-2024-001-draft.pdf
[COMPLETE] Incident contained. Evidence preserved. Next: Forensic analysis phase.
[AI SUMMARY] "Ransomware 'BlackCat' variant detected via process behavioral analysis. 
Containment successful. Data exfiltration not detected (network logs show no outbound >100MB)."
[ESCALATION] Legal team requested law enforcement notification. Required action: `ultra incident notify law-enforcement --case IR-2024-001`
[FOLLOW-UP] Recommended playbook: post-incident-review (run after forensic analysis)
```

### Example 3: Compliance audit - PCI-DSS Evidence Collection
```bash
# Real user command
$ ultra compliance check pci-dss --level 3 --output html,evidence --ticket PCI-2024-0301 --sign-off required:ciso,auditor --dry-run

# Real system output
[COMPLIANCE] PCI-DSS v4.0 assessment (Level 3 - All requirements)
[SCOPE] 52 systems in CDE (Cardholder Data Environment)
[AI] Loading compliance model: pci-dss-v4 (trained on 1,200 assessments)
[CHECK] Requirement 1: Install and maintain firewall configs
  1.1.1 - Firewall rules documented: ✓ (12/12 controls compliant)
  1.2.2 - Firewall changes approved: ⚠ (3 unapproved changes detected)
  1.3.2 - DMZ segregation: ✓
[CHECK] Requirement 2: Do not use vendor defaults
  2.2.1 - System hardening: ⚠ (5 servers missing CIS baseline)
  2.2.2 - Encrypted admin access: ✓
[...]
[AI GAP ANALYSIS] 18 non-compliant items total:
  Critical: 2 (1.2.2, 8.2.3)
  High: 6 (2.2.1, 3.4.1, 4.1.1, 6.4.3, 10.5.2, 12.10.1)
  Medium: 7
  Low: 3
[REPORT] Would generate: 
  - Executive summary: /reports/pci-dss-exec-20240301.html
  - Technical report: /reports/pci-dss-tech-20240301.html
  - Evidence package: /evidence/pci-dss-20240301/ (234 files, 1.2GB)
  - Remediation plan: /remediation/PCI-2024-0301-plan.md
[TICKET] Would create audit ticket: PCI-2024-0301 (JIRA)
[SIGN-OFF] Would require: CISO, External Auditor
[DRY-RUN] No evidence collected. Remove '--dry-run' to execute.
```

### Example 4: Automated credential rotation
```bash
# Real user command
$ ultra credential rotate all-services --schedule quarterly --emergency false --dry-run

# Real system output
[CREDENTIAL] Ultra credential rotation engine v1.5
[INVENTORY] 1,247 service accounts discovered across 4 environments
[FILTER] Eligible for rotation (90+ days old): 892 accounts
[EXCLUSIONS] 47 accounts excluded (system-critical, ldap-bind, legacy-apps)
[PLAN] Rotation sequence (rolling, no downtime):
  - Batch 1: Non-critical (02:00-04:00) - 412 accounts
  - Batch 2: Semi-critical (04:00-06:00) - 312 accounts
  - Batch 3: Critical (06:00-08:00) - 168 accounts
[VAULT] Destination: Vault secret/data/credentials/prod/ (mount: kv-v2)
[ANSIBLE] Ansible playbook: rotate-credentials.yml (idempotent)
[PRE-CHECK] All target services support hot-reload? 892/892 ✓
[BACKUP] Would create Vault snapshot: /backups/vault/creds-$(date +%Y%m%d).snap
[DRY-RUN] No rotations executed. To execute:
  ultra credential rotate batch-1 --apply --window "02:00-04:00"
```

### Example 5: Threat intelligence automation - IOCs from OSINT
```bash
# Real user command
$ ultra threat-intel enrich --file /tmp/iocs.csv --sources virus-total,alien-vault --ai-correlation --auto-block --ticket THREAT-2024-001

# Input file /tmp/iocs.csv format:
# type,value,source,confidence
# ip,185.220.101.45,alien-vault,0.95
# domain,malicious.example.com,virustotal,0.87
# hash,44d88612fea8a8f36c82d4,internal,0.99

# Real system output
[THREAT-INTEL] Enrichment engine v2.3
[FILE] Parsing /tmp/iocs.csv: 124 IOCs (23 duplicate)
[API] VirusTotal: 121 queries (rate limit: 500/min) ✓
[API] AlienVault OTX: 121 queries ✓
[API] Internal feed: 124 lookups ✓
[AI] Correlating IOCs with historical incidents...
[AI] Correlation found! IOC 185.220.101.45 linked to IR-2023-045 (previous attack)
[ENRICHMENT] Results:
  - 87 IOCs confirmed malicious (confidence ≥0.85)
  - 32 IOCs suspicious (0.7-0.85)
  - 5 IOCs benign (<0.7)
[AUTO-BLOCK] Planning firewall/proxy rules for 87 confirmed IOCs
[ROLLBACK] Would generate rollback plan: /var/lib/ultra/rollback/threat-block-$(date +%Y%m%d).json
[TICKET] Would create: THREAT-2024-001 (JIRA Security)
[APPROVAL] Required: SOC Manager approval for auto-block
[DRY-RUN] No blocking applied. To execute:
  ultra threat-intel block /var/lib/ultra/threat-intel/enriched-$(date +%Y%m%d).json --apply
```

## Dependencies and Requirements

### System Requirements
- **OS**: Ubuntu 22.04 LTS+, RHEL 8+, Debian 12+, macOS 12+
- **CPU**: 8+ cores (16 recommended for parallel scanning)
- **RAM**: 16GB minimum, 32GB recommended for AI models
- **Disk**: 200GB SSD (100GB for reports, 50GB for evidence, 50GB for models)
- **Network**: Outbound HTTPS (443) to all security tool APIs; inbound for monitoring agents

### Python Environment
```bash
# Required Python version and packages
python3 --version  # >= 3.9.0, < 3.12
pip install -r requirements.txt

# Core dependencies (requirements.txt)
ultra-orchestrator==2.1.0
ultra-ai-risk-engine>=1.5.0
ultra-playbook-engine>=2.0.0
vuln-scanner-client>=3.2.0      # Nessus/OpenVAS integration
threat-intel-client>=1.8.0      # VT, OTX, Mandiant
jira-client>=3.0.0              # JIRA integration
servicenow-client>=2.1.0        # ServiceNow integration
slack-sdk>=3.0.0                # Slack notifications
pagerduty>=2.2.0                # PagerDuty integration
splunk-sdk>=1.7.0               # Splunk HEC
vault-auth>=0.9.0               # HashiCorp Vault
elasticsearch>=8.0.0            # ELK stack
cryptography>=3.4               # Encryption
pyyaml>=6.0                     # Config parsing
jmespath>=0.10                  # JSON querying
pandas>=1.5                    # Data analysis (AI models)
numpy>=1.21                    # Numerical computing
scikit-learn>=1.2              # ML models
torch>=1.13                    # Deep learning (optional)
```

### External Tools and Services
```bash
# Security scanners (one or more)
openvas-scanner   # Greenbone Security Assistant (port 9390)
nessus            # Tenable Nessus (port 8834)
qualys            # Qualys Cloud Platform
aws-inspector     # Amazon Inspector

# SIEM and log aggregation
splunk            # Splunk Enterprise/Cloud (port 8089, 8088)
elasticsearch     # ELK Stack (port 9200, 5044)
sumologic         # Sumo Logic
datadog           # Datadog

# Secrets management
vault             # HashiCorp Vault (port 8200)

# Configuration management
ansible           # Ansible (for remediation playbooks)
salt              # SaltStack (alternative)

# ITSM integration
jira              # JIRA Service Management
servicenow        # ServiceNow

# Notification channels
slack             # Slack Workflow/Webhooks
pagerduty         # PagerDuty
twilio            # SMS notifications (Twilio)
smtp              # Email notifications
mattermost        # Mattermost
teams             # Microsoft Teams

# Storage and backup
postgresql        # PostgreSQL database (ultra_db)
redis             # Redis cache (optional)
s3                # AWS S3 (report storage)
minio             # MinIO (self-hosted S3)
```

### Docker Images (Containerized Components)
```yaml
services:
  ultra-orchestrator:
    image: ultra/orchestrator:2.1.0
    ports:
      - "8080:8080"  # API
      - "9090:9090"  # Metrics
    volumes:
      - /etc/ultra:/config
      - /var/reports:/reports
      - /var/lib/ultra:/data
      - /backups:/backups
  
  ultra-ai-engine:
    image: ultra/ai-engine:1.5.0
    ports:
      - "5000:5000"  # AI inference
    volumes:
      - /models:/models
  
  ultra-scanner:
    image: ultra/scanner:3.2.0
    cap_add:
      - NET_RAW  # For nmap operations
```

### Configuration Files
```bash
# Main configuration
/etc/ultra-orchestrator/config.yaml

# Secrets directory (Vault tokens, API keys)
/etc/ultra-orchestrator/secrets/
  ├── vault-token
  ├── nessus-api-key
  ├── virus-total-api-key
  ├── slack-webhook
  └── jira-api-token

# Playbooks directory
/var/lib/ultra/playbooks/
  ├── ransomware-response.yml
  ├── patch-deployment.yml
  ├── data-breach.yml
  └── compliance-evidence.yml
```

### Environment Variables
```bash
# Core Ultra configuration
export ULTRA_HOME=/opt/ultra-orchestrator
export ULTRA_CONFIG=/etc/ultra-orchestrator/config.yaml
export ULTRA_LOG_LEVEL=INFO  # DEBUG, INFO, WARNING, ERROR, CRITICAL
export ULTRA_LOG_FILE=/var/log/ultra/ultra.log
export ULTRA_AUDIT_LOG=/var/log/ultra/audit.log

# Database
export ULTRA_DB_HOST=postgresql.internal
export ULTRA_DB_PORT=5432
export ULTRA_DB_NAME=ultra_db
export ULTRA_DB_USER=ultra_user
export ULTRA_DB_PASSWORD_FILE=/etc/ultra/secrets/db-password

# External services
export VAULT_ADDR=https://vault.company.com:8200
export VAULT_TOKEN_FILE=/etc/ultra/secrets/vault-token
export VAULT_SSL_VERIFY=true
export VAULT_CA_CERT=/etc/ultra/ssl/vault-ca.pem

export NESSUS_HOST=scanner01.company.com
export NESSUS_PORT=8834
export NESSUS_ACCESS_KEY_FILE=/etc/ultra/secrets/nessus-access-key

export OPENVAS_HOST=openvas.company.com
export OPENVAS_PORT=9390
export OPENVAS_USER=ultra-scanner
export OPENVAS_PASSWORD_FILE=/etc/ultra/secrets/openvas-password

export SPLUNK_HEC_TOKEN_FILE=/etc/ultra/secrets/splunk-hec-token
export SPLUNK_HEC_URL=https://splunk.company.com:8088/services/collector

export JIRA_URL=https://jira.company.com
export JIRA_USER=ultra-bot
export JIRA_API_TOKEN_FILE=/etc/ultra/secrets/jira-token

# AI models
export ULTRA_AI_MODEL_PATH=/models/risk-scoring,/models/anomaly-detection
export ULTRA_AI_MODEL_CACHE_SIZE=4096
export ULTRA_AI_BATCH_SIZE=32
export ULTRA_AI_CONFIDENCE_THRESHOLD=0.85
export ULTRA_AI_GPU_ENABLED=false  # Set true if GPU available

# Operational flags
export ULTRA_AUTO_BACKUP=true
export ULTRA_AUTO_UPDATE_CHECK=true
export ULTRA_DRY_RUN_DEFAULT=false
export ULTRA_AI_REVIEW_REQUIRED=true  # Human approval for AI actions
export ULTRA_SCAN_RATE_LIMIT=10  # requests per minute
export ULTRA_MAX_CONCURRENT_SCANS=4
export ULTRA_PLAYBOOK_TIMEOUT=7200  # seconds

# Notification channels
export SLACK_WEBHOOK_URL_FILE=/etc/ultra/secrets/slack-webhook
export PAGERDUTY_INTEGRATION_KEY_FILE=/etc/ultra/secrets/pagerduty-key
export EMAIL_SMTP_SERVER=smtp.company.com
export EMAIL_FROM=ultra-orchestrator@company.com
export EMAIL_ALERTS=secops@company.com

# Compliance
export ULTRA_COMPLIANCE_EVIDENCE_RETENTION=3650  # days (10 years)
export ULTRA_AUDIT_RETENTION=2555  # days (7 years)
export ULTRA_CHAIN_OF_CUSTODY_ENABLED=true
```

## Verification Steps

### Post-installation verification checklist
```bash
# 1. Run full system diagnostics
sudo ultra doctor --full --json

# Expected output (summarized):
{
  "status": "healthy",
  "components": {
    "python": {"version": "3.9.12", "status": "ok"},
    "database": {"type": "postgresql", "version": "14.5", "status": "ok"},
    "vault": {"connected": true, "version": "1.15.0"},
    "openvas": {"status": "running", "port": 9390, "status": "ok"},
    "nessus": {"status": "running", "port": 8834, "status": "ok"},
    "splunk": {"hec": true, "status": "ok"},
    "ai_models": [
      {"name": "risk-v2.1", "status": "loaded", "checksum": "abc123"},
      {"name": "anomaly-detection-v3", "status": "loaded", "checksum": "def456"}
    ],
    "ansible": {"version": "2.9.6", "status": "ok"},
    "disk_space": {"reports": "45%", "evidence": "62%", "models": "34%"},
    "network_ports": {"8200": "open", "9390": "open", "8834": "open", "8088": "open"}
  }
}
```

### 2. Test external integrations
```bash
# Test Vault connectivity
ultra vault test-auth --secret secret/data/test

# Expected: "Vault authentication successful (token TTL: 24h)"

# Test scanner connectivity
ultra scan engines list --test

# Expected:
# Engine         Status    Latency   Version
# openvas        OK        45ms      22.04
# nessus         OK        120ms     10.5.2

# Test AI models
ultra ai models list --validate

# Expected:
# Model Name              Version   Status    Validity
# risk-scoring            v2.1      OK        Valid until: 2024-12-31
# anomaly-detection       v3.0      OK        Valid until: 2025-06-30
# threat-correlation      v1.2      OK        Valid until: 2024-09-30

# Test notification channels
ultra notify test --channel slack,email,pagerduty --template test-alert

# Expected:
# [SLACK] Message sent to #security-testing (ts: 123456.789)
# [EMAIL] Sent to secops@company.com (message-id: <xxx>)
# [PAGERDUTY] Incident created: PTR-789abc (status: triggered)

# Test ticket system
ultra ticket validate --system jira --dry-run --template security-incident

# Expected: "JIRA connection OK. Project SEC exists. Required fields: priority, severity, assignee"
```

### 3. Verify backup and rollback systems
```bash
# Test backup creation
ultra backup create --type full --dry-run

# Expected: "[DRY-RUN] Would create backup: /backups/ultra/full-20240301-1430.tar.gz (size: 45.2GB)"

# Test snapshot system (if using LVM/ZFS)
sudo ultra snapshot create test-snapshot --volume ultra_data

# Expected: "Snapshot created: ultra_data-snap-test-snapshot (size: 0B, COW)"

# Test rollback plan generation
ultra remediate generate-rollback CVE-2024-0001 --targets web01,web02

# Expected: "Rollback plan written to: /var/lib/ultra/rollback/CVE-2024-0001-plan.json"
```

### 4. Verify audit logging
```bash
# Check audit log integrity
ultra audit verify --log /var/log/ultra/audit.log --since "2024-03-01"

# Expected:
# [VERIFY] Log file: /var/log/ultra/audit.log (142,567 lines)
# [CHECK] SHA256 chain verified up to: entry #142567 (2024-03-01 14:23:01)
# [CHECK] No gaps or tampering detected.
# [SUMMARY] 89 audit entries in last 24h, all signed successfully.

# Test audit log rotation
sudo logrotate -vf /etc/logrotate.d/ultra-orchestrator

# Expected: "logrotate: /var/log/ultra/audit.log"
```

### 5. Compliance and security checks
```bash
# Run security configuration audit
ultra config audit --check security

# Expected:
# [AUDIT] Security configuration check (89 controls)
# ✓ All passwords stored in Vault (not in config)
# ✓ TLS 1.3+ enforced for external APIs
# ✓ Audit logging enabled and immutable
# ✓ No default credentials in use
# ✓ RBAC properly configured
# ✓ API rate limiting active
# [PASS] 89/89 controls compliant

# Check RBAC assignments
ultra role list --users

# Expected output:
User          Roles               Expires
------------  ------------------  -----------
alice         operator,analyst   never
bob           admin              2024-12-31
service-acct scanner-engine     never
```

### 6. Health monitoring setup
```bash
# Verify metrics endpoint
curl -s http://localhost:9090/metrics | grep ultra_ | head -10

# Expected:
# ultra_scans_total{status="completed"} 1427
# ultra_incidents_total{severity="critical"} 12
# ultra_remediations_total{status="success"} 856
# ultra_ai_inference_duration_seconds_bucket{model="risk-v2"} 1024 2048 4096

# Check Prometheus alert rules
ultra alerts list --active

# Expected:
# Alert: UltraScannerDown
#   Condition: up{job="ultra-scanner"} == 0
#   For: 5m
#   Severity: critical
#   Runbook: /var/lib/ultra/runbooks/scanner-down.md
```

## Troubleshooting

### Issue: Scan fails with "Authentication failed for engine openvas"
**Symptoms**: `ultra scan run` fails immediately with auth error despite valid credentials in Vault.

**Root causes**:
- Vault token expired
- Wrong secret path in config
- OpenVAS user password changed
- Network connectivity issue

**Diagnosis**:
```bash
# 1. Check Vault token validity
vault token lookup -format=json | jq '.data.expire_time'

# 2. Test credential retrieval
vault kv get -field=password secret/data/scan-creds/openvas

# 3. Validate OpenVAS connection manually
gvm-tool --hostname openvas.company.com --port 9390 --protocol=OSP \
  --certificate=/etc/ultra/ssl/ca.pem \
  --get-version

# 4. Check Ultra's credential cache
ultra creds status --engine openvas
```

**Solution**:
```bash
# If Vault token expired:
vault login -method=ldap username=ultra-svc

# Update token in Ultra config:
echo $VAULT_TOKEN | sudo tee /etc/ultra/secrets/vault-token

# If OpenVAS password changed, update Vault secret:
vault kv put secret/data/scan-creds/openvas password="new-pass"

# Rotate Ultra's credential cache:
ultra creds rotate --engine openvas --force

# Retry scan with debug:
ultra scan run test-scan --engine openvas --debug 2>&1 | grep -A5 "AUTH"
```

### Issue: AI risk scoring returns "Unknown model" or "Model load failed"
**Symptoms**: Scan completes but AI analysis fails: `[AI] Error: Model 'risk-v2' not found`.

**Root causes**:
- Model files missing from `ULTRA_AI_MODEL_PATH`
- Model version mismatch
- Corrupted model files
- GPU/Torch incompatibility

**Diagnosis**:
```bash
# 1. List available models
ultra ai models list --local --verbose

# Expected:
# /models/risk-scoring/risk-v2.1/
#   model.pth (size: 2.1GB, sha256: abc123)
#   config.yaml
#   metadata.json

# 2. Check model integrity
ultra ai models verify risk-v2.1 --checksum

# 3. Test model inference
ultra ai test risk-v2.1 --input /var/lib/ultra/test/vulnerability-sample.json

# 4. Check GPU/Torch compatibility
python3 -c "import torch; print(torch.__version__, torch.cuda.is_available())"
```

**Solution**:
```bash
# Download missing model (from internal model registry)
ultra ai models download risk-v2.1 \
  --source s3://ultra-models/risk-scoring/v2.1/ \
  --verify-checksum \
  --target /models/risk-scoring/

# Or from backup:
sudo cp -r /backups/models/risk-v2.1 /models/risk-scoring/

# Update model path in config:
ultra config set ai.model_path /models/risk-scoring,/models/anomaly-detection

# If GPU issues, disable GPU in config:
ultra config set ai.gpu_enabled false

# Reload configuration:
ultra config reload --validate
```

### Issue: Playbook execution hangs at step N indefinitely
**Symptoms**: `ultra playbook status <incident-id>` shows step stuck with no progress for >30 minutes.

**Root causes**:
- External API timeout (Vault, Ansible, cloud provider)
- Network partition
- Resource exhaustion (disk full, memory pressure)
- Infinite loop in custom playbook step
- Authentication token expired mid-execution

**Diagnosis**:
```bash
# 1. Get detailed step status
ultra playbook status <incident-id> --step 12 --debug

# Output includes:
# Step: "rotate-credentials"
# Started: 2024-03-01 14:23:01
# Command: ansible-playbook -i inventory rotate.yml
# PID: 12345
# Status: RUNNING (2h 15m)

# 2. Check process status
sudo ps aux | grep ansible-playbook | grep 12345

# 3. Check system resources
free -h
df -h /var
dmesg | tail -20

# 4. Check playbook logs
sudo tail -100 /var/log/ultra/playbooks/<incident-id>.log

# 5. Test external service connectivity
ultra connectivity test --target vault,ansible-controller,aws

# 6. Check for zombie processes
ps aux | grep defunct
```

**Solution**:
```bash
# If Ansible hung due to unreachable host:
# Force-kill the playbook (last resort)
sudo kill -9 12345

# Mark step as failed and continue:
ultra playbook step fail <incident-id> 12 \
  --reason "Ansible timeout - host db01 unreachable" \
  --continue

# OR abort entire playbook:
ultra playbook abort <incident-id> \
  --reason "Manual intervention required - network partition" \
  --preserve-state

# For token expiry issues, refresh tokens:
vault token renew -i 24h
ultra creds refresh-all

# If disk full, clean up:
ultra cleanup old-reports --older-than 90d
ultra cleanup old-evidence --older-than 365d

# Prevent future hangs:
ultra config set playbook.default_timeout 7200
ultra config set playbook.step_timeout 1800
ultra config set ansible.ssh_timeout 60
```

### Issue: High memory usage during AI inference causing OOM kills
**Symptoms**: `dmesg` shows "Killed process 12345 (python) total-vm:..., OOM", scans fail with "AI analysis failed: out of memory".

**Root causes**:
- Too many concurrent scans loading multiple AI models
- Model too large for available RAM
- Memory leak in AI engine
- No swap space configured

**Diagnosis**:
```bash
# 1. Check memory usage
free -h
ps aux --sort=-%mem | head -10

# Expected: ultra-ai-engine using 12GB+, python processes for scan using 4GB each

# 2. Check model sizes
ls -lh /models/risk-scoring/risk-v2.1/model.pth
# Expected: ~2.1GB

# 3. Check concurrent AI tasks
ultra ai tasks list --running

# 4. Check swap usage
swapon --show
free -h
```

**Solution**:
```bash
# Limit concurrent AI inference
ultra config set ai.max_concurrent_inferences 2
ultra config set scan.ai_batch_size 16  # Reduce from 32

# Add swap space (emergency):
sudo fallocate -l 8G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab

# Configure memory limits for AI container (if using Docker):
docker update --memory=16g --memory-swap=20g ultra-ai-engine

# Use smaller model variant:
ultra config set ai.model risk-v2.1-lite  # 512MB vs 2GB

# Restart AI engine with memory limits:
sudo systemctl stop ultra-ai-engine
sudo systemctl set Environment="ULIMIT_-MEM=16777216"  # 16GB
sudo systemctl start ultra-ai-engine

# Monitor memory after changes:
watch -n 5 'free -h && ps aux | grep ultra-ai | grep -v grep'
```

### Issue: Compliance reports missing data or showing "N/A" for controls
**Symptoms**: Compliance report has many "N/A" or "No data" entries despite scanning all targets.

**Root causes**:
- Collection agent not running on target systems
- SSH connectivity issues
- Missing compliance plugins in scanner
- Evidence collection permissions denied
- Timeline mismatch (scan ran outside reporting window)

**Diagnosis**:
```bash
# 1. Check compliance agent status on target
ultra compliance agents status --server web01

# 2. Verify scan actually collected compliance data
grep -i "compliance" /var/reports/scan-web01-20240301.json | head

# 3. Check control mapping
ultra compliance controls list --framework pci-dss | grep "1.2.2"

# 4. Verify evidence collection
ls -la /var/lib/ultra/evidence/pci-dss-20240301/web01/

# 5. Check agent logs on target server
ssh web01 "journalctl -u ultra-compliance-agent -n 100"

# 6. Validate scanner plugins
ultra scan plugins list --engine openvas | grep -i "compliance"
```

**Solution**:
```bash
# Deploy compliance agent to target
ultra agent deploy --server web01 --type compliance \
  --ssh-key /etc/ultra/ssh/compliance-key \
  --verify

# Manually trigger evidence collection:
ultra compliance collect --server web01 --framework pci-dss \
  --controls 1.2.2,2.2.1,4.1.1 \
  --debug

# If SSH issues, check keys:
ssh -i /etc/ultra/ssh/compliance-key web01 "hostname"

# Install missing compliance plugins:
ultra scan plugin install openvas --plugin-family "Policy"
ultra scan engines reload openvas

# Force re-scan with compliance focus:
ultra scan run web01-compliance \
  --target web01 \
  --engine openvas \
  --scan-type full+compliance \
  --compliance pci-dss,cis \
  --plugins-family Policy,Hardening
```

### Issue: Threat intelligence correlation returns "No matches" for known bad IOCs
**Symptoms**: Enrich IOCs known to be malicious, but AI correlation returns 0 matches.

**Root causes**:
- Threat intel feeds not updated
- AI model confidence threshold too high
- IOC format mismatch
- Time window too narrow
- Internal threat database empty

**Diagnosis**:
```bash
# 1. Check threat intel feed freshness
ultra threat-intel feeds status

# Expected:
# Feed Name            Last Update    Status    Records
# virus-total          5m ago         OK        1,234,567
# alien-vault          12m ago        OK        876,543
# mandiant             2h ago         WARNING   (API rate limit)

# 2. Check IOC format in input file
head -5 /tmp/iocs.csv
# Should be: type,value,source

# 3. Check AI correlation cache
ultra ai cache status --model threat-correlation

# 4. Test single IOC lookup manually
ultra threat-intel lookup --type ip --value 185.220.101.45 --sources all

# 5. Check internal threat database
sudo psql ultra_db -c "SELECT COUNT(*) FROM threat_iocs WHERE confidence >= 0.8;"
```

**Solution**:
```bash
# Update threat feeds
ultra threat-intel feeds update --all --force

# Lower confidence threshold
ultra threat-intel enrich /tmp/iocs.csv \
  --sources virus-total,alien-vault \
  --confidence-threshold 0.6  # Default is 0.8

# Extend time window for correlation
ultra threat-intel correlate \
  --enriched-iocs /var/lib/ultra/threat-intel/enriched.json \
  --siem-source splunk \
  --time-range "30d"  # Default might be 7d

# If internal DB empty, import historical IOCs:
ultra threat-intel import /var/backups/threat-iocs-2023.json \
  --source internal-archive \
  --confidence 0.9

# Verify feed API limits and billing:
curl -H "x-apikey: $(cat /etc/ultra/secrets/virus-total-api-key)" \
  https://www.virustotal.com/api/v3/users/rate_limit
```

### Issue: Remediation fails with "Rollback plan not found" or "Backup invalid"
**Symptoms**: `ultra remediate apply` fails when attempting rollback or when pre-check runs.

**Root causes**:
- Auto-backup disabled (`ULTRA_AUTO_BACKUP=false`)
- Backup storage full or corrupted
- Snapshot LVM/ZFS not available
- Permission issues accessing backup directory
- Backup taken after remediation started (race condition)

**Diagnosis**:
```bash
# 1. Check backup directory
ls -la /backups/ultra/pre-remediate-*
ls -lh /backups/ultra/ | tail -10

# 2. Verify backup integrity
ultra backup verify --snapshot /backups/ultra/pre-remediate-20240301-143022

# 3. Check LVM snapshots (if using LVM)
sudo lvs | grep snap
sudo lvs "vg_name/ultra_data-snap-pre-remediate-20240301-143022"

# 4. Check ZFS snapshots
zfs list -t snapshot | grep ultra

# 5. Check Ultra's backup record in database
sudo psql ultra_db -c "SELECT * FROM backups WHERE snapshot='pre-remediate-20240301-143022';"

# 6. Check permissions
namei -l /backups/ultra/pre-remediate-20240301-143022
```

**Solution**:
```bash
# If backups missing, create manual snapshot before remediation:
# For LVM:
sudo lvcreate -s -n ultra_data-snap-pre-rem-$(date +%s) /dev/vg/ultra_data

# For ZFS:
zfs snapshot ultra_data@pre-remediate-$(date +%Y%m%d-%H%M%S)

# Record in Ultra database:
ultra backup record create \
  --snapshot ultra_data-snap-pre-rem-$(date +%s) \
  --type lvm \
  --volume ultra_data \
  --pre-remediate \
  --ticket SEC-2024-001

# If backups corrupted, restore from last known good:
ultra backup restore --from /backups/ultra/weekly-20240225.tar.gz \
  --target-db ultra_db \
  --target-volume /var/lib/ultra \
  --dry-run

# Enable auto-backup in config:
ultra config set backup.auto_create true
ultra config set backup.retention_pre_remediate 30

# Verify backup permissions:
sudo chown -R ultra:ultra /backups/ultra
sudo chmod 750 /backups/ultra
```

## Rollback Commands

### Rollback a completed remediation
```bash
# Comprehensive rollback with verification and notifications
ultra remediate rollback CVE-2024-0001 \
  --snapshot /backups/ultra/pre-remediate-20240301-143022 \
  --verify-services https://healthchecks.io/ultra/xxx,https://internal-health.company.com/ \
  --post-checks "ultra scan run post-remediate-check --engine openvas --target web01,web02" \
  --notify slack:#security,email:secops@company.com \
  --maintenance-window-approved \
  --dry-run  # Remove after validation

# Production execution:
ultra remediate rollback CVE-2024-0001 \
  --snapshot /backups/ultra/pre-remediate-20240301-143022 \
  --verify-services \
  --post-checks \
  --notify \
  --force  # Only if verification fails but rollback needed

# Expected output:
# [ROLLBACK] Starting rollback for CVE-2024-0001
# [SNAPSHOT] Found: ultra_data-snap-pre-remediate-20240301-143022 (size: 45GB)
# [RESTORE] Restoring LVM snapshot... ✓ (5m 23s)
# [VERIFY] Service health check: web01=200 OK, web02=200 OK ✓
# [POST-CHECK] Running security scan... ✓ (0 new vulnerabilities)
# [NOTIFY] Slack message sent, email delivered to 12 recipients
# [COMPLETE] Rollback successful. System stable. Ticket SEC-2024-001 updated.
```

### Rollback a scan (delete reports and findings)
```bash
# Remove scan artifacts and database entries
ultra scan rollback scan-20240301-1430 \
  --delete-reports \
  --delete-findings \
  --notify-team secops \
  --audit-reason "False positive - test in staging environment"

# If scan created tickets, close them:
ultra ticket close --from-scan scan-20240301-1430 \
  --reason "Test scan - no action required" \
  --comment "Rolled back via ultra scan rollback"

# Output:
# [ROLLBACK] Deleting scan report: /var/reports/scan-20240301-1430.json (234MB)
# [ROLLBACK] Removing 1,247 findings from database
# [TICKET] Closing 12 JIRA tickets (SEC-2024-03-01-001 through 012)
# [AUDIT] Rollback recorded in audit log (event: scan_rollback)
```

### Configuration rollback
```bash
# View configuration change history
ultra config history --days 30

# Output:
# Timestamp              User         Change
# 2024-03-01 14:30:15   alice        Set: scan.max_concurrent=4
# 2024-03-01 12:15:42   bob          Set: ai.confidence_threshold=0.87
# 2024-02-29 09:22:11   system       Auto-backup created

# Dry-run rollback to specific timestamp
ultra config rollback --to "2024-03-01-12:00:00" --dry-run --diff

# Expected:
# Would revert 3 changes:
#   - Revert ai.confidence_threshold 0.87 → 0.85
#   - Revert scan.max_concurrent 4 → 2
#   - Restore config file from backup: /backups/config/ultra-20240301-121500.yaml

# Perform actual rollback with validation
ultra config rollback --to "2024-03-01-12:00:00" \
  --validate \
  --test-connections \
  --restart-services \
  --notify-slack \
  --ticket CONFIG-ROLLBACK-2024-001

# Output:
# [ROLLBACK] Loading backup: /backups/config/ultra-20240301-121500.yaml
# [VALIDATE] YAML syntax: OK
# [VALIDATE] Schema: OK
# [TEST] Vault connection: OK
# [TEST] Scanner connectivity: OK
# [RESTART] ultra-orchestrator: ✓
# [RESTART] ultra-ai-engine: ✓
# [NOTIFY] Slack message sent to #ops-alerts
# [TICKET] Created: CONFIG-ROLLBACK-2024-001
```

### Database point-in-time recovery (PITR)
```bash
# Restore database to specific time with transaction replay
ultra db restore \
  --backup /backups/postgresql/ultra_db-20240301-1200.base.tar.gz \
  --wal-archive /backups/postgresql/wal/ \
  --recovery-target "2024-03-01 14:23:45 UTC" \
  --verify-checksum \
  --maintenance-mode \
  --notify db-team,secops \
  --ticket DB-RESTORE-2024-001

# WAL streaming recovery (continuous):
ultra db restore streaming \
  --restore-point /backups/postgresql/full-20240301-0000 \
  --wal-target "2024-03-01 14:30:00" \
  --replication-slot ultra_recovery_slot \
  --standby-mode \
  --switchover

# Expected output:
# [RESTORE] Restoring base backup (45GB)... ✓ (12m 34s)
# [WAL] Replaying WAL files from 2024-03-01 00:00:00 to 2024-03-01 14:23:45
# [WAL] Replayed 15,423 WAL records (2.1GB)
# [RECOVERY] Database recovered to consistent state at 2024-03-01 14:23:45 UTC
# [VERIFY] Checksum: OK (sha256: abc123...)
# [MAINTENANCE] Bringing primary back online...
# [NOTIFY] Email sent to db-team@company.com
```

### Full system rollback (emergency)
```bash
# Complete system rollback to known-good snapshot
# WARNING: This reverts ALL components (DB, config, reports, evidence)

ultra system rollback \
  --snapshot 2024-03-01-full-system \
  --components ultra-orchestrator,ultra-ai-engine,ultra-db,ultra-config \
  --verify-boot \
  --keep-forensics \
  --maintenance-window "now+2h" \
  --approval required:2 \
  --ticket EMERGENCY-2024-001 \
  --escalate \
  --dry-run

# After two approvals received (from CISO and Head of Ops):
ultra system rollback \
  --snapshot 2024-03-01-full-system \
  --components all \
  --verify-boot \
  --preserve-forensics /forensics/rollback-$(date +%Y%m%d-%H%M%S) \
  --force \
  --ticket EMERGENCY-2024-001

# Output:
# [EMERGENCY] Full system rollback initiated (approved by: alice, bob)
# [SNAPSHOT] Full system snapshot: ultra-full-20240301-0000.tar.gz (125GB)
# [COMPONENT] Shutting down: ultra-orchestrator, ultra-ai-engine, ultra-db
# [RESTORE] Restoring PostgreSQL from backup... ✓
# [RESTORE] Restoring Ultra config from /backups/config/ultra-20240301-0000.yaml ✓
# [RESTORE] Restoring AI models from /backups/models/20240301/ ✓
# [BOOT] Starting services: ultra-db → ultra-ai-engine → ultra-orchestrator
# [VERIFY] Health check: http://localhost:8080/health → 200 OK
# [VERIFY] Database integrity: OK
# [PRESERVE] Current state saved to: /forensics/rollback-20240301-1430/
# [NOTIFY] Emergency broadcast sent to all-staff@company.com
# [COMPLETE] System rolled back to 2024-03-01 00:00:00 UTC
# [ACTION REQUIRED] Post-rollback: Replay events from 00:00 to now
#   ultra db replay-wal --start 2024-03-01T00:00:00 --end now
```

### Threat intelligence rollback (auto-block rollback)
```bash
# Remove firewall rules created by threat-intel block
ultra threat-intel rollback \
  --block-id threat-block-20240301-1430 \
  --targets firewall01,proxy01 \
  --verify-removal \
  --notify soc-team \
  --ticket THREAT-ROLLBACK-2024-001

# If rollback fails (e.g., rule in use):
ultra threat-intel rollback \
  --block-id threat-block-20240301-1430 \
  --disruptive false \
  --log-only \
  --create-ticket manual-removal-required

# Manual rollback (if auto fails):
# On firewall01:
iptables -L INPUT -n | grep "185.220.101.45"
iptables -D INPUT -s 185.220.101.45 -j DROP
# On proxy:
squidclient -m GET http://proxy01:3128/acl?delete=blacklist_185.220.101.45
```

### Playbook step rollback (during execution)
```bash
# If a playbook step fails and automatic rollback is defined
ultra playbook step rollback <incident-id> 15 \
  --from-step "deploy-patch" \
  --to-checkpoint "pre-patch-backup" \
  --verify \
  --continue

# If manual rollback after playbook completion:
ultra playbook rollback <incident-id> \
  --to-step 10 \
  --snapshot /backups/ultra/playbooks/IR-2024-001-snap-20240301-1430 \
  --notify \
  --ticket IR-ROLLBACK-2024-001

# Expected:
# [ROLLBACK] Playbook IR-2024-001: Rolling back from step 20 to step 10
# [SNAPSHOT] Restoring /backups/ultra/playbooks/IR-2024-001-snap-20240301-1430
# [STEP] Undo step 20: "rotate-credentials" ✓
# [STEP] Undo step 19: "block-iocs" ✓
# [...]
# [STEP] Undo step 11: "isolate-network" ✓
# [COMPLETE] System returned to state after step 10 (containment only, no remediation)
```

### Report and evidence rollback
```bash
# Delete mistakenly generated report and remove evidence
ultra report rollback \
  --report /reports/pci-dess-exec-20240301.html \
  --evidence-dir /var/lib/ultra/evidence/pci-dss-20240301 \
  --notify compliance@company.com \
  --reason "Wrong template used" \
  --audit

# Output:
# [ROLLBACK] Deleting report: /reports/pci-dss-exec-20240301.html (4.2MB)
# [ROLLBACK] Removing evidence directory: /var/lib/ultra/evidence/pci-dss-20240301/ (1.2GB)
# [AUDIT] Recorded in audit log: report_rollback user=alice reason="Wrong template"
# [NOTIFY] Email sent to compliance@company.com
```

### AI model rollback (bad model causing false positives)
```bash
# Revert to previous AI model version
ultra ai models rollback \
  --model risk-v2 \
  --to-version 2.0.3 \
  --verify \
  --validate-on-samples /var/lib/ultra/test/samples-v2.0.3/ \
  --update-config \
  --notify \
  --ticket AI-MODEL-ROLLBACK-2024-001

# Expected:
# [ROLLBACK] Risk model: v2.1 → v2.0.3
# [LOAD] Loading previous model from /models/risk-scoring/v2.0.3/ ✓
# [VALIDATE] Testing on 1,000 sample vulnerabilities... 
#   False positive rate: 2.1% (target: <5%) ✓
#   True positive rate: 94.3% (target: >90%) ✓
# [CONFIG] Updated: ai.model_version = "2.0.3"
# [NOTIFY] Slack: :warning: AI model rolled back to v2.0.3 due to increased FPs
# [TICKET] AI-MODEL-ROLLBACK-2024-001 created with analysis
```

## Security Considerations

### Access Control and Authentication
```bash
# Enable MFA for all production operations
ultra auth mfa enable --required-for production,remediate,playbook-execute

# Configure role-based access control
ultra role create security-analyst \
  --permissions scan:run,analyze:view,ticket:create \
  --no-remediate,no-config,no-delete

ultra role create secops-admin \
  --permissions all \
  --require-approval-for remediate,playbook:execute,config:set

ultra user assign alice --role security-analyst --expires "2024-12-31"
ultra user assign bob --role secops-admin --mfa-required

# Test RBAC:
sudo -u alice ultra scan run test  # Should work
sudo -u alice ultra remediate CVE-1234  # Should FAIL: Permission denied

# Enable session recording for privileged actions
ultra session record start --user root --ttl 30d
ultra session replay --session-id sess-20240301-143022
```

### Data Protection
```bash
# Encrypt all stored reports
ultra config set storage.encryption aes-256-gcm
ultra config set storage.key_vault_path secret/data/ultra/encryption-key

# Enable evidence chain-of-custody
ultra config set evidence.chain_of_custody true
ultra config set evidence.immutable_storage s3://ultra-evidence/immutable/

# Configure secure deletion
ultra config set data_retention.secure_delete_after 3650d
ultra cleanup secure --older-than 3650d --shred passes:3

# Export encryption keys to HSM (if available):
ultra keys export --to hsm://pkcs11:lib/usr/lib/softhsm/libsofthsm2.so \
  --pin-file /etc/ultra/secrets/hsm-pin \
  --key-label ultra-master-key
```

### Network Security
```bash
# Restrict API to internal network
ultra api bind 10.0.0.0/8 --tls-cert /etc/ultra/ssl/api.pem \
  --tls-key /etc/ultra/ssl/api-key.pem \
  --client-ca /etc/ultra/ssl/ca.pem \
  --require-client-cert

# Configure API rate limiting per user
ultra api rate-limit set --user alice --requests-per-minute 60
ultra api rate-limit set --user bob --requests-per-minute 120

# Set up firewall rules
sudo ufw allow from 10.0.0.0/8 to any port 8080,9090 proto tcp
sudo ufw deny 8080,9090

# Enable VPN-only access (if required):
ultra api bind 100.64.0.0/10  # WireGuard internal IP range
```

### Logging and Monitoring
```bash
# Enable detailed audit logging
ultra config set audit.enabled true
ultra config set audit.log_level detailed
ultra config set audit.sign_entries true
ultra config set audit.retention 2555d  # 7 years

# Forward logs to SIEM
ultra logging forward --type audit --target splunk://splunk-hec:8088 \
  --token $SPLUNK_HEC_TOKEN \
  --index ultra_audit

ultra logging forward --type operational --target elasticsearch://es01:9200 \
  --index ultra-operations

# Set up alerts for critical events
ultra alert create "Unauthorized access attempt" \
  --condition "ultra_audit_event{event_type='auth_failure'} > 0" \
  --severity critical \
  --runbook "https://runbooks.company.com/unauth-access" \
  --notify pagerduty,slack:#security-alerts \
  --for 5m
```

## Performance Tuning

### Scan Optimization
```bash
# Adjust parallel scan settings based on available resources
ultra config set scan.max_concurrent_jobs 4          # Default: 2
ultra config set scan.max_hosts_per_job 50         # Default: 25
ultra config set scan.max_ports_per_host 100       # Default: 50
ultra config set scan.rate_limit_global 100        # Global req/min

# Per-engine rate limits (to avoid overwhelming targets)
ultra config set scan.rate_limits.openvas 10       # req/min
ultra config set scan.rate_limits.nessus 20        # req/min

# AI batch processing (reduce memory footprint)
ultra config set ai.inference_batch_size 16         # Default: 32
ultra config set ai.model_cache_size 2048          # Cache entries

# Database connection pooling
ultra config set db.pool_size 20                   # Default: 10
ultra config set db.max_overflow 30                # Temp connections

# Redis caching for frequent queries (optional)
ultra config set cache.redis_url redis://redis01:6379/0
ultra config set cache.ttl_scan_results 3600      # 1 hour
ultra config set cache.ttl_ai_inference 1800      # 30 minutes
```

### Memory Management
```bash
# Set memory limits per component (systemd)
sudo systemctl edit ultra-orchestrator

# Add:
[Service]
MemoryLimit=8G
MemorySwapMax=12G

# For AI engine (memory hog):
sudo systemctl edit ultra-ai-engine
[Service]
MemoryMax=16G
IOWeight=100  # Prioritize IO

# Configure ULTRA to respect system limits
ultra config set resources.memory_limit 24G
ultra config set resources.soft_limit 20G
ultra config set resources.oom_score_adj -500  # Less likely killed

# Enable swap as safety net
sudo fallocate -l 8G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon -p 10 /swapfile  # Priority 10

# Monitor memory pressure
ultra metrics watch memory_usage --alert-threshold 85%
```

### Database Performance
```bash
# Tune PostgreSQL for Ultra workload
sudo nano /etc/postgresql/14/main/ultra.conf

# Add:
shared_buffers = 4GB
effective_cache_size = 12GB
work_mem = 64MB
maintenance_work_mem = 1GB
max_connections = 100
max_parallel_workers_per_gather = 4

# Create indexes for common queries
sudo -u postgres psql ultra_db -c "
CREATE INDEX CONCURRENTLY idx_scans_completed ON scans (completed_at) WHERE status='completed';
CREATE INDEX CONCURRENTLY idx_vulnerabilities_cvss ON vulnerabilities (cvss_score DESC);
CREATE INDEX CONCURRENTLY idx_incidents_severity ON incidents (severity, created_at);
"

# Regular vacuum and analyze
ultra db maintenance --auto-vacuum --analyze --daily 02:00

# Partition large tables by date
ultra db partition scans --by month --retention 90d
```

### Network Optimization
```bash
# Use connection pooling for external APIs
ultra config set http.connection_pool_size 50
ultra config set http.keepalive_timeout 60
ultra config set http.max_retries 3
ultra config set http.backoff_factor 0.5

# Configure parallel download of scan results
ultra config set scan.download_concurrency 10

# Use local caching for threat intel feeds
ultra config set threat_intel.cache_ttl 3600  # 1 hour
ultra config set threat_intel.cache_size 10000  # entries
```

## Maintenance

### Daily Health Checks (Automated)
```bash
#!/bin/bash
# /opt/ultra/scripts/healthcheck.sh

/usr/local/bin/ultra status --detailed > /var/log/ultra/healthcheck-$(date +%Y%m%d).log

# Check critical services
if ! systemctl is-active --quiet ultra-orchestrator; then
  echo "CRITICAL: ultra-orchestrator service down" | mail -s "Ultra Alert" secops@company.com
fi

# Disk space check
df -h /var/reports | awk '$5+0 > 90 {print "WARNING: Reports partition >90% full"}'

# Database connection check
/usr/local/bin/ultra db ping || echo "CRITICAL: Database unreachable"

# Log rotation check
if [ $(find /var/log/ultra -name "*.log" -size +100M | wc -l) -gt 0 ]; then
  echo "WARNING: Large log files detected" | mail -s "Ultra Log Alert" secops@company.com
fi

# AI model freshness
ultra ai models list | grep -i "expired" && echo "WARNING: AI model expired"

# Add to cron: 0 6 * * * /opt/ultra/scripts/healthcheck.sh
```

### Weekly Maintenance Tasks
```bash
#!/bin/bash
# /opt/ultra/scripts/weekly-maintenance.sh

# 1. Clean up old reports (keep 90 days)
ultra cleanup old-reports --older-than 90d --dry-run
ultra cleanup old-reports --older-than 90d

# 2. Archive old evidence to S3 (keep 1 year local, 7 years S3)
ultra evidence archive --older-than 365d --to s3://ultra-evidence/long-term/ --dry-run
ultra evidence archive --older-than 365d --to s3://ultra-evidence/long-term/

# 3. Update AI models (from staging after validation)
ultra ai models update --channel staging --dry-run
# [Human] Validate staging models on sample data
ultra ai models update --channel production

# 4. Rotate logs and audit files
sudo logrotate -f /etc/logrotate.d/ultra-orchestrator
ulra audit compress --older-than 90d

# 5. Database maintenance
ultra db vacuum --full --analyze
ultra db reindex --concurrently

# 6. Check and notify about failed scans/incidents
ultra scans list --status failed --last-week | mail -s "Weekly Failed Scans" secops@company.com
ultra incidents list --status open --older-than 7d | mail -s "Stale Incidents" secops@company.com

# Add to weekly cron: 0 2 * * 0 /opt/ultra/scripts/weekly-maintenance.sh
```

### Monthly Tasks
```bash
#!/bin/bash
# /opt/ultra/scripts/monthly-maintenance.sh

# 1. Full system backup
ultra backup create --type full --compression gz --verify

# 2. Security audit
ultra audit compliance --check all --output html --email compliance@company.com

# 3. RBAC review
ultra role list --inactive-users > /tmp/inactive-users.txt
ultra user review --inactive-file /tmp/inactive-users.txt --disable

# 4. Third-party dependency updates
ultra dependencies check-updates
# Review and approve updates in staging first

# 5. Capacity planning report
ultra metrics report --period 30d --output /reports/capacity-$(date +%Y%m).pdf

# 6. License and subscription validation
ultra licenses check --all --notify finance@company.com

# Add to monthly cron: 0 3 1 * * /opt/ultra/scripts/monthly-maintenance.sh
```

### Quarterly Tasks
```bash
#!/bin/bash
# /opt/ultra/scripts/quarterly-maintenance.sh

# 1. Disaster recovery drill
ultra disaster-recovery drill \
  --scenario full-outage \
  --components db,config,models \
  --target-backup /backups/ultra/quarterly/$(date +%Y%m) \
  --verify-restore \
  --document-results

# 2. Penetration test coordination (provide evidence to pentesters)
ultra pentest provide-evidence --targets all-prod \
  --evidence-dir /var/lib/ultra/evidence/pentest-Q$(date +%q)-$(date +%Y) \
  --read-only-access \
  --ttl 30d

# 3. AI model retraining validation
ultra ai models validate-training-data \
  --period 90d \
  --feedback-from incident-reports,analyst-ratings \
  --report /reports/ai-model-validation-Q$(date +%q).pdf

# 4. Configuration security audit
ultra config audit --check security --output html --fix-drift

# 5. Update playbooks based on incident lessons learned
ultra playbook update-from-incidents --last-quarter

# 6. Vendor security assessment renewal
ultra vendors security-review --all --document /reports/vendor-security-Q$(date +%q).pdf

# Quarterly: Run manually after coordination with security team
```

### Emergency Procedures

#### Complete System Failure
```bash
# 1. Assess scope
ultra status --emergency
# If API down, check via SSH:
sudo systemctl status ultra-*

# 2. Check logs
sudo journalctl -u ultra-orchestrator -n 1000 --no-pager
sudo tail -100 /var/log/ultra/ultra.log

# 3. Restart in order
sudo systemctl restart postgresql
sudo systemctl restart ultra-db
sudo systemctl restart ultra-ai-engine
sudo systemctl restart ultra-orchestrator

# 4. If still failing, restore from backup
ultra system restore --backup /backups/ultra/full-20240301-0000.tar.gz \
  --verify \
  --emergency

# 5. If data loss suspected, contact support with:
# - ultra doctor --full --json output
# - /var/log/ultra/audit.log (last 10,000 lines)
# - Database dumps: sudo pg_dump ultra_db > ultra_db-dump-$(date).sql
```

#### Security Incident Involving Ultra
```bash
# If Ultra orchestrator itself compromised:
# 1. Isolate immediately
sudo iptables -I INPUT -p tcp --dport 8080 -j DROP
sudo iptables -I INPUT -p tcp --dport 9090 -j DROP

# 2. Preserve evidence
sudo cp -a /var/log/ultra /forensics/ultra-logs-$(date +%Y%m%d-%H%M%S)
sudo tar czf /forensics/ultra-filesystem-$(date +%Y%m%d-%H%M%S).tar.gz /opt/ultra

# 3. Capture memory
sudo dd if=/dev/mem of=/forensics/ultra-memory-$(date +%Y%m%d-%H%M%S).raw bs=1M

# 4. Revoke all credentials
ultra credentials revoke-all --emergency --notify secops

# 5. Restore from known-good backup
ultra system restore --from-backup /backups/ultra/full-trusted.tar.gz \
  --clean-install \
  --rotate-all-keys \
  --notify

# 6. Post-incident forensics
ultra incident create --type security --title "Ultra Orchestrator compromise" \
  --evidence-dir /forensics/ultra-* \
  --forensic-analysis required
```

## Policy Compliance

### Data Retention Policy
```bash
# Configure immutable retention schedules
ultra config set retention.audit_logs 2555d      # 7 years (SOX)
ultra config set retention.incident_data 3650d   # 10 years (PCI-DSS)
ultra config set retention.vulnerability_data 1095d  # 3 years
ultra config set retention.reports 2555d

# Enforce WORM (Write Once Read Many) for compliance data
ultra storage set-policy compliance-evidence \
  --worm-enabled \
  --retention 3650d \
  --path /var/lib/ultra/evidence/

# Automated cleanup verification
ultra cleanup verify --dry-run --report /reports/cleanup-plan-$(date +%Y%m).pdf
```

### GDPR and Privacy
```bash
# Right to be forgotten - data deletion
ultra privacy delete-user --user-id johndoe@example.com \
  --from-logs \
  --from-reports \
  --from-evidence \
  --anonymize-analytics \
  --generate-certificate

# Data export for GDPR requests
ultra privacy export-user --user-id johndoe@example.com \
  --format json,pdf \
  --include scans,incidents,reports \
  --deliver-to secure-ftp://gdpr-exports/

# Privacy impact assessment report
ultra privacy generate-pia --period 2024 --output /reports/PIA-2024.pdf
```

### ISO 27001 Controls
```bash
# Generate ISO 27001 evidence package
ultra compliance evidence collect \
  --framework iso-27001 \
  --controls A.9.1,A.9.2,A.12.2,A.12.4,A.16.1 \
  --auditor "KPMG" \
  --period 2024-01-01:2024-03-31 \
  --output /evidence/iso27001-A1-Q1-2024.zip \
  --signed-by cisoc@company.com
```

---
```

This SKILL.md provides comprehensive, specific documentation for Ultra Orchestrator with real commands, examples, dependencies, troubleshooting, and rollback procedures tailored to a security orchestration platform.