# IntegrityViolationsRequest

Body for POST /v1/nodes/{id}/integrity-violations. Carries a batch of integrity-violation reports plexd detected on the local Node and is pushing upstream for persistence and alert emission. The handler canonicalises every entry through the `tenancy.NewIntegrityViolation` value-object constructor and the batch through the Node aggregate's `RecordIntegrityViolations` method before any database write. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**violations** | [**List[IntegrityViolationRequest]**](IntegrityViolationRequest.md) | The per-violation reports the agent observed. Empty or larger-than-128 batches are rejected with 400 &#x60;integrity_violations_empty&#x60; or &#x60;integrity_violations_too_many&#x60; respectively.  | 

## Example

```python
from plexsphere.models.integrity_violations_request import IntegrityViolationsRequest

# TODO update the JSON string below
json = "{}"
# create an instance of IntegrityViolationsRequest from a JSON string
integrity_violations_request_instance = IntegrityViolationsRequest.from_json(json)
# print the JSON string representation of the object
print(IntegrityViolationsRequest.to_json())

# convert the object into a dict
integrity_violations_request_dict = integrity_violations_request_instance.to_dict()
# create an instance of IntegrityViolationsRequest from a dict
integrity_violations_request_from_dict = IntegrityViolationsRequest.from_dict(integrity_violations_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


