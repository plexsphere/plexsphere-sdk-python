# CloudPatchRequest

Body for `PATCH /v1/clouds/{id}`. All properties are optional — but the body MUST set at least one of `display_name`, `endpoint`, or `region_defaults`. An empty body is rejected at the handler with `400 empty_patch`.  The immutable `slug` and `provider` are intentionally absent from this schema; the handler rejects a body carrying `slug` with `400 slug_immutable` and one carrying `provider` with `400 provider_immutable`. `provider` is the validator-routing key for the per-provider validator family — changing it would invalidate every previously-stored endpoint blob. See the `cloud` tag description and the DECISION on `cloud.Cloud`. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**display_name** | **str** | New human-readable Cloud name. The aggregate&#39;s &#x60;Rename&#x60; mutator validates the same constraints as &#x60;NewCloud&#x60;.  | [optional] 
**endpoint** | **Dict[str, object]** | New provider-specific connection metadata. Triggers a re-run of the per-provider validator on the merged next- state.  | [optional] 
**region_defaults** | **Dict[str, object]** | New provider-specific region/default metadata. Triggers a re-run of the per-provider validator on the merged next- state.  | [optional] 

## Example

```python
from plexsphere.models.cloud_patch_request import CloudPatchRequest

# TODO update the JSON string below
json = "{}"
# create an instance of CloudPatchRequest from a JSON string
cloud_patch_request_instance = CloudPatchRequest.from_json(json)
# print the JSON string representation of the object
print(CloudPatchRequest.to_json())

# convert the object into a dict
cloud_patch_request_dict = cloud_patch_request_instance.to_dict()
# create an instance of CloudPatchRequest from a dict
cloud_patch_request_from_dict = CloudPatchRequest.from_dict(cloud_patch_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


