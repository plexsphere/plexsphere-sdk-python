# IntegrityViolationsResponse

Body for POST /v1/nodes/{id}/integrity-violations success responses. Carries the server-side commit timestamp and the echoed count of persisted violations so the agent can reconcile the batch with its local replay-queue. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**accepted_at** | **datetime** | Server-side timestamp at which the integrity-violations batch was committed.  | 
**violation_count** | **int** | Number of &#x60;IntegrityViolation&#x60; rows persisted by this request. Matches the &#x60;violations&#x60; array length on input.  | 

## Example

```python
from plexsphere.models.integrity_violations_response import IntegrityViolationsResponse

# TODO update the JSON string below
json = "{}"
# create an instance of IntegrityViolationsResponse from a JSON string
integrity_violations_response_instance = IntegrityViolationsResponse.from_json(json)
# print the JSON string representation of the object
print(IntegrityViolationsResponse.to_json())

# convert the object into a dict
integrity_violations_response_dict = integrity_violations_response_instance.to_dict()
# create an instance of IntegrityViolationsResponse from a dict
integrity_violations_response_from_dict = IntegrityViolationsResponse.from_dict(integrity_violations_response_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


