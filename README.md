# Autonomous Incident Triage & Root Cause Analyzer

An AI-assisted incident triage and root-cause analysis prototype built with Langflow. It accepts incident reports, combines them with relevant information from a local knowledge base (RAG) and available diagnostic evidence, then uses a large language model (LLM) to produce an initial assessment and troubleshooting recommendations.

**The project is not limited to CPU and RAM monitoring.** CPU and RAM are the example metrics used in the current prototype to demonstrate how monitoring data can support incident analysis. The same approach can be extended to other evidence sources, such as application logs, database health, API response times, network metrics, and service status, when those sources are integrated and tested.

## Project Goal

Incident investigation often requires checking error reports, previous incidents, and system metrics across different tools. This project explores how Langflow and an LLM can bring some of that information together to help a user understand an incident more quickly.

The system provides an initial diagnosis and suggested next steps. It does not independently prove the root cause or automatically fix production incidents.

## How It Works

1. **Incident input:** A report is sent to the Langflow Webhook.
2. **Parsing:** The Parser extracts incident details such as the incident ID, service, severity, and error message.
3. **Knowledge retrieval (RAG):** The flow searches the local knowledge base for related guidance or examples.
4. **Monitoring evidence:** API Request nodes query Prometheus for the CPU and RAM metrics configured in the current prototype.
5. **Prompt assembly:** The Prompt Template combines the incident report, retrieved knowledge, and available metrics.
6. **LLM analysis:** The configured language model generates an initial incident summary, possible root-cause hypotheses, and troubleshooting recommendations.
7. **Result:** Chat Output displays the model's response for human review.

## Current Prototype Scope

### Implemented and tested in the current setup

- Receiving an incident report through a Langflow Webhook.
- Parsing incident information with the Parser.
- Retrieving relevant context from the local knowledge base using RAG.
- Querying the configured Prometheus instance for CPU and RAM metrics.
- Combining available inputs in the Prompt Template and generating an initial diagnosis with the configured LLM.

### Example used in this project

The current demonstration uses a database-connection incident for a sample service such as `payment-api`. CPU and RAM are included as supporting monitoring signals. They are not the incident categories the system is restricted to, and they do not necessarily explain a database or application failure on their own.

### Experimental / not confirmed in the main flow

Elasticsearch was tested separately with simulated incident logs, but its integration into the main diagnosis flow has not been confirmed. For that reason, Elasticsearch is not described as a completed feature of the main flow.

## Technology Stack

- **Langflow** — visual workflow orchestration.
- **LLM (Gemini in the current tested configuration)** — generates the initial analysis and recommendations.
- **RAG / Langflow Knowledge** — retrieves relevant context from the local knowledge base.
- **Prometheus** — supplies the CPU and RAM metrics used in the current example.
- **Python and JSON** — used for supporting scripts and incident payloads.

The design is not inherently tied to Gemini. Another LLM provider may be used if it is supported by the Langflow components and configured correctly. Gemini is named here because it is the model used in the current tested configuration.

## Repository Contents

Example structure; adjust the filenames to match the files actually uploaded to this repository.

```text
autonomous-incident-triage/
├── <exported-Langflow-flow>.json
├── sample_incident_payload.json
├── README.md
└── screenshots/
    ├── main-flow.png
    └── diagnosis-result.png
```

Only include files and screenshots that actually exist in the repository. Replace the placeholder flow filename with the actual exported JSON filename.

## Requirements

- A compatible Langflow installation.
- An LLM provider configured in Langflow. The current tested setup uses Gemini.
- Prometheus running and reachable from the machine running Langflow if you want to retrieve the example CPU/RAM metrics.
- The local knowledge base used by the flow, with its content ingested.
- A sample incident payload matching the fields expected by the Parser template.

## Setup and Testing

1. Clone or download this repository.
2. Start Langflow and import the exported flow JSON.
3. Configure the LLM provider and credentials in your own Langflow environment.
4. Confirm that the knowledge base exists and its ingestion has completed.
5. Update the Prometheus API Request URLs if your Prometheus address differs from the original setup.
6. Copy the Webhook endpoint from your own Langflow flow.
7. Send a test incident payload to that endpoint using an HTTP `POST` request and the authentication required by your Langflow configuration.
8. Inspect the Parser output and the final LLM diagnosis.

Webhook URLs, API keys, and local service addresses may differ between installations. Never commit API keys, passwords, access tokens, or other secrets to GitHub.

## Example Incident Payload

Use this as a starting example only. The exact field names must match the Parser template configured in the flow.

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

## Example Output

Depending on the supplied incident, knowledge-base content, and available metrics, the LLM may return:

- An incident summary and severity assessment.
- Possible root causes, clearly treated as hypotheses.
- Evidence available from the input, RAG context, and monitoring results.
- Recommended troubleshooting and verification steps.
- Missing information that should be collected before acting.

The exact output is generated by the model and can vary between runs.

## Limitations and Safety

- This is a prototype, not a production incident-response platform.
- CPU and RAM are example monitoring metrics in the current setup; other signals require additional integration and testing.
- Prometheus results reflect the target configured by the user. In the current local setup, they represent the configured local Windows machine, not a production server unless explicitly configured that way.
- A model-generated root cause is a hypothesis, not confirmed proof.
- Results depend on the incident payload, knowledge-base quality, metrics availability, and model configuration.
- Human review is recommended before applying troubleshooting or remediation steps.
- Elasticsearch integration into the main diagnosis flow is not confirmed.

## Screenshots

Add screenshots of the actual Langflow canvas and diagnosis result to a `screenshots/` folder. Check that they do not reveal API keys, tokens, private URLs, or other sensitive information.

## License

No open-source license has been added yet. A public repository does not automatically grant others permission to reuse or redistribute its contents.
