# CloudUsageRef

Reference to a single Cloud a Cloud Credential serves. Returned as the item shape of `CloudCredentialCloudList` and as the body of a successful `AttachCloudCredentialCloud` call. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**cloud_id** | **UUID** | Identifier of a Cloud the credential serves. | 

## Example

```python
from plexsphere.models.cloud_usage_ref import CloudUsageRef

# TODO update the JSON string below
json = "{}"
# create an instance of CloudUsageRef from a JSON string
cloud_usage_ref_instance = CloudUsageRef.from_json(json)
# print the JSON string representation of the object
print(CloudUsageRef.to_json())

# convert the object into a dict
cloud_usage_ref_dict = cloud_usage_ref_instance.to_dict()
# create an instance of CloudUsageRef from a dict
cloud_usage_ref_from_dict = CloudUsageRef.from_dict(cloud_usage_ref_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


