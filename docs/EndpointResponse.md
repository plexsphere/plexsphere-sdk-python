# EndpointResponse

Body for PUT /v1/nodes/{id}/endpoint success responses. Carries the server-side commit timestamp and the freshness deadline the agent must beat before the per-Domain sweeper tombstones the endpoint. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**accepted_at** | **datetime** | Server-side timestamp at which the endpoint observation was committed. Distinct from &#x60;reported_at&#x60; so the caller can estimate one-way latency and clock-skew.  | 
**stale_after** | **datetime** | Server-side deadline by which the agent must submit a fresh observation before the per-Domain endpoint sweeper stamps the endpoint as stale and emits a tombstone event. Equal to &#x60;accepted_at&#x60; plus the per-Domain endpoint TTL (defaults to 5 minutes).  | 

## Example

```python
from plexsphere.models.endpoint_response import EndpointResponse

# TODO update the JSON string below
json = "{}"
# create an instance of EndpointResponse from a JSON string
endpoint_response_instance = EndpointResponse.from_json(json)
# print the JSON string representation of the object
print(EndpointResponse.to_json())

# convert the object into a dict
endpoint_response_dict = endpoint_response_instance.to_dict()
# create an instance of EndpointResponse from a dict
endpoint_response_from_dict = EndpointResponse.from_dict(endpoint_response_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


