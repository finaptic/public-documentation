# Core Card API
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

##### LockCard
> **rpc** LockCard([LockCardRequest](#thebaasco.tenant.corecard.v1.LockCardRequest))
[LockCardResponse](#thebaasco.tenant.corecard.v1.LockCardResponse)

LockCard is an asynchronous function used to temporarily lock a card.
Performing this operation will generate an asynchronous response of type LockCardResponse on the response topic or an error if applicable.
This feature allows customers to prevent transactions from getting authorized while they confirm if a card is effectively lost.
After finding the card if it is the case, they can re-enable the card by using the unlock function above.


##### UnlockCard
> **rpc** UnlockCard([UnlockCardRequest](#thebaasco.tenant.corecard.v1.UnlockCardRequest))
[UnlockCardResponse](#thebaasco.tenant.corecard.v1.UnlockCardResponse)

UnlockCard is an asynchronous function used to re-enable a locked card.
Performing this operation will generate an asynchronous response of type UnlockCardResponse on the response topic or an error if applicable.
This feature allows customers to re-enable transactions on the card if they find it after suspecting it lost.

##### CreateAuthorizedUserCard
> **rpc** CreateAuthorizedUserCard([CreateAuthorizedUserCardRequest](#thebaasco.tenant.corecard.v1.CreateAuthorizedUserCardRequest))
[CreateAuthorizedUserCardResponse](#thebaasco.tenant.corecard.v1.CreateAuthorizedUserCardResponse)
> 
CreateAuthorizedCard is an asynchronous function for the creation of a new Card for an authorized user of an account.
Performing this operation will generate an asynchronous response of type CreateAuthorizedUserCardResponse on the response topic.
When creating the card, the Client-Profile for the authorized user will be fetched to populate information such as name on card and billing address.

 <!-- end services -->
### Enums

<a name="thebaasco.tenant.corecard.v1.CardState"></a>

#### CardState
CardState is used to describe the state of a card.

| Name | Number | Description |
| ---- | ------ | ----------- |
| CARD_STATE_UNSPECIFIED | 0 | CARD_TYPE_UNSPECIFIED should never be used directly. It is returned when a value has not been specified, and is present by convention, for error-detection purposes. |
| CARD_STATE_ACTIVE | 1 | CARD_STATE_ACTIVE indicates that the card is ready to be used. |
| CARD_STATE_LOCKED | 2 | CARD_STATE_LOCKED indicates that the card is temporarily blocked for usage. |



<a name="thebaasco.tenant.corecard.v1.CardType"></a>

#### CardType
CardType provides indication of the Card's type.

| Name | Number | Description |
| ---- | ------ | ----------- |
| CARD_TYPE_UNSPECIFIED | 0 | CARD_TYPE_UNSPECIFIED should never be used directly. It is returned when a value has not been specified, and is present by convention, for error-detection purposes. Trying to create a Card with this type will result in an error. |
| CARD_TYPE_PREPAID | 1 | CARD_TYPE_PREPAID represents a Prepaid Card. A Prepaid Card uses the linked account's funds when processing operations. |



<a name="thebaasco.tenant.corecard.v1.ErrorCodes"></a>

#### ErrorCodes
A Domain-specific list of error codes (For Core Card) documenting the error situations that could arrive while consuming synchronous or asynchronous APIs
In all cases these error codes will be communicated to the consumer in a structured manner within an instance of AppError (see https://github.com/thebaasco/api-contracts/tree/master/proto/types/errordetails.proto)
that will be included within the error status response: status.Status.Details

| Name | Number | Description |
| ---- | ------ | ----------- |
| UNSPECIFIED | 0 | UNSPECIFIED should never be used directly. It is returned when a value has not been specified, and is present by convention, for error-detection purposes. |
| CARDS_INVALID_ACCOUNT | 1 | CARDS_INVALID_ACCOUNT - Invalid Account. gRPC Code=InvalidArgument. Returned when an account ID passed as argument for a given operation does not resolve to an existing account or the account is not authorized for the caller (e.g. the customer does not own the account) |
| CARDS_INVALID_AUTHORIZED_USER | 2 | CARDS_INVALID_AUTHORIZED_USER - Invalid authorized user. gRPC Code==InvalidArgument. Returned while creating a card for an authorized user if the ID passed as argument does not resolve to an existing user or the user is not an authorized user of the given account |
| CARDS_INVALID_CARD_ID | 3 | CARDS_INVALID_CARD_ID - Invalid Card ID. gRPC Code=InvalidArgument. Returned while executing card servicing operations (e.g Lock/Unlock) if the card ID passed as argument does not resolve to a valid card or the card is not owned by the current user or the underlying account is not owned by the current user |


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
| state | [CardState](#thebaasco.tenant.corecard.v1.CardState) |  | Describes the current state of the card. Output-only |
| owner_id | [string](#string) |  | The user ID of the card owner. Output-only |






<a name="thebaasco.tenant.corecard.v1.CardCreatedEvent"></a>

#### CardCreatedEvent
CardCreatedEvent is an event raised when a Card is successfuly created.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| event_id | [string](#string) |  | The unique ID of this event. Can be used by consumers to determine if an event has already been processed or not. This ID will always be a valid UUID. |
| card | [Card](#thebaasco.tenant.corecard.v1.Card) |  | The Card that was created. |
| tenant_id | [int32](#int32) |  | The tenant ID, to identify the tenant on which the operation occurred. This can be used when processing events at the global scope to differentiate between tenants. |
| card_creation_time | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  | The UTC timestamp for the creation of the Card. |
| event_time | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  | The UTC timestamp when this event was issued. |






<a name="thebaasco.tenant.corecard.v1.CardUpdatedEvent"></a>

#### CardUpdatedEvent
CardUpdatedEvent is an event raised when a card is updated. E.g. The card description could be changed or the card state change from Active to Locked,


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| event_id | [string](#string) |  | The unique ID of this event. Can be used by consumers to determine if an event has already been processed or not. This ID will always be a valid UUID. |
| event_time | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  | The UTC timestamp when this event was issued. |
| card | [Card](#thebaasco.tenant.corecard.v1.Card) |  | Snapshot of the card at its resulting state after the update took place. |
| user_id | [string](#string) |  | The ID of the user who triggered the operation that resulted in the creation of the current event |
| tenant_id | [int32](#int32) |  | The tenant ID, to identify the tenant on which the operation occurred. This can be used when processing events at the global scope to differentiate between tenants. |






<a name="thebaasco.tenant.corecard.v1.CreateAuthorizedUserCardRequest"></a>

#### CreateAuthorizedUserCardRequest
CreateAuthorizedCardRequest is message used to perform the asynchronous operation of creating a new Card for an authorized user of an account.
Performing this operation will generate an asynchronous response of type Card on the response topic.
When creating the card, the information contained in the Client-Profile for the authorized user will be fetched to
populate information such as name on card and billing address.
Errors:
ErrorCodes.CARDS_INVALID_ACCOUNT if the account passed as argument does not exist or is not owned by the current user
ErrorCodes.CARDS_INVALID_AUTH_USER if the user ID passed as argument does not exist or is not an authorized user of the account


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| card | [Card](#thebaasco.tenant.corecard.v1.Card) |  | A valid Card message. Refer to Card message for details. |
| authorized_user_id | [string](#string) |  | The ID of the authorized user who will be the owner of the newly created card |






<a name="thebaasco.tenant.corecard.v1.CreateAuthorizedUserCardResponse"></a>

#### CreateAuthorizedUserCardResponse
CreateAuthorizedUserCardResponse is the response model for the asynchronous operation of creating a new Card for an authorized user of an account.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| card | [Card](#thebaasco.tenant.corecard.v1.Card) |  | A valid Card message. Refer to Card message for details. |






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
| card_ref_number | [string](#string) |  | A reference number assigned to the selected card by the card payment processor provider. This value must be passed along with the token to the card payment processor's SDK in order to reveal the card's authentication details (number, CVV, expiration) |






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






<a name="thebaasco.tenant.corecard.v1.LockCardRequest"></a>

#### LockCardRequest
LockCardRequest is message used to perform the asynchronous operation of temporarily locking a card.
Performing this operation will generate an asynchronous response of type Card on the response topic or an error if applicable.
This feature allows customers to prevent transactions from getting authorized while they confirm if a card is effectively lost.
After finding the card if it is the case, they can re-enable the card by using the unlock function, see UnlockCardRequest
Errors:
ErrorCodes.CARDS_INVALID_CARD_ID if the card ID passed as argument does not resolve to a valid card or the card is not owned by the current user or the underlying account is not owned by the current user


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| card_id | [string](#string) |  | The ID of the card to be locked |






<a name="thebaasco.tenant.corecard.v1.LockCardResponse"></a>

#### LockCardResponse
LockCardResponse is message used as return type of the async operation LockCard


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| card | [Card](#thebaasco.tenant.corecard.v1.Card) |  | The card in its current status as result of the operation. |






<a name="thebaasco.tenant.corecard.v1.UnlockCardRequest"></a>

#### UnlockCardRequest
UnlockCardRequest is message used to perform the asynchronous operation of unlocking a locked card.
Performing this operation will generate an asynchronous response of type UnlockCardResponse on the response topic or an error if applicable.
This feature allows customers to re-enable transactions on the card if they find it after suspecting it lost.
Errors:
ErrorCodes.CARDS_INVALID_CARD_ID if the card ID passed as argument does not resolve to a valid card or the card is not owned by the current user or the underlying account is not owned by the current user


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| card_id | [string](#string) |  | The ID of the locked card to be unlocked |






<a name="thebaasco.tenant.corecard.v1.UnlockCardResponse"></a>

#### UnlockCardResponse
UnlockCardResponse is message used as return type of the async operation UnlockCard


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| card | [Card](#thebaasco.tenant.corecard.v1.Card) |  | The card in its current as result of of the operation. |






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
| network_transaction_time | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  | The UTC timestamp for the point in time when the transaction took place from the payment network's standpoint Its value is extracted from the Data Element DE-007 of the ISO-8583 messages |
| mti | [string](#string) |  | The 4 digits representation of the originating message type (e.g. "0100", "0400", ... ). |
| card_id | [string](#string) |  | The identifier of the card involved in this card transaction. |






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



<a name="txsimulation_v1_cardpayments.proto"></a>

## txsimulation_v1_cardpayments.proto



### Services

<a name="thebaasco.txsimulation.v1.CardPaymentsSimulationService"></a>

#### CardPaymentsSimulationService
CardPaymentsSimulationService is a synchronous gRPC service for the simulation of card payments transactions

##### Purchase

> **rpc** Purchase([PurchaseRequest](#thebaasco.txsimulation.v1.PurchaseRequest))
[PurchaseResponse](#thebaasco.txsimulation.v1.PurchaseResponse)

Used to simulate a purchase made in one step. The transaction authorization and confirmation occurs in a single shot.
At the end of the operation the committed balance of the underlying account will be reduced by the amount specified
in the purchase request

##### AuthorizePurchase

> **rpc** AuthorizePurchase([AuthorizePurchaseRequest](#thebaasco.txsimulation.v1.AuthorizePurchaseRequest))
[AuthorizePurchaseResponse](#thebaasco.txsimulation.v1.AuthorizePurchaseResponse)

Used to simulate a purchase made in two steps. The transaction authorization occurs during this step
and it must be followed by a finalization call: ConfirmPurchase() or CancelPurchase()
At the end of the operation the overall transaction ends up in an authorized status and will be open until the
finalization operation is called for this transfer

##### ConfirmPurchase

> **rpc** ConfirmPurchase([ConfirmPurchaseRequest](#thebaasco.txsimulation.v1.ConfirmPurchaseRequest))
[ConfirmPurchaseResponse](#thebaasco.txsimulation.v1.ConfirmPurchaseResponse)

Used to simulate a purchase made in two steps.
This is the second step of a successful 2-steps purchase, where the merchant confirms the transaction
closing it and triggering the actual moving of funds in the underlying account

##### CancelPurchase

> **rpc** CancelPurchase([CancelPurchaseRequest](#thebaasco.txsimulation.v1.CancelPurchaseRequest))
[CancelPurchaseResponse](#thebaasco.txsimulation.v1.CancelPurchaseResponse)

Used to simulate a purchase made in two steps.
This is the second step of a cancelled 2-steps purchase, where the merchant voids the purchase authorization
closing it and triggering releasing any funds withheld by the authorization

##### Refund

> **rpc** Refund([RefundRequest](#thebaasco.txsimulation.v1.RefundRequest))
[RefundResponse](#thebaasco.txsimulation.v1.RefundResponse)

Used to simulate a purchase refund.
E.g. when the customer returns all or part of the merchandise to receive a total or partial refund
or when the merchant needs to return part of the funds because some item ran out of stock after the order was completed and the payment completed.
Notice that this operation is meant to be used for cases where the original payment already completed.
in order to cancel an on-going payments, use CancelPurchase() instead.


 <!-- end services -->


### API-specific types

<a name="thebaasco.txsimulation.v1.AuthorizePurchaseRequest"></a>

#### AuthorizePurchaseRequest
message AuthorizePurchaseRequest is the request model for the AuthorizePurchase() operation.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| payment | [PaymentDetails](#thebaasco.txsimulation.v1.PaymentDetails) |  | The identifier of the card that will be used for the purchase simulation |






<a name="thebaasco.txsimulation.v1.AuthorizePurchaseResponse"></a>

#### AuthorizePurchaseResponse
AuthorizePurchaseResponse is returned when the purchase authorization operation finishes


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| lifecycle_transaction_id | [string](#string) |  | Unique identifier that represents the overarching transaction, across all phases of it's lifecycle (e.g. authorization, settlement, reversal, ...). |
| transaction_id | [string](#string) |  | Unique identifier that represents this single financial transaction (operation). This ID is stable across all domains for a given transaction. |
| response_code | [string](#string) |  | an ISO 8583 response code describing the status of the card payment "00" means success other values indicate error e.g. "51" -> Insufficient funds |
| id | [string](#string) |  | Unique identifier that will be used to 'link' transactions for one purchase ( for example: auth, settlement is one purchase. Settlement, refund is another.. etc) |






<a name="thebaasco.txsimulation.v1.CancelPurchaseRequest"></a>

#### CancelPurchaseRequest
CancelPurchaseRequest is the request model for the operation CancelPurchase().


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| lifecycle_transaction_id | [string](#string) |  | Unique identifier that represents the overarching transaction, across all phases of it's lifecycle (e.g. authorization, settlement, reversal, ...). |
| id | [string](#string) |  | CancelPurchaseRequest is the second step of a cancelled 2-steps purchase and this field is used to specify which purchase should be cancelled. It is required. |
| card_id | [string](#string) |  | The identifier of the card that will be used for the purchase simulation |






<a name="thebaasco.txsimulation.v1.CancelPurchaseResponse"></a>

#### CancelPurchaseResponse
AuthorizePurchaseResponse is returned when the purchase authorization operation finishes


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| lifecycle_transaction_id | [string](#string) |  | Unique identifier that represents the overarching transaction, across all phases of it's lifecycle (e.g. authorization, settlement, reversal, ...). |
| transaction_id | [string](#string) |  | Unique identifier that represents this single financial transaction (operation). This ID is stable across all domains for a given transaction. |
| response_code | [string](#string) |  | an ISO 8583 response code describing the status of the card payment "00" means success other values indicate error e.g. "51" -> Insufficient funds |






<a name="thebaasco.txsimulation.v1.ConfirmPurchaseRequest"></a>

#### ConfirmPurchaseRequest
ConfirmPurchaseRequest is the request model for the ConfirmPurchase() operation.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| lifecycle_transaction_id | [string](#string) |  | Unique identifier that represents the overarching transaction, across all phases of it's lifecycle (e.g. authorization, settlement, reversal, ...). |
| amount | [thebaasco.types.Amount](#thebaasco.types.Amount) |  | Amount to be used for the authorization confirmation. Sometimes it can be a smaller than what was originally authorized e.g. for gas station transactions where there is an original authorization of 200$ but the confirmed amount depends on the actual gas the customer ended up buying |
| id | [string](#string) |  | Unique identifier that will be used to 'link' transactions for one purchase ( for example: auth, settlement is one purchase. Settlement, refund is another.. etc) |
| card_id | [string](#string) |  | The identifier of the card that will be used for the purchase simulation |






<a name="thebaasco.txsimulation.v1.ConfirmPurchaseResponse"></a>

#### ConfirmPurchaseResponse
ConfirmPurchaseResponse is returned when the purchase confirmation operation finishes


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| lifecycle_transaction_id | [string](#string) |  | Unique identifier that represents the overarching transaction, across all phases of it's lifecycle (e.g. authorization, settlement, reversal, ...). |
| transaction_id | [string](#string) |  | Unique identifier that represents this single financial transaction (operation). This ID is stable across all domains for a given transaction. |
| response_code | [string](#string) |  | an ISO 8583 response code describing the status of the card payment "00" means success other values indicate error e.g. "51" -> Insufficient funds |






<a name="thebaasco.txsimulation.v1.PaymentDetails"></a>

#### PaymentDetails
PaymentDetails is a model that describes the detail of a card payment


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| card_id | [string](#string) |  | The identifier of the card that will be used for the purchase simulation |
| amount | [thebaasco.types.Amount](#thebaasco.types.Amount) |  | Amount to be paid in this transaction |
| card_acceptor_name | [string](#string) |  | This field contains the name of the merchant. An example: "AMAZON.COM". |
| merchant_category_code | [string](#string) |  | Category code of the merchant to use for the payment simulation For example values see: https://www.citibank.com/tts/solutions/commercial-cards/assets/docs/govt/Merchant-Category-Codes.pdf If left blank will get a default value assigned. |






<a name="thebaasco.txsimulation.v1.PurchaseRequest"></a>

#### PurchaseRequest
PurchaseRequest is used when distribution partner wants to simulate a card transaction.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| payment | [PaymentDetails](#thebaasco.txsimulation.v1.PaymentDetails) |  | The identifier of the card that will be used for the purchase simulation |






<a name="thebaasco.txsimulation.v1.PurchaseResponse"></a>

#### PurchaseResponse
PurchaseResponse is returned when the purchase operation finishes


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| lifecycle_transaction_id | [string](#string) |  | Unique identifier that represents the overarching transaction, across all phases of it's lifecycle (e.g. authorization, settlement, reversal, ...). |
| transaction_id | [string](#string) |  | Unique identifier that represents this single financial transaction (operation). This ID is stable across all domains for a given transaction. |
| response_code | [string](#string) |  | an ISO 8583 response code describing the status of the card payment "00" means success other values indicate error e.g. "51" -> Insufficient funds |






<a name="thebaasco.txsimulation.v1.RefundRequest"></a>

#### RefundRequest
RefundRequest is the request model for the operation Refund()
It is used to simulate that a customer is returning all or part of the merchandise he bought
to obtain a total or partial refund
Notice that this operation is expected to happen afther the original transaction had already been completed
To reverse/cancel an on-going transaction before it is confirmed, use the CancelPurchase() operation instead


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| lifecycle_transaction_id | [string](#string) |  | Unique identifier that represents the overarching transaction, across all phases of it's lifecycle (e.g. authorization, settlement, reversal, ...). |
| amount | [thebaasco.types.Amount](#thebaasco.types.Amount) |  | Indicates the amount to be returned to the customer's account it could be the total value of the original purchase or it could be a smaller amount in case of partial refunds. |
| card_id | [string](#string) |  | The identifier of the card that will be used for the purchase simulation |






<a name="thebaasco.txsimulation.v1.RefundResponse"></a>

#### RefundResponse
RefundResponse is returned when the purchase authorization operation finishes


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| lifecycle_transaction_id | [string](#string) |  | Unique identifier that represents the overarching transaction, across all phases of it's lifecycle (e.g. authorization, settlement, reversal, ...). |
| transaction_id | [string](#string) |  | Unique identifier that represents this single financial transaction (operation). This ID is stable across all domains for a given transaction. |
| response_code | [string](#string) |  | an ISO 8583 response code describing the status of the card payment "00" means success other values indicate error e.g. "51" -> Insufficient funds |





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
