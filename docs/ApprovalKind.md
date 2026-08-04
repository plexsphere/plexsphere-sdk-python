# ApprovalKind

Source family the queue row was projected from. `approval` marks a dual-control Approval raised against a Domain `ApprovalPolicy`; `credential_assignment` marks a Credential Assignment awaiting a decision on the Cloud Credential it spends; `cloud_assignment` marks a Cloud Assignment awaiting a decision on the Cloud it spends. The kind selects which per-row authorisation check and which decide path apply. 

## Enum

* `APPROVAL` (value: `'approval'`)

* `CREDENTIAL_ASSIGNMENT` (value: `'credential_assignment'`)

* `CLOUD_ASSIGNMENT` (value: `'cloud_assignment'`)

[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


