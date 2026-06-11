# CloudCreateRequest

Body for `POST /v1/clouds`. Field set mirrors the Cloud aggregate's `NewCloud` invariants. The handler authorises the call against the platform-level `manage` relation BEFORE invoking the service so an unauthorised caller never produces a `CloudCreated` outbox row. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**display_name** | **str** | Human-readable Cloud name. Whitespace-only is rejected. | 
**slug** | **str** | Kebab-case URL handle. The aggregate&#39;s &#x60;ParseSlug&#x60; enforces the same regex; surfacing the pattern here lets the generated client validate before the round-trip.  | 
**provider** | [**CloudProvider**](CloudProvider.md) |  | 
**endpoint** | **Dict[str, object]** | Provider-specific connection metadata. The per-provider validator runs at decode time; field-level rejections surface as &#x60;400 invalid_cloud_endpoint&#x60;.  | 
**region_defaults** | **Dict[str, object]** | Provider-specific region/default metadata. The per- provider validator runs at decode time; field-level rejections surface as &#x60;400 invalid_cloud_region_defaults&#x60;.  | 
**external_id** | **str** | Upstream provider account identifier. Combined with &#x60;provider&#x60; must be unique across all Clouds.  | 

## Example

```python
from plexsphere.models.cloud_create_request import CloudCreateRequest

# TODO update the JSON string below
json = "{}"
# create an instance of CloudCreateRequest from a JSON string
cloud_create_request_instance = CloudCreateRequest.from_json(json)
# print the JSON string representation of the object
print(CloudCreateRequest.to_json())

# convert the object into a dict
cloud_create_request_dict = cloud_create_request_instance.to_dict()
# create an instance of CloudCreateRequest from a dict
cloud_create_request_from_dict = CloudCreateRequest.from_dict(cloud_create_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


