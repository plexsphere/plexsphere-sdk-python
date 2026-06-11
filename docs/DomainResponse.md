# DomainResponse

Hydrated projection of a tenancy Domain aggregate. The shape is shared by `CreateDomain`, `GetDomain`, `ListDomains`, and `PatchDomain` — every read surface returns the same wire bytes for the same Domain so clients only need one binding. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**id** | **UUID** | Domain identifier (UUIDv7). | 
**name** | **str** | Human-readable Domain name (trimmed at the aggregate). | 
**slug** | **str** | Kebab-case URL handle. Stable for the lifetime of the Domain — see the &#x60;tenancy&#x60; tag description for the immutability rationale.  | 
**description** | **str** | Optional free-form description. Empty string when the operator did not set one (the wire shape never carries &#x60;null&#x60; for this field).  | [optional] 
**region** | **str** | The region the Domain is pinned to. Unlike &#x60;description&#x60;, this field is OMITTED from the wire entirely when the Domain is unpinned — it is never carried as an empty string, so an absent key means \&quot;no region\&quot;.  | [optional] 
**mesh_cidr** | **str** | Canonical RFC 4632 mesh-IP pool the Domain owns. Cross- Domain non-overlap is enforced by the SQL GIST exclusion constraint.  | 
**reachability** | [**DomainReachabilityPolicy**](DomainReachabilityPolicy.md) |  | 
**created_at** | **datetime** | Aggregate creation timestamp (UTC). | 
**updated_at** | **datetime** | Last-modified timestamp (UTC). Bumped by every mutator — &#x60;Rename&#x60;, &#x60;ChangeDescription&#x60;, &#x60;RetargetMeshCIDR&#x60;, and &#x60;ChangeReachabilityPolicy&#x60;.  | 

## Example

```python
from plexsphere.models.domain_response import DomainResponse

# TODO update the JSON string below
json = "{}"
# create an instance of DomainResponse from a JSON string
domain_response_instance = DomainResponse.from_json(json)
# print the JSON string representation of the object
print(DomainResponse.to_json())

# convert the object into a dict
domain_response_dict = domain_response_instance.to_dict()
# create an instance of DomainResponse from a dict
domain_response_from_dict = DomainResponse.from_dict(domain_response_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


