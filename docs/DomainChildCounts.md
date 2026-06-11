# DomainChildCounts

Per-aggregate count of children still attached to a Domain at the moment a `DeleteDomain` call ran. Returned in the Problem detail of a `409 domain_not_empty` response so the operator knows exactly which sub-aggregate is blocking the delete . 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**projects** | **int** | Number of &#x60;Project&#x60; aggregates still attached. | 
**groups** | **int** | Number of &#x60;Group&#x60; aggregates still attached. | 
**identities** | **int** | Number of identities (users + service identities) still attached.  | 
**idp_bindings** | **int** | Number of &#x60;IdPBinding&#x60; rows still attached. | 
**nodes** | **int** | Number of &#x60;Node&#x60; aggregates still attached. | 

## Example

```python
from plexsphere.models.domain_child_counts import DomainChildCounts

# TODO update the JSON string below
json = "{}"
# create an instance of DomainChildCounts from a JSON string
domain_child_counts_instance = DomainChildCounts.from_json(json)
# print the JSON string representation of the object
print(DomainChildCounts.to_json())

# convert the object into a dict
domain_child_counts_dict = domain_child_counts_instance.to_dict()
# create an instance of DomainChildCounts from a dict
domain_child_counts_from_dict = DomainChildCounts.from_dict(domain_child_counts_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


