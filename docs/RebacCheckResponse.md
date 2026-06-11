# RebacCheckResponse

Result of `POST /v1/authz/check`. `decision` is the machine-readable outcome; `relation_path` is set only on `allowed`; `reason` is set only on `denied`. `correlation_id` is always set and pairs the response with the matching audit entry. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**decision** | **str** | ReBAC decision. &#x60;allowed&#x60; is the affirmative outcome; &#x60;denied&#x60; is a policy denial — NEVER an HTTP error.  | 
**relation_path** | **List[str]** | Ordered sequence of relations the authorizer traversed while evaluating the Check. Set only when &#x60;decision&#x60; is &#x60;allowed&#x60;. Empty array when the decision was reached without a relation lookup (typically a direct-binding allowance).  | [optional] 
**reason** | **str** | Machine-readable denial reason. Set only when &#x60;decision&#x60; is &#x60;denied&#x60;. Values match &#x60;internal/audit.Reason.String&#x60; extended with &#x60;granted&#x60; (allowance bookkeeping) and &#x60;unknown&#x60; (defensive default the authorizer never emits on a happy path). &#x60;out_of_scope&#x60;, &#x60;insufficient_relation&#x60;, and &#x60;caveat_violation&#x60; mirror the &#x60;PermissionDenied.reason&#x60; enum used by the 403 surface elsewhere in this spec.  | [optional] 
**correlation_id** | **str** | Correlation id that pairs this decision with the matching audit entry emitted by &#x60;internal/audit&#x60;.  | 

## Example

```python
from plexsphere.models.rebac_check_response import RebacCheckResponse

# TODO update the JSON string below
json = "{}"
# create an instance of RebacCheckResponse from a JSON string
rebac_check_response_instance = RebacCheckResponse.from_json(json)
# print the JSON string representation of the object
print(RebacCheckResponse.to_json())

# convert the object into a dict
rebac_check_response_dict = rebac_check_response_instance.to_dict()
# create an instance of RebacCheckResponse from a dict
rebac_check_response_from_dict = RebacCheckResponse.from_dict(rebac_check_response_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


