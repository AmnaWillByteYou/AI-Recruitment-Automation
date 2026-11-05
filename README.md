# AI-Assisted Recruitment Management System (n8n)

A modular n8n automation project for application intake, evidence-based résumé assessment, recruiter-controlled decisions, audit logging, daily pipeline reporting, and workflow error notifications.

> **Project status: importable implementation draft — not a verified production deployment.** The exported workflows have not been executed end-to-end in a live n8n workspace. Credentials, spreadsheet mappings, node-version compatibility, and all branches must be configured and tested before use.

## Why this project exists

Recruitment pipelines often involve repetitive intake, document handling, status updates, and reporting. This project demonstrates how those operational steps can be coordinated with n8n while keeping hiring decisions under human control.

## Included workflows

| File | Purpose | Main capabilities |
|---|---|---|
| `01-application-intake.json` | Application intake and assessment | Form intake, PDF text extraction, input validation, candidate ID generation, duplicate flagging, AI evidence assessment, weighted rubric calculation, Sheets record creation, receipt email |
| `02-human-review-and-audit.json` | Recruiter review | Candidate lookup, validated recruiter decision, pipeline status update, append-only review event, human-approved candidate communication |
| `03-daily-pipeline-analytics.json` | Daily reporting | Scheduled pipeline counts, average advisory score, duplicate/pending counts, email report |
| `04-central-error-monitor.json` | Error monitoring | Receives workflow errors and sends a concise alert without forwarding résumé contents |

## Architecture

See [`docs/architecture.md`](docs/architecture.md) for the data flow and component responsibilities.

## Key design decisions

- **Human-in-the-loop:** AI output is advisory. It does not shortlist or reject candidates.
- **Evidence-based assessment:** ratings must be tied to job-related requirements and résumé evidence. Missing information is recorded as “not evidenced,” not treated as proof of inability.
- **Structured output:** an AI response is parsed and validated before a score is calculated.
- **Weighted rubric:** essential skills 35%, relevant experience 25%, role qualifications 20%, evidence quality 10%, preferred skills 10%.
- **Auditability:** recruiter decisions update the candidate pipeline and create a separate review event.
- **Privacy-aware alerts:** error notifications avoid including résumé text or unnecessary candidate details.

The 0–100 score is a document-evidence signal, not a probability of job performance or a measure of a candidate's worth.

## Requirements

- An n8n workspace with versions compatible with the exported node types (verify node versions after import).
- Google Sheets credentials and two spreadsheets with the headers in `docs/`.
- An AI provider credential that supports the operation configured in the AI nodes. n8n AI Gateway credit support can vary by operation; do not assume every model operation is covered.
- Gmail credentials for email nodes.
- A test mailbox and synthetic résumé PDFs for validation.

## Privacy and responsible use

This project processes personal information such as names, contact details, and résumés. Use synthetic data for demos. For any real deployment, obtain appropriate authorization, restrict access, define retention/deletion rules, protect candidate records, and provide a way to correct inaccurate information. Do not infer or score protected characteristics or demographic proxies. Human reviewers must independently evaluate evidence and make all hiring decisions.

## Known limitations

- The duplicate check scans the Google Sheet and is not optimized for large datasets.
- PDF text extraction may not work on scanned/image-only PDFs; OCR is not included.
- The recruiter interface is an n8n form, not a dedicated HR dashboard.
- Email delivery and AI-provider compatibility depend on external credentials and account configuration.
- The workflows require live configuration and end-to-end testing; import success alone does not prove correct operation.

## Repository layout

```text
ai-recruitment-automation/
├── workflows/                 # Four importable n8n workflow exports
├── docs/
│   ├── architecture.md
│   ├── setup-guide.md
│   ├── candidate-data-headers.csv
│   └── review-audit-headers.csv
├── .gitignore
├── LICENSE
└── README.md
```
