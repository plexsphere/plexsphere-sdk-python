# AuditChainVerifyRequest

Bounds for the verify run. Omit both fields to verify the full chain anchored at the genesis row. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**from_seq** | **int** | Inclusive lower bound on &#x60;seq&#x60;. Defaults to &#x60;1&#x60; when absent — i.e. the genesis row.  | [optional] 
**to_seq** | **int** | Inclusive upper bound on &#x60;seq&#x60;. Defaults to the current chain head when absent.  | [optional] 

## Example

```python
from plexsphere.models.audit_chain_verify_request import AuditChainVerifyRequest

# TODO update the JSON string below
json = "{}"
# create an instance of AuditChainVerifyRequest from a JSON string
audit_chain_verify_request_instance = AuditChainVerifyRequest.from_json(json)
# print the JSON string representation of the object
print(AuditChainVerifyRequest.to_json())

# convert the object into a dict
audit_chain_verify_request_dict = audit_chain_verify_request_instance.to_dict()
# create an instance of AuditChainVerifyRequest from a dict
audit_chain_verify_request_from_dict = AuditChainVerifyRequest.from_dict(audit_chain_verify_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


