# ManagementClusterList

The slug-ordered registered management fleet returned by `GET /v1/management-clusters`. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**items** | [**List[ManagementClusterResponse]**](ManagementClusterResponse.md) | Registered management clusters, slug-ordered. | 

## Example

```python
from plexsphere.models.management_cluster_list import ManagementClusterList

# TODO update the JSON string below
json = "{}"
# create an instance of ManagementClusterList from a JSON string
management_cluster_list_instance = ManagementClusterList.from_json(json)
# print the JSON string representation of the object
print(ManagementClusterList.to_json())

# convert the object into a dict
management_cluster_list_dict = management_cluster_list_instance.to_dict()
# create an instance of ManagementClusterList from a dict
management_cluster_list_from_dict = ManagementClusterList.from_dict(management_cluster_list_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


