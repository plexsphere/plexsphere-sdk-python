# DomainCapacityDimensionReading

One capacity dimension's reading within a `DomainCapacitySnapshot`: the dimension identifier, its unit, the latest sampled usage, the per-Domain target, and the `used / target` ratio the Dashboard renders as a percentage. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**dimension** | [**CapacityDimension**](CapacityDimension.md) |  | 
**unit** | [**CapacityUnit**](CapacityUnit.md) |  | 
**used** | **float** | Latest sampled usage for the dimension, in the dimension&#39;s &#x60;unit&#x60; (an absolute tally for &#x60;count&#x60;, a sustained rate for the &#x60;_per_second&#x60; units).  | 
**target** | **float** | The per-Domain target for the dimension, in the dimension&#39;s &#x60;unit&#x60;. A target of &#x60;0&#x60; means no target is configured.  | 
**ratio** | **float** | &#x60;used / target&#x60;, the fraction of the target currently consumed. &#x60;0&#x60; when no target is configured. A value at or above &#x60;0.8&#x60; is the threshold the collector records a crossing for.  | 

## Example

```python
from plexsphere.models.domain_capacity_dimension_reading import DomainCapacityDimensionReading

# TODO update the JSON string below
json = "{}"
# create an instance of DomainCapacityDimensionReading from a JSON string
domain_capacity_dimension_reading_instance = DomainCapacityDimensionReading.from_json(json)
# print the JSON string representation of the object
print(DomainCapacityDimensionReading.to_json())

# convert the object into a dict
domain_capacity_dimension_reading_dict = domain_capacity_dimension_reading_instance.to_dict()
# create an instance of DomainCapacityDimensionReading from a dict
domain_capacity_dimension_reading_from_dict = DomainCapacityDimensionReading.from_dict(domain_capacity_dimension_reading_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


