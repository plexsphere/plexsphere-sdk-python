# BootstrapTokenList

Page of BootstrapToken metadata returned by GET /v1/projects/{project_id}/bootstrap-tokens. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**items** | [**List[BootstrapTokenMetadata]**](BootstrapTokenMetadata.md) | BootstrapToken metadata in the current page. | 
**next_cursor** | **str** | Continuation token for the next page. Null or omitted when the iteration has reached end-of-stream.  | [optional] 

## Example

```python
from plexsphere.models.bootstrap_token_list import BootstrapTokenList

# TODO update the JSON string below
json = "{}"
# create an instance of BootstrapTokenList from a JSON string
bootstrap_token_list_instance = BootstrapTokenList.from_json(json)
# print the JSON string representation of the object
print(BootstrapTokenList.to_json())

# convert the object into a dict
bootstrap_token_list_dict = bootstrap_token_list_instance.to_dict()
# create an instance of BootstrapTokenList from a dict
bootstrap_token_list_from_dict = BootstrapTokenList.from_dict(bootstrap_token_list_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


