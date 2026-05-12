---
title: "Creating Splunk Dashboards for SOC Teams"
date: 2025-04-17 15:30:00 +0800
categories: [Splunk]
tags: [splunk, soc, dashboards, siem]
author: kynigoi
description: >
  Building operational Splunk dashboards that help SOC analysts rapidly
  identify threats and triage incidents.
severity: MEDIUM
read_time: 7
featured: true
---

## Dashboard goals

Good SOC dashboards should prioritize:

- Visibility
- Fast triage
- Minimal noise

## Example SPL

```spl
index=wineventlog EventCode=4625
| stats count by TargetUserName
```

## Recommended panels

| Panel | Purpose |
|-------|---------|
| Failed logons | Brute force visibility |
| DNS spikes | Beaconing |

## Source

Available on GitHub at
[github.com/unresolvedhost](https://github.com/unresolvedhost)