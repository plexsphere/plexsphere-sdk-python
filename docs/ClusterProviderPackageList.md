# ClusterProviderPackageList

The package-source-ordered set of provider packages the platform manages on a management cluster, returned by `GET /v1/management-clusters/{id}/provider-packages`. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**items** | [**List[ClusterProviderPackageResponse]**](ClusterProviderPackageResponse.md) | Provider packages managed on the cluster. | 

## Example

```python
from plexsphere.models.cluster_provider_package_list import ClusterProviderPackageList

# TODO update the JSON string below
json = "{}"
# create an instance of ClusterProviderPackageList from a JSON string
cluster_provider_package_list_instance = ClusterProviderPackageList.from_json(json)
# print the JSON string representation of the object
print(ClusterProviderPackageList.to_json())

# convert the object into a dict
cluster_provider_package_list_dict = cluster_provider_package_list_instance.to_dict()
# create an instance of ClusterProviderPackageList from a dict
cluster_provider_package_list_from_dict = ClusterProviderPackageList.from_dict(cluster_provider_package_list_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


