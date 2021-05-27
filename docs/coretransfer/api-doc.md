# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [coretransfer_v1_routinginformation.proto](#coretransfer_v1_routinginformation.proto)
    - [AccountRouting](#thebaasco.tenant.coretransfer.v1.AccountRouting)
    - [AccountRoutingLinkedEvent](#thebaasco.tenant.coretransfer.v1.AccountRoutingLinkedEvent)
    - [CreateAccountRoutingRequest](#thebaasco.tenant.coretransfer.v1.CreateAccountRoutingRequest)
    - [CreateAccountRoutingResponse](#thebaasco.tenant.coretransfer.v1.CreateAccountRoutingResponse)
    - [GetAccountRoutingRequest](#thebaasco.tenant.coretransfer.v1.GetAccountRoutingRequest)
  
    - [Routing](#thebaasco.tenant.coretransfer.v1.Routing)
  
- [Scalar Value Types](#scalar-value-types)



<a name="coretransfer_v1_routinginformation.proto"></a>
<p align="right"><a href="#top">Top</a></p>

## coretransfer_v1_routinginformation.proto



<a name="thebaasco.tenant.coretransfer.v1.AccountRouting"></a>

### AccountRouting



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| transit_number | [int32](#int32) |  |  |
| institution_number | [int32](#int32) |  |  |
| account_number | [int64](#int64) |  |  |






<a name="thebaasco.tenant.coretransfer.v1.AccountRoutingLinkedEvent"></a>

### AccountRoutingLinkedEvent



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| event_id | [string](#string) |  |  |
| account_id | [string](#string) |  |  |
| account_routing | [AccountRouting](#thebaasco.tenant.coretransfer.v1.AccountRouting) |  |  |
| tenant_id | [int32](#int32) |  |  |
| account_routing_linked_time | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  |  |






<a name="thebaasco.tenant.coretransfer.v1.CreateAccountRoutingRequest"></a>

### CreateAccountRoutingRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| account_id | [string](#string) |  |  |






<a name="thebaasco.tenant.coretransfer.v1.CreateAccountRoutingResponse"></a>

### CreateAccountRoutingResponse



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| account_id | [string](#string) |  |  |
| account_routing | [AccountRouting](#thebaasco.tenant.coretransfer.v1.AccountRouting) |  |  |






<a name="thebaasco.tenant.coretransfer.v1.GetAccountRoutingRequest"></a>

### GetAccountRoutingRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| account_id | [string](#string) |  |  |





 

 

 


<a name="thebaasco.tenant.coretransfer.v1.Routing"></a>

### Routing


| Method Name | Request Type | Response Type | Description |
| ----------- | ------------ | ------------- | ------------|
| GetAccountRouting | [GetAccountRoutingRequest](#thebaasco.tenant.coretransfer.v1.GetAccountRoutingRequest) | [AccountRouting](#thebaasco.tenant.coretransfer.v1.AccountRouting) |  |

 



## Scalar Value Types

| .proto Type | Notes | C++ | Java | Python | Go | C# | PHP | Ruby |
| ----------- | ----- | --- | ---- | ------ | -- | -- | --- | ---- |
| <a name="double" /> double |  | double | double | float | float64 | double | float | Float |
| <a name="float" /> float |  | float | float | float | float32 | float | float | Float |
| <a name="int32" /> int32 | Uses variable-length encoding. Inefficient for encoding negative numbers – if your field is likely to have negative values, use sint32 instead. | int32 | int | int | int32 | int | integer | Bignum or Fixnum (as required) |
| <a name="int64" /> int64 | Uses variable-length encoding. Inefficient for encoding negative numbers – if your field is likely to have negative values, use sint64 instead. | int64 | long | int/long | int64 | long | integer/string | Bignum |
| <a name="uint32" /> uint32 | Uses variable-length encoding. | uint32 | int | int/long | uint32 | uint | integer | Bignum or Fixnum (as required) |
| <a name="uint64" /> uint64 | Uses variable-length encoding. | uint64 | long | int/long | uint64 | ulong | integer/string | Bignum or Fixnum (as required) |
| <a name="sint32" /> sint32 | Uses variable-length encoding. Signed int value. These more efficiently encode negative numbers than regular int32s. | int32 | int | int | int32 | int | integer | Bignum or Fixnum (as required) |
| <a name="sint64" /> sint64 | Uses variable-length encoding. Signed int value. These more efficiently encode negative numbers than regular int64s. | int64 | long | int/long | int64 | long | integer/string | Bignum |
| <a name="fixed32" /> fixed32 | Always four bytes. More efficient than uint32 if values are often greater than 2^28. | uint32 | int | int | uint32 | uint | integer | Bignum or Fixnum (as required) |
| <a name="fixed64" /> fixed64 | Always eight bytes. More efficient than uint64 if values are often greater than 2^56. | uint64 | long | int/long | uint64 | ulong | integer/string | Bignum |
| <a name="sfixed32" /> sfixed32 | Always four bytes. | int32 | int | int | int32 | int | integer | Bignum or Fixnum (as required) |
| <a name="sfixed64" /> sfixed64 | Always eight bytes. | int64 | long | int/long | int64 | long | integer/string | Bignum |
| <a name="bool" /> bool |  | bool | boolean | boolean | bool | bool | boolean | TrueClass/FalseClass |
| <a name="string" /> string | A string must always contain UTF-8 encoded or 7-bit ASCII text. | string | String | str/unicode | string | string | string | String (UTF-8) |
| <a name="bytes" /> bytes | May contain any arbitrary sequence of bytes. | string | ByteString | str | []byte | ByteString | string | String (ASCII-8BIT) |

