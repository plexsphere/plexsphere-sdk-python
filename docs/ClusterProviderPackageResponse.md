# ClusterProviderPackageResponse

One Crossplane provider package the platform manages on a management cluster: the package it converged, the version it converged to, and the phase the cluster last reported. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**source** | **str** | OCI repository of the provider package, without a tag or digest (for example &#x60;xpkg.upbound.io/upbound/provider-aws-s3&#x60;). Together with the cluster it identifies the record.  | 
**version** | **str** | Package version the platform converged the cluster to — an OCI tag, optionally pinned with a digest.  | 
**phase** | **str** | Lifecycle phase of the package on this cluster, one of &#x60;Pending&#x60;, &#x60;Installing&#x60;, &#x60;Serving&#x60;, &#x60;Failed&#x60;, or &#x60;Conflict&#x60;. The values carry the meanings documented on &#x60;CloudAssignmentProviderInstall.phase&#x60;.  | 
**message** | **str** | Operator-facing explanation behind the phase: the package manager&#39;s own condition message for &#x60;Failed&#x60;, the competing versions or sources for &#x60;Conflict&#x60;, and absent for &#x60;Serving&#x60;.  | [optional] 
**observed_at** | **datetime** | When the platform last observed the package on the cluster (UTC).  | [optional] 

## Example

```python
from plexsphere.models.cluster_provider_package_response import ClusterProviderPackageResponse

# TODO update the JSON string below
json = "{}"
# create an instance of ClusterProviderPackageResponse from a JSON string
cluster_provider_package_response_instance = ClusterProviderPackageResponse.from_json(json)
# print the JSON string representation of the object
print(ClusterProviderPackageResponse.to_json())

# convert the object into a dict
cluster_provider_package_response_dict = cluster_provider_package_response_instance.to_dict()
# create an instance of ClusterProviderPackageResponse from a dict
cluster_provider_package_response_from_dict = ClusterProviderPackageResponse.from_dict(cluster_provider_package_response_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


