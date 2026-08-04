# Reachability

Per-Node reachability projection driven by the heartbeat handler. A Node enters the projection in `never_reported` and leaves it for a heartbeat-derived verdict once its first heartbeat is admitted. From there the state-machine transitions `healthy` → `stale` after 90 seconds without an accepted heartbeat and `stale` → `unreachable` after 300 seconds. `last_heartbeat_at` is absent until the first heartbeat is accepted; `changed_at` is always present and tracks the most recent state transition. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**state** | **str** | Current reachability state. &#x60;never_reported&#x60; means no heartbeat has ever been admitted for the Node; it is the state a Node is born in at enrolment. The other three values are derived from the age of the most recent admitted heartbeat: &#x60;healthy&#x60; means it is younger than 90s, &#x60;stale&#x60; means it is between 90s and 300s old, and &#x60;unreachable&#x60; means it is older than 300s. &#x60;unreachable&#x60; therefore applies only to a Node whose heartbeats stopped.  | 
**last_heartbeat_at** | **datetime** | Server-side timestamp of the most recently accepted heartbeat; absent until the first heartbeat is accepted.  | [optional] 
**changed_at** | **datetime** | Server-side timestamp of the most recent transition into the current &#x60;state&#x60;. Always present — for a Node that has never sent a heartbeat this is the timestamp at which the reachability row was first materialised.  | 

## Example

```python
from plexsphere.models.reachability import Reachability

# TODO update the JSON string below
json = "{}"
# create an instance of Reachability from a JSON string
reachability_instance = Reachability.from_json(json)
# print the JSON string representation of the object
print(Reachability.to_json())

# convert the object into a dict
reachability_dict = reachability_instance.to_dict()
# create an instance of Reachability from a dict
reachability_from_dict = Reachability.from_dict(reachability_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


