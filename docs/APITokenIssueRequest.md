# APITokenIssueRequest

Body for POST /v1/auth/tokens. `env_prefix` is the environment segment embedded in the psk-shaped plaintext; the aggregate accepts any lowercase ASCII letter run but the public API is restricted to the three plexsphere-supported environments . 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**identity_ref** | **str** | Identifier of the user or ServiceIdentity the token binds to. Format is &#x60;user:&lt;uuid&gt;&#x60; or &#x60;service:&lt;uuid&gt;&#x60;.  | 
**env_prefix** | **str** | Environment segment embedded in the plaintext. | 
**ttl_seconds** | **int** | Optional TTL in seconds. If omitted, the server applies the deployment default.  | [optional] 

## Example

```python
from plexsphere.models.api_token_issue_request import APITokenIssueRequest

# TODO update the JSON string below
json = "{}"
# create an instance of APITokenIssueRequest from a JSON string
api_token_issue_request_instance = APITokenIssueRequest.from_json(json)
# print the JSON string representation of the object
print(APITokenIssueRequest.to_json())

# convert the object into a dict
api_token_issue_request_dict = api_token_issue_request_instance.to_dict()
# create an instance of APITokenIssueRequest from a dict
api_token_issue_request_from_dict = APITokenIssueRequest.from_dict(api_token_issue_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


