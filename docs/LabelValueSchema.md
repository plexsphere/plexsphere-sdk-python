# LabelValueSchema

Polymorphic descriptor of a Label Definition's value contract. Callers must set `kind` to one of the five supported values; the sibling fields that accompany each kind are optional on the wire but enforced by the aggregate:   * `string` — optional `max_len` (default 256 bytes).   * `enum` — required `values` (non-empty, unique,     non-blank).   * `numeric` — optional `min` / `max` (int64).   * `boolean` — no extra fields.   * `regex` — required `pattern` (Go regexp). The `kind` property doubles as the OpenAPI discriminator so a generated client can fan out into per-kind narrowing. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**kind** | [**ValueSchemaKind**](ValueSchemaKind.md) |  | 
**max_len** | **int** | Maximum byte length for &#x60;kind&#x3D;string&#x60; (default 256 when omitted; 0 is equivalent to the default).  | [optional] 
**values** | **List[str]** | Permitted string values for &#x60;kind&#x3D;enum&#x60;. Non-empty, unique, non-blank.  | [optional] 
**min** | **int** | Inclusive lower bound for &#x60;kind&#x3D;numeric&#x60;. Omit for an open lower bound.  | [optional] 
**max** | **int** | Inclusive upper bound for &#x60;kind&#x3D;numeric&#x60;. Omit for an open upper bound.  | [optional] 
**pattern** | **str** | Go regexp pattern for &#x60;kind&#x3D;regex&#x60;. Compiled at aggregate build time; malformed patterns are rejected.  | [optional] 

## Example

```python
from plexsphere.models.label_value_schema import LabelValueSchema

# TODO update the JSON string below
json = "{}"
# create an instance of LabelValueSchema from a JSON string
label_value_schema_instance = LabelValueSchema.from_json(json)
# print the JSON string representation of the object
print(LabelValueSchema.to_json())

# convert the object into a dict
label_value_schema_dict = label_value_schema_instance.to_dict()
# create an instance of LabelValueSchema from a dict
label_value_schema_from_dict = LabelValueSchema.from_dict(label_value_schema_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


