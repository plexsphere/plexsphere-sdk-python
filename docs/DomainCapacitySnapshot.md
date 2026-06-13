# DomainCapacitySnapshot

Per-Domain capacity snapshot returned by `GET /v1/domains/{domainId}/capacity`: the wall-clock instant the collector last sampled, and one reading per catalogued capacity dimension in the canonical Dashboard order. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**sampled_at** | **datetime** | RFC 3339 timestamp at which the collector last sampled the Domain&#39;s usage. The snapshot is unavailable (&#x60;503&#x60;) until the first sample completes.  | 
**dimensions** | [**List[DomainCapacityDimensionReading]**](DomainCapacityDimensionReading.md) | One reading per catalogued capacity dimension, in the canonical order used by the Dashboard capacity view.  | 

## Example

```python
from plexsphere.models.domain_capacity_snapshot import DomainCapacitySnapshot

# TODO update the JSON string below
json = "{}"
# create an instance of DomainCapacitySnapshot from a JSON string
domain_capacity_snapshot_instance = DomainCapacitySnapshot.from_json(json)
# print the JSON string representation of the object
print(DomainCapacitySnapshot.to_json())

# convert the object into a dict
domain_capacity_snapshot_dict = domain_capacity_snapshot_instance.to_dict()
# create an instance of DomainCapacitySnapshot from a dict
domain_capacity_snapshot_from_dict = DomainCapacitySnapshot.from_dict(domain_capacity_snapshot_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


