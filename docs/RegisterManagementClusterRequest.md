# RegisterManagementClusterRequest

Body for `POST /v1/management-clusters`. `slug` is the unique human-facing shard key; a duplicate surfaces as `409 management_cluster_conflict`. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**name** | **str** | Human-readable cluster name. | 
**slug** | **str** | Unique human-facing shard key. Required and immutable once registered.  | 
**region** | **str** | Region the cluster serves. Optional; omit or send an empty string for a single unscoped cluster.  | [optional] 
**kubeconfig_secret_ref** | **str** | Name of the Kubernetes Secret holding the cluster kubeconfig. This is a reference NAME only — no credential material crosses this surface.  | 
**status** | **str** | Initial lifecycle status string. Optional; defaults to &#x60;active&#x60; when omitted.  | [optional] 

## Example

```python
from plexsphere.models.register_management_cluster_request import RegisterManagementClusterRequest

# TODO update the JSON string below
json = "{}"
# create an instance of RegisterManagementClusterRequest from a JSON string
register_management_cluster_request_instance = RegisterManagementClusterRequest.from_json(json)
# print the JSON string representation of the object
print(RegisterManagementClusterRequest.to_json())

# convert the object into a dict
register_management_cluster_request_dict = register_management_cluster_request_instance.to_dict()
# create an instance of RegisterManagementClusterRequest from a dict
register_management_cluster_request_from_dict = RegisterManagementClusterRequest.from_dict(register_management_cluster_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


