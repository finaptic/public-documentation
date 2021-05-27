# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [corecard_v1_cards.proto](#corecard_v1_cards.proto)
    - [ActivateCardRequest](#thebaasco.tenant.corecard.v1.ActivateCardRequest)
    - [Card](#thebaasco.tenant.corecard.v1.Card)
    - [CardCreatedEvent](#thebaasco.tenant.corecard.v1.CardCreatedEvent)
    - [CardDetails](#thebaasco.tenant.corecard.v1.CardDetails)
    - [ChangePinRequest](#thebaasco.tenant.corecard.v1.ChangePinRequest)
    - [CreateCardRequest](#thebaasco.tenant.corecard.v1.CreateCardRequest)
    - [CreateCardSDKSignOnTokenRequest](#thebaasco.tenant.corecard.v1.CreateCardSDKSignOnTokenRequest)
    - [CreateCardSDKSignOnTokenResponse](#thebaasco.tenant.corecard.v1.CreateCardSDKSignOnTokenResponse)
    - [DeleteCardRequest](#thebaasco.tenant.corecard.v1.DeleteCardRequest)
    - [GetCardDetailsRequest](#thebaasco.tenant.corecard.v1.GetCardDetailsRequest)
    - [GetCardRequest](#thebaasco.tenant.corecard.v1.GetCardRequest)
    - [ListCardsRequest](#thebaasco.tenant.corecard.v1.ListCardsRequest)
    - [ListCardsResponse](#thebaasco.tenant.corecard.v1.ListCardsResponse)
    - [UpdateCardRequest](#thebaasco.tenant.corecard.v1.UpdateCardRequest)
  
    - [CardType](#thebaasco.tenant.corecard.v1.CardType)
  
    - [Cards](#thebaasco.tenant.corecard.v1.Cards)
  
- [corecard_v1_transactiondetails.proto](#corecard_v1_transactiondetails.proto)
    - [TransactionDetails](#thebaasco.tenant.corecard.v1.TransactionDetails)
    - [TransactionDetailsCreatedEvent](#thebaasco.tenant.corecard.v1.TransactionDetailsCreatedEvent)
  
- [temporary_incoming_data.proto](#temporary_incoming_data.proto)
    - [TransactionMetadataReceivedEvent](#thebaasco.tenant.corecard.v1.TransactionMetadataReceivedEvent)
    - [TransactionMetadataReceivedEvent.DataElementsEntry](#thebaasco.tenant.corecard.v1.TransactionMetadataReceivedEvent.DataElementsEntry)
  
- [Scalar Value Types](#scalar-value-types)



<a name="corecard_v1_cards.proto"></a>
<p align="right"><a href="#top">Top</a></p>

## corecard_v1_cards.proto



<a name="thebaasco.tenant.corecard.v1.ActivateCardRequest"></a>

### ActivateCardRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| card_id | [string](#string) |  |  |






<a name="thebaasco.tenant.corecard.v1.Card"></a>

### Card



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| id | [string](#string) |  | Output only |
| card_type | [CardType](#thebaasco.tenant.corecard.v1.CardType) |  |  |
| account_id | [string](#string) |  |  |
| description | [string](#string) |  |  |






<a name="thebaasco.tenant.corecard.v1.CardCreatedEvent"></a>

### CardCreatedEvent



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| id | [string](#string) |  |  |
| card | [Card](#thebaasco.tenant.corecard.v1.Card) |  |  |
| tenant_id | [int64](#int64) |  | mask that ensure we can&#39;t use more than 56bits |
| card_creation_time | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  |  |






<a name="thebaasco.tenant.corecard.v1.CardDetails"></a>

### CardDetails



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| card_id | [string](#string) |  | Output only |
| card_number | [string](#string) |  | Output only |
| expiration | [google.type.Date](#google.type.Date) |  | Output only |
| cvv | [string](#string) |  | Output only |






<a name="thebaasco.tenant.corecard.v1.ChangePinRequest"></a>

### ChangePinRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| card_id | [string](#string) |  |  |
| pin | [string](#string) |  |  |






<a name="thebaasco.tenant.corecard.v1.CreateCardRequest"></a>

### CreateCardRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| card | [Card](#thebaasco.tenant.corecard.v1.Card) |  |  |






<a name="thebaasco.tenant.corecard.v1.CreateCardSDKSignOnTokenRequest"></a>

### CreateCardSDKSignOnTokenRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| card_id | [string](#string) |  |  |






<a name="thebaasco.tenant.corecard.v1.CreateCardSDKSignOnTokenResponse"></a>

### CreateCardSDKSignOnTokenResponse



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| token | [string](#string) |  |  |






<a name="thebaasco.tenant.corecard.v1.DeleteCardRequest"></a>

### DeleteCardRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| card_id | [string](#string) |  |  |






<a name="thebaasco.tenant.corecard.v1.GetCardDetailsRequest"></a>

### GetCardDetailsRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| card_id | [string](#string) |  |  |






<a name="thebaasco.tenant.corecard.v1.GetCardRequest"></a>

### GetCardRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| card_id | [string](#string) |  |  |






<a name="thebaasco.tenant.corecard.v1.ListCardsRequest"></a>

### ListCardsRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| user_id | [string](#string) |  |  |
| page_size | [int32](#int32) |  |  |
| page_token | [string](#string) |  |  |






<a name="thebaasco.tenant.corecard.v1.ListCardsResponse"></a>

### ListCardsResponse



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| cards | [Card](#thebaasco.tenant.corecard.v1.Card) | repeated |  |
| next_page_token | [string](#string) |  |  |






<a name="thebaasco.tenant.corecard.v1.UpdateCardRequest"></a>

### UpdateCardRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| card_id | [string](#string) |  |  |
| card | [Card](#thebaasco.tenant.corecard.v1.Card) |  |  |





 


<a name="thebaasco.tenant.corecard.v1.CardType"></a>

### CardType


| Name | Number | Description |
| ---- | ------ | ----------- |
| CARD_TYPE_UNSPECIFIED | 0 |  |
| CARD_TYPE_PREPAID | 1 |  |


 

 


<a name="thebaasco.tenant.corecard.v1.Cards"></a>

### Cards


| Method Name | Request Type | Response Type | Description |
| ----------- | ------------ | ------------- | ------------|
| GetCard | [GetCardRequest](#thebaasco.tenant.corecard.v1.GetCardRequest) | [Card](#thebaasco.tenant.corecard.v1.Card) |  |
| ListCards | [ListCardsRequest](#thebaasco.tenant.corecard.v1.ListCardsRequest) | [ListCardsResponse](#thebaasco.tenant.corecard.v1.ListCardsResponse) |  |
| GetCardDetails | [GetCardDetailsRequest](#thebaasco.tenant.corecard.v1.GetCardDetailsRequest) | [CardDetails](#thebaasco.tenant.corecard.v1.CardDetails) |  |
| CreateCardSDKSignOnToken | [CreateCardSDKSignOnTokenRequest](#thebaasco.tenant.corecard.v1.CreateCardSDKSignOnTokenRequest) | [CreateCardSDKSignOnTokenResponse](#thebaasco.tenant.corecard.v1.CreateCardSDKSignOnTokenResponse) | To obtain a sign-on token to be used with the provided card SDK To do sensitive operations like retrieving Card PAN and CVV or resetting card PIN |

 



<a name="corecard_v1_transactiondetails.proto"></a>
<p align="right"><a href="#top">Top</a></p>

## corecard_v1_transactiondetails.proto



<a name="thebaasco.tenant.corecard.v1.TransactionDetails"></a>

### TransactionDetails



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| transaction_id | [string](#string) |  | Unique identifier that represents this single financial transaction (operation). |
| lifecycle_transaction_id | [string](#string) |  | Unique identifier that represents the overarching transaction, across all phases of it&#39;s lifecycle (e.g. authorization, settlement, reversal, ...) (there might be multiple unique &#34;transaction_id&#34; for a single &#34;lifecycle_transaction_id&#34;) |
| merchant_type | [int32](#int32) |  | DE-018-The classification of the merchant’s type of business product or service // |
| card_acceptor_terminal_identification | [string](#string) |  | DE-041-A unique code identifying a terminal at the card acceptor location This is 8 digit alpha numeric value // |
| card_acceptor_identification_code | [string](#string) |  | DE-042-A code identifying the card acceptor that defines the point of the transaction in both local and interchange environments. This is a 15 digit alpha numeric value.// |
| card_acceptor_name_location | [string](#string) |  | DE-043 This field contains the name and location of the card acceptor (merchant), including the city name, state and country code. |
| transaction_time | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  | UTC timestamp for the transaction |
| mti | [string](#string) |  | String representation of the originating message type (e.g. &#34;0100&#34;, &#34;0400&#34;, ... ) |






<a name="thebaasco.tenant.corecard.v1.TransactionDetailsCreatedEvent"></a>

### TransactionDetailsCreatedEvent



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| event_id | [string](#string) |  | Identifies this event uniquely, to ensure it&#39;s processed only 1 time |
| details | [TransactionDetails](#thebaasco.tenant.corecard.v1.TransactionDetails) |  |  |
| tenant_id | [int32](#int32) |  |  |





 

 

 

 



<a name="temporary_incoming_data.proto"></a>
<p align="right"><a href="#top">Top</a></p>

## temporary_incoming_data.proto



<a name="thebaasco.tenant.corecard.v1.TransactionMetadataReceivedEvent"></a>

### TransactionMetadataReceivedEvent



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| authorization_transaction_id | [string](#string) |  | UUID, generated internally, that represents a unique request/response pair for I2C. |
| transaction_id | [string](#string) |  | This is the UUID used across all domains to uniquely represent the transaction. This is taken from Thought Machine Vault&#39;s PostingInstructionBatch ID |
| raw | [bytes](#bytes) |  | Raw, unprocessed binary data for the I2C message |
| mti | [string](#string) |  | String representation of the originating message type (e.g. &#34;0100&#34;, &#34;0400&#34;, ... ) |
| primary_bitmap | [string](#string) |  | Hexadecimal encoded value, as a string for primary bit map. |
| secondary_bitmap | [string](#string) |  | Hexadecimal encoded value, as a string for primary bit map. |
| data_elements | [TransactionMetadataReceivedEvent.DataElementsEntry](#thebaasco.tenant.corecard.v1.TransactionMetadataReceivedEvent.DataElementsEntry) | repeated | Map of all data elements received in original message, not filtered. The key would be in the following format: &#34;DE-025&#34;, &#34;DE-003&#34;, ... The value would be encoded as a string, with following guidelines: It will be the parsing&#39;s code ( core-card ) that will be responsible to know the format of each data element and consume it accordingly, as needed. n : series of digits, encoded as a string --&gt; &#34;1234&#34; an : Alpha numeric or digis --&gt; &#34;A123B&#34; ans : Alpha numeric and special chars --&gt; &#34;A123$&#34; x : single char indicator of Credit or Debit --&gt; x &#43; n 8 = &#34;C12345678&#34; LLVAR//LLLVAR: variable length chain of chars --&gt; &#34;POTATO is the BEST!&#34; b : base64 encoded string for binary content. |
| message_time | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  | Timestamp for the I2C message |
| tenant_id | [int32](#int32) |  |  |






<a name="thebaasco.tenant.corecard.v1.TransactionMetadataReceivedEvent.DataElementsEntry"></a>

### TransactionMetadataReceivedEvent.DataElementsEntry



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| key | [string](#string) |  |  |
| value | [string](#string) |  |  |





 

 

 

 



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

