# DomainList

Page of Domains returned by `GET /v1/domains`. The window is computed by the persistence layer in slug order; per-row visibility is layered on top so the `items` array is the subset the caller is authorised to see. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**items** | [**List[DomainResponse]**](DomainResponse.md) | Domains in the current page. | 
**next_cursor** | **str** | Continuation token for the next page. &#x60;null&#x60; or omitted when the iteration has reached end-of-stream. The encoding is HMAC-signed by the server so a tampered cursor surfaces as &#x60;400&#x60; on the next call.  | [optional] 
**empty_reason** | **str** | Disambiguates the &#x60;items: []&#x60; page. Omitted (or &#x60;null&#x60;) when &#x60;items&#x60; is non-empty OR when the persistence layer itself returned zero rows (the \&quot;no Domains exist\&quot; case). Set to &#x60;no_rebac_membership&#x60; when the persistence layer returned at least one row but the per-row ReBAC visibility filter dropped every one — the chicken-and-egg state a freshly-provisioned operator hits before any &#x60;domain#admin&#x60; / &#x60;domain#auditor&#x60; / &#x60;domain#member&#x60; tuple has been seeded for the calling Principal. Operators triaging \&quot;domain list is empty after sign-in\&quot; key off this hint and run the canonical post-login bootstrap step (the &#x60;grant-domain-admin&#x60; step on the &#x60;identity-e2e-demo&#x60; binary, mirrored by &#x60;make dev&#x60; for the dev stack — see &#x60;docs/contexts/identity/rebac.md#domaincreated-owner-tuple-seed&#x60;).  | [optional] 

## Example

```python
from plexsphere.models.domain_list import DomainList

# TODO update the JSON string below
json = "{}"
# create an instance of DomainList from a JSON string
domain_list_instance = DomainList.from_json(json)
# print the JSON string representation of the object
print(DomainList.to_json())

# convert the object into a dict
domain_list_dict = domain_list_instance.to_dict()
# create an instance of DomainList from a dict
domain_list_from_dict = DomainList.from_dict(domain_list_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


