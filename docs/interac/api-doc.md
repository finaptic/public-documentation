# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [interac_v1_registration.proto](#interac_v1_registration.proto)
    - [CreateRegistrationRequest](#thebaasco.tenant.interac.v1.CreateRegistrationRequest)
    - [CreateRegistrationResponse](#thebaasco.tenant.interac.v1.CreateRegistrationResponse)
    - [InteracRegistrationCompletedEvent](#thebaasco.tenant.interac.v1.InteracRegistrationCompletedEvent)
    - [PhoneNumber](#thebaasco.tenant.interac.v1.PhoneNumber)
    - [Registration](#thebaasco.tenant.interac.v1.Registration)
  
- [Scalar Value Types](#scalar-value-types)



<a name="interac_v1_registration.proto"></a>
<p align="right"><a href="#top">Top</a></p>

## interac_v1_registration.proto



<a name="thebaasco.tenant.interac.v1.CreateRegistrationRequest"></a>

### CreateRegistrationRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| registration | [Registration](#thebaasco.tenant.interac.v1.Registration) |  |  |






<a name="thebaasco.tenant.interac.v1.CreateRegistrationResponse"></a>

### CreateRegistrationResponse



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| registration | [Registration](#thebaasco.tenant.interac.v1.Registration) |  |  |






<a name="thebaasco.tenant.interac.v1.InteracRegistrationCompletedEvent"></a>

### InteracRegistrationCompletedEvent



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| event_id | [string](#string) |  |  |
| registration_id | [string](#string) |  |  |
| individual_party_id | [string](#string) |  |  |
| expiry_date | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  |  |
| account_id | [string](#string) |  |  |
| alias_email | [string](#string) |  |  |
| alias_phone_number | [PhoneNumber](#thebaasco.tenant.interac.v1.PhoneNumber) |  |  |






<a name="thebaasco.tenant.interac.v1.PhoneNumber"></a>

### PhoneNumber



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| phone_number | [string](#string) |  |  |






<a name="thebaasco.tenant.interac.v1.Registration"></a>

### Registration



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| registration_id | [string](#string) |  | Output only |
| expiry_date | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  | Output only. Alias cannot be used passed this timestamp |
| account_id | [string](#string) |  |  |
| alias_email | [string](#string) |  |  |
| alias_phone_number | [PhoneNumber](#thebaasco.tenant.interac.v1.PhoneNumber) |  |  |





 

 

 

 



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

