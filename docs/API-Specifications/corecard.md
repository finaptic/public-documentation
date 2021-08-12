# Core Card API (Draft)
<a name="top"></a>



<a name="corecard_v1_cards.proto"></a>

## corecard_v1_cards.proto
This section of Core Card API includes the main data types and services that make the API surface if this domain


### Services

<a name="thebaasco.tenant.corecard.v1.Cards"></a>

#### Cards
Cards service exposes operations related to listing and retrieving details for Cards issued.

It is a real gRPC service accessible in a synchronous manner.
As such it is also used for generation of Card SDK sign on tokens needed to reveal card authentication details
like CVV and expiration date

##### GetCard

> **rpc** GetCard([GetCardRequest](#thebaasco.tenant.corecard.v1.GetCardRequest))
    [Card](#thebaasco.tenant.corecard.v1.Card)

GetCard retrieves generic details about a single Card. The card data returned does not contain any sensitive information such as card number or expiration date. 
Only users associated with the Card will be able to invoke this endpoint and retrieve details for a given Card.

##### ListCards

> **rpc** ListCards([ListCardsRequest](#thebaasco.tenant.corecard.v1.ListCardsRequest))
    [ListCardsResponse](#thebaasco.tenant.corecard.v1.ListCardsResponse)

ListCards lists all Cards associated to the current user. The information returned does not contain any sensitive information such as card number or expiration date.
This endpoint uses pagination to allow listing the Cards in smaller, easy to manage, chunks.

##### CreateCardSDKSignOnToken

> **rpc** CreateCardSDKSignOnToken([CreateCardSDKSignOnTokenRequest](#thebaasco.tenant.corecard.v1.CreateCardSDKSignOnTokenRequest))
    [CreateCardSDKSignOnTokenResponse](#thebaasco.tenant.corecard.v1.CreateCardSDKSignOnTokenResponse)

CreateCardSDKSignOnToken generates a single-use, limited duration token, which is used by the Front End SDK to retrieve sensitive information about Cards.
The SDK is necessary when performing sensitive operations like retrieving the Card's number (PAN), expiration date, CVV, or resetting the Card's PIN.



<a name="thebaasco.tenant.corecard.v1.CardsCommands"></a>

#### CardsCommands
CardsCommandsService is a key component of Core Card API thar exposes asynchronous operations that alter the state of a Card.

It is not directly accessible by consumers as a gRPC service, it instead uses Google PubSub as delivery mechanism for requests and responses.

We include it in the documentation as a reference for a better understanding of the responsibilities and API surface of this domain

##### CreateCard

> **rpc** CreateCard([CreateCardRequest](#thebaasco.tenant.corecard.v1.CreateCardRequest))
    [Card](#thebaasco.tenant.corecard.v1.Card)

CreateCard is an asynchronous operation used to create a new Card. 
Performing this operation will generate an asynchronous response of type Card on the response topic.
When creating the card, the information contained in the Client-Profile domain for current user will be used to
populate Card information such as name on card, and billing address.
As result of a successful card creation, a business event of type CardCreatedEvent will be issued

##### UpdateCard

> **rpc** UpdateCard([UpdateCardRequest](#thebaasco.tenant.corecard.v1.UpdateCardRequest))
    [Card](#thebaasco.tenant.corecard.v1.Card)

UpdateCard is an asynchronous operation used to update details of a Card. 
Performing this operation will generate an asynchronous response of type Card on the response topic.
As result of a successful card update, a business event of type CardUpdatedEvent will be issued


 <!-- end services -->



### Enums

<a name="thebaasco.tenant.corecard.v1.CardType"></a>

#### CardType
CardType provides indication of the Card's type.

| Name | Number | Description |
| ---- | ------ | ----------- |
| CARD_TYPE_UNSPECIFIED | 0 | CARD_TYPE_UNSPECIFIED should never be used directly. It is returned when a value has not been specified, and is present by convention, for error-detection purposes. Trying to create a Card with this type will result in an error. |
| CARD_TYPE_PREPAID | 1 | CARD_TYPE_PREPAID represents a Prepaid Card. A Prepaid Card uses the linked account's funds when processing operations. |


 <!-- end enums -->


### API-specific types

<a name="thebaasco.tenant.corecard.v1.Card"></a>

#### Card
Card represents generic details about a given Card. The message does not contain any sensitive information, such as Card number (PAN), expiration date, or CVV.
Access to such sensitive information must be done throuh other mechanism, such as via an external SDK (using CreateCardSDKSignOnToken operation)


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| id | [string](#string) |  | Output only. The unique ID of this Card. It is not nescessary to specify the ID when creating a Card, but the resulting Card will have this value populated with the generated ID. The ID generated follows the UUID v4 format, always. |
| card_type | [CardType](#thebaasco.tenant.corecard.v1.CardType) |  | The type of the Card. See documentation on CardType enum for details on enum values. |
| account_id | [string](#string) |  | The ID of the Account linked to this Card. The value of this field must follow the UUID format, and must map to a valid Account, for which the user has proper access. The list of valid Accounts can be retrieved from Core-Transaction domain. All Cards must have a linked Account. |
| description | [string](#string) |  | The description for this Card. Specifying a description for a Card is optional. |






<a name="thebaasco.tenant.corecard.v1.CardCreatedEvent"></a>

#### CardCreatedEvent
CardCreatedEvent is an event raised when a Card is successfuly created.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| event_id | [string](#string) |  | The unique ID of this event. Can be used by consumers to determine if an event has already been processed or not. This ID will always be a valid UUID. |
| card | [Card](#thebaasco.tenant.corecard.v1.Card) |  | The Card that was created. |
| tenant_id | [int32](#int32) |  | The tenant ID, to identify the tenant on which the operation occurred. This can be used when processing events at the global scope to differentiate between tenants. |
| card_creation_time | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  | The UTC timestamp for the creation of the Card. |






<a name="thebaasco.tenant.corecard.v1.CreateCardRequest"></a>

#### CreateCardRequest
CreateCardRequest is message used to perform the asynchronous operation of creating a new Card. Performing this operation will generate an asynchronous response of type Card, on the response topic.
When creating the card, the information contained in the Client-Profile domain for current user will be used to
populate Card information such as name on card, and billing address.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| card | [Card](#thebaasco.tenant.corecard.v1.Card) |  | A valid Card message. Refer to Card message for details. |






<a name="thebaasco.tenant.corecard.v1.CreateCardSDKSignOnTokenRequest"></a>

#### CreateCardSDKSignOnTokenRequest
CreateCardSDKSignOnTokenRequest is a message passed as parameter to the CreateCardSDKSignOnToken operation.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| card_id | [string](#string) |  | The ID of the Card for which a SDK Sign-On Token must be created. The value must be a valid UUID, and current user must have proper access to the Card. |






<a name="thebaasco.tenant.corecard.v1.CreateCardSDKSignOnTokenResponse"></a>

#### CreateCardSDKSignOnTokenResponse
CreateCardSDKSignOnTokenResponse is a message returned in response to the CreateCardSDKSignOnToken operation, which contains the token needed to use the SDK.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| token | [string](#string) |  | The token to be used by the Front End SDK t . This token can only be used to sign-on a single device, for a single session, and has limited duration during which it can be used. |






<a name="thebaasco.tenant.corecard.v1.GetCardRequest"></a>

#### GetCardRequest
GetCardRequest is a message passed as parameter to the GetCard operation.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| card_id | [string](#string) |  | The ID of the Card to retrieve. The value must be a valid UUID, and current user must have proper access to the Card to retrieve it. |






<a name="thebaasco.tenant.corecard.v1.ListCardsRequest"></a>

#### ListCardsRequest
ListCardsRequest is a message passed as parameter to the ListCards operation. See ListCards for details on operation.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| page_size | [int32](#int32) |  | The maximum number of Cards to retrieve as part of this request. This value needs to be superior, or equal to 1. |
| page_token | [string](#string) |  | When retrieving page other than the initial page, the value for the page_token must be specified, and taken from the previous page's next_page_token value (see ListCardsResponse). When no value is specified, then the initial page will be returned. |






<a name="thebaasco.tenant.corecard.v1.ListCardsResponse"></a>

#### ListCardsResponse
ListCardsResponse is a message returned in response to the ListCards operation, which contains the requested data.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| cards | [Card](#thebaasco.tenant.corecard.v1.Card) | repeated | The list of Card messages requested. |
| next_page_token | [string](#string) |  | The token to be used to retrieve the next page of data. Must be passed in the page_token field of the next request (see ListCardsRequest). When no value is returned in this field, then no further data exists to be listed. |






<a name="thebaasco.tenant.corecard.v1.UpdateCardRequest"></a>

#### UpdateCardRequest
UpdateCardRequest is message used to perform the asynchronous operation of updating a Card. Performing this operation will generate an asynchronous response of type Card, on the response topic.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| card_id | [string](#string) |  | The ID of the Card to update. This must be specified as part of the UpdateCardRequest, instead of as part of the Card message, since the Card ID is specified as "Output Only" (not considered during input). User must have proper access to the Card to call this endpoint. |
| card | [Card](#thebaasco.tenant.corecard.v1.Card) |  | A valid Card message. Refer to Card message for details. |





 <!-- end messages -->



<a name="corecard_v1_transactiondetails.proto"></a>

## corecard_v1_transactiondetails.proto
This section of Core Card API is used to contain models related to card payments transactions.
As result of payment operations, the core-card domain will issue transaction detail events
so that other interested domains get notified.
An example of such interested/subscriber domains is Personal Finance Management (PFM)





### API-specific types

<a name="thebaasco.tenant.corecard.v1.TransactionDetails"></a>

#### TransactionDetails
TransactionDetails message contains details about a given transaction, relevant and specific to the Core-Card domain.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| transaction_id | [string](#string) |  | Unique identifier that represents this single financial transaction (operation). This ID is stable across all domains for a given transaction. |
| lifecycle_transaction_id | [string](#string) |  | Unique identifier that represents the overarching transaction, across all phases of it's lifecycle (e.g. authorization, settlement, reversal, ...). Note: there might be multiple unique "transaction_id" for a single "lifecycle_transaction_id". |
| merchant_type | [int32](#int32) |  | The classification of the merchant’s type of business product or service. This is the MCC (Merchant Category Code), and is extracted from Data Element DE-018 of the ISO-8583 messages. Refer to this github project for details on MCC values: https://github.com/greggles/mcc-codes |
| card_acceptor_identification_code | [string](#string) |  | This Data element denotes the Merchant ID as part of the tansaction In a Point of sale transaction i.e Maestro or debit MasterCard transaction, this data element type is an alphanumeric value |
| card_acceptor_name_location | [string](#string) |  | The name and location of the card acceptor (merchant), including the city name, state and country code. This is extracted from Data Element DE-043 from ISO-8583 messages. |
| transaction_time | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  | The UTC timestamp for the transaction This is extracted from Data Element DE-007 of the ISO-8583 messages. |
| mti | [string](#string) |  | The 4 digits representation of the originating message type (e.g. "0100", "0400", ... ). |






<a name="thebaasco.tenant.corecard.v1.TransactionDetailsCreatedEvent"></a>

#### TransactionDetailsCreatedEvent
TransactionDetailsCreatedEvent event is raised by the Core-Card domain when transactions are created and Core-Card specific details are available. 
Note: There are no timestamps in this event message, since `details` field will contain the transaction timestamp.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| event_id | [string](#string) |  | The unique event ID for this transaction. Can be used by consumers to determine if an event has already been processed or not. This ID will always be a valid UUID. |
| details | [TransactionDetails](#thebaasco.tenant.corecard.v1.TransactionDetails) |  | The transaction details for this event. |
| tenant_id | [int32](#int32) |  | The tenant ID, to identify the tenant on which the operation occurred. This can be used when processing events at the global scope to differentiate between tenants. |





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
