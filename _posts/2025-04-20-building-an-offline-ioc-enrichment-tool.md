---
title: "Building an Offline IOC Enrichment Tool"
date: 2025-04-20 09:10:00 +0800
categories: [Tools]
tags: [ioc, threat-intelligence, python, enrichment]
author: kynigoi
description: >
  A Python-based offline IOC enrichment tool for analysts operating in
  isolated or restricted environments.
severity: LOW
read_time: 6
featured: false
---

## The problem

Restricted environments often prevent internet-based IOC enrichment.

## Tool capabilities

- SHA256 lookups
- Domain categorization
- MITRE ATT&CK tagging

## Example usage

```bash
python enrich.py --ioc suspicious.exe
```

## Sample output

```text
Type      : SHA256
Risk      : HIGH
Family    : RedLine Stealer
```

## Source

Available on GitHub at
[github.com/unresolvedhost](https://github.com/unresolvedhost)