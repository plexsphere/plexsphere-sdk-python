# APITokenIssueResponse

Response for POST /v1/auth/tokens. Plaintext is returned exactly once — it can never be retrieved again. A `Sunset` response header is set when this response retires a previous plaintext (i.e. when the call was a rotation); first issuances omit the header entirely. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**id** | **UUID** | Token identifier (UUIDv7). | 
**plaintext** | **str** | The psk-shaped plaintext; returned exactly once. | 
**env_prefix** | **str** | Environment segment embedded in the plaintext. | 
**expires_at** | **datetime** | RFC 3339 expiry timestamp. | 

## Example

```python
from plexsphere.models.api_token_issue_response import APITokenIssueResponse

# TODO update the JSON string below
json = "{}"
# create an instance of APITokenIssueResponse from a JSON string
api_token_issue_response_instance = APITokenIssueResponse.from_json(json)
# print the JSON string representation of the object
print(APITokenIssueResponse.to_json())

# convert the object into a dict
api_token_issue_response_dict = api_token_issue_response_instance.to_dict()
# create an instance of APITokenIssueResponse from a dict
api_token_issue_response_from_dict = APITokenIssueResponse.from_dict(api_token_issue_response_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


