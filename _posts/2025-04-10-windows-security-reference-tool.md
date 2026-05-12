---
title: "Windows Security Reference Tool: Sysmon & Event ID Lookup"
date: 2025-04-10 09:00:00 +0800
categories: [Tools]
tags: [sysmon, windows-events, splunk, elastic, python, open-source]
author: kynigoi
description: >
  A fast CLI and web tool for looking up Windows Security and Sysmon
  Event IDs with automatic SIEM query generation for Splunk and Elastic.
  Built to cut analyst lookup time during live investigations.
severity: LOW
read_time: 6
featured: true
---

## The problem

During a live incident, looking up what Sysmon Event ID 22 means, what
fields it emits, and then hand-writing a Splunk or Elastic query for it
costs time. Multiply that across a 20-event investigation and you've
lost 30 minutes to documentation lookups.

## What it does

**Windows Security Reference Tool** is a local-first CLI and optional
Streamlit web UI that gives you:

- Instant lookup of any Windows Security Event ID (1–5000) or Sysmon
  Event ID (1–29)
- Field-level descriptions for each event
- One-click SIEM query generation for Splunk SPL, Elastic KQL, and EQL
- Correlation suggestions — related event IDs worth hunting alongside

## Usage

```bash
# Install
pip install -r requirements.txt

# CLI lookup
python wsr.py lookup --id 4624 --siem splunk

# Output
Event ID : 4624 — An account was successfully logged on
Category : Logon
Fields   : SubjectUserName, TargetUserName, LogonType, IpAddress ...

Generated SPL:
index=wineventlog EventCode=4624
| table _time, SubjectUserName, TargetUserName, LogonType, IpAddress
| where LogonType IN (3, 10)

# Web UI
streamlit run app.py
```

## Sysmon quick reference

| Event ID | Event | Key fields |
|----------|-------|------------|
| 1 | Process creation | CommandLine, ParentImage, Hashes |
| 3 | Network connection | DestinationIp, DestinationPort, Image |
| 7 | Image loaded | ImageLoaded, Signed, Signature |
| 10 | Process access | SourceImage, TargetImage, GrantedAccess |
| 22 | DNS query | QueryName, QueryResults, Image |
| 25 | Process tampering | Image, Type |

## Query generation example

```python
def generate_kql(event_id: int) -> str:
    meta = EVENT_DB[event_id]
    fields = ", ".join(meta["fields"][:6])
    return (
        f'event.code: "{event_id}" and '
        f'event.provider: "{meta["provider"]}"\n'
        f'// Key fields: {fields}'
    )
```

## Roadmap

- Sigma rule export for each event ID
- ATT&CK technique mapping per event
- Offline mode with bundled event database

## Source

Available on GitHub at
[github.com/unresolvedhost](https://github.com/unresolvedhost)