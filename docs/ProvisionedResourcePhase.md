# ProvisionedResourcePhase

Closed-set lifecycle phase of a broker-owned ProvisionedResource. `Pending` is the entry phase set when the broker accepts a provisioning declaration; the reconcile loop drives the aggregate forward through `Provisioning` and `Enrolling` to the terminal `Ready` or `Failed`. `Deregistering` and `Deprovisioning` are teardown phases entered when an operator deletes the parent Resource; `Deleted` is the post-teardown terminal. 

## Enum

* `PENDING` (value: `'Pending'`)

* `PROVISIONING` (value: `'Provisioning'`)

* `ENROLLING` (value: `'Enrolling'`)

* `READY` (value: `'Ready'`)

* `FAILED` (value: `'Failed'`)

* `DEREGISTERING` (value: `'Deregistering'`)

* `DEPROVISIONING` (value: `'Deprovisioning'`)

* `DELETED` (value: `'Deleted'`)

[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


