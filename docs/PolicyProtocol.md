# PolicyProtocol

L3/L4 protocol the rule governs. L7-aware matching is out of scope by design. Closed set — values outside this enum reject with `422 unknown_protocol`. `icmp` and `any` MUST be paired with an absent `ports` field. 

## Enum

* `TCP` (value: `'tcp'`)

* `UDP` (value: `'udp'`)

* `ICMP` (value: `'icmp'`)

* `ANY` (value: `'any'`)

[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


