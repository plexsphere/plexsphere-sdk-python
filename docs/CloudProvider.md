# CloudProvider

Closed-set discriminator naming the upstream cloud provider for a Cloud aggregate. The values mirror the `cloud.ParseProvider` allowlist and the SQL CHECK constraint on `plexsphere.clouds.provider`. The provider is the validator-routing key for the per-provider validator family and is INTENTIONALLY immutable post-creation — see the `cloud` tag description for the rationale.  Each provider requires its own `endpoint` and `region_defaults` fields; the per-provider table is in the Clouds API reference. 

## Enum

* `CloudProviderAws` (value: `'aws'`)

* `CloudProviderAzure` (value: `'azure'`)

* `CloudProviderGcp` (value: `'gcp'`)

* `CloudProviderHetzner` (value: `'hetzner'`)

* `CloudProviderOpenstack` (value: `'openstack'`)

[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


