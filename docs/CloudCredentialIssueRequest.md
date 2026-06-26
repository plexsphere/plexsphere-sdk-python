# CloudCredentialIssueRequest

Body for `POST /v1/clouds/{id}/cloud-credentials`. Carries the operator-supplied display name plus the secret material the Cloud Credentials Custodian writes into OpenBao KV-v2. Accepted INBOUND only — no field of this object is ever echoed back on any read surface. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**display_name** | **str** | Human-readable Cloud Credential name. Empty or whitespace-only is rejected with &#x60;400 invalid_display_name&#x60;.  | 
**payload** | **bytes** | Base64-encoded secret bytes written under &#x60;data.payload&#x60; in KV-v2. An empty payload is rejected with &#x60;400 invalid_payload&#x60;.  | 
**key_values** | **Dict[str, str]** | Optional flat data map written alongside the payload in KV-v2. Keys and values are caller-owned and never returned.  | [optional] 

## Example

```python
from plexsphere.models.cloud_credential_issue_request import CloudCredentialIssueRequest

# TODO update the JSON string below
json = "{}"
# create an instance of CloudCredentialIssueRequest from a JSON string
cloud_credential_issue_request_instance = CloudCredentialIssueRequest.from_json(json)
# print the JSON string representation of the object
print(CloudCredentialIssueRequest.to_json())

# convert the object into a dict
cloud_credential_issue_request_dict = cloud_credential_issue_request_instance.to_dict()
# create an instance of CloudCredentialIssueRequest from a dict
cloud_credential_issue_request_from_dict = CloudCredentialIssueRequest.from_dict(cloud_credential_issue_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


