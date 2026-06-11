# AuditDecision

Decision-side projection on an audit row. `result` is the rendered Reason; `caveat_context` lists the caveat field NAMES referenced by the check — values never cross this boundary. The `x-plexsphere-names-only` marker on `caveat_context` flags the names-only invariant for review tooling and is policed by the `plexsphere-audit-caveat-names-only` Spectral rule. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**result** | [**AuditReason**](AuditReason.md) |  | 
**caveat_context** | **Dict[str, List[str]]** | Map from caveat NAME to the array of caveat-parameter NAMES referenced by the check. Values are never present on the wire — is structural: the only string content here is identifiers. &#x60;internal/audit.Entry&#x60; and &#x60;internal/audit.AppendInput&#x60; enforce the same discipline structurally on the Go side.  | 

## Example

```python
from plexsphere.models.audit_decision import AuditDecision

# TODO update the JSON string below
json = "{}"
# create an instance of AuditDecision from a JSON string
audit_decision_instance = AuditDecision.from_json(json)
# print the JSON string representation of the object
print(AuditDecision.to_json())

# convert the object into a dict
audit_decision_dict = audit_decision_instance.to_dict()
# create an instance of AuditDecision from a dict
audit_decision_from_dict = AuditDecision.from_dict(audit_decision_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


