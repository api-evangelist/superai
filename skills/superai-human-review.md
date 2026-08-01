---
name: Handle a human review task
description: Create, start, and complete a human-in-the-loop review task within a SuperAI Flows execution.
api: openapi/superai-flows-openapi-original.json
operations:
- create_human_review_task_api_human_review_tasks_post
- list_human_review_tasks_api_human_review_tasks_get
- start_human_review_task_api_human_review_tasks__task_id__start_post
- complete_human_review_task_api_human_review_tasks__task_id__complete_post
- unstart_human_review_task_assignment_api_human_review_tasks__task_id__unstart_post
---

# Handle a human review task

Route low-confidence extractions to a person and fold their correction back into the flow.

## Auth
JWT bearer token or `X-API-Key` service-account key. Base URL `https://flows.super.ai/api`.

## Steps
1. **Create the review task.** Call `create_human_review_task_api_human_review_tasks_post` with the flow execution/task reference and the assigned users.
2. **List assigned work.** Reviewers find their queue with `list_human_review_tasks_api_human_review_tasks_get`.
3. **Start.** Call `start_human_review_task_api_human_review_tasks__task_id__start_post` to claim it (use `unstart_human_review_task_assignment_...` to release).
4. **Complete.** Submit the reviewed result with `complete_human_review_task_api_human_review_tasks__task_id__complete_post`; the flow resumes and super.AI learns from the correction.

## Errors
Custom error envelope with `request_id`; `conflict` (409) if a task is already started/completed, `not_found` (404) for an unknown task id. See errors/superai-problem-types.yml.
