---
name: Provision a service account and API key
description: Create a SuperAI Flows service account for machine-to-machine access and issue an API key.
api: openapi/superai-flows-openapi-original.json
operations:
- create_service_account_api_service_accounts_post
- list_service_accounts_api_service_accounts_get
- create_service_account_api_key_api_service_accounts__service_account_id__api_keys_post
- list_service_account_api_keys_api_service_accounts__service_account_id__api_keys_get
- delete_api_key_api_service_accounts__service_account_id__api_keys__api_key_id__delete
---

# Provision a service account and API key

Set up unattended, non-user credentials for integrating the SuperAI Flows API.

## Auth
Authenticate the setup call with a JWT bearer token (an admin user). The issued key is used later as `X-API-Key: saf_...`. Base URL `https://flows.super.ai/api`.

## Steps
1. **Create the service account.** Call `create_service_account_api_service_accounts_post`; keep the returned `service_account_id`. (`list_service_accounts_api_service_accounts_get` enumerates existing ones.)
2. **Issue a key.** Call `create_service_account_api_key_api_service_accounts__service_account_id__api_keys_post`. Store the `saf_...` secret securely — it is shown once.
3. **Manage keys.** List with `list_service_account_api_keys_...`; revoke with `delete_api_key_...`.

## Errors
Custom envelope with `request_id`; `conflict` (409) on duplicate name, `user_info_not_found` (401) if the admin token is invalid/expired. See errors/superai-problem-types.yml.
