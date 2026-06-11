# RebacLookupResponse

Result of `POST /v1/authz/lookup-resources` and `POST /v1/authz/lookup-subjects`. `items` is the full materialised set of object references — an empty array is a normal answer, never an error. `correlation_id` pairs the result with the matching audit entry. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**items** | **List[str]** | Full materialised set of object references the lookup resolved (&#x60;&lt;type&gt;:&lt;id&gt;&#x60;). Ordering is the authorizer&#39;s insertion order; callers that need a stable order MUST sort client-side.  | 
**correlation_id** | **str** | Correlation id that pairs this lookup with the matching audit entry emitted by &#x60;internal/audit&#x60;.  | 

## Example

```python
from plexsphere.models.rebac_lookup_response import RebacLookupResponse

# TODO update the JSON string below
json = "{}"
# create an instance of RebacLookupResponse from a JSON string
rebac_lookup_response_instance = RebacLookupResponse.from_json(json)
# print the JSON string representation of the object
print(RebacLookupResponse.to_json())

# convert the object into a dict
rebac_lookup_response_dict = rebac_lookup_response_instance.to_dict()
# create an instance of RebacLookupResponse from a dict
rebac_lookup_response_from_dict = RebacLookupResponse.from_dict(rebac_lookup_response_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


