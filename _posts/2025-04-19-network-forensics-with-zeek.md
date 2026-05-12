---
title: "Network Forensics with Zeek"
date: 2025-04-19 10:45:00 +0800
categories: [Network Forensics]
tags: [zeek, network-forensics, dns, traffic-analysis]
author: kynigoi
description: >
  Using Zeek logs for network forensics, threat hunting, and anomaly
  detection in enterprise environments.
severity: MEDIUM
read_time: 9
featured: true
---

## Why Zeek

Zeek transforms packets into structured security telemetry.

## Key log types

| Log | Purpose |
|----|---------|
| conn.log | Network sessions |
| dns.log | DNS activity |
| ssl.log | TLS metadata |

## Hunting example

```bash
cat dns.log | zeek-cut query answers
```

## Source

Available on GitHub at
[github.com/unresolvedhost](https://github.com/unresolvedhost)