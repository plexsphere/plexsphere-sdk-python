# CloudResponse

Hydrated projection of a Cloud aggregate. The shape is shared by `CreateCloud`, `GetCloud`, `ListClouds`, and `PatchCloud` — every read surface returns the same wire bytes for the same Cloud so clients only need one binding. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**id** | **UUID** | Cloud identifier (UUIDv7). | 
**display_name** | **str** | Human-readable Cloud name (trimmed at the aggregate). | 
**slug** | **str** | Kebab-case URL handle. Stable for the lifetime of the Cloud — see the &#x60;cloud&#x60; tag description for the immutability rationale.  | 
**provider** | [**CloudProvider**](CloudProvider.md) |  | 
**endpoint** | **Dict[str, object]** | Provider-specific connection metadata stored as a JSONB blob. The per-provider validator owns the field-shape contract; this schema only declares the wire envelope.  | 
**region_defaults** | **Dict[str, object]** | Provider-specific region/default metadata stored as a JSONB blob. The per-provider validator owns the field- shape contract.  | 
**external_id** | **str** | Upstream provider account identifier (e.g. AWS account id, Azure tenant id). Combined with &#x60;provider&#x60; it must be unique across all Clouds.  | 
**created_at** | **datetime** | Aggregate creation timestamp (UTC). | 
**updated_at** | **datetime** | Last-modified timestamp (UTC). Bumped by every mutator — &#x60;Rename&#x60;, &#x60;ChangeEndpoint&#x60;, &#x60;ChangeRegionDefaults&#x60;.  | 

## Example

```python
from plexsphere.models.cloud_response import CloudResponse

# TODO update the JSON string below
json = "{}"
# create an instance of CloudResponse from a JSON string
cloud_response_instance = CloudResponse.from_json(json)
# print the JSON string representation of the object
print(CloudResponse.to_json())

# convert the object into a dict
cloud_response_dict = cloud_response_instance.to_dict()
# create an instance of CloudResponse from a dict
cloud_response_from_dict = CloudResponse.from_dict(cloud_response_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


