# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [corebanking_v1_accounts.proto](#corebanking_v1_accounts.proto)
    - [AccountCreatedEvent](#thebaasco.tenant.corebanking.v1.AccountCreatedEvent)
    - [AccountDetails](#thebaasco.tenant.corebanking.v1.AccountDetails)
    - [AccountStatusChangedEvent](#thebaasco.tenant.corebanking.v1.AccountStatusChangedEvent)
    - [ChangeAccountStatusRequest](#thebaasco.tenant.corebanking.v1.ChangeAccountStatusRequest)
    - [ChangeAccountStatusResponse](#thebaasco.tenant.corebanking.v1.ChangeAccountStatusResponse)
    - [CreateAccountRequest](#thebaasco.tenant.corebanking.v1.CreateAccountRequest)
    - [CreateAccountRequest.AccountParametersEntry](#thebaasco.tenant.corebanking.v1.CreateAccountRequest.AccountParametersEntry)
    - [CreateAccountResponse](#thebaasco.tenant.corebanking.v1.CreateAccountResponse)
    - [GetAccountDetailsRequest](#thebaasco.tenant.corebanking.v1.GetAccountDetailsRequest)
  
    - [AccountStatus](#thebaasco.tenant.corebanking.v1.AccountStatus)
  
    - [Accounts](#thebaasco.tenant.corebanking.v1.Accounts)
  
- [Scalar Value Types](#scalar-value-types)



<a name="corebanking_v1_accounts.proto"></a>
<p align="right"><a href="#top">Top</a></p>

## corebanking_v1_accounts.proto



<a name="thebaasco.tenant.corebanking.v1.AccountCreatedEvent"></a>

### AccountCreatedEvent



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| event_id | [string](#string) |  |  |
| account_details | [AccountDetails](#thebaasco.tenant.corebanking.v1.AccountDetails) |  |  |
| tenant_id | [int32](#int32) |  |  |
| account_creation_time | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  |  |






<a name="thebaasco.tenant.corebanking.v1.AccountDetails"></a>

### AccountDetails



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| account_id | [string](#string) |  | Output only |
| status | [AccountStatus](#thebaasco.tenant.corebanking.v1.AccountStatus) |  | Output only |
| owner_client_id | [string](#string) |  | Output only |
| authorized_client_ids | [string](#string) | repeated | Output only |
| accrued_interest | [thebaasco.types.Amount](#thebaasco.types.Amount) |  | Output only

interest that was accumulated, but hasn&#39;t been applied to the balance yet |






<a name="thebaasco.tenant.corebanking.v1.AccountStatusChangedEvent"></a>

### AccountStatusChangedEvent



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| event_id | [string](#string) |  |  |
| account_details | [AccountDetails](#thebaasco.tenant.corebanking.v1.AccountDetails) |  |  |
| tenant_id | [int32](#int32) |  | mask that ensure we can&#39;t use more than 56bits |
| account_status_changed_time | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  |  |






<a name="thebaasco.tenant.corebanking.v1.ChangeAccountStatusRequest"></a>

### ChangeAccountStatusRequest
Asynchronous request to change the state of an account.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| account_id | [string](#string) |  |  |
| status | [AccountStatus](#thebaasco.tenant.corebanking.v1.AccountStatus) |  |  |






<a name="thebaasco.tenant.corebanking.v1.ChangeAccountStatusResponse"></a>

### ChangeAccountStatusResponse



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| account_details | [AccountDetails](#thebaasco.tenant.corebanking.v1.AccountDetails) |  |  |






<a name="thebaasco.tenant.corebanking.v1.CreateAccountRequest"></a>

### CreateAccountRequest
Asynchronous request to open an account.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| account_type | [string](#string) |  | Type of account to open, this is a string that is composed in conjunction between product bundling endine and requestor domain (e.g. &#34;current_account.v39&#34; ) |
| account_parameters | [CreateAccountRequest.AccountParametersEntry](#thebaasco.tenant.corebanking.v1.CreateAccountRequest.AccountParametersEntry) | repeated | Parameters for the type of account, this is composed in conjunction between product bundling engine, and requestor domain. |






<a name="thebaasco.tenant.corebanking.v1.CreateAccountRequest.AccountParametersEntry"></a>

### CreateAccountRequest.AccountParametersEntry



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| key | [string](#string) |  |  |
| value | [string](#string) |  |  |






<a name="thebaasco.tenant.corebanking.v1.CreateAccountResponse"></a>

### CreateAccountResponse



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| account_details | [AccountDetails](#thebaasco.tenant.corebanking.v1.AccountDetails) |  |  |






<a name="thebaasco.tenant.corebanking.v1.GetAccountDetailsRequest"></a>

### GetAccountDetailsRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| account_id | [string](#string) |  |  |





 


<a name="thebaasco.tenant.corebanking.v1.AccountStatus"></a>

### AccountStatus
Indicate the status of the account. All accounts are created in the &#34;INACTIVE&#34; state.

| Name | Number | Description |
| ---- | ------ | ----------- |
| ACCOUNT_STATUS_UNSPECIFIED | 0 |  |
| ACCOUNT_STATUS_INACTIVE | 1 | Initial state after creation. Once moved from INACTIVE to anything else, cannot come back. |
| ACCOUNT_STATUS_ACTIVE | 2 | Normal operations can proceed. |


 

 


<a name="thebaasco.tenant.corebanking.v1.Accounts"></a>

### Accounts


| Method Name | Request Type | Response Type | Description |
| ----------- | ------------ | ------------- | ------------|
| GetAccountDetails | [GetAccountDetailsRequest](#thebaasco.tenant.corebanking.v1.GetAccountDetailsRequest) | [AccountDetails](#thebaasco.tenant.corebanking.v1.AccountDetails) |  |

 



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

