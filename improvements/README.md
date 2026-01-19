# KQL Query Improvements

This folder contains enhanced versions of all KQL queries from the main repository, with significant improvements in documentation, functionality, and usability.

## 📋 Overview

All 20 KQL queries have been upgraded with:
- **Comprehensive documentation headers** with threat context and MITRE ATT&CK mappings
- **Parameterized configurations** for easy customization
- **Inline comments** explaining complex logic
- **Performance notes** and estimated execution times
- **Troubleshooting guides** for common issues
- **Investigation workflows** and follow-up queries
- **Response action recommendations**
- **Security best practices**

## 🗂️ Folder Structure

```
improvements/
├── tools_detection/          # Azure AD attack tool detection (8 files)
├── powershell/               # PowerShell forensics and recovery (4 files)
├── users/                    # User risk and authentication monitoring (3 files)
├── azure_diagnostics_logs/   # Azure infrastructure security (3 files)
├── anyfile_file_union_detection/ # Multi-source forensic hunting (1 file)
└── README.md                 # This file
```

## 🔍 What's Improved?

### 1. Standardized Documentation Headers

Every query now includes:
- **PURPOSE**: Clear explanation of what the query detects
- **MITRE ATT&CK**: Relevant techniques and tactics
- **THREAT CONTEXT**: Why this detection matters
- **DATA SOURCES**: Required tables and licensing
- **DETECTION METHOD**: How the query works
- **RECOMMENDATIONS**: What to do with findings
- **PERFORMANCE NOTES**: Expected execution time and optimization tips
- **KNOWN FALSE POSITIVES**: Common benign scenarios and rates

### 2. Parameterized Configurations

Queries now use `let` statements for easy customization:
```kql
// Example: device_code_graphrunner.kql
let lookbackPeriod = 30d;  // Adjust based on retention
let focusOnSuccessfulAuth = true;  // Filter to successful only
```

No more hardcoded values scattered throughout queries!

### 3. Enhanced Logic and Fixes

- **Fixed syntax errors** (e.g., `detect_azurehound.kql` line 9 division operator)
- **Completed incomplete queries** (e.g., `mfa_sweep_detection.kql` now has full UserAgent list)
- **Optimized filters** for better performance
- **Added risk scoring** and severity classification
- **Improved result enrichment** with actionable metadata

### 4. Comprehensive Investigation Support

Each query includes:
- **Troubleshooting section** for common issues
- **Example investigation workflows** with step-by-step guidance
- **Follow-up queries** for deeper analysis
- **Response action checklists** for incident response
- **Integration guidance** for SIEM/SOAR platforms

## 📚 Query Categories

### 🛡️ Tools Detection (8 queries)

Detects offensive security tools used in Azure AD/Microsoft 365 attacks:

| Query | Purpose | Key Improvement |
|-------|---------|-----------------|
| `token_tactics.kql` | TokenTactics tool detection via UserAgent | Added comprehensive threat context and correlation guidance |
| `detect_azurehound.kql` | AzureHound via Graph API patterns | **FIXED syntax error**, added confidence scoring |
| `device_code_graphrunner.kql` | Device code flow abuse | Added severity classification and mitigation guidance |
| `graphrunner_detection.kql` | GraphRunner via UserAgent | Consolidated UserAgent lists, added response actions |
| `detect_roadrecon.kql` | ROADrecon via Python UserAgent | Added baseline tuning and service account filtering |
| `detect_azhound_baseline.kql` | Simple AzureHound UserAgent detection | Enhanced with activity summarization |
| `mfa_sweep_detection.kql` | MFA fatigue/sweep attacks | **COMPLETED** with full UserAgent list and pattern analysis |
| `detecting_azurehound_unique_token.kql` | Token-specific investigation | Added forensic analysis workflow |
| `detect_azurehound_request_uri.kql` | URI pattern frequency analysis | Added endpoint categorization and mapping |

### 💻 PowerShell Forensics (4 queries)

Recovers PowerShell script content and tracks execution:

| Query | Purpose | Key Improvement |
|-------|---------|-----------------|
| `track_file.kql` | Multi-source PS file tracking | Added 6-source union with comprehensive timeline |
| `find_script_variable.kql` | Variable assignment recovery | Added credential/download command detection |
| `pwsh_logs.kql` | Script content recovery | Multi-method recovery with reconstruction |
| `detailed_pwsh.kql` | Transcription log recovery | Added content quality scoring and risk indicators |

### 👤 User Security (3 queries)

Monitors user authentication and risk:

| Query | Purpose | Key Improvement |
|-------|---------|-----------------|
| `risky_users.kql` | High-risk user detection | Added severity classification and response workflows |
| `pw_spray.kql` | Password spray detection | Added pattern analysis and rate calculation |
| `pass_spray-msgraph.kql` | Risky users + Graph API | Added data sensitivity scoring and exfiltration detection |

### ☁️ Azure Infrastructure (3 queries)

Monitors Azure resources for security issues:

| Query | Purpose | Key Improvement |
|-------|---------|-----------------|
| `malicious_log_events.kql` | SQL Database error monitoring | Added error classification and threat detection |
| `get_secrets.kql` | Key Vault secret access | Added access pattern analysis and bulk detection |
| `egress_data.kql` | Storage data egress monitoring | Added baseline guidance and exfiltration indicators |

### 🔬 Forensic Hunting (1 query)

Multi-source correlation for incident investigation:

| Query | Purpose | Key Improvement |
|-------|---------|-----------------|
| `lnk_file_union_hunt.kql` | LNK file forensic analysis | Added 6-source union with threat indicators and timeline |

## 🚀 Getting Started

### Prerequisites

- Azure AD Premium P1/P2 or Microsoft Entra ID P2 (for AAD logs)
- Microsoft 365 E5 or Graph Activity Logs license (for Graph queries)
- Microsoft Defender for Endpoint (for device telemetry)
- Azure Log Analytics workspace with appropriate data ingestion

### Usage

1. **Select a query** based on your investigation or monitoring needs
2. **Read the header documentation** to understand requirements and context
3. **Customize parameters** in the configuration section (typically at the top)
4. **Run the query** in Azure Log Analytics, Microsoft Sentinel, or Defender portal
5. **Review results** and follow investigation workflows as needed
6. **Implement response actions** based on findings

### Example: Using token_tactics.kql

```kql
// 1. Open the file and review the header
// 2. Customize parameters if needed:
let lookbackPeriod = 30d;  // Change to 7d for recent activity only
let minConfidence = "high";

// 3. Run the query in your Log Analytics workspace
// 4. Review results for ThreatIndicator = "TokenTactics UserAgent Detected"
// 5. Use follow-up queries to investigate flagged users
```

## 📊 Performance Considerations

- **Light queries** (< 10 seconds): Most tool detection queries, user queries
- **Medium queries** (10-30 seconds): Graph API analysis, multi-day aggregations
- **Heavy queries** (30-90 seconds): Multi-source union queries, PowerShell recovery

**Optimization tips:**
- Reduce `lookbackPeriod` for faster results during initial testing
- Use specific filters (device names, user IDs) for targeted investigations
- Run heavy queries during off-peak hours for historical analysis
- Consider scheduled queries for regular monitoring vs. on-demand investigation

## 🔄 Migration from Original Queries

To migrate from the original queries to improved versions:

1. **Identify your query** in the original folder structure
2. **Find the improved version** in this folder (same name, same subfolder)
3. **Review new parameters** and customize for your environment
4. **Update any automation** (alerts, playbooks) to use the new query
5. **Leverage new features** like investigation workflows and response actions

**Note:** Improved queries maintain compatibility with original data sources but may return additional enriched fields.

## 🛠️ Troubleshooting

### Common Issues

**"No results found"**
- Check if data sources are properly configured and ingesting
- Verify lookback period covers the time range of interest
- Confirm required licenses (P2, E5, MDE) are active

**"Query timeout"**
- Reduce lookback period (e.g., from 30d to 7d)
- Add more specific filters (device, user, resource)
- Run during off-peak hours

**"Too many results"**
- Increase threshold parameters (e.g., `minFailedAttempts`)
- Enable service account filtering where available
- Focus on high-severity findings first

### Getting Help

- Review the **Troubleshooting section** within each query
- Check **Example Investigation Workflows** for guidance
- Refer to **Follow-up Queries** for deeper analysis

## 🔐 Security Best Practices

1. **Least Privilege**: Only grant query access to security team members
2. **Sensitive Data**: Queries may return PII (emails, IPs) - handle appropriately
3. **Retention**: Consider data retention requirements for compliance
4. **Alerting**: Implement automated alerts for critical findings
5. **Documentation**: Document custom parameters and thresholds in your environment
6. **Testing**: Test queries in non-production before deploying alerts
7. **Maintenance**: Regularly update UserAgent lists and IOC patterns

## 📈 Integration

### Azure Sentinel
- Import queries as Analytics Rules for automated detection
- Use Workbooks for visualization and dashboards
- Integrate with Incident Response playbooks

### Microsoft Defender
- Run queries in Advanced Hunting
- Create custom detection rules
- Export findings to Incidents

### Third-Party SIEM
- Export queries via Azure Monitor
- Use Logic Apps for integration
- Format results for your SIEM schema

## 📝 Changelog

### Version 2.0 (2026-01-19)
- Complete rewrite of all 20 queries with comprehensive improvements
- Added standardized documentation headers
- Implemented parameterized configurations
- Fixed syntax errors and completed incomplete queries
- Added investigation workflows and response actions
- Enhanced with MITRE ATT&CK mappings
- Included performance notes and troubleshooting guides

## 🤝 Contributing

To improve these queries further:
1. Test in your environment and document findings
2. Add new UserAgent patterns or IOCs as they emerge
3. Contribute optimizations or additional investigation workflows
4. Share false positive patterns for better filtering
5. Document integration examples with specific SIEM platforms

## 📚 Additional Resources

- [Microsoft Sentinel Documentation](https://learn.microsoft.com/azure/sentinel/)
- [KQL Quick Reference](https://learn.microsoft.com/azure/data-explorer/kql-quick-reference)
- [MITRE ATT&CK Framework](https://attack.mitre.org/)
- [Microsoft Defender for Endpoint](https://learn.microsoft.com/microsoft-365/security/defender-endpoint/)
- [Azure AD Identity Protection](https://learn.microsoft.com/azure/active-directory/identity-protection/)

---

**Note:** These improved queries are designed for production use but should be tested and tuned in your specific environment. Always validate findings before taking action, and document customizations for your organization's needs.
