# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [coretransaction_v1_accounts.proto](#coretransaction_v1_accounts.proto)
    - [Account](#thebaasco.tenant.coretransaction.v1.Account)
    - [GetAccountRequest](#thebaasco.tenant.coretransaction.v1.GetAccountRequest)
    - [ListAccountsRequest](#thebaasco.tenant.coretransaction.v1.ListAccountsRequest)
    - [ListAccountsResponse](#thebaasco.tenant.coretransaction.v1.ListAccountsResponse)
  
    - [Accounts](#thebaasco.tenant.coretransaction.v1.Accounts)
  
- [coretransaction_v1_transactions.proto](#coretransaction_v1_transactions.proto)
    - [ListTransactionsRequest](#thebaasco.tenant.coretransaction.v1.ListTransactionsRequest)
    - [ListTransactionsResponse](#thebaasco.tenant.coretransaction.v1.ListTransactionsResponse)
    - [Transaction](#thebaasco.tenant.coretransaction.v1.Transaction)
    - [TransactionCreatedEvent](#thebaasco.tenant.coretransaction.v1.TransactionCreatedEvent)
  
    - [Transactions](#thebaasco.tenant.coretransaction.v1.Transactions)
  
- [Scalar Value Types](#scalar-value-types)



<a name="coretransaction_v1_accounts.proto"></a>
<p align="right"><a href="#top">Top</a></p>

## coretransaction_v1_accounts.proto



<a name="thebaasco.tenant.coretransaction.v1.Account"></a>

### Account



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| id | [string](#string) |  |  |
| balance | [thebaasco.types.Balance](#thebaasco.types.Balance) |  |  |
| name | [string](#string) |  |  |
| customers | [string](#string) | repeated | TODO: PB-3374 - review concept for customers/authorized users/owners |
| source_domain | [string](#string) |  |  |






<a name="thebaasco.tenant.coretransaction.v1.GetAccountRequest"></a>

### GetAccountRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| account_id | [string](#string) |  |  |






<a name="thebaasco.tenant.coretransaction.v1.ListAccountsRequest"></a>

### ListAccountsRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| user_id | [string](#string) |  |  |
| page_size | [int32](#int32) |  |  |
| page_token | [string](#string) |  |  |






<a name="thebaasco.tenant.coretransaction.v1.ListAccountsResponse"></a>

### ListAccountsResponse



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| accounts | [Account](#thebaasco.tenant.coretransaction.v1.Account) | repeated |  |
| next_page_token | [string](#string) |  |  |





 

 

 


<a name="thebaasco.tenant.coretransaction.v1.Accounts"></a>

### Accounts


| Method Name | Request Type | Response Type | Description |
| ----------- | ------------ | ------------- | ------------|
| ListAccounts | [ListAccountsRequest](#thebaasco.tenant.coretransaction.v1.ListAccountsRequest) | [ListAccountsResponse](#thebaasco.tenant.coretransaction.v1.ListAccountsResponse) |  |
| GetAccount | [GetAccountRequest](#thebaasco.tenant.coretransaction.v1.GetAccountRequest) | [Account](#thebaasco.tenant.coretransaction.v1.Account) |  |

 



<a name="coretransaction_v1_transactions.proto"></a>
<p align="right"><a href="#top">Top</a></p>

## coretransaction_v1_transactions.proto



<a name="thebaasco.tenant.coretransaction.v1.ListTransactionsRequest"></a>

### ListTransactionsRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| account_id | [string](#string) |  |  |
| from | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  |  |
| to | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  |  |
| page_size | [int32](#int32) |  |  |
| page_token | [string](#string) |  |  |






<a name="thebaasco.tenant.coretransaction.v1.ListTransactionsResponse"></a>

### ListTransactionsResponse



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| transactions | [Transaction](#thebaasco.tenant.coretransaction.v1.Transaction) | repeated |  |
| next_page_token | [string](#string) |  |  |






<a name="thebaasco.tenant.coretransaction.v1.Transaction"></a>

### Transaction



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| transaction_id | [string](#string) |  | Output only. ID of the transaction ( uuid v4 ). This is not a globally unique ID, but uniquely identify the operation that resulted in the change in account balance. In the case of transfer between accounts, there could be 2 transactions that have the same transaction id, but different account ids. |
| lifecycle_transaction_id | [string](#string) |  | This is the client_transaction_id in TM |
| account_id | [string](#string) |  | Output only. ID of the Account ( uuid v4 ). This is a globally unique ID.

Aggregate Root ID |
| transaction_time | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  | Output only. |
| amount | [thebaasco.types.Amount](#thebaasco.types.Amount) |  | Output only. |
| balance | [thebaasco.types.Balance](#thebaasco.types.Balance) |  | Output only. |






<a name="thebaasco.tenant.coretransaction.v1.TransactionCreatedEvent"></a>

### TransactionCreatedEvent



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| event_id | [string](#string) |  | This event ID is a repeatable, globally unique ID ( uuid v5 ) that can be used as idempotency key to determine if the event was already processed or not. This ID is created from the account and transaction ids to ensure repeatability and globaly uniqueness |
| transaction | [Transaction](#thebaasco.tenant.coretransaction.v1.Transaction) |  |  |
| tenant_id | [int32](#int32) |  |  |
| event_time | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  |  |





 

 

 


<a name="thebaasco.tenant.coretransaction.v1.Transactions"></a>

### Transactions


| Method Name | Request Type | Response Type | Description |
| ----------- | ------------ | ------------- | ------------|
| ListTransactions | [ListTransactionsRequest](#thebaasco.tenant.coretransaction.v1.ListTransactionsRequest) | [ListTransactionsResponse](#thebaasco.tenant.coretransaction.v1.ListTransactionsResponse) |  |

 



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

