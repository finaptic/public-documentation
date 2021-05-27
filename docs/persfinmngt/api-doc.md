# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [notifications_v1.proto](#notifications_v1.proto)
    - [RegisterChannelRequest](#thebaasco.tenant.persfinmngt.v1.RegisterChannelRequest)
  
    - [Notifications](#thebaasco.tenant.persfinmngt.v1.Notifications)
  
- [persfinmngt_v1_transactions.proto](#persfinmngt_v1_transactions.proto)
    - [GetTransactionsRequest](#thebaasco.tenant.persfinmngt.v1.GetTransactionsRequest)
    - [GetTransactionsResponse](#thebaasco.tenant.persfinmngt.v1.GetTransactionsResponse)
    - [Transaction](#thebaasco.tenant.persfinmngt.v1.Transaction)
    - [TransactionCreatedEvent](#thebaasco.tenant.persfinmngt.v1.TransactionCreatedEvent)
  
    - [Order](#thebaasco.tenant.persfinmngt.v1.Order)
    - [Transaction.TransactionState](#thebaasco.tenant.persfinmngt.v1.Transaction.TransactionState)
    - [Transaction.TransactionType](#thebaasco.tenant.persfinmngt.v1.Transaction.TransactionType)
  
    - [Transactions](#thebaasco.tenant.persfinmngt.v1.Transactions)
  
- [Scalar Value Types](#scalar-value-types)



<a name="notifications_v1.proto"></a>
<p align="right"><a href="#top">Top</a></p>

## notifications_v1.proto



<a name="thebaasco.tenant.persfinmngt.v1.RegisterChannelRequest"></a>

### RegisterChannelRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| device_id | [string](#string) |  |  |
| registration_id | [string](#string) |  |  |





 

 

 


<a name="thebaasco.tenant.persfinmngt.v1.Notifications"></a>

### Notifications


| Method Name | Request Type | Response Type | Description |
| ----------- | ------------ | ------------- | ------------|
| RegisterChannel | [RegisterChannelRequest](#thebaasco.tenant.persfinmngt.v1.RegisterChannelRequest) | [.google.protobuf.Empty](#google.protobuf.Empty) |  |

 



<a name="persfinmngt_v1_transactions.proto"></a>
<p align="right"><a href="#top">Top</a></p>

## persfinmngt_v1_transactions.proto



<a name="thebaasco.tenant.persfinmngt.v1.GetTransactionsRequest"></a>

### GetTransactionsRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| account_id | [string](#string) |  |  |
| page_size | [int32](#int32) |  |  |
| page_token | [string](#string) |  |  |
| sort_order | [Order](#thebaasco.tenant.persfinmngt.v1.Order) |  |  |






<a name="thebaasco.tenant.persfinmngt.v1.GetTransactionsResponse"></a>

### GetTransactionsResponse



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| previous_page_token | [string](#string) |  |  |
| transactions | [Transaction](#thebaasco.tenant.persfinmngt.v1.Transaction) | repeated |  |
| next_page_token | [string](#string) |  |  |
| sort_order | [Order](#thebaasco.tenant.persfinmngt.v1.Order) |  |  |






<a name="thebaasco.tenant.persfinmngt.v1.Transaction"></a>

### Transaction



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| transaction_id | [string](#string) |  | Unique identifier that represents this single transaction (operation). |
| lifecycle_transaction_id | [string](#string) |  | Unique identifier that represents the overarching transaction, across all phases of it&#39;s lifecycle (e.g. authorization, settlement, reversal, ...) |
| amount | [thebaasco.types.Amount](#thebaasco.types.Amount) |  | This is the transaction amount // |
| transactor_name | [string](#string) |  | The name and location of the vendor where the card payment was processed |
| balance | [thebaasco.types.Balance](#thebaasco.types.Balance) |  | The remaining avialable balance for that specified account// |
| transaction_state | [Transaction.TransactionState](#thebaasco.tenant.persfinmngt.v1.Transaction.TransactionState) |  | This is defined in PFM to make it easier for the end user |
| transaction_type | [Transaction.TransactionType](#thebaasco.tenant.persfinmngt.v1.Transaction.TransactionType) |  | The transaction states will be a subset of the transaction type |
| account_id | [string](#string) |  | The authorised account id of that user on which a debit or credit transaction has occured . |
| category | [string](#string) |  | The localized description for the transaction&#39;s category (i.e. Groceries, Transport, ... based off the MCC ). |
| transaction_time | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  | The transaction time when the card/ interac transfer took place // |
| source_domain | [string](#string) |  | Name of the domain that can be querried for additional details on this transaction ( e.g. core-card, core-transfer, ... ) |






<a name="thebaasco.tenant.persfinmngt.v1.TransactionCreatedEvent"></a>

### TransactionCreatedEvent



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| event_id | [string](#string) |  | This needs to be unique per transaction_id/customer_id pair, to ensure proper idempotency handling for consumers. |
| customer_id | [string](#string) |  | cuid for the customer impacted by this transaction. If there are multiple customers associated with an account, then transactions on the account will generate multiple events. |
| transaction | [Transaction](#thebaasco.tenant.persfinmngt.v1.Transaction) |  |  |
| tenant_id | [int32](#int32) |  |  |
| event_time | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  |  |





 


<a name="thebaasco.tenant.persfinmngt.v1.Order"></a>

### Order


| Name | Number | Description |
| ---- | ------ | ----------- |
| ORDER_UNSPECIFIED | 0 |  |
| DESCENDING | 1 |  |
| ASCENDING | 2 |  |



<a name="thebaasco.tenant.persfinmngt.v1.Transaction.TransactionState"></a>

### Transaction.TransactionState


| Name | Number | Description |
| ---- | ------ | ----------- |
| TRANSACTION_STATE_UNSPECIFIED | 0 |  |
| POSTED | 1 |  |
| PENDING | 2 |  |
| UNKNOWN | 3 | For use only when available information is insufficient to determine state |



<a name="thebaasco.tenant.persfinmngt.v1.Transaction.TransactionType"></a>

### Transaction.TransactionType


| Name | Number | Description |
| ---- | ------ | ----------- |
| TRANSACTION_TYPE_UNSPECIFIED | 0 |  |
| AUTHORIZED | 1 |  |
| DEBIT | 2 |  |
| REFUND | 3 |  |
| DECLINED | 4 |  |
| CHARGEBACK | 5 |  |
| CREDIT | 6 |  |
| PRODUCT_NOT_DEFINED | 7 | For use only when available information is insufficient to determine type |


 

 


<a name="thebaasco.tenant.persfinmngt.v1.Transactions"></a>

### Transactions


| Method Name | Request Type | Response Type | Description |
| ----------- | ------------ | ------------- | ------------|
| GetTransactions | [GetTransactionsRequest](#thebaasco.tenant.persfinmngt.v1.GetTransactionsRequest) | [GetTransactionsResponse](#thebaasco.tenant.persfinmngt.v1.GetTransactionsResponse) |  |

 



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

