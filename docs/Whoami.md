# Whoami

Resolved principal metadata for the current request.

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**principal_type** | **str** | Whether the principal is a human user or a ServiceIdentity. | 
**subject** | **str** | Principal identifier (UUIDv7 serialised as a string). | 
**domain_id** | **UUID** | Domain the principal belongs to. | [optional] 
**email** | **str** | Primary email of the principal as projected by the upstream IdP at sign-in. Omitted when the IdP did not supply an &#x60;email&#x60; claim or the principal is a ServiceIdentity. A browser client renders it as the human-readable identity label.  | [optional] 
**acr** | **str** | Authentication Context Class Reference, if available. | [optional] 
**amr** | **List[str]** | Authentication Methods References (e.g. \&quot;pwd\&quot;, \&quot;mfa\&quot;). | [optional] 

## Example

```python
from plexsphere.models.whoami import Whoami

# TODO update the JSON string below
json = "{}"
# create an instance of Whoami from a JSON string
whoami_instance = Whoami.from_json(json)
# print the JSON string representation of the object
print(Whoami.to_json())

# convert the object into a dict
whoami_dict = whoami_instance.to_dict()
# create an instance of Whoami from a dict
whoami_from_dict = Whoami.from_dict(whoami_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


