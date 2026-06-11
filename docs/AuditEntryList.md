# AuditEntryList

Cursor-paginated page returned by `ListAuditEntries`. The `next_cursor` field is the value to pass back as the `cursor` query parameter on the next call; it is `null` (or absent) when the page is short — that is the end-of-stream signal callers stop on. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**entries** | [**List[AuditEntry]**](AuditEntry.md) | Audit rows in ascending &#x60;seq&#x60; order. | 
**next_cursor** | **str** | Opaque, HMAC-signed cursor scoped to the addressed Domain. Replaying a cursor minted for a different Domain surfaces as 400 with &#x60;code: cursor_invalid&#x60;.  | [optional] 

## Example

```python
from plexsphere.models.audit_entry_list import AuditEntryList

# TODO update the JSON string below
json = "{}"
# create an instance of AuditEntryList from a JSON string
audit_entry_list_instance = AuditEntryList.from_json(json)
# print the JSON string representation of the object
print(AuditEntryList.to_json())

# convert the object into a dict
audit_entry_list_dict = audit_entry_list_instance.to_dict()
# create an instance of AuditEntryList from a dict
audit_entry_list_from_dict = AuditEntryList.from_dict(audit_entry_list_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


