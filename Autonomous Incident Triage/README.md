# Autonomous Incident Triage & Root Cause Analyzer

An AI-assisted incident analysis project built with Langflow and Google Gemini. The system receives incident reports through a webhook, retrieves relevant knowledge using RAG, checks CPU and RAM metrics through Prometheus, and generates an initial diagnosis with troubleshooting recommendations.

## Features

- Receives incident reports through a Langflow Webhook.
- Parses incident details such as incident ID, service, severity, and error message.
- Retrieves relevant context from a local knowledge base using RAG.
- Reads CPU and RAM metrics from Prometheus.
- Uses Google Gemini to produce an initial diagnosis and recommended next steps.
- Includes a sample JSON payload for testing.

## Workflow

1. An incident payload is sent to the Webhook.
2. The Parser extracts the incident information.
3. Knowledge Retrieve finds relevant information from the knowledge base.
4. Prometheus API requests provide CPU and RAM metrics.
5. The Prompt Template combines the incident, retrieved context, and available metrics.
6. Gemini generates the diagnosis, which is sent to Chat Output.

## Tech Stack

- Langflow
- Google Gemini
- Retrieval-Augmented Generation (RAG)
- Prometheus
- Python
- JSON
- Elasticsearch: tested separately with simulated logs; integration into the main diagnosis flow is not confirmed.

## Repository Structure

```text
autonomous-incident-triage/
├── Autonomous_Incident_Triage.json
├── sample_incident_payload.json
├── README.md
└── screenshots/
    ├── main-flow.png
    ├── diagnosis-result.png
    └── prometheus-metrics.png
```

The filenames above are suggested names. Use the actual exported flow filename and include only screenshots you have.

## Requirements

- A compatible Langflow installation.
- A Google Gemini API key configured in Langflow.
- Prometheus running and accessible from the machine where Langflow runs, if you want live CPU/RAM metrics.
- The knowledge base and sample files used by the flow.

## Getting Started

1. Clone or download this repository.
2. Open Langflow.
3. Import `Autonomous_Incident_Triage.json` into Langflow.
4. Configure the Gemini API key in Langflow.
5. Configure the Prometheus API Request URLs to match your environment.
6. Check that the knowledge base is available and that its ingestion has completed.
7. Send the contents of `sample_incident_payload.json` to the flow's Webhook endpoint using an HTTP POST request.
8. Inspect the diagnosis output.

> The Webhook endpoint and API key are specific to your Langflow installation. Do not publish your API key or other credentials.

## Example Test Payload

```json
{
  "incident_id": "INC-001",
  "source": "manual",
  "service": "payment-api",
  "severity": "high",
  "description": "Database connection timeout after 30 seconds",
  "error": "Database connection timeout",
  "message": "Database connection timeout after 30 seconds",
  "timestamp": "2026-10-09T19:00:00Z"
}
```

## Testing Notes

This project has been tested with simulated incident payloads, a local Prometheus setup, and a local knowledge base. The diagnosis is an AI-generated initial assessment and should be reviewed by a human before any remediation is performed.

Elasticsearch was tested separately with simulated incident logs. Its integration into the main Langflow diagnosis flow has not been confirmed, so it should be treated as experimental rather than a completed feature.

## Limitations

- The project is a prototype, not a production incident-response system.
- Prometheus metrics reflect the configured monitoring target; in the current test setup, this is a local Windows machine.
- Some evidence in diagnosis output may be simulated and should not be interpreted as confirmed production data.
- Diagnosis quality depends on the incident payload, available metrics, and knowledge-base content.
- Human review is required before making operational changes.

## Screenshots

Add screenshots of the actual Langflow canvas, diagnosis output, and Prometheus metrics to the `screenshots/` folder. Ensure screenshots do not reveal API keys, tokens, or private information.

## License

## License

This project was created for learning and portfolio purposes. No open-source license has been added yet.
