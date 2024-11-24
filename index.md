---

layout: col-sidebar
title: OWASP appsec-discovery
tags: example-tag
level: 2
type: code
pitch: A very brief, one-line description of your project

---

Service components:

* discovery_scanner - scheduled background parallel tasks for fetching projects, MRs info and code from Gitlab, scanning with semgrep, scoring results, calculating diffs and sending alerts;
* discovery_ui - React Admin crud for managing scoring rules;
* discovery_api - FastAPI for scoring rules crud;
* discovery_nginx - router for React static js and api in one origin
* discovery_db - storing code projects, branches, open mrs, scanning state and extracted objects

Workflow:

1. Appsec set risk scoring rules in discovery_ui, gitlab url, project prefixes and api token for scans, tg creds for alerts in env vars for discovery_scanner.
2. discovery_scanner fetch projects list for provided prefixes and add projects updated from last fetch to updateing branches and MRs queue.
3. discovery_scanner fetch info about updated branches and mrs and add new ones to clone code queue.
4. discovery_scanner clone code for main branches and source and target branches code for MRs, add successfully cloned to scan queue.
5. discovery_scanner extract assets objects, score risk for particular object fields, store objects into local database.
6. discovery_scanner calculate objects and fields diff for source and target branches in MRs, alert about new risky fields.
7. discovery_scanner clean code and scan results cache for processed tasks.
8. Appsec triage alerted changes in Telegram or Gitlab UIs.
9. Appsec connect discovery_db to analytics services like Grafana or Superset via Trino, setup business dashboards for vuln and risk prioritisation, thresholds and alerts

[https://github.com/dmarushkin/appsec-discovery|https://github.com/dmarushkin/appsec-discovery]

### Road Map
For 2025 planned:

* add extra assets discovery rules for popular ORMs, DTOs in Python, Go, Java, JS
* add extra integrations with other SVNs like github, bitbucket
* add predefined rules and dashboards
