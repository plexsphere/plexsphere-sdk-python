# CloudCredentialCloudList

Page of the Clouds a Cloud Credential serves, returned by `GET /v1/cloud-credentials/{id}/clouds`. The window is the credential's home Cloud plus every attached usage Cloud, ordered by `cloud_id`. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**items** | [**List[CloudUsageRef]**](CloudUsageRef.md) | Clouds the credential serves in the current page. | 
**next_cursor** | **str** | Continuation token for the next page. &#x60;null&#x60; or omitted when the iteration has reached end-of-stream. The encoding is HMAC-signed by the server so a tampered cursor surfaces as &#x60;400&#x60; on the next call.  | [optional] 

## Example

```python
from plexsphere.models.cloud_credential_cloud_list import CloudCredentialCloudList

# TODO update the JSON string below
json = "{}"
# create an instance of CloudCredentialCloudList from a JSON string
cloud_credential_cloud_list_instance = CloudCredentialCloudList.from_json(json)
# print the JSON string representation of the object
print(CloudCredentialCloudList.to_json())

# convert the object into a dict
cloud_credential_cloud_list_dict = cloud_credential_cloud_list_instance.to_dict()
# create an instance of CloudCredentialCloudList from a dict
cloud_credential_cloud_list_from_dict = CloudCredentialCloudList.from_dict(cloud_credential_cloud_list_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


