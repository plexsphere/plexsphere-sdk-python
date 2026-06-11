# ProjectList

Page of Projects returned by `GET /v1/projects`. The window is computed by the persistence layer in slug order; per-row visibility is layered on top so the `items` array is the subset the caller is authorised to see. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**items** | [**List[ProjectResponse]**](ProjectResponse.md) | Projects in the current page. | 
**next_cursor** | **str** | Continuation token for the next page. &#x60;null&#x60; or omitted when the iteration has reached end-of-stream. The encoding is HMAC-signed by the server so a tampered cursor surfaces as &#x60;400&#x60; on the next call.  | [optional] 
**empty_reason** | **str** | Disambiguates the &#x60;items: []&#x60; page. Omitted (or &#x60;null&#x60;) when &#x60;items&#x60; is non-empty OR when the persistence layer itself returned zero rows (the \&quot;no Projects exist\&quot; case — possibly scoped by a &#x60;domain_id&#x60; filter). Set to &#x60;no_rebac_membership&#x60; when the persistence layer returned at least one row but the per-row ReBAC visibility filter dropped every one — the chicken-and-egg state a freshly-provisioned operator hits before any &#x60;project#admin&#x60; (or parent &#x60;domain#admin&#x60; / &#x60;domain#member&#x60;) tuple has been seeded for the calling Principal. Mirrors the field of the same name on &#x60;DomainList&#x60;.  | [optional] 

## Example

```python
from plexsphere.models.project_list import ProjectList

# TODO update the JSON string below
json = "{}"
# create an instance of ProjectList from a JSON string
project_list_instance = ProjectList.from_json(json)
# print the JSON string representation of the object
print(ProjectList.to_json())

# convert the object into a dict
project_list_dict = project_list_instance.to_dict()
# create an instance of ProjectList from a dict
project_list_from_dict = ProjectList.from_dict(project_list_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


