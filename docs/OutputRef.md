# OutputRef

Tagged union pointing at an action invocation's collected output. The `inline` variant carries the bounded output bytes (≤ 16 KiB) inside the control plane; the `object_store` variant carries the `(bucket, key, sha256)` coordinates addressing the over-ceiling output uploaded to the object store. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**kind** | **str** | Union discriminator: &#x60;inline&#x60; for control-plane-resident output bytes, &#x60;object_store&#x60; for an uploaded object.  | 
**inline_bytes** | **bytes** | Base64-encoded output bytes. Present only on the &#x60;inline&#x60; variant; bounded at 16 KiB.  | [optional] 
**bucket** | **str** | Object-store bucket holding the uploaded output. Present only on the &#x60;object_store&#x60; variant.  | [optional] 
**key** | **str** | Object-store key addressing the uploaded output. Present only on the &#x60;object_store&#x60; variant.  | [optional] 
**sha256** | **str** | Hex-encoded SHA-256 digest of the uploaded output. Present only on the &#x60;object_store&#x60; variant.  | [optional] 

## Example

```python
from plexsphere.models.output_ref import OutputRef

# TODO update the JSON string below
json = "{}"
# create an instance of OutputRef from a JSON string
output_ref_instance = OutputRef.from_json(json)
# print the JSON string representation of the object
print(OutputRef.to_json())

# convert the object into a dict
output_ref_dict = output_ref_instance.to_dict()
# create an instance of OutputRef from a dict
output_ref_from_dict = OutputRef.from_dict(output_ref_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


