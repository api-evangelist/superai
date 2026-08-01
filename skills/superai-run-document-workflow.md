---
name: Run a document workflow and retrieve results
description: Start a SuperAI Flows execution against a flow, poll it to completion, and pull the extracted task outputs.
api: openapi/superai-flows-openapi-original.json
operations:
- list_flows_api_flows_get
- create_flow_execution_api_flow_executions_post
- get_flow_execution_api_flow_executions__flow_execution_id__get
- list_task_outputs_api_task_outputs_get
- get_task_output_api_task_outputs__task_output_id__get
- download_file_api_files_download_get
---

# Run a document workflow and retrieve results

Use the SuperAI Flows API to process a document through an existing flow and read the structured output.

## Auth
Send either a JWT bearer token (`Authorization: Bearer <token>`) or a service-account API key (`X-API-Key: saf_...`). Base URL is `https://flows.super.ai/api`.

## Steps
1. **Find the flow.** Call `list_flows_api_flows_get` and select the target flow id. List endpoints paginate with `page`/`page_size` (also `limit`/`offset`).
2. **Start an execution.** Call `create_flow_execution_api_flow_executions_post` with the flow id and inputs (document reference). Keep the returned `flow_execution_id`.
3. **Poll to completion.** Call `get_flow_execution_api_flow_executions__flow_execution_id__get` until the status is terminal. For production, prefer a Webhook Notification task over polling (see asyncapi/superai-flows-webhooks.yml).
4. **Read outputs.** Call `list_task_outputs_api_task_outputs_get` (filter by the execution) then `get_task_output_api_task_outputs__task_output_id__get` for detail.
5. **Fetch any files.** Output files come back as `gs://` URIs; resolve/download with `download_file_api_files_download_get`.

## Errors
Errors use a custom envelope `{"error":{"message","code","details"},"request_id"}`. Correlate with the `X-Request-ID` response header. Common codes: `validation_error` (422), `not_found` (404), `bad_request` (400), `user_info_not_found` (401). See errors/superai-problem-types.yml.
