# ImportRequest

Body for `POST /v1/blueprint-catalogs/{id}/import`. Supply exactly one of `slugs` (a non-empty subset to import) or `all: true` (import every Blueprint the source offers). Supplying neither, both, or an empty `slugs` array surfaces as `400 invalid_import_request`. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**slugs** | **List[str]** | The subset of offered Blueprint slugs to import. Mutually exclusive with &#x60;all&#x60;; must be non-empty when present and may carry at most 256 entries — the server caps the per-request import subset so one call cannot fan out into an unbounded number of upstream registry round-trips.  | [optional] 
**all** | **bool** | Import every Blueprint the source offers. Mutually exclusive with &#x60;slugs&#x60;.  | [optional] 

## Example

```python
from plexsphere.models.import_request import ImportRequest

# TODO update the JSON string below
json = "{}"
# create an instance of ImportRequest from a JSON string
import_request_instance = ImportRequest.from_json(json)
# print the JSON string representation of the object
print(ImportRequest.to_json())

# convert the object into a dict
import_request_dict = import_request_instance.to_dict()
# create an instance of ImportRequest from a dict
import_request_from_dict = ImportRequest.from_dict(import_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


