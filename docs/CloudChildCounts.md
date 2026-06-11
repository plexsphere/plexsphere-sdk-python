# CloudChildCounts

Per-aggregate count of children still attached to a Cloud at the moment a `DeleteCloud` call ran. Returned in the Problem detail of a `409 cloud_not_empty` response so the operator knows how many credentials are still attached. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**cloud_credentials** | **int** | Number of &#x60;CloudCredential&#x60; rows still attached. | 

## Example

```python
from plexsphere.models.cloud_child_counts import CloudChildCounts

# TODO update the JSON string below
json = "{}"
# create an instance of CloudChildCounts from a JSON string
cloud_child_counts_instance = CloudChildCounts.from_json(json)
# print the JSON string representation of the object
print(CloudChildCounts.to_json())

# convert the object into a dict
cloud_child_counts_dict = cloud_child_counts_instance.to_dict()
# create an instance of CloudChildCounts from a dict
cloud_child_counts_from_dict = CloudChildCounts.from_dict(cloud_child_counts_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


