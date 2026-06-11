# NamespacePhase

Lifecycle phase of a Project's namespace on its management cluster, advanced only by the reconciliation loop (and to `Terminating` by the operator terminate endpoint). `Pending` before the cluster is first observed; `Provisioning` while resources converge; `Ready` once the verify gate passes; `Degraded` after a previously-Ready namespace loses a resource; `Terminating` once teardown is requested; `Deleted` once all resources are removed. 

## Enum

* `PENDING` (value: `'Pending'`)

* `PROVISIONING` (value: `'Provisioning'`)

* `READY` (value: `'Ready'`)

* `DEGRADED` (value: `'Degraded'`)

* `TERMINATING` (value: `'Terminating'`)

* `DELETED` (value: `'Deleted'`)

[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


