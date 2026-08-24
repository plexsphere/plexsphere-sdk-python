# CloudAssignmentResponse

Metadata projection of a Cloud Assignment. The shape is shared by `RequestCloudAssignment`, `ListCloudAssignments`, `GrantCloudAssignment`, and `RevokeCloudAssignment` so clients only need one binding. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**id** | **UUID** | Cloud Assignment identifier (UUIDv7). | 
**project_id** | **UUID** | Identifier of the consuming Project the Cloud is assigned to.  | 
**cloud_id** | **UUID** | Identifier of the Cloud being made usable in the Project — the residency pivot the operator ReBAC gate authorises against.  | 
**state** | **str** | Lifecycle state of a Cloud Assignment, one of &#x60;requested&#x60;, &#x60;approved&#x60;, &#x60;rejected&#x60;, or &#x60;revoked&#x60;. &#x60;requested&#x60; is the opening state of a project-initiated request; &#x60;approved&#x60; materialises the &#x60;cloud#uses&#x60; binding so the Cloud is usable in the Project; &#x60;rejected&#x60; closes an unapproved request; &#x60;revoked&#x60; tears down a previously approved binding. An operator grant creates the assignment directly in the &#x60;approved&#x60; state. The state is a stored column advanced only by the application service&#39;s transition rules.  | 
**materialised** | **bool** | Whether the assignment&#39;s &#x60;cloud#uses&#x60; binding is currently live. &#x60;true&#x60; only while the assignment is in the &#x60;approved&#x60; state; &#x60;false&#x60; for &#x60;requested&#x60;, &#x60;rejected&#x60;, and &#x60;revoked&#x60;.  | 
**created_at** | **datetime** | Aggregate creation timestamp (UTC). | 
**updated_at** | **datetime** | Last-modified timestamp (UTC). Bumped by every lifecycle transition — approve, reject, revoke.  | 
**provider_installs** | [**List[CloudAssignmentProviderInstall]**](CloudAssignmentProviderInstall.md) | Readiness of every Crossplane provider package the assigned Cloud declares, one entry per package, ordered by &#x60;source&#x60; the same way &#x60;provider_packages&#x60; is on the Cloud itself. Present only while the assignment is &#x60;approved&#x60;: a &#x60;requested&#x60;, &#x60;rejected&#x60;, or &#x60;revoked&#x60; assignment installs nothing, so the property is absent rather than reporting phases that do not apply. An absent array means nothing is installed for this assignment; a readiness the platform cannot read fails the request instead.  Assigning a Cloud to a Project is what causes its provider packages to be installed, so this is the answer to \&quot;can the Project provision against this Cloud yet\&quot;: yes only once every entry reports &#x60;Serving&#x60;.  | [optional] 

## Example

```python
from plexsphere.models.cloud_assignment_response import CloudAssignmentResponse

# TODO update the JSON string below
json = "{}"
# create an instance of CloudAssignmentResponse from a JSON string
cloud_assignment_response_instance = CloudAssignmentResponse.from_json(json)
# print the JSON string representation of the object
print(CloudAssignmentResponse.to_json())

# convert the object into a dict
cloud_assignment_response_dict = cloud_assignment_response_instance.to_dict()
# create an instance of CloudAssignmentResponse from a dict
cloud_assignment_response_from_dict = CloudAssignmentResponse.from_dict(cloud_assignment_response_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


