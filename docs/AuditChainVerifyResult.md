# AuditChainVerifyResult

Outcome of a verify run. DIVERGENCE IS NOT AN ERROR — a tampered chain surfaces here as `ok: false` with the offending `divergent_seq` and the expected/observed hashes pinned. The handler MUST NEVER return a 5xx for tampering; the 5xx channel is reserved for infrastructure faults the verifier cannot reason about. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**ok** | **bool** | &#x60;true&#x60; when every row in the inspected segment hashes correctly and chains to its predecessor; &#x60;false&#x60; when the verifier observed a divergence at &#x60;divergent_seq&#x60;.  | 
**divergent_seq** | **int** | Seq of the first row whose recomputed hash did not match the stored value, or whose &#x60;prev_hash&#x60; did not match the previous row&#39;s &#x60;entry_hash&#x60;. &#x60;null&#x60; on a clean run.  | [optional] 
**expected_hash** | **str** | Hash the verifier computed for &#x60;divergent_seq&#x60;, lowercase hex. &#x60;null&#x60; on a clean run.  | [optional] 
**observed_hash** | **str** | Hash actually stored at &#x60;divergent_seq&#x60;, lowercase hex. &#x60;null&#x60; on a clean run.  | [optional] 
**segment_from** | **int** | Inclusive lower bound the verifier actually walked. | 
**segment_to** | **int** | Inclusive upper bound the verifier actually walked. | 

## Example

```python
from plexsphere.models.audit_chain_verify_result import AuditChainVerifyResult

# TODO update the JSON string below
json = "{}"
# create an instance of AuditChainVerifyResult from a JSON string
audit_chain_verify_result_instance = AuditChainVerifyResult.from_json(json)
# print the JSON string representation of the object
print(AuditChainVerifyResult.to_json())

# convert the object into a dict
audit_chain_verify_result_dict = audit_chain_verify_result_instance.to_dict()
# create an instance of AuditChainVerifyResult from a dict
audit_chain_verify_result_from_dict = AuditChainVerifyResult.from_dict(audit_chain_verify_result_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


