# Whoami

Resolved principal metadata for the current request.

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**principal_type** | **str** | Whether the principal is a human user or a ServiceIdentity. | 
**principal_scope** | **str** | Whether the principal is scoped to a tenant Domain (&#x60;domain&#x60;) or is a Domain-independent platform-operator session (&#x60;platform&#x60;). &#x60;domain_id&#x60; is omitted when the scope is &#x60;platform&#x60;.  | [optional] 
**subject** | **str** | Principal identifier (UUIDv7 serialised as a string). | 
**domain_id** | **UUID** | Domain the principal belongs to. Omitted for a platform-scoped principal, which belongs to no Domain.  | [optional] 
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


