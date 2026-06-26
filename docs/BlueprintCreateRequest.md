# BlueprintCreateRequest

Body for `POST /v1/blueprints`. The field set mirrors the Blueprint aggregate's `NewBlueprint` invariants. The handler authorises the call against the platform-level `manage` relation BEFORE invoking the service so an unauthorised caller never produces a `BlueprintRegistered` outbox row. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**slug** | **str** | Kebab-case URL handle, unique across the catalogue. The aggregate&#39;s &#x60;ParseSlug&#x60; enforces the same regex; surfacing the pattern here lets the generated client validate before the round-trip.  | 
**display_name** | **str** | Human-readable Blueprint name. Whitespace-only is rejected. | 
**description** | **str** | Optional free-form Blueprint description. | [optional] 
**domain_id** | **UUID** | Owning Domain when the Blueprint is scoped to a single Domain. Omit or &#x60;null&#x60; for a catalogue-wide entry.  | [optional] 

## Example

```python
from plexsphere.models.blueprint_create_request import BlueprintCreateRequest

# TODO update the JSON string below
json = "{}"
# create an instance of BlueprintCreateRequest from a JSON string
blueprint_create_request_instance = BlueprintCreateRequest.from_json(json)
# print the JSON string representation of the object
print(BlueprintCreateRequest.to_json())

# convert the object into a dict
blueprint_create_request_dict = blueprint_create_request_instance.to_dict()
# create an instance of BlueprintCreateRequest from a dict
blueprint_create_request_from_dict = BlueprintCreateRequest.from_dict(blueprint_create_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


