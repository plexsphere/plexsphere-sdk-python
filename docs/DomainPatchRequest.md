# DomainPatchRequest

Body for `PATCH /v1/domains/{id}`. All properties are optional — but the body MUST set at least one of `name`, `description`, `region`, `mesh_cidr`, or `reachability`. An empty body is rejected at the handler with `400 empty_patch`.  x-domain-slug-immutable: The `slug` is intentionally absent from this schema. The handler rejects any request body that carries a `slug` key (even with the same value) at decode time with `400 slug_immutable`. See the `tenancy` tag description and the DECISION block on `tenancy.Domain` for the rationale. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**name** | **str** | New human-readable Domain name. The aggregate&#39;s &#x60;Rename&#x60; mutator validates the same constraints as &#x60;NewDomain&#x60;.  | [optional] 
**description** | **str** | New free-form description. The empty string clears the description; a whitespace-only string is rejected so an operator typo cannot silently wipe the field.  | [optional] 
**region** | **str** | Re-pin the Domain to a region. The aggregate&#39;s &#x60;ParseRegion&#x60; enforces the same regex. Unlike the immutable &#x60;slug&#x60;, the region is freely re-pinnable through this mutator.  | [optional] 
**mesh_cidr** | **str** | New canonical RFC 4632 mesh-IP pool. Triggers the cross- Domain GIST exclusion check (&#x60;409 mesh_cidr_overlap&#x60;) and the service-level containment guard against &#x60;project_mesh_ip_reservations.sub_range&#x60; (&#x60;422 mesh_cidr_invalidates_subrange&#x60;).  | [optional] 
**reachability** | [**DomainReachabilityPolicy**](DomainReachabilityPolicy.md) |  | [optional] 

## Example

```python
from plexsphere.models.domain_patch_request import DomainPatchRequest

# TODO update the JSON string below
json = "{}"
# create an instance of DomainPatchRequest from a JSON string
domain_patch_request_instance = DomainPatchRequest.from_json(json)
# print the JSON string representation of the object
print(DomainPatchRequest.to_json())

# convert the object into a dict
domain_patch_request_dict = domain_patch_request_instance.to_dict()
# create an instance of DomainPatchRequest from a dict
domain_patch_request_from_dict = DomainPatchRequest.from_dict(domain_patch_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


