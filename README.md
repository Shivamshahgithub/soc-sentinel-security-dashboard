# SOC Sentinel — SOC Alert Triage & Investigation Assistant

A self-contained, browser-based cybersecurity portfolio application built with HTML, CSS and JavaScript. It uses **fictional security logs** and five deterministic security detection rules. The app does **not** use an AI model for detection, and findings are **not proof of malicious activity**.

## Start

1. Download `index.html` and open it in Chrome or Edge (or use the standalone `SOC_Alert_Triage_Assistant.html`).
2. Click **Load demo data** to review the 50-record built-in example.
3. Select an incident in **Alert investigations**; edit status, analyst severity, disposition, and add notes.
4. Review the activity timeline and use **Print / Save PDF** for an individual incident.
5. Use **Export incidents CSV**, **Backup JSON** and **Restore JSON** from Data management.

## Detection rules

| Rule | Logic | Initial severity |
|------|-------|------------------|
| R1 | Five or more failed logins from one IP in an inclusive 10-minute interval | High |
| R2 | Three or more failed logins followed by success for same user/IP within 10 minutes | High |
| R3 | Successful privileged login between 00:00 and 04:59 UTC | Medium |
| R4 | Malware event with `detected` outcome | Critical |
| R5 | Fictional watchlisted IP `203.0.113.200` | High |

Each incident maintains original rule severity alongside analyst-adjusted severity. Detection uses event timestamps in UTC. One rule finding can include multiple supporting events.

## Expected demonstration dataset results

- 50 source events; 48 valid; two rejected (EVT-049 invalid timestamp, EVT-050 missing username).
- Exactly five alerts: R1 => EVT-011 through EVT-015; R2 => EVT-021 through EVT-024; R3 => EVT-031; R4 => EVT-041; R5 => EVT-046 and EVT-047.
- Severity: 1 Critical, 3 High, 1 Medium.
- EVT-048 is not independently flagged.

## CSV schema

`event_id,timestamp_utc,source_ip,username,event_type,outcome,asset`

Accepted event types: `login`, `privileged_login`, `file_access`, `malware_detection`. Supported outcomes: `success`, `failure` for login/file access; `detected`, `clean` for malware detection.

## Tested

Run automated detection and validation regression tests with:

```bash
node test_logic.js
```

These tests verify demo import counts, rejected IDs, five triggered rules, matched event IDs, severity distribution, and that the benign control record is not independently flagged. JavaScript syntax was checked as well.

**Manual browser checks still recommended before publishing:** demo load and repeated import, search/filter/sort, editing notes and statuses, refresh persistence, JSON restore, CSV export in Excel, browser Print / Save PDF layout, reset confirmation, responsive design, and keyboard navigation. Mark results accurately after testing; do not represent them as already completed.

## Limitations and privacy

- The application is single-user and stores investigation data in the current browser only (`localStorage`). It is not an enterprise SIEM, tamper-proof audit trail, or suitable for production logs.
- Example timestamps and systems are fictional. All demo IPs are from documentation address ranges.
- Limit CSV uploads to 3 MB. Browser storage may fill, be cleared, or be unavailable in private browsing. Use JSON backups for portability.
- Charts are time-of-day aggregations in UTC, rather than a multi-day timeseries.
- The PDF feature opens a print-friendly individual incident report and relies on the browser's Save as PDF command.
- Local CSV input is expected to follow the documented schema. Malformed lines and unsupported event types are reported as validation errors.

## Suggested resume bullet

Developed a browser-based SOC alert triage dashboard with five explainable detection rules, synthetic CSV log ingestion, incident investigation workflows, activity timelines, browser-local persistence, and CSV/PDF reporting; validated alert logic against a 50-event synthetic test dataset.
