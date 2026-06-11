# DomainCreateRequest

Body for `POST /v1/domains`. Field set mirrors the Domain aggregate's `NewDomain` invariants. The handler authorises the call against the platform-level `manage` relation BEFORE invoking the service so an unauthorised caller never produces a `DomainCreated` outbox row. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**name** | **str** | Human-readable Domain name. Whitespace-only is rejected. | 
**slug** | **str** | Kebab-case URL handle. The aggregate&#39;s &#x60;ParseSlug&#x60; enforces the same regex; surfacing the pattern here lets the generated client validate before the round-trip.  | 
**description** | **str** | Optional free-form description. Whitespace-only strings are rejected by the aggregate (the operator most likely fat- fingered the field instead of meaning to clear it).  | [optional] 
**region** | **str** | Optional region the Domain is pinned to at creation; absent or empty means the Domain is left unpinned. The aggregate&#39;s &#x60;ParseRegion&#x60; enforces the same regex, so surfacing the pattern here lets the generated client validate before the round-trip.  | [optional] 
**mesh_cidr** | **str** | Canonical RFC 4632 mesh-IP pool. Cross-Domain non-overlap is enforced by the SQL GIST exclusion constraint and surfaces as &#x60;409 mesh_cidr_overlap&#x60;.  | 
**reachability** | [**DomainReachabilityPolicy**](DomainReachabilityPolicy.md) |  | [optional] 

## Example

```python
from plexsphere.models.domain_create_request import DomainCreateRequest

# TODO update the JSON string below
json = "{}"
# create an instance of DomainCreateRequest from a JSON string
domain_create_request_instance = DomainCreateRequest.from_json(json)
# print the JSON string representation of the object
print(DomainCreateRequest.to_json())

# convert the object into a dict
domain_create_request_dict = domain_create_request_instance.to_dict()
# create an instance of DomainCreateRequest from a dict
domain_create_request_from_dict = DomainCreateRequest.from_dict(domain_create_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


