# DomainIdPBinding

A display-safe projection of an IdP binding for the unauthenticated sign-in chooser (GET /v1/auth/idp-bindings). Carries only the fields a login page needs to render a provider button: the binding id the caller pins on POST /v1/auth/sign-in, a human-friendly label, the issuer host, and the primary / platform-scoped markers. Secret-store references, claim mappings, and required ACR/AMR policy are deliberately absent — they never reach a pre-auth surface. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**id** | **UUID** | Binding id to pin on POST /v1/auth/sign-in. | 
**alias** | **str** | Operator-assigned alias, when the binding carries one. | [optional] 
**label** | **str** | Human-friendly button label — the alias when set, otherwise the issuer host.  | 
**issuer_host** | **str** | Host component of the binding&#39;s OIDC issuer URL. | 
**primary** | **bool** | Whether the binding is the Domain&#39;s default. | 
**platform_scoped** | **bool** | Whether the binding is a shared platform binding. | 

## Example

```python
from plexsphere.models.domain_id_p_binding import DomainIdPBinding

# TODO update the JSON string below
json = "{}"
# create an instance of DomainIdPBinding from a JSON string
domain_id_p_binding_instance = DomainIdPBinding.from_json(json)
# print the JSON string representation of the object
print(DomainIdPBinding.to_json())

# convert the object into a dict
domain_id_p_binding_dict = domain_id_p_binding_instance.to_dict()
# create an instance of DomainIdPBinding from a dict
domain_id_p_binding_from_dict = DomainIdPBinding.from_dict(domain_id_p_binding_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


