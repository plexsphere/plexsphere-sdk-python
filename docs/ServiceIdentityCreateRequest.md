# ServiceIdentityCreateRequest

Body for `POST /v1/domains/{id}/service-identities`. Provisions a service identity (a machine principal) on the addressed Domain. Every field is validated at the aggregate boundary; an empty or whitespace-only string is rejected with `400 invalid_service_identity`. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**display_name** | **str** | Operator-facing label for the service identity (for example &#x60;CI deploy bot&#x60;). Trimmed; must be non-empty.  | 
**subject** | **str** | Upstream workload subject the presented credential must carry — the OIDC &#x60;sub&#x60; claim or the SPIFFE ID, depending on &#x60;federation_kind&#x60;. Unique per Domain: a second identity reusing the same subject is rejected with &#x60;409&#x60;.  | 
**audience** | **str** | Expected audience claim for the presented credential.  | 
**federation_kind** | [**ServiceFederationKind**](ServiceFederationKind.md) |  | 

## Example

```python
from plexsphere.models.service_identity_create_request import ServiceIdentityCreateRequest

# TODO update the JSON string below
json = "{}"
# create an instance of ServiceIdentityCreateRequest from a JSON string
service_identity_create_request_instance = ServiceIdentityCreateRequest.from_json(json)
# print the JSON string representation of the object
print(ServiceIdentityCreateRequest.to_json())

# convert the object into a dict
service_identity_create_request_dict = service_identity_create_request_instance.to_dict()
# create an instance of ServiceIdentityCreateRequest from a dict
service_identity_create_request_from_dict = ServiceIdentityCreateRequest.from_dict(service_identity_create_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


