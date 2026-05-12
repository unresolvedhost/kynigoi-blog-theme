---
title: "Threat Hunting with Sysmon and Sigma Rules"
date: 2025-04-11 08:30:00 +0800
categories: [Threat Hunting]
tags: [sysmon, sigma, detection-engineering, windows, hunting]
author: kynigoi
description: >
  Using Sysmon telemetry and Sigma rules to rapidly identify suspicious
  process execution and lateral movement activity in enterprise networks.
severity: MEDIUM
read_time: 7
featured: false
---

## Why Sysmon matters

Native Windows logs often miss the context analysts need during
investigations. Sysmon fills those gaps with detailed process, network,
and image load telemetry.

## Detection workflow

- Collect Sysmon telemetry centrally
- Normalize logs into ECS or CIM
- Apply Sigma detections
- Tune noisy process relationships

## Example Sigma rule

```yaml
title: Suspicious PowerShell Download

logsource:
  product: windows
  service: sysmon

detection:
  selection:
    EventID: 1
    Image|endswith: '\powershell.exe'
```

## Useful Sysmon events

| Event ID | Purpose |
|----------|---------|
| 1 | Process creation |
| 3 | Network connections |
| 22 | DNS queries |

## Source

Available on GitHub at
[github.com/unresolvedhost](https://github.com/unresolvedhost)