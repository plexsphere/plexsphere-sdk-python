# CloudAssignmentProviderInstall

Readiness of ONE Crossplane provider package the assigned Cloud declares, on the management cluster hosting the Project. The entry names the package it reports on in `source`, so a Cloud declaring several packages yields several entries in `provider_installs`. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**source** | **str** | OCI repository of the declared package this entry reports on, without a tag or digest (e.g. &#x60;xpkg.upbound.io/upbound/provider-aws-s3&#x60;). It matches one of the &#x60;source&#x60; values in the Cloud&#39;s &#x60;provider_packages&#x60;.  | 
**phase** | **str** | Lifecycle phase of the provider package on the hosting cluster, one of &#x60;Pending&#x60;, &#x60;Installing&#x60;, &#x60;Serving&#x60;, &#x60;Failed&#x60;, or &#x60;Conflict&#x60;. &#x60;Pending&#x60; means the platform has not yet reconciled the package — including the window before the Project has been placed on a management cluster. &#x60;Installing&#x60; means the package is applied and the package manager has not yet reported it healthy. &#x60;Serving&#x60; means the Project can provision against the Cloud. &#x60;Failed&#x60; means the package manager reported it unhealthy, with the reason in &#x60;message&#x60;. &#x60;Conflict&#x60; means the package cannot be applied without overwriting something the platform does not own — two Clouds naming the same package at different versions, or an existing provider serving a different package — and &#x60;message&#x60; names both sides.  | 
**message** | **str** | Operator-facing explanation behind the phase: the package manager&#39;s own condition message for &#x60;Failed&#x60;, the competing versions or sources for &#x60;Conflict&#x60;, and absent for &#x60;Serving&#x60;.  | [optional] 
**observed_at** | **datetime** | When the platform last observed the package on the cluster (UTC). Absent while the phase is &#x60;Pending&#x60;, which is the state that has not been observed yet.  | [optional] 

## Example

```python
from plexsphere.models.cloud_assignment_provider_install import CloudAssignmentProviderInstall

# TODO update the JSON string below
json = "{}"
# create an instance of CloudAssignmentProviderInstall from a JSON string
cloud_assignment_provider_install_instance = CloudAssignmentProviderInstall.from_json(json)
# print the JSON string representation of the object
print(CloudAssignmentProviderInstall.to_json())

# convert the object into a dict
cloud_assignment_provider_install_dict = cloud_assignment_provider_install_instance.to_dict()
# create an instance of CloudAssignmentProviderInstall from a dict
cloud_assignment_provider_install_from_dict = CloudAssignmentProviderInstall.from_dict(cloud_assignment_provider_install_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


