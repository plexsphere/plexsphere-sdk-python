# ManagementClusterResponse

Metadata-only projection of a registered management cluster. The shape is shared by `RegisterManagementCluster`, `ListManagementClusters`, and `GetManagementCluster`. It deliberately omits the kubeconfig Secret reference contents — only the operator-supplied reference name is a storage-internal detail the surface has no reason to expose. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**id** | **UUID** | Management cluster identifier (UUIDv7). | 
**name** | **str** | Human-readable cluster name. | 
**slug** | **str** | Unique human-facing shard key. Stable for the cluster&#39;s lifetime and used for deterministic region selection.  | 
**region** | **str** | Region the cluster serves. Empty string when the fleet is a single unscoped cluster.  | 
**status** | **str** | Last-observed lifecycle status string maintained by the reconciliation loop (for example &#x60;active&#x60;).  | 
**created_at** | **datetime** | Aggregate creation timestamp (UTC). | 
**updated_at** | **datetime** | Last-modified timestamp (UTC). | 

## Example

```python
from plexsphere.models.management_cluster_response import ManagementClusterResponse

# TODO update the JSON string below
json = "{}"
# create an instance of ManagementClusterResponse from a JSON string
management_cluster_response_instance = ManagementClusterResponse.from_json(json)
# print the JSON string representation of the object
print(ManagementClusterResponse.to_json())

# convert the object into a dict
management_cluster_response_dict = management_cluster_response_instance.to_dict()
# create an instance of ManagementClusterResponse from a dict
management_cluster_response_from_dict = ManagementClusterResponse.from_dict(management_cluster_response_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


