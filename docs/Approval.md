# Approval

Metadata projection of an Approval. The shape is shared by `ListApprovals`, `GetApproval`, `ApproveApproval`, `RejectApproval`, and `BreakGlassApproval` so clients only need one binding. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**id** | **UUID** | Approval identifier (UUIDv7). | 
**domain_id** | **UUID** | Identifier of the owning Domain — the residency pivot the ReBAC gate authorises against.  | 
**proposer_subject** | **str** | ReBAC subject string of the principal that raised the proposal. A caller may never approve a proposal whose &#x60;proposer_subject&#x60; is themselves.  | 
**action_kind** | **str** | Kind of action the proposal would perform once approved. Matched against the Domain &#x60;ApprovalPolicy&#x60; rules to decide whether the proposal is gated.  | 
**target_resource** | **str** | Resource the proposed action targets. Matched against the optional &#x60;target_resource&#x60; of a policy rule.  | 
**payload** | **Dict[str, object]** | Raw JSON action payload applied verbatim once the proposal is approved. Opaque to the approval workflow — it carries the parameters of the action the proposer intends to run.  | [optional] 
**state** | [**ApprovalState**](ApprovalState.md) |  | 
**created_at** | **datetime** | Aggregate creation timestamp (UTC). | 
**decided_at** | **datetime** | Timestamp the proposal reached a terminal state (UTC). &#x60;null&#x60; or omitted while the proposal is still &#x60;proposed&#x60; or &#x60;pending-approval&#x60;.  | [optional] 
**decided_by_subject** | **str** | ReBAC subject string of the principal that decided the proposal. &#x60;null&#x60; or omitted while undecided and for the unattended &#x60;expired&#x60; path.  | [optional] 
**decision_reason** | **str** | Free-text rationale recorded with the decision. &#x60;null&#x60; or omitted while undecided and for the unattended &#x60;expired&#x60; path. For a break-glass override the rationale value is PII and is NOT surfaced here verbatim — only its field name is projected onto &#x60;caveat_context&#x60;.  | [optional] 
**expires_at** | **datetime** | Deadline past which the background sweeper expires an un-decided proposal (UTC).  | 
**caveat_context** | **Dict[str, List[str]]** | Names-only projection of the caveat field NAMES referenced on the decision&#39;s audit row — for a break-glass override this carries the &#x60;reason&#x60; field name. Values never cross this boundary: the map keys are caveat NAMES and the arrays are caveat-parameter NAMES, mirroring the Platform Audit Log invariant. &#x60;null&#x60; or omitted while the proposal carries no decision audit row.  | [optional] 

## Example

```python
from plexsphere.models.approval import Approval

# TODO update the JSON string below
json = "{}"
# create an instance of Approval from a JSON string
approval_instance = Approval.from_json(json)
# print the JSON string representation of the object
print(Approval.to_json())

# convert the object into a dict
approval_dict = approval_instance.to_dict()
# create an instance of Approval from a dict
approval_from_dict = Approval.from_dict(approval_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


