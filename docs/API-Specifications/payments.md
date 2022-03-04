# Payments API (Experimental)
<a name="top"></a>



<a name="b2cpayments_v1_transactions.proto"></a>

## b2cpayments_v1_transactions.proto
Experimental do not try to use this API in any PROD use case


### Services

<a name="thebaasco.payments.v1.B2CPaymentsService"></a>

#### B2CPaymentsService
B2CPaymentsService is a synchronous gRPC service for the execution of different payment types between the Business
represented by the distribution partner and its end customers whose banking products are serviced by Finaptic

##### SubmitPayment

> **rpc** SubmitPayment([SubmitPaymentRequest](#thebaasco.payments.v1.SubmitPaymentRequest))
    [SubmitPaymentResponse](#thebaasco.payments.v1.SubmitPaymentResponse)

Generic service for a distribution partner to initiate a payment towards a customer's card/account


 <!-- end services -->




### API-specific types

<a name="thebaasco.payments.v1.B2CTransactionDetailsCreatedEvent"></a>

#### B2CTransactionDetailsCreatedEvent
B2CTransactionDetailsCreatedEvent event is raised by the Payments domain when transactions are created and Payment-specific details are available.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| event_id | [string](#string) |  | The unique event ID for this transaction. Can be used by consumers to determine if an event has already been processed or not. This ID will always be a valid UUID. |
| event_time | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  | The UTC timestamp when this event was issued. |
| tenant_id | [int32](#int32) |  | The tenant ID, to identify the tenant on which the operation occurred. This can be used when processing events at the global scope to differentiate between tenants. |
| details | [TransactionDetails](#thebaasco.payments.v1.TransactionDetails) |  | The transaction details for this event. |






<a name="thebaasco.payments.v1.PaymentInstructionDetails"></a>

#### PaymentInstructionDetails
PaymentDetails is a model that describes the detail of a payment


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| card_id | [string](#string) |  | The identifier of the card whose balance will be affected by this payment |
| amount | [thebaasco.types.Amount](#thebaasco.types.Amount) |  | Amount to be paid in this transaction If the amount is negative the payment instruction is outbound (takes money out of the customer's account) If the amount is positive the payment instruction is inbound (puts money into the customer's account) |
| payment_ref_number | [string](#string) |  | Identifier of this payment in the source system (e.g. the distribution partner's payments engine or loyalty platform) |
| payment_time | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  | The UTC timestamp when this payment/operation took place on the source system (e.g. when the user made a purchase). |
| description | [string](#string) |  | Short description/memo describing this payment for visualization to customers |
| tags | [Tag](#thebaasco.payments.v1.Tag) | repeated | Collection of metadata with additional details describing this payment |






<a name="thebaasco.payments.v1.SubmitPaymentRequest"></a>

#### SubmitPaymentRequest
PurchaseRequest is used when distribution partner wants to simulate a card transaction.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| request_id | [string](#string) |  | Used as an idempotency key to uniquely identify this request and avoid duplicating its side effects in case of a retry |
| payment | [PaymentInstructionDetails](#thebaasco.payments.v1.PaymentInstructionDetails) |  | The identifier of the card that will be used for the purchase simulation |






<a name="thebaasco.payments.v1.SubmitPaymentResponse"></a>

#### SubmitPaymentResponse
PurchaseResponse is returned when the purchase operation finishes


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| lifecycle_transaction_id | [string](#string) |  | Unique identifier that represents the overarching transaction, across all phases of it's lifecycle (e.g. authorization, settlement, reversal, ...). |
| transaction_id | [string](#string) |  | Unique identifier that represents this single financial transaction (operation). This ID is stable across all domains for a given transaction. |
| response_code | [string](#string) |  | Outcome of the transaction Success/Failure TODO will need to decide the best way to communicate this, see if it can be made an enum |
| response_code_reason | [string](#string) |  | Describes the reason for the returned response code. Applicable in cases of unsuccessful response codes. |






<a name="thebaasco.payments.v1.Tag"></a>

#### Tag
Used as a mechanism to add meta-data for categorization of a payment according to the source system


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| key | [string](#string) |  | To identify the value category been described/set by the corresponding value |
| value | [string](#string) |  | The actual value of the category been described |






<a name="thebaasco.payments.v1.TransactionDetails"></a>

#### TransactionDetails
TransactionDetails message contains details about a given transaction, relevant and specific to the B2C Payments domain.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| transaction_id | [string](#string) |  | Unique identifier that represents this single financial transaction (operation). This ID is stable across all domains for a given transaction. |
| lifecycle_transaction_id | [string](#string) |  | Unique identifier that represents the overarching transaction, across all phases of it's lifecycle (e.g. authorization, settlement, reversal, ...). Note: there might be multiple unique "transaction_id" for a single "lifecycle_transaction_id". |
| card_id | [string](#string) |  | The identifier of the card involved in this B2C payment transaction. |
| payment_instruction | [PaymentInstructionDetails](#thebaasco.payments.v1.PaymentInstructionDetails) |  | Details of the payment instruction that originated this transaction |
| response_code | [string](#string) |  | Outcome of the transaction Success/Failure TODO will need to decide the best way to communicate this, see if it can be made an enum |
| response_code_reason | [string](#string) |  | Describes the reason for the returned response code. Applicable in cases of unsuccessful response codes. |





 <!-- end messages -->



## Shared types
TODO: write shared types here if they exist

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

