# Autonomous Incident Triage & Root Cause Analyzer

An AI-assisted incident triage prototype built with Langflow. It receives an incident report, retrieves related guidance from a local knowledge base using RAG, checks Prometheus CPU/RAM metrics as supporting evidence, and generates an initial root-cause assessment with troubleshooting recommendations.

The current flow has been tested with Gemini. Other LLM providers may be usable if supported by the Langflow components and configured correctly.

## What It Does

- Receives incident reports through a Langflow Webhook.
- Parses fields such as incident ID, service, severity, and error message.
- Retrieves relevant context from a local knowledge base using RAG.
- Retrieves CPU and RAM metrics from the configured Prometheus instance.
- Combines the incident details, retrieved knowledge, and available metrics in a prompt.
- Uses an LLM to produce an initial diagnosis and recommended troubleshooting steps.

**Main purpose:** help with initial incident analysis and troubleshooting. CPU/RAM monitoring is supporting evidence, not the project's sole purpose.

## Workflow

1. An incident payload is sent to the Webhook.
2. The Parser extracts the incident information.
3. Knowledge Retrieve searches the local knowledge base for related information.
4. Prometheus API requests retrieve the configured CPU and RAM metrics.
5. The Prompt Template combines the incident, RAG context, and metrics.
6. The LLM generates an initial diagnosis, which is sent to Chat Output.

## Technology

- Langflow
- LLM (Gemini in the current tested configuration)
- Retrieval-Augmented Generation (RAG)
- Prometheus
- Python
- JSON

### Elasticsearch status

Elasticsearch was tested separately using simulated incident logs, but its integration into the main diagnosis flow has not been confirmed. It is therefore not listed as a completed feature of the main flow.

## Repository Structure

Example structure (adjust filenames to match the files you actually upload):

```text
autonomous-incident-triage/
├── <your-exported-Langflow-flow>.json
├── sample_incident_payload.json
├── README.md
└── screenshots/
    ├── main-flow.png
    └── diagnosis-result.png
```

Only include files and screenshots that are actually present in your repository. Remove any placeholder names from this example if they do not match your files.

## Requirements

- A compatible Langflow installation.
- An LLM provider configured in Langflow. The current tested configuration uses Gemini.
- Prometheus running and reachable from the machine where Langflow runs, if live CPU/RAM metrics are required.
- The knowledge base used by the flow, with ingestion completed.
- A sample incident payload to test the Webhook.

## Getting Started

1. Clone or download this repository.
2. Open Langflow.
3. Import your exported flow JSON file into Langflow.
4. Configure the LLM provider and credentials in your own Langflow environment.
5. Update the Prometheus API Request URLs if your Prometheus address differs from the original setup.
6. Make sure the knowledge base is available and its data has been ingested.
7. Send the sample payload below to the Webhook endpoint using an HTTP `POST` request.
8. Inspect the component outputs or the final diagnosis produced by the flow.

The Webhook URL and any API key are specific to your Langflow installation. Do not commit API keys, passwords, tokens, or other credentials to GitHub.

## Example Incident Payload

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

Use the field names expected by the Parser template in your flow. If your Parser uses different field names, update the sample payload to match it.

## Testing Notes

This project is a prototype tested with simulated incident data, a local Prometheus setup, and a local knowledge base. In the current test setup, Prometheus metrics represent the configured local Windows machine.

The LLM's output is an initial assessment, not proof of the root cause. Review the incident details, logs, metrics, and recommendations before taking operational action.

## Limitations

- This is a prototype, not a production incident-response system.
- CPU/RAM metrics depend on the Prometheus target configured by the user.
- The diagnosis depends on the quality of the incident payload, knowledge-base content, and available metrics.
- Elasticsearch integration with the main diagnosis flow is not confirmed.
- Human review is recommended before applying remediation steps.

## Screenshots

You can add screenshots of the actual Langflow canvas and diagnosis output to a `screenshots/` folder. Make sure screenshots do not reveal API keys, tokens, or private information.

## License

No open-source license has been added yet. The repository is public, but that does not automatically grant permission to reuse or redistribute its contents.
