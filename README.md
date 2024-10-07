# TMV1 Mock

TMV1 Mock is a HTTP-based mock server built using WireMock, designed to simulate the TrendMicro V1 API. It allows developers and testers to easily replicate different responses from the API without making actual API calls.

## Setup
You will need to have WireMock and Maven installed on your system. Additionally, the pytmv1 python library is needed to interact with the mock server. Ensure that you have these set up before proceeding.

## Adding Additional Mock Responses
If you would like to add more mock responses, follow these steps:

0. **Check the API Docs**: [API Docs](https://automation.trendmicro.com/xdr/api-v3)
1. **Run Wiremock on Docker**: docker run -dit -p 127.0.0.1:8080:8080 --name tmv1mock -v $PWD:/home/wiremock wiremock/wiremock:latest –verbose
2. **Identify the Request**: Determine the API endpoint, HTTP method, and any necessary parameters or headers that your new mock response will react to.
3. **Create the Response**: Define the status code, body, and headers that your mock response will return.
4. **Register the Stub**: Using WireMock's API, you can register your new stub. This will typically involve adding a new file in the `mappings` directory.
5. **Test New Stubs**: Restart the WireMock Docker, and run the action via that pytmv1 client that has been initialized with the correct localhost params
6. **Rebuild the Project**: After adding your new stub, you will need to rebuild the project with Maven by running the command `mvn clean package`.
7. **Upload New Code to Server**: SCP the new mock tar into the server /tmp/
8. **Restart the Mock Server**: You'll then need to restart the mock server, so it picks up the new stub. You can do this with the command `docker stop apimock && docker start apimock`.

## Example Usage

Here's a basic example of how to interact with the mock server using the pytmv1 library:

```python
import pytmv1
client = pytmv1.client("AppName", "API-KEY", "https://tmv1-mock.trendmicro.com/")
client.get_alert_details(alert_id="bad_request")
```

In this example, we initialize a client with the parameters appname, apikey, and url. We then call an action method `get_alert_details()` with a specific parameter `alert_id`.

The mock server is set up to respond based on the specific parameters provided. For example, if a random string is provided to the `alert_id` parameter, a 200 response will be gotten from the mock. However, if "bad_request" or "server_error" is passed as the `alert_id` parameter, a 400 or 500 response will be gotten respectively.

In some cases, since some actions do not require parameters, the API-KEY is checked by WireMock for a specific string to determine the response.

Below is the full list of the currently available parameters to produce the various available responses:

| Action                                 | Response | Parameter                                                             | Comment                         |
|----------------------------------------|----------|-----------------------------------------------------------------------|---------------------------------|
| disable_account                        | 207-202  | accountName == random string                                          |                                 |
| disable_account                        | 207-400  | accountName == 'action_not_supported'                                 |                                 |
| disable_account                        | 207-400  | accountName == 'fields_not_found'                                     |                                 |
| disable_account                        | 207-400  | accountName == 'invalid_field_format'                                 |                                 |
| disable_account                        | 207-400  | accountName == 'target_not_found'                                     |                                 |
| disable_account                        | 207-400  | accountName == 'task_duplication'                                     |                                 |
| disable_account                        | 207-403  | accountName == 'access_denied_no_scope'                               |                                 |
| disable_account                        | 207-403  | accountName == 'feature_disabled'                                     |                                 |
| disable_account                        | 207-403  | accountName == 'insufficient_permissions'                             |                                 |
| disable_account                        | 207-403  | accountName == 'unsupported_response'                                 |                                 |
| disable_account                        | 207-500  | accountName == 'internal_server_error'                                |                                 |
| disable_account                        | 400      | accountName == 'invalid_format'                                       |                                 |
| disable_account                        | 403      | accountName == 'access_denied'                                        |                                 |
| disable_account                        | 500      | accountName == 'server_error'                                         |                                 |
| enable_account                         | 207-202  | accountName == random string                                          |                                 |
| enable_account                         | 207-400  | accountName == 'action_not_supported'                                 |                                 |
| enable_account                         | 207-400  | accountName == 'fields_not_found'                                     |                                 |
| enable_account                         | 207-400  | accountName == 'invalid_field_format'                                 |                                 |
| enable_account                         | 207-400  | accountName == 'target_not_found'                                     |                                 |
| enable_account                         | 207-400  | accountName == 'task_duplication'                                     |                                 |
| enable_account                         | 207-403  | accountName == 'access_denied_no_scope'                               |                                 |
| enable_account                         | 207-403  | accountName == 'feature_disabled'                                     |                                 |
| enable_account                         | 207-403  | accountName == 'insufficient_permissions'                             |                                 |
| enable_account                         | 207-403  | accountName == 'unsupported_response'                                 |                                 |
| enable_account                         | 207-500  | accountName == 'internal_server_error'                                |                                 |
| enable_account                         | 400      | accountName == 'invalid_format'                                       |                                 |
| enable_account                         | 403      | accountName == 'access_denied'                                        |                                 |
| enable_account                         | 500      | accountName == 'server_error'                                         |                                 |
| reset_password_account                 | 207-202  | accountName == random string                                          |                                 |
| reset_password_account                 | 207-400  | accountName == 'action_not_supported'                                 |                                 |
| reset_password_account                 | 207-400  | accountName == 'fields_not_found'                                     |                                 |
| reset_password_account                 | 207-400  | accountName == 'invalid_field_format'                                 |                                 |
| reset_password_account                 | 207-400  | accountName == 'target_not_found'                                     |                                 |
| reset_password_account                 | 207-400  | accountName == 'task_duplication'                                     |                                 |
| reset_password_account                 | 207-403  | accountName == 'access_denied_no_scope'                               |                                 |
| reset_password_account                 | 207-403  | accountName == 'feature_disabled'                                     |                                 |
| reset_password_account                 | 207-403  | accountName == 'insufficient_permissions'                             |                                 |
| reset_password_account                 | 207-403  | accountName == 'unsupported_response'                                 |                                 |
| reset_password_account                 | 207-500  | accountName == 'internal_server_error'                                |                                 |
| reset_password_account                 | 400      | accountName == 'invalid_format'                                       |                                 |
| reset_password_account                 | 403      | accountName == 'access_denied'                                        |                                 |
| reset_password_account                 | 500      | accountName == 'server_error'                                         |                                 |
| sign_out_account                       | 207-202  | accountName == random string                                          |                                 |
| sign_out_account                       | 207-400  | accountName == 'action_not_supported'                                 |                                 |
| sign_out_account                       | 207-400  | accountName == 'fields_not_found'                                     |                                 |
| sign_out_account                       | 207-400  | accountName == 'invalid_field_format'                                 |                                 |
| sign_out_account                       | 207-400  | accountName == 'target_not_found'                                     |                                 |
| sign_out_account                       | 207-400  | accountName == 'task_duplication'                                     |                                 |
| sign_out_account                       | 207-403  | accountName == 'access_denied_no_scope'                               |                                 |
| sign_out_account                       | 207-403  | accountName == 'feature_disabled'                                     |                                 |
| sign_out_account                       | 207-403  | accountName == 'insufficient_permissions'                             |                                 |
| sign_out_account                       | 207-403  | accountName == 'unsupported_response'                                 |                                 |
| sign_out_account                       | 207-500  | accountName == 'internal_server_error'                                |                                 |
| sign_out_account                       | 400      | accountName == 'invalid_format'                                       |                                 |
| sign_out_account                       | 403      | accountName == 'access_denied'                                        |                                 |
| sign_out_account                       | 500      | accountName == 'server_error'                                         |                                 |
| get_task_result                        | 200      | task_id == random string                                              | Use client.get_base_task_result |
| get_task_result                        | 200      | task_id == 'block_suspicious'                                         |                                 |
| get_task_result                        | 200      | task_id == 'collect_file'                                             |                                 |
| get_task_result                        | 200      | task_id == 'delete_message'                                           |                                 |
| get_task_result                        | 200      | task_id == 'disable_account'                                          |                                 |
| get_task_result                        | 200      | task_id == 'enable_account'                                           |                                 |
| get_task_result                        | 200      | task_id == 'force_sign_out'                                           |                                 |
| get_task_result                        | 200      | task_id == 'isolate_endpoint'                                         |                                 |
| get_task_result                        | 200      | task_id == 'quarantine_message'                                       |                                 |
| get_task_result                        | 200      | task_id == 'remove_suspicious'                                        |                                 |
| get_task_result                        | 200      | task_id == 'reset_password'                                           |                                 |
| get_task_result                        | 200      | task_id == 'reset_endpoint'                                           |                                 |
| get_task_result                        | 200      | task_id == 'restore_message'                                          |                                 |
| get_task_result                        | 200      | task_id == 'submit_sandbox'                                           |                                 |
| get_task_result                        | 200      | task_id == 'terminate_process'                                        |                                 |
| get_task_result                        | 400      | task_id == 'bad_request'                                              |                                 |
| get_task_result                        | 403      | task_id == 'access_denied'                                            |                                 |
| get_task_result                        | 404      | task_id == 'not_found'                                                |                                 |
| get_task_result                        | 500      | task_id == 'internal_error'                                           |                                 |
| check_connectivity                     | 200      | None                                                                  |                                 |
| check_connectivity                     | 500      | API_KEY == 'SERVER_ERROR'                                             |                                 |
| delete_email_message                   | 207-202  | messageId OR uniqueId == random string                                |                                 |
| delete_email_message                   | 207-400  | messageId == 'action_not_supported'                                   |                                 |
| delete_email_message                   | 207-400  | messageId == 'fields_not_found'                                       |                                 |
| delete_email_message                   | 207-400  | messageId == 'invalid_field_format'                                   |                                 |
| delete_email_message                   | 207-400  | messageId == 'target_not_found'                                       |                                 |
| delete_email_message                   | 207-400  | messageId == 'task_duplication'                                       |                                 |
| delete_email_message                   | 207-403  | messageId == 'access_denied_no_scope'                                 |                                 |
| delete_email_message                   | 207-403  | messageId == 'feature_disabled'                                       |                                 |
| delete_email_message                   | 207-403  | messageId == 'insufficient_permissions'                               |                                 |
| delete_email_message                   | 207-403  | messageId == 'unsupported_response'                                   |                                 |
| delete_email_message                   | 207-500  | messageId == 'internal_server_error'                                  |                                 |
| delete_email_message                   | 400      | messageId == 'invalid_format'                                         |                                 |
| delete_email_message                   | 403      | messageId == 'access_denied'                                          |                                 |
| delete_email_message                   | 500      | messageId == 'server_error'                                           |                                 |
| quarantine_email_message               | 207-202  | messageId OR uniqueId == random string                                |                                 |
| quarantine_email_message               | 207-400  | messageId == 'action_not_supported'                                   |                                 |
| quarantine_email_message               | 207-400  | messageId == 'fields_not_found'                                       |                                 |
| quarantine_email_message               | 207-400  | messageId == 'invalid_field_format'                                   |                                 |
| quarantine_email_message               | 207-400  | messageId == 'target_not_found'                                       |                                 |
| quarantine_email_message               | 207-400  | messageId == 'task_duplication'                                       |                                 |
| quarantine_email_message               | 207-403  | messageId == 'access_denied_no_scope'                                 |                                 |
| quarantine_email_message               | 207-403  | messageId == 'feature_disabled'                                       |                                 |
| quarantine_email_message               | 207-403  | messageId == 'insufficient_permissions'                               |                                 |
| quarantine_email_message               | 207-403  | messageId == 'unsupported_response'                                   |                                 |
| quarantine_email_message               | 207-500  | messageId == 'internal_server_error'                                  |                                 |
| quarantine_email_message               | 400      | messageId == 'invalid_format'                                         |                                 |
| quarantine_email_message               | 403      | messageId == 'access_denied'                                          |                                 |
| quarantine_email_message               | 500      | messageId == 'server_error'                                           |                                 |
| restore_email_message                  | 207-202  | messageId OR uniqueId == random string                                |                                 |
| restore_email_message                  | 207-400  | messageId == 'action_not_supported'                                   |                                 |
| restore_email_message                  | 207-400  | messageId == 'fields_not_found'                                       |                                 |
| restore_email_message                  | 207-400  | messageId == 'invalid_field_format'                                   |                                 |
| restore_email_message                  | 207-400  | messageId == 'target_not_found'                                       |                                 |
| restore_email_message                  | 207-400  | messageId == 'task_duplication'                                       |                                 |
| restore_email_message                  | 207-403  | messageId == 'access_denied_no_scope'                                 |                                 |
| restore_email_message                  | 207-403  | messageId == 'feature_disabled'                                       |                                 |
| restore_email_message                  | 207-403  | messageId == 'insufficient_permissions'                               |                                 |
| restore_email_message                  | 207-403  | messageId == 'unsupported_response'                                   |                                 |
| restore_email_message                  | 207-500  | messageId == 'internal_server_error'                                  |                                 |
| restore_email_message                  | 400      | messageId == 'invalid_format'                                         |                                 |
| restore_email_message                  | 403      | messageId == 'access_denied'                                          |                                 |
| restore_email_message                  | 500      | messageId == 'server_error'                                           |                                 |
| collect_file                           | 207-202  | agentGuid OR endpointName == random string                            |                                 |
| collect_file                           | 207-400  | endpointName == 'action_not_supported'                                |                                 |
| collect_file                           | 207-400  | endpointName == 'fields_not_found'                                    |                                 |
| collect_file                           | 207-400  | endpointName == 'invalid_field_format'                                |                                 |
| collect_file                           | 207-400  | endpointName == 'target_not_found'                                    |                                 |
| collect_file                           | 207-400  | endpointName == 'task_duplication'                                    |                                 |
| collect_file                           | 207-403  | endpointName == 'access_denied_no_scope'                              |                                 |
| collect_file                           | 207-403  | endpointName == 'feature_disabled'                                    |                                 |
| collect_file                           | 207-403  | endpointName == 'insufficient_permissions'                            |                                 |
| collect_file                           | 207-403  | endpointName == 'unsupported_response'                                |                                 |
| collect_file                           | 207-500  | endpointName == 'internal_server_error'                               |                                 |
| collect_file                           | 400      | endpointName == 'invalid_format'                                      |                                 |
| collect_file                           | 403      | endpointName == 'access_denied'                                       |                                 |
| collect_file                           | 500      | endpointName == 'server_error'                                        |                                 |
| isolate_endpoint                       | 207-202  | agentGuid OR endpointName == random string                            |                                 |
| isolate_endpoint                       | 207-400  | endpointName == 'action_not_supported'                                |                                 |
| isolate_endpoint                       | 207-400  | endpointName == 'fields_not_found'                                    |                                 |
| isolate_endpoint                       | 207-400  | endpointName == 'invalid_field_format'                                |                                 |
| isolate_endpoint                       | 207-400  | endpointName == 'target_not_found'                                    |                                 |
| isolate_endpoint                       | 207-400  | endpointName == 'task_duplication'                                    |                                 |
| isolate_endpoint                       | 207-403  | endpointName == 'access_denied_no_scope'                              |                                 |
| isolate_endpoint                       | 207-403  | endpointName == 'feature_disabled'                                    |                                 |
| isolate_endpoint                       | 207-403  | endpointName == 'insufficient_permissions'                            |                                 |
| isolate_endpoint                       | 207-403  | endpointName == 'unsupported_response'                                |                                 |
| isolate_endpoint                       | 207-500  | endpointName == 'internal_server_error'                               |                                 |
| isolate_endpoint                       | 400      | endpointName == 'invalid_format'                                      |                                 |
| isolate_endpoint                       | 403      | endpointName == 'access_denied'                                       |                                 |
| isolate_endpoint                       | 500      | endpointName == 'server_error'                                        |                                 |
| restore_endpoint                       | 207-202  | agentGuid OR endpointName == random string                            |                                 |
| restore_endpoint                       | 207-400  | endpointName == 'action_not_supported'                                |                                 |
| restore_endpoint                       | 207-400  | endpointName == 'fields_not_found'                                    |                                 |
| restore_endpoint                       | 207-400  | endpointName == 'invalid_field_format'                                |                                 |
| restore_endpoint                       | 207-400  | endpointName == 'target_not_found'                                    |                                 |
| restore_endpoint                       | 207-400  | endpointName == 'task_duplication'                                    |                                 |
| restore_endpoint                       | 207-403  | endpointName == 'access_denied_no_scope'                              |                                 |
| restore_endpoint                       | 207-403  | endpointName == 'feature_disabled'                                    |                                 |
| restore_endpoint                       | 207-403  | endpointName == 'insufficient_permissions'                            |                                 |
| restore_endpoint                       | 207-403  | endpointName == 'unsupported_response'                                |                                 |
| restore_endpoint                       | 207-500  | endpointName == 'internal_server_error'                               |                                 |
| restore_endpoint                       | 400      | endpointName == 'invalid_format'                                      |                                 |
| restore_endpoint                       | 403      | endpointName == 'access_denied'                                       |                                 |
| restore_endpoint                       | 500      | endpointName == 'server_error'                                        |                                 |
| terminate_process                      | 207-202  | agentGuid OR endpointName == random string                            |                                 |
| terminate_process                      | 207-400  | endpointName == 'action_not_supported'                                |                                 |
| terminate_process                      | 207-400  | endpointName == 'fields_not_found'                                    |                                 |
| terminate_process                      | 207-400  | endpointName == 'invalid_field_format'                                |                                 |
| terminate_process                      | 207-400  | endpointName == 'target_not_found'                                    |                                 |
| terminate_process                      | 207-400  | endpointName == 'task_duplication'                                    |                                 |
| terminate_process                      | 207-403  | endpointName == 'access_denied_no_scope'                              |                                 |
| terminate_process                      | 207-403  | endpointName == 'feature_disabled'                                    |                                 |
| terminate_process                      | 207-403  | endpointName == 'insufficient_permissions'                            |                                 |
| terminate_process                      | 207-403  | endpointName == 'unsupported_response'                                |                                 |
| terminate_process                      | 207-500  | endpointName == 'internal_server_error'                               |                                 |
| terminate_process                      | 400      | endpointName == 'invalid_format'                                      |                                 |
| terminate_process                      | 403      | endpointName == 'access_denied'                                       |                                 |
| terminate_process                      | 500      | endpointName == 'server_error'                                        |                                 |
| add_to_block_list                      | 207-202  | url OR domain OR fileSha1 OR senderMailAddress OR ip == random string |                                 |
| add_to_block_list                      | 207-400  | url == 'action_not_supported'                                         |                                 |
| add_to_block_list                      | 207-400  | url == 'fields_not_found'                                             |                                 |
| add_to_block_list                      | 207-400  | url == 'invalid_field_format'                                         |                                 |
| add_to_block_list                      | 207-400  | url == 'target_not_found'                                             |                                 |
| add_to_block_list                      | 207-400  | url == 'task_duplication'                                             |                                 |
| add_to_block_list                      | 207-403  | url == 'access_denied_no_scope'                                       |                                 |
| add_to_block_list                      | 207-403  | url == 'feature_disabled'                                             |                                 |
| add_to_block_list                      | 207-403  | url == 'insufficient_permissions'                                     |                                 |
| add_to_block_list                      | 207-403  | url == 'unsupported_response'                                         |                                 |
| add_to_block_list                      | 207-500  | url == 'internal_server_error'                                        |                                 |
| add_to_block_list                      | 400      | url == 'invalid_format'                                               |                                 |
| add_to_block_list                      | 403      | url == 'access_denied'                                                |                                 |
| add_to_block_list                      | 500      | url == 'server_error'                                                 |                                 |
| remove_from_block_list                 | 207-202  | url OR domain OR fileSha1 OR senderMailAddress OR ip == random string |                                 |
| remove_from_block_list                 | 207-400  | url == 'action_not_supported'                                         |                                 |
| remove_from_block_list                 | 207-400  | url == 'fields_not_found'                                             |                                 |
| remove_from_block_list                 | 207-400  | url == 'invalid_field_format'                                         |                                 |
| remove_from_block_list                 | 207-400  | url == 'target_not_found'                                             |                                 |
| remove_from_block_list                 | 207-400  | url == 'task_duplication'                                             |                                 |
| remove_from_block_list                 | 207-403  | url == 'access_denied_no_scope'                                       |                                 |
| remove_from_block_list                 | 207-403  | url == 'feature_disabled'                                             |                                 |
| remove_from_block_list                 | 207-403  | url == 'insufficient_permissions'                                     |                                 |
| remove_from_block_list                 | 207-403  | url == 'unsupported_response'                                         |                                 |
| remove_from_block_list                 | 207-500  | url == 'internal_server_error'                                        |                                 |
| remove_from_block_list                 | 400      | url == 'invalid_format'                                               |                                 |
| remove_from_block_list                 | 403      | url == 'access_denied'                                                |                                 |
| remove_from_block_list                 | 500      | url == 'server_error'                                                 |                                 |
| add_to_exception_list                  | 207-201  | url OR domain OR fileSha1 OR senderMailAddress OR ip == random string |                                 |
| add_to_exception_list                  | 207-400  | url == 'bad_request'                                                  |                                 |
| add_to_exception_list                  | 207-500  | url == 'internal_server_error'                                        |                                 |
| add_to_exception_list                  | 400      | url == 'invalid_format'                                               |                                 |
| add_to_exception_list                  | 403      | url == 'access_denied'                                                |                                 |
| add_to_exception_list                  | 429      | url == 'too_many_requests'                                            |                                 |
| add_to_exception_list                  | 500      | url == 'server_error'                                                 |                                 |
| add_to_suspicious_list                 | 207-201  | url OR domain OR fileSha1 OR senderMailAddress OR ip == random string |                                 |
| add_to_suspicious_list                 | 207-400  | url == 'bad_request'                                                  |                                 |
| add_to_suspicious_list                 | 207-500  | url == 'internal_server_error'                                        |                                 |
| add_to_suspicious_list                 | 400      | url == 'invalid_format'                                               |                                 |
| add_to_suspicious_list                 | 403      | url == 'access_denied'                                                |                                 |
| add_to_suspicious_list                 | 429      | url == 'too_many_requests'                                            |                                 |
| add_to_suspicious_list                 | 500      | url == 'server_error'                                                 |                                 |
| remove_from_exception_list             | 207-204  | url OR domain OR fileSha1 OR senderMailAddress OR ip == random string |                                 |
| remove_from_exception_list             | 207-400  | url == 'bad_request'                                                  |                                 |
| remove_from_exception_list             | 207-404  | url == 'not_found'                                                    |                                 |
| remove_from_exception_list             | 207-500  | url == 'internal_server_error'                                        |                                 |
| remove_from_exception_list             | 400      | url == 'invalid_format'                                               |                                 |
| remove_from_exception_list             | 403      | url == 'access_denied'                                                |                                 |
| remove_from_exception_list             | 429      | url == 'too_many_requests'                                            |                                 |
| remove_from_exception_list             | 500      | url == 'server_error'                                                 |                                 |
| remove_from_suspicious_list            | 207-204  | url OR domain OR fileSha1 OR senderMailAddress OR ip == random string |                                 |
| remove_from_suspicious_list            | 207-400  | url == 'bad_request'                                                  |                                 |
| remove_from_suspicious_list            | 207-404  | url == 'not_found'                                                    |                                 |
| remove_from_suspicious_list            | 207-500  | url == 'internal_server_error'                                        |                                 |
| remove_from_suspicious_list            | 400      | url == 'invalid_format'                                               |                                 |
| remove_from_suspicious_list            | 403      | url == 'access_denied'                                                |                                 |
| remove_from_suspicious_list            | 429      | url == 'too_many_requests'                                            |                                 |
| remove_from_suspicious_list            | 500      | url == 'server_error'                                                 |                                 |
| get_exception_list                     | 200      | API_KEY == random string                                              |                                 |
| get_exception_list                     | 400      | API_KEY == 'BAD_REQUEST'                                              |                                 |
| get_exception_list                     | 403      | API_KEY == 'ACCESS_DENIED'                                            |                                 |
| get_exception_list                     | 429      | API_KEY == 'TOO_MANY_REQUESTS'                                        |                                 |
| get_exception_list                     | 500      | API_KEY == 'SERVER_ERROR'                                             |                                 |
| get_suspicious_list                    | 200      | API_KEY == random string                                              |                                 |
| get_suspicious_list                    | 400      | API_KEY == 'BAD_REQUEST'                                              |                                 |
| get_suspicious_list                    | 403      | API_KEY == 'ACCESS_DENIED'                                            |                                 |
| get_suspicious_list                    | 429      | API_KEY == 'TOO_MANY_REQUESTS'                                        |                                 |
| get_suspicious_list                    | 500      | API_KEY == 'SERVER_ERROR'                                             |                                 |
| get_sandbox_analysis_result            | 200      | report_id == random string                                            |                                 |
| get_sandbox_analysis_result            | 404      | report_id == 'not_found'                                              |                                 |
| get_sandbox_analysis_result            | 500      | report_id == 'server_error'                                           |                                 |
| get_sandbox_submission_status          | 200      | task_id == random string                                              |                                 |
| get_sandbox_submission_status          | 404      | task_id == 'not_found'                                                |                                 |
| get_sandbox_submission_status          | 500      | task_id == 'server_error'                                             |                                 |
| submit_file_to_sandbox                 | 202      | file_name == random string                                            |                                 |
| submit_file_to_sandbox                 | 400      | file_name == 'badRequest.txt'                                         |                                 |
| submit_file_to_sandbox                 | 413      | file_name == 'tooBig.txt'                                             |                                 |
| submit_file_to_sandbox                 | 429      | file_name == 'tooMany.txt'                                            |                                 |
| submit_file_to_sandbox                 | 500      | file_name == 'serverError.txt'                                        |                                 |
| submit_urls_to_sandbox                 | 207-202  | url == random string                                                  |                                 |
| submit_urls_to_sandbox                 | 207-400  | url ==  'bad_request'                                                 |                                 |
| submit_urls_to_sandbox                 | 400      | url == 'invalid_request'                                              |                                 |
| submit_urls_to_sandbox                 | 413      | url == 'too_large'                                                    |                                 |
| submit_urls_to_sandbox                 | 429      | url == 'too_many'                                                     |                                 |
| submit_urls_to_sandbox                 | 500      | url == 'server_error'                                                 |                                 |
| download_sandbox_analysis_result       | 200      | submit_id == random string                                            |                                 |
| download_sandbox_analysis_result       | 404      | submit_id == 'not_found'                                              |                                 |
| download_sandbox_analysis_result       | 500      | submit_id == 'server_error'                                           |                                 |
| download_sandbox_investigation_package | 200      | submit_id == random string                                            |                                 |
| download_sandbox_investigation_package | 404      | submit_id == 'not_found'                                              |                                 |
| download_sandbox_investigation_package | 500      | submit_id == 'server_error'                                           |                                 |
| get_sandbox_suspicious_list            | 200      | submit_id == random string                                            |                                 |
| get_sandbox_suspicious_list            | 404      | submit_id == 'not_found'                                              |                                 |
| get_sandbox_suspicious_list            | 500      | submit_id == 'server_error'                                           |                                 |
| get_email_activity_data                | 200      | endpoint == random string                                             |                                 |
| get_email_activity_data                | 400      | endpoint == 'bad_request'                                             |                                 |
| get_email_activity_data                | 408      | endpoint == 'request_timeout'                                         |                                 |
| get_email_activity_data                | 500      | endpoint == 'server_error'                                            |                                 |
| get_endpoint_activity_data             | 200      | endpoint == random string                                             |                                 |
| get_endpoint_activity_data             | 400      | endpoint == 'bad_request'                                             |                                 |
| get_endpoint_activity_data             | 408      | endpoint == 'request_timeout'                                         |                                 |
| get_endpoint_activity_data             | 500      | endpoint == 'server_error'                                            |                                 |
| get_endpoint_data                      | 400      | endpoint == 'bad_request'                                             |                                 |
| get_endpoint_data                      | 500      | endpoint == 'server_error'                                            |                                 | 
| get_endpoint_data                      | 200      | endpoint == random string                                             |                                 |
| get_endpoint_data                      | 200      | endpoint == 'missing_prodcode'                                        |                                 |
| add_alert_note                         | 201      | content == random string                                              |                                 |
| add_alert_note                         | 400      | content == 'bad_request'                                              |                                 |
| add_alert_note                         | 404      | content == 'not_found'                                                |                                 |
| add_alert_note                         | 500      | content == 'server_error'                                             |                                 |
| edit_alert_status                      | 204      | if_match == random string                                             |                                 |
| edit_alert_status                      | 404      | if_match == 'not_found'                                               |                                 |
| edit_alert_status                      | 412      | if_match == 'precondition_failed'                                     |                                 |
| edit_alert_status                      | 500      | if_match == 'server_error'                                            |                                 |
| get_alert_details                      | 200      | alert_id == random string                                             |                                 |
| get_alert_details                      | 400      | alert_id == 'bad_request'                                             |                                 |
| get_alert_details                      | 500      | alert_id == 'server_error'                                            |                                 |
| get_alert_list                         | 200      | start_date_time == random string                                      |                                 |
| get_alert_list                         | 200      | start_date_time == 'next_link'                                        |                                 |
| get_alert_list                         | 200      | skip_token == 'c2tpcFRva2Vu'                                          |                                 |
| get_alert_list                         | 400      | start_date_time == 'bad_request'                                      |                                 |
| get_alert_list                         | 500      | start_date_time == 'server_error'                                     |                                 |
| get_oat_list                           | 200      | detected_start_date_time == random string                             |                                 |
| get_oat_list                           | 200      | detected_start_date_time == 'next_link'                               |                                 |
| get_oat_list                           | 200      | nextBatchToken == 'c2tpcFRva2Vu'                                      |                                 |
| get_oat_list                           | 400      | detected_start_date_time == 'bad_request'                             |                                 |
| get_oat_list                           | 500      | detected_start_date_time == 'server_error'                            |                                 |
| add_attachment                         | 201      | None                                                                  |                                 |
| add_attachment                         | 400      | API_KEY == 'BAD_REQUEST'                                              |                                 |
| add_attachment                         | 403      | API_KEY == 'ACCESS_DENIED'                                            |                                 |
| add_attachment                         | 404      | API_KEY == 'NOT_FOUND'                                                |                                 |
| add_attachment                         | 500      | API_KEY == 'SERVER_ERROR'                                             |                                 |
| add_content_to_case                    | 201      | None                                                                  |                                 |
| add_content_to_case                    | 400      | API_KEY == 'BAD_REQUEST'                                              |                                 |
| add_content_to_case                    | 403      | API_KEY == 'ACCESS_DENIED'                                            |                                 |
| add_content_to_case                    | 404      | API_KEY == 'NOT_FOUND'                                                |                                 |
| add_content_to_case                    | 500      | API_KEY == 'SERVER_ERROR'                                             |                                 |
| create_case                            | 201      | None                                                                  |                                 |
| create_case                            | 400      | API_KEY == 'BAD_REQUEST'                                              |                                 |
| create_case                            | 403      | API_KEY == 'ACCESS_DENIED'                                            |                                 |
| create_case                            | 500      | API_KEY == 'SERVER_ERROR'                                             |                                 |
| delete_attachment                      | 204      | None                                                                  |                                 |
| delete_attachment                      | 403      | API_KEY == 'ACCESS_DENIED'                                            |                                 |
| delete_attachment                      | 404      | API_KEY == 'NOT_FOUND'                                                |                                 |
| delete_attachment                      | 500      | API_KEY == 'SERVER_ERROR'                                             |                                 |
| delete_case_by_content_id              | 204      | None                                                                  |                                 |
| delete_case_by_content_id              | 403      | API_KEY == 'ACCESS_DENIED'                                            |                                 |
| delete_case_by_content_id              | 404      | API_KEY == 'NOT_FOUND'                                                |                                 |
| delete_case_by_content_id              | 500      | API_KEY == 'SERVER_ERROR'                                             |                                 |
| download_attachment_by_id              | 200      | None                                                                  |                                 |
| download_attachment_by_id              | 403      | API_KEY == 'ACCESS_DENIED'                                            |                                 |
| download_attachment_by_id              | 404      | API_KEY == 'NOT_FOUND'                                                |                                 |
| download_attachment_by_id              | 500      | API_KEY == 'SERVER_ERROR'                                             |                                 |
| get_case_content_by_content_id         | 200      | None                                                                  |                                 |
| get_case_content_by_content_id         | 403      | API_KEY == 'ACCESS_DENIED'                                            |                                 |
| get_case_content_by_content_id         | 404      | API_KEY == 'NOT_FOUND'                                                |                                 |
| get_case_content_by_content_id         | 500      | API_KEY == 'SERVER_ERROR'                                             |                                 |
| get_case_contents_by_case_id           | 200      | skip_token == 'c2tpcFRva2Vu'                                          |                                 |
| get_case_contents_by_case_id           | 200      | None                                                                  |                                 |
| get_case_contents_by_case_id           | 400      | API_KEY == 'BAD_REQUEST'                                              |                                 |
| get_case_contents_by_case_id           | 403      | API_KEY == 'ACCESS_DENIED'                                            |                                 |
| get_case_contents_by_case_id           | 404      | API_KEY == 'NOT_FOUND'                                                |                                 |
| get_case_contents_by_case_id           | 500      | API_KEY == 'SERVER_ERROR'                                             |                                 |
| get_case_details_by_id                 | 200      | None                                                                  |                                 |
| get_case_details_by_id                 | 403      | API_KEY == 'ACCESS_DENIED'                                            |                                 |
| get_case_details_by_id                 | 404      | API_KEY == 'NOT_FOUND'                                                |                                 |
| get_case_details_by_id                 | 500      | API_KEY == 'SERVER_ERROR'                                             |                                 |
| get_cases                              | 200      | skip_token == 'c2tpcFRva2Vu'                                          |                                 |
| get_cases                              | 200      | None                                                                  |                                 |
| get_cases                              | 400      | API_KEY == 'BAD_REQUEST'                                              |                                 |
| get_cases                              | 403      | API_KEY == 'ACCESS_DENIED'                                            |                                 |
| get_cases                              | 500      | API_KEY == 'SERVER_ERROR'                                             |                                 |
| update_case                            | 204      | None                                                                  |                                 |
| update_case                            | 400      | API_KEY == 'BAD_REQUEST'                                              |                                 |
| update_case                            | 403      | API_KEY == 'ACCESS_DENIED'                                            |                                 |
| update_case                            | 404      | API_KEY == 'NOT_FOUND'                                                |                                 |
| update_case                            | 412      | API_KEY == 'CONDITION_NOT_MET'                                        |                                 |
| update_case                            | 500      | API_KEY == 'SERVER_ERROR'                                             |                                 |
| update_case_content                    | 204      | None                                                                  |                                 |
| update_case_content                    | 400      | API_KEY == 'BAD_REQUEST'                                              |                                 |
| update_case_content                    | 403      | API_KEY == 'ACCESS_DENIED'                                            |                                 |
| update_case_content                    | 404      | API_KEY == 'NOT_FOUND'                                                |                                 |
| update_case_content                    | 412      | API_KEY == 'CONDITION_NOT_MET'                                        |                                 |
| update_case_content                    | 500      | API_KEY == 'SERVER_ERROR'                                             |                                 |
| get_active_data_pipelines              | 200      | None                                                                  |                                 |
| get_active_data_pipelines              | 500      | API_KEY == 'SERVER_ERROR'                                             |                                 |
| get_oat_event_pkgs                     | 200      | pageToken == 'c2tpcFRva2Vu'                                           |                                 |
| get_oat_event_pkgs                     | 200      | None                                                                  |                                 |
| get_oat_event_pkgs                     | 400      | API_KEY == 'BAD_REQUEST'                                              |                                 |
| get_oat_event_pkgs                     | 403      | API_KEY == 'ACCESS_DENIED'                                            |                                 |
| get_oat_event_pkgs                     | 404      | API_KEY == 'NOT_FOUND'                                                |                                 |
| get_oat_event_pkgs                     | 500      | API_KEY == 'SERVER_ERROR'                                             |                                 |
| get_oat_pkg                            | 200      | None                                                                  |                                 |
| get_oat_pkg                            | 400      | API_KEY == 'BAD_REQUEST'                                              |                                 |
| get_oat_pkg                            | 403      | API_KEY == 'ACCESS_DENIED'                                            |                                 |
| get_oat_pkg                            | 404      | API_KEY == 'NOT_FOUND'                                                |                                 |
| get_oat_pkg                            | 500      | API_KEY == 'SERVER_ERROR'                                             |                                 |
| get_pipeline_settings                  | 200      | None                                                                  |                                 |
| get_pipeline_settings                  | 400      | API_KEY == 'BAD_REQUEST'                                              |                                 |
| get_pipeline_settings                  | 404      | API_KEY == 'NOT_FOUND'                                                |                                 |
| get_pipeline_settings                  | 500      | API_KEY == 'SERVER_ERROR'                                             |                                 |
| modify_settings                        | 204      | None                                                                  |                                 |
| modify_settings                        | 400      | API_KEY == 'BAD_REQUEST'                                              |                                 |
| modify_settings                        | 404      | API_KEY == 'NOT_FOUND'                                                |                                 |
| modify_settings                        | 412      | if_match == \"precondition_failed\"                                   |                                 |
| modify_settings                        | 500      | API_KEY == 'SERVER_ERROR'                                             |                                 |
| register                               | 201      | None                                                                  |                                 |
| register                               | 400      | API_KEY == 'BAD_REQUEST'                                              |                                 |
| register                               | 500      | API_KEY == 'SERVER_ERROR'                                             |                                 |
| unregister                             | 207      | None                                                                  |                                 |
| unregister                             | 500      | API_KEY == 'SERVER_ERROR'                                             |                                 |

## Contributing

### Code of Conduct

Trend Micro has adopted a [Code of Conduct](https://github.com/trendmicro/tm-v1/blob/main/CODE_OF_CONDUCT.md) that we expect project participants to adhere to. Please read the [full text](https://github.com/trendmicro/tm-v1/blob/main/CODE_OF_CONDUCT.md) so that you can understand what actions will and will not be tolerated.

Read our [contributing guide](https://github.com/trendmicro/tm-v1/blob/main/CONTRIBUTING.md) to learn about our development process, how to propose bugfixes and improvements, and how to build and test your changes to Trend Vision One.

### Development

If you want to actively help Trend Vision One by creating connectors, we created [dedicated documentation](https://docs.trendmicro.com/en-us/enterprise/trend-micro-xdr-help/Home) about the deployment of a development environment and how to start.

## Community

### Status & bugs

Currently Trend Vision One GitHub is under heavy development. If you wish to report bugs or request new features, you can use the [Github issues module](https://github.com/trendmicro/tm-v1-api-mockup/issues).

### Discussion

If you need support or you wish to engage a discussion about the Trend Vision One platform, feel free to join us on our [Forums](https://success.trendmicro.com/forum/s/topic/0TO4T000000LH90WAG/trend-micro-vision-one).

### Support

Supports will be provided through the community. As this is an OSS project but not a formal Trend Micro product, formal product support is not applied.

## About

### Authors

Trend Vision One is a product designed and developed by the company [Trend Micro](https://www.trendmicro.com).

<a href="https://www.trendmicro.com" alt="Trend Micro"><img src="https://www.trendmicro.com/content/dam/trendmicro/global/en/core/images/logos/tm-logo-red-white-t.svg" width="230" /></a>
