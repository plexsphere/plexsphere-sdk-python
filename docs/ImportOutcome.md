# ImportOutcome

One Blueprint's import result. `version` carries the metadata version label for the `imported`, `unchanged`, and `drift` statuses; `detail` carries the drift diff for `drift`; `reason` carries a machine-readable code for the `conflict` and `error` statuses. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**slug** | **str** | The Blueprint slug this outcome reports on. | 
**status** | **str** | Per-Blueprint outcome. &#x60;imported&#x60; — a new Blueprint or version was published. &#x60;unchanged&#x60; — an identical re-import. &#x60;drift&#x60; — the same version label with changed content (a diagnostic, never an overwrite). &#x60;conflict&#x60; — the slug is owned in this scope by a different source. &#x60;error&#x60; — a fetch, signature, verification, or publish failure.  | 
**version** | **str** | The version label, present for &#x60;imported&#x60;, &#x60;unchanged&#x60;, and &#x60;drift&#x60;.  | [optional] 
**detail** | **str** | The drift diff, present for the &#x60;drift&#x60; status. | [optional] 
**reason** | **str** | Machine-readable cause for the &#x60;conflict&#x60; and &#x60;error&#x60; statuses. &#x60;blueprint_slug_conflict&#x60; — the slug is owned by a different source. &#x60;catalog_artifact_not_found&#x60; — the bundle did not offer the slug. &#x60;import_failed&#x60; — any other per-Blueprint failure; the cause is recorded server-side and never echoed on the wire.  | [optional] 

## Example

```python
from plexsphere.models.import_outcome import ImportOutcome

# TODO update the JSON string below
json = "{}"
# create an instance of ImportOutcome from a JSON string
import_outcome_instance = ImportOutcome.from_json(json)
# print the JSON string representation of the object
print(ImportOutcome.to_json())

# convert the object into a dict
import_outcome_dict = import_outcome_instance.to_dict()
# create an instance of ImportOutcome from a dict
import_outcome_from_dict = ImportOutcome.from_dict(import_outcome_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


