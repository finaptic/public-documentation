# Personal Finance Management API (Draft)
<a name="top"></a>



<a name="persfinmngt_v1_transactions.proto"></a>

## persfinmngt_v1_transactions.proto



### Services

<a name="thebaasco.tenant.persfinmngt.v1.Transactions"></a>

#### Transactions
Transactions service exposes operations related to the transactions of an end-user.

##### GetTransactions

> **rpc** GetTransactions([GetTransactionsRequest](#thebaasco.tenant.persfinmngt.v1.GetTransactionsRequest))
[GetTransactionsResponse](#thebaasco.tenant.persfinmngt.v1.GetTransactionsResponse)

GetTransactions lists transactions for a given Account with pagination.
Only users associated with the Account will be able to invoke this endpoint.


 <!-- end services -->



### Enums

<a name="thebaasco.tenant.persfinmngt.v1.Order"></a>

#### Order
Order enumeration represents the chronological ordering of transactions.

| Name | Number | Description |
| ---- | ------ | ----------- |
| ORDER_UNSPECIFIED | 0 | ORDER_UNSPECIFIED should never be used directly. It is returned when a value has not been specified, and is present by convention, for error-detection purposes. Trying to make a request with this type will result in an error. |
| DESCENDING | 1 | DESCENDING represents newest first. |
| ASCENDING | 2 | ASCENDING represents oldest first. |



<a name="thebaasco.tenant.persfinmngt.v1.Transaction.TransactionState"></a>

#### Transaction.TransactionState
TransactionState enumeration represents the state of a Transaction.

| Name | Number | Description |
| ---- | ------ | ----------- |
| TRANSACTION_STATE_UNSPECIFIED | 0 | TRANSACTION_STATE_UNSPECIFIED is reserved for error detection purposes. It should not be used directly. |
| POSTED | 1 | POSTED represents a transaction that has been completed. |
| PENDING | 2 | PENDING represents a transaction in an intermediate state, dependent on the transaction type. |
| UNKNOWN | 3 | UNKNOWN represents a transaction type where available information is insufficient to determine TransactionState. This can occur if information from the originating domain of the transaction (core-card or core-transfer) has not been received. |
| CANCELLED | 4 | CANCELLED represents a transaction that has been reverted while it was still pending (before settlement) |



<a name="thebaasco.tenant.persfinmngt.v1.Transaction.TransactionType"></a>

#### Transaction.TransactionType
TransactionType enumeration represents the type of a Transaction.

| Name | Number | Description |
| ---- | ------ | ----------- |
| TRANSACTION_TYPE_UNSPECIFIED | 0 | TRANSACTION_TYPE_UNSPECIFIED is reserved for error detection purposes. It should not be used directly. |
| AUTHORIZED | 1 | AUTHORIZED represents a transaction type that is a purchase for which the merchant has received approval from the bank. An AUTHORIZED transaction will always have state PENDING. |
| DEBIT | 2 | DEBIT represents a transaction type when there is an addition to the amount in the customer's account. |
| REFUND | 3 | REFUND represents a transaction type when businesses and merchants issue a reversal of the transaction that had previously occurred (e.g. a previously credited transaction will be debited back into the customers account). |
| DECLINED | 4 | DECLINED represents a transaction type when the transaction fails for any reason (e.g. fraud, operations). |
| CHARGEBACK | 5 | CHARGEBACK represents a transaction type that is returned to a payment card after a customer successfully disputes an item on their account statement or transactions report. A chargeback may occur on debit cards (and the underlying bank account) or on credit cards. |
| CREDIT | 6 | CREDIT represents a transaction type when there is a deduction to the amount in the customer's account. |
| PRODUCT_NOT_DEFINED | 7 | PRODUCT_NOT_DEFINED represents a transaction state where available information is insufficient to determine TransactionType. This can occur if information from the originating domain of the transaction (core-card or core-transfer) has not been received. |


 <!-- end enums -->


### API-specific types

<a name="thebaasco.tenant.persfinmngt.v1.GetTransactionsRequest"></a>

#### GetTransactionsRequest
GetTransactionsRequest is a message passed as parameter to the GetTransactions operation.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| account_id | [string](#string) |  | The Account ID for which Transactions are returned. Must be a valid UUID. User must have proper access to this Account to call this endpoint. |
| page_size | [int32](#int32) |  | The maximum number of Transactions to return. This value must be greater or equal to one. |
| page_token | [string](#string) |  | When retrieving page other than the initial page, the value for the page_token must be specified, and taken from the previous page's next_page_token value (see GetTransactionsResponse). When no value is specified, then the initial page will be returned. |
| sort_order | [Order](#thebaasco.tenant.persfinmngt.v1.Order) |  | The order the returned Transactions are sorted in. Transactions are sorted by the Transaction's transaction_time field. |






<a name="thebaasco.tenant.persfinmngt.v1.GetTransactionsResponse"></a>

#### GetTransactionsResponse
GetTransactionsResponse is the response of a successful GetTransactions request.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| previous_page_token | [string](#string) |  | The token to be used to retrieve the previous page of data. Must be passed in the page_token field of the next request (see GetTransactionsRequest). When no value is returned in this field, then there is no previous data to be listed. |
| transactions | [Transaction](#thebaasco.tenant.persfinmngt.v1.Transaction) | repeated | The requested list of Transactions. |
| next_page_token | [string](#string) |  | The token to be used to retrieve the next page of data. Must be passed in the page_token field of the next request (see GetTransactionsRequest). When no value is returned in this field, then no further data exists to be listed. |
| sort_order | [Order](#thebaasco.tenant.persfinmngt.v1.Order) |  | The order the Transactions are listed in. |






<a name="thebaasco.tenant.persfinmngt.v1.Transaction"></a>

#### Transaction
Transaction is the representation of a transaction. It contains details about the transaction and information regarding the account the transaction has occurred on.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| transaction_id | [string](#string) |  | Unique identifier that represents this single financial transaction (operation). This ID is stable across all domains for a given transaction. |
| lifecycle_transaction_id | [string](#string) |  | Unique identifier that represents the overarching transaction, across all phases of it's lifecycle (e.g. authorization, settlement, reversal, ...). Note: there might be multiple unique "transaction_id" for a single "lifecycle_transaction_id". |
| amount | [thebaasco.types.Amount](#thebaasco.types.Amount) |  | The Amount of the Transaction. |
| transactor_name | [string](#string) |  | The name and location of the vendor where the payment was processed. |
| balance | [thebaasco.types.Balance](#thebaasco.types.Balance) |  | The resulting Balance of the Account after the Transaction has occurred. |
| transaction_state | [Transaction.TransactionState](#thebaasco.tenant.persfinmngt.v1.Transaction.TransactionState) |  | The current TransactionState of the Transaction. This is subject to change based on processing of the transaction. |
| transaction_type | [Transaction.TransactionType](#thebaasco.tenant.persfinmngt.v1.Transaction.TransactionType) |  | The TransactionType of the Transaction. |
| account_id | [string](#string) |  | ID of the Account on which the Transaction was authorised. This can also be utilised to fetch the Account. |
| category | [string](#string) |  | A localized descriptor of the transaction's category (i.e. Groceries, Transport, etc.). |
| transaction_time | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  | The UTC timestamp when the Transaction occurred. |
| account_domain | [string](#string) |  | The name of the domain that can be queried for additional details regarding the Account of this transaction (e.g. core-banking) |
| initiating_domain | [string](#string) |  | The name of the domain that can be queried for additional details regarding this Transaction (e.g. core-card, core-transfer or core-banking) |






<a name="thebaasco.tenant.persfinmngt.v1.TransactionCreatedEvent"></a>

#### TransactionCreatedEvent
TransactionCreatedEvent event is raised when a Transaction occurs.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| event_id | [string](#string) |  | The unique event ID for this transaction. Can be used by consumers to determine if an event has already been processed or not. This ID will always be a valid UUID. |
| customer_id | [string](#string) |  | The ID of the customer that the event is directed to. If a single transaction occurs on an account with multiple customers (account owner or authorized users), an event will be raised for each customer, each with a different event_id. |
| transaction | [Transaction](#thebaasco.tenant.persfinmngt.v1.Transaction) |  | The Transaction that triggered the event. |
| tenant_id | [int32](#int32) |  | The tenant ID, to identify the tenant on which the operation occurred. This can be used when processing events at the global scope to differentiate between tenants. |
| event_time | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  | The UTC timestamp when this event was issued. |





 <!-- end messages -->



## Shared types

<a name="thebaasco.types.Amount"></a>
### Amount



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| amount | [string](#string) |  | Only accept decimal representation of a value |
| currency | [string](#string) |  |  |


<a name="thebaasco.types.Balance"></a>

### Balance



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| actual | [Amount](#thebaasco.types.Amount) |  |  |
| available | [Amount](#thebaasco.types.Amount) |  |  |

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

