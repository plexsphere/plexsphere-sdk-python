# APITokenSummary

Plaintext-free summary entry for GET /v1/auth/tokens. Matches the persisted APIToken aggregate minus any secret material . 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**id** | **UUID** | Token identifier (UUIDv7). | 
**env_prefix** | **str** | Environment segment embedded in the plaintext. | 
**last_used_at** | **datetime** | Most-recent authentication timestamp, if any. | [optional] 
**created_at** | **datetime** | Issuance timestamp. | 
**expires_at** | **datetime** | Expiry timestamp. | 
**rotated_at** | **datetime** | Timestamp the token was most recently rotated. | [optional] 

## Example

```python
from plexsphere.models.api_token_summary import APITokenSummary

# TODO update the JSON string below
json = "{}"
# create an instance of APITokenSummary from a JSON string
api_token_summary_instance = APITokenSummary.from_json(json)
# print the JSON string representation of the object
print(APITokenSummary.to_json())

# convert the object into a dict
api_token_summary_dict = api_token_summary_instance.to_dict()
# create an instance of APITokenSummary from a dict
api_token_summary_from_dict = APITokenSummary.from_dict(api_token_summary_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


