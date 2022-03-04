# Core Transfers API
<a name="top"></a>



<a name="coretransfer_v1_interacregistration.proto"></a>

## coretransfer_v1_interacregistration.proto



### Services

<a name="thebaasco.tenant.interac.v1.AutodepositRegistrations"></a>

#### AutodepositRegistrations
AutodepositRegistrations service exposes synchronous endpoints used to list/retrieve Autodeposit Account Alias Registrations
This service must be called with a valid user authentication
Unauthenticated calls will generate an error with status code UNAUTHENTICATED

##### ListAutodepositRegistrations

> **rpc** ListAutodepositRegistrations([ListAutodepositRegistrationsRequest](#thebaasco.tenant.interac.v1.ListAutodepositRegistrationsRequest))
    [ListAutodepositRegistrationsResponse](#thebaasco.tenant.interac.v1.ListAutodepositRegistrationsResponse)

ListRegistrations lists all Autodeposit Registrations for a given account 
enforcing that the authorized user has permissions to see them. For illustration purposes, 
users with permissions to see autodeposit registrations include the account owner and possibly the authorized users of that account

The function returns an empty list if no registrations are found that match the conditions above
Calls made with a non-existent account ID or unauthorized account ID, generate an error with status code INVALID_ARGUMENT desc: "account number is invalid"



<a name="thebaasco.tenant.interac.v1.AutodepositRegistrationsCommands"></a>

#### AutodepositRegistrationsCommands
AutodepositRegistrationsCommands is a key component of Core Transfers API that exposes asynchronous operations that alter the state of a Interac autodeposit registrations.
It is not directly accessible by consumers as a gRPC service, it instead uses Google PubSub as delivery mechanism for requests and responses.
We include it in the documentation as a reference for a better understanding of the responsibilities and API surface of this domain

##### CreateRegistration

> **rpc** CreateRegistration([CreateRegistrationRequest](#thebaasco.tenant.interac.v1.CreateRegistrationRequest))
    [CreateRegistrationResponse](#thebaasco.tenant.interac.v1.CreateRegistrationResponse)

CreateRegistration is an asynchronous function used to create a new Interac account alias registration.
An account alias registration allows a customer to receive direct deposits to a selected account given an alias that can be an email or phone number.
Performing this operation will generate an asynchronous response of type CreateRegistrationResponse, on the asynchronous command's response topic.
When creating the account alias registration, the information contained in the Client-Profile domain for the current user will be used to
populate customer profile information created at Interac, e.g. Legal name, address, etc.
Different errors can arise when creating account alias registrations. For details see [ErrorCodes](#thebaasco.tenant.interac.v1.ErrorCodes)

##### DeleteRegistration

> **rpc** DeleteRegistration([DeleteRegistrationRequest](#thebaasco.tenant.interac.v1.DeleteRegistrationRequest))
[DeleteRegistrationResponse](#thebaasco.tenant.interac.v1.DeleteRegistrationResponse)

DeleteRegistration is an asynchronous function used to initiate the deletion/deactivation of an existing Interac account alias registration.
Performing this operation will generate an asynchronous response of type DeleteRegistrationResponse on the asynchronous command's response topic.
Deleting an account alias registration will invalidate/inactivate the link between an account alias handle (email/phone) and an account, so that
the account will no longer receive deposits automatically when sent using the given alias.
At that moment, the customer is free to use the same alias handle to create a new registration pointing to the same or a different account.

##### UpdateRegistration

> **rpc** UpdateRegistration([UpdateRegistrationRequest](#thebaasco.tenant.interac.v1.UpdateRegistrationRequest))
[UpdateRegistrationResponse](#thebaasco.tenant.interac.v1.UpdateRegistrationResponse)

UpdateRegistration is an asynchronous function used to initiate the update of an existing Interac account alias registration.
Performing this operation will generate an asynchronous response of type UpdateRegistrationResponse on the response topic.
When updating the account alias registration, minimal changes are allowed, notably it is meant to change the target account linked to the alias.
Notice that it is not possible to change the account alias handle itself (e.g. email/phone number). If more important changes are needed, the customer
could always use the delete registration service and start a new account alias from scratch after that.
 <!-- end services -->


### Enums

<a name="thebaasco.tenant.interac.v1.ErrorCodes"></a>

#### ErrorCodes
A Domain-specific list of error codes (For Transfers/Interac) documenting the error situations that could arrive while consuming synchronous or asynchronous APIs
In all cases these error codes will be communicated to the consumer in a structured manner within an instance of [AppError](../Global-types/errordetails/#apperror)
that will be included within the error status response: status.Status.Details

| Name | Number | Description |
| ---- | ------ | ----------- |
| UNSPECIFIED | 0 | UNSPECIFIED should never be used directly. It is returned when a value has not been specified, and is present by convention, for error-detection purposes. |
| TRANS_INTERAC_INVALID_ACCOUNT | 1 | TRANS_INTERAC_INVALID_ACCOUNT - Invalid Account. gRPC Code=InvalidArgument. Returned when an account ID passed as argument for a given operation does not resolve to an existing account or the account is not authorized for the caller (e.g. the customer does not own the account) |
| TRANS_INTERAC_ALIAS_IN_USE | 2 | TRANS_INTERAC_ALIAS_IN_USE - Autodeposit alias in use. gRPC Code==InvalidArgument. Returned while creating an autodeposit registration with an alias (email or phone number) that is already in use by another active registration |
| TRANS_INTERAC_REG_COUNT_EXCEEDED | 3 | TRANS_INTERAC_REG_COUNT_EXCEEDED - Maximum number of account-alias registrations threshold exceeded. gRPC Code==FailedPrecondition. Returned when the customer has reached the allowed limit for account alias registrations on Interac |
| TRANS_INTERAC_INV_MOB_NUMBER | 4 | TRANS_INTERAC_INV_MOB_NUMBER - Invalid mobile phone number. gRPC Code==InvalidArgument. Returned during creation of an an account alias registration based on phone number, if Interac does not recognize the phone number as valid. |
| TRANS_INTERAC_INV_MOB_AREACODE | 5 | TRANS_INTERAC_INV_MOB_AREACODE - Invalid mobile phone area code. gRPC Code==InvalidArgument. Returned during creation of an an account alias registration based on phone number, if Interac does not recognize the phone number area code as valid. |


 <!-- end enums -->


### API-specific types

<a name="thebaasco.tenant.interac.v1.CreateRegistrationRequest"></a>

#### CreateRegistrationRequest
CreateRegistrationRequest is message used to initiate the creation of a new Interac account alias registration.
Performing this operation will generate an asynchronous response of type CreateRegistrationResponse, on the response topic.
When creating the account alias registration, the information contained in the Client-Profile domain for the current user will be used to
populate customer profile information created at Interac, e.g. Legal name, address, etc.
Different errors can arise when creating account alias registrations. For details see [ErrorCodes](#thebaasco.tenant.interac.v1.ErrorCodes)


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| registration | [Registration](#thebaasco.tenant.interac.v1.Registration) |  |  |






<a name="thebaasco.tenant.interac.v1.CreateRegistrationResponse"></a>

#### CreateRegistrationResponse
CreateRegistrationResponse is a wrapper model used to contain the result of a create account alias registration operation


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| registration | [Registration](#thebaasco.tenant.interac.v1.Registration) |  | Registration is the newly created registration, returned to the consumer for reference of the registration's current status and expiration date In case some feedback message needs to be presented to the user with that information |


<a name="thebaasco.tenant.interac.v1.DeleteRegistrationRequest"></a>

#### DeleteRegistrationRequest
DeleteRegistrationRequest is message used to initiate the deletion/deactivation of an existing Interac account alias registration.
Performing this operation will generate an asynchronous response of type DeleteRegistrationResponse on the response topic.
Deleting an account alias registration will invalidate/inactivate the link between an account alias handle (email/phone) and an account, so that
the account will no longer receive deposits automatically when sent using the given alias.
At that moment, the customer is free to use the same alias handle to create a new registration pointing to the same or a different account.
Errors, for more details see [ErrorCodes](#thebaasco.tenant.interac.v1.ErrorCodes)
- AppError: code=TRANS_INTERAC_INVALID_ACCOUNT if the account ID passed as argument is not owned or authorized for the current user.
- AppError: type=codes.Unauthorized if the registration ID passed as argument is not owned by the current user.
- AppError: type codes.Internal, code=GEN_INTERNAL if there is an unexpected error while processing the request, e.g. the execution of the operation on Interac's systems returns an unexpected error.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| registration_id | [string](#string) |  |  |



<a name="thebaasco.tenant.interac.v1.DeleteRegistrationResponse"></a>

#### DeleteRegistrationResponse
DeleteRegistrationResponse is a wrapper model used to contain the result of a delete account alias registration operation


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| registration | [Registration](#thebaasco.tenant.interac.v1.Registration) |  |  |




<a name="thebaasco.tenant.interac.v1.InteracRegistrationCompletedEvent"></a>

#### InteracRegistrationCompletedEvent
Business event produced when an Interac account alias registration is created


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| event_id | [string](#string) |  | The unique ID of this event. Can be used by consumers to determine if an event has already been processed or not. This ID will always be a valid UUID. |
| registration_id | [string](#string) |  | Identifier of the account alias registration created |
| individual_party_id | [string](#string) |  | Identifier of the customer for which this account alias was created |
| expiry_date | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  | Expiration date of the account alias |
| account_id | [string](#string) |  | Account for which the alias was created |
| alias_email | [string](#string) |  |  |
| alias_phone_number | [PhoneNumber](#thebaasco.tenant.interac.v1.PhoneNumber) |  |  |
| event_time | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  | The UTC timestamp when this event was issued. |






<a name="thebaasco.tenant.interac.v1.ListAutodepositRegistrationsRequest"></a>

#### ListAutodepositRegistrationsRequest
ListRegistrationsRequest is a message passed as argument to the ListRegistrations operation.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| account_id | [string](#string) |  | The Account ID for the Registrations that are to be listed. Must be a valid UUID. Must be an existing account owned by or authorized for the current user |






<a name="thebaasco.tenant.interac.v1.ListAutodepositRegistrationsResponse"></a>

#### ListAutodepositRegistrationsResponse
ListRegistrationsResponse is a message returned in response to the ListRegistrations operation.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| Registrations | [Registration](#thebaasco.tenant.interac.v1.Registration) | repeated | The requested list of Registrations. Will be empty if no registrations exist |






<a name="thebaasco.tenant.interac.v1.PhoneNumber"></a>

#### PhoneNumber
Model that represents a valid phone number


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| phone_number | [string](#string) |  |  |






<a name="thebaasco.tenant.interac.v1.Registration"></a>

#### Registration
Model that contains the data of an Interac account alias registration


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| registration_id | [string](#string) |  | The unique identifier of this registration Output only.
| expiry_date | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  | UTC point in time after which this account alias registration expires No transfer will succeed if sent through the given alias (email/phone) past this date Output only. |
| account_id | [string](#string) |  | Unique identifier of the account whose alias this registration involves |
| alias_email | [string](#string) |  |  |
| alias_phone_number | [PhoneNumber](#thebaasco.tenant.interac.v1.PhoneNumber) |  |  |


<a name="thebaasco.tenant.interac.v1.UpdateRegistrationRequest"></a>

#### UpdateRegistrationRequest
UpdateRegistrationRequest is message used to initiate the update of an existing Interac account alias registration.
Performing this operation will generate an asynchronous response of type UpdateRegistrationResponse on the response topic.
When updating the account alias registration, minimal changes are allowed, notably it is meant to change the target account linked to the alias.
Notice that it is not possible to change the account alias handle itself (e.g. email/phone number). If more important changes are needed, the customer
could always use the delete registration service and start a new account alias from scratch after that.
Errors, for more details see [ErrorCodes](#thebaasco.tenant.interac.v1.ErrorCodes)
- AppError: type=codes.InvalidArgument, code=TRANS_INTERAC_INVALID_ACCOUNT if the account ID passed as argument is not owned or authorized for the current user.
- AppError: type=codes.Unauthorized if the registration ID passed as argument is not owned by the current user.
- AppError: type codes.Internal, code=GEN_INTERNAL if there is an unexpected error while processing the request, e.g. the execution of the operation on Interac's systems returns an unexpected error.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| registration_id | [string](#string) |  |  |
| account_id | [string](#string) |  |  |


<a name="thebaasco.tenant.interac.v1.UpdateRegistrationResponse"></a>

#### UpdateRegistrationResponse
UpdateRegistrationResponse is a wrapper model used to contain the result of an update account alias registration operation


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| registration | [Registration](#thebaasco.tenant.interac.v1.Registration) |  |  |




 <!-- end messages -->



<a name="coretransfer_v1_remittance.proto"></a>

## coretransfer_v1_remittance.proto






### API-specific types

<a name="thebaasco.tenant.coretransfer.v1.RemittanceInformation"></a>

#### RemittanceInformation
RemittanceInformation includes additional information provided by the sender giving context for a transfer
for instance, a transfer may be sent to a business by one of its clients in order to pay for a bill, the remittance data will
include a reference to that bill payment
Remittance data could be unstructured or structured or both.

- Unstructured remittance data is a free-format text of up to 420 characters, e.g. the notes that clients put on a regular Interac transfer

- Structured remittance data is packaged as a json payload which is a variant of the ISO20022 standard for remittance data (originally defined as XML)

For illustration purposes, a sample structured payload looks like this:
```
 [
    {
       "referred_document_information": [
           {
               "type": {
                   "code_or_proprietary": {
                       "code": "MSIN"
                   }
               },
               "number": "4U1LH53P0R68OFFL0PN0NT4QOCFW4QUVJVE",
               "related_date": "2021-08-19"
           }
       ],
       "referred_document_amount": {
       ...
       },
       "creditor_reference_information": {
       ...
       },
       "invoicer": {
       ...
       },
       "invoicee": {
        ...
       },
       "additional_remittance_information": [
           "Deposit Req"
       ]
   },
]
```

| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| unstructured | [Unstructured](#thebaasco.tenant.coretransfer.v1.Unstructured) |  |  |
| structured | [Structured](#thebaasco.tenant.coretransfer.v1.Structured) |  |  |






<a name="thebaasco.tenant.coretransfer.v1.Structured"></a>

#### Structured
StructuredRemittance is a container of an json version of an ISO20022 structured remittance data block
The root of the object is an array that can contain up to 5 remittance document instances.
TODO: Update with details of this structure once the documentation is available in our public site


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| data | [string](#string) |  | JSON content for this structured remittance data The max size has been estimated assuming 5 instances of remittance documents, each one filled with all possible fields set to test values with their max documented length |






<a name="thebaasco.tenant.coretransfer.v1.Unstructured"></a>

#### Unstructured
UnstructuredRemittance data is comprised of up to 3 sender-provided strings with no more than 140 characters per string.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| data | [string](#string) |  |  |





 <!-- end messages -->

<a name="coretransfer_v1_incoming_interac_transfer.proto"></a>

## coretransfer_v1_incoming_interac_transfer.proto



### Services

<a name="thebaasco.tenant.coretransfer.interac.v1.IncomingInteracTransfers"></a>

#### IncomingInteracTransfers
IncomingInteracTransfers service exposes synchronous endpoints used to retrieve details of an incoming Q&A transfer and to authenticate the transfer
This service must be called with a valid user authentication
Unauthenticated calls will generate an error with status code UNAUTHENTICATED

##### GetIncomingTransferDetails

> **rpc** GetIncomingTransferDetails([GetIncomingTransferDetailsRequest](#thebaasco.tenant.coretransfer.interac.v1.GetIncomingTransferDetailsRequest))
[GetIncomingTransferDetailsResponse](#thebaasco.tenant.coretransfer.interac.v1.GetIncomingTransferDetailsResponse)

GetIncomingTransferDetails retrieves the details and state of an incoming Interac Q&A transfer

An error with error code TRANS_INTERAC_INVALID_TRANSFER desc: "payment number is invalid" will be generated in the following scenarios:
- calls made with a non-existent interac_payment_number
  An error with error code TRANS_INTERAC_INCONSISTENT_CUSTOMER desc: "Transfer was previously authenticated with a different customer ID" will be generated when:
- calls made with a different customer than was previously authenticated


 <!-- end services -->



### Enums

<a name="thebaasco.tenant.coretransfer.interac.v1.HashAlgorithm"></a>

#### HashAlgorithm
HashAlgorithm represents the algorithm that must be used when hashing a security answer

| Name | Number | Description |
| ---- | ------ | ----------- |
| HASH_ALGORITHM_UNSPECIFIED | 0 | INCOMING_TRANSFER_STATUS_UNSPECIFIED is reserved for error detection purposes. It should not be used directly. |
| SHA256 | 1 | SHA256 - The SHA-256 hash algorithm. |



<a name="thebaasco.tenant.coretransfer.interac.v1.IncomingTransferErrorCodes"></a>

#### IncomingTransferErrorCodes
A Domain-specific list of error codes (For Transfers/Interac) documenting the error situations that could arrive while consuming synchronous or asynchronous APIs
In all cases these error codes will be communicated to the consumer in a structured manner within an instance of AppError (see https://github.com/thebaasco/api-contracts/tree/master/proto/types/errordetails)
that will be included within the error status response: status.Status.Details

| Name | Number | Description |
| ---- | ------ | ----------- |
| UNSPECIFIED | 0 | UNSPECIFIED should never be used directly. It is returned when a value has not been specified, and is present by convention, for error-detection purposes. |
| TRANS_INTERAC_INVALID_TRANSFER | 1 | TRANS_INTERAC_INVALID_TRANSFER - Transfer does not exist. gRPC Code=InvalidArgument. Returned when an Interac payment number passed as argument for a given operation does not resolve to a transfer or, in the case of a `complete transfer` operation, the user does not have access to the target account. |
| TRANS_INTERAC_ALREADY_CANCELLED | 2 | TRANS_INTERAC_ALREADY_CANCELLED - Operation cannot be performed due to state of the transfer. gRPC Code==FailedPrecondition. Returned when attempting to authenticate a transfer that has already been canceled. |
| TRANS_INTERAC_ALREADY_ACCEPTED | 3 | TRANS_INTERAC_ALREADY_ACCEPTED - Operation cannot be performed due to the state of the transfer. gRPC Code==FailedPrecondition. Returned when attempting to authenticate a transfer that has already been accepted. |
| TRANS_INTERAC_PREVIOUS_TRANSFER_IN_PROGRESS | 4 | TRANS_INTERAC_PREVIOUS_TRANSFER_IN_PROGRESS - A previous transaction between this sender and recipient has not been completed. gRPC Code==FailedPrecondition. Returned when there already exists a transfer between sender and recipient that has not been completed or canceled. |
| TRANS_INTERAC_INCONSISTENT_CUSTOMER | 5 | TRANS_INTERAC_INCONSISTENT_CUSTOMER - Transfer was previously authenticated with a different customer ID. gRPC Code==InvalidArgument. Returned when performing any operation a transfer has been authenticated by different customer ID. |
| TRANS_INTERAC_TRANSFER_LIMITS_EXCEEDED | 6 | TRANS_INTERAC_TRANSFER_LIMITS_EXCEEDED - Rolling or per-transaction limits exceeded . gRPC Code==FailedPrecondition. Returned when the current transfer exceeds the rolling or per-transaction limits configured in the Interac system |
| TRANS_INTERAC_AUTHENTICATION_REQUIRED | 7 | TRANS_INTERAC_AUTHENTICATION_REQUIRED - Authentication required for transfer . gRPC Code==FailedPrecondition. Returned when attempting to complete an incoming Q&A transfer whose authentication step has not yet been completed successfully. E.g. the user has not provided the correct answer to the security question. |



<a name="thebaasco.tenant.coretransfer.interac.v1.IncomingTransferStatus"></a>

#### IncomingTransferStatus
IncomingTransferStatus represents the state of the transfer.

| Name | Number | Description |
| ---- | ------ | ----------- |
| INCOMING_TRANSFER_STATUS_UNSPECIFIED | 0 | INCOMING_TRANSFER_STATUS_UNSPECIFIED is reserved for error detection purposes. It should not be used directly. |
| AVAILABLE | 1 | AVAILABLE represents that a transfer has not been processed and is in a valid state to be authorized. |
| AUTHORIZED | 2 | AUTHORIZED represents that a transfer has been authorized and is in a valid state to be accepted or declined. |
| ACCEPTED | 3 | ACCEPTED represents that a transfer has been accepted. The transfer is complete and deposited into the target account. |
| CANCELED | 4 | CANCELED represents that a transfer is complete and not deposited to an account. Ways to achieve this state are: if the transfer is canceled by the sender, if the receiver declines the transfer, the transfer has expired, the receiver is unable to authenticate the transfer within the allotted attempts. |


 <!-- end enums -->


### API-specific types

<a name="thebaasco.tenant.coretransfer.interac.v1.AuthenticateIncomingTransferRequest"></a>

#### AuthenticateIncomingTransferRequest
AuthenticateIncomingTransferRequest is a message used to authenticate an inbound Interac transfer.
Performing this operation will generate an asynchronous response of type AuthenticateIncomingTransferResponse, on the response topic.
Different errors can arise when completing an incoming transfer. For details see [ErrorCodes](#thebaasco.core-transfer.interac.v1.IncomingTransferErrorCodes)


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| interac_payment_number | [string](#string) |  | The payment number, as provided by Interac, which this request authenticates |
| security_answer | [string](#string) |  | The answer to the security question (as provided by the customer) with the following algorithm performed: - leading and trailing whitespace removed - converted to upper case - appended with the hash salt specified in hash_salt received from GetIncomingTransferDetailsRequest - encoded to ISO-8859-1 - hashed with the algorithm specified in hash_algorithm received from GetIncomingTransferDetailsRequest |






<a name="thebaasco.tenant.coretransfer.interac.v1.AuthenticateIncomingTransferResponse"></a>

#### AuthenticateIncomingTransferResponse
AuthenticateIncomingTransferResponse is a wrapper model used to contain the result of a authenticate incoming transfer operation


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| interac_payment_number | [string](#string) |  | The payment number, as provided by Interac, which this authentication response pertains to |
| success | [bool](#bool) |  | Indicates if the provided response was correct |
| retry_allowed | [bool](#bool) |  | In case of an incorrect response (success=false), indicates if the user can try again providing a new answer. |






<a name="thebaasco.tenant.coretransfer.interac.v1.CompleteIncomingTransferRequest"></a>

#### CompleteIncomingTransferRequest
CompleteIncomingTransferRequest is a message used to complete the deposit of an inbound Interac transfer.
Performing this operation will generate an asynchronous response of type CompleteIncomingTransferResponse, on the response topic.
When completing the deposit, information contained in the Client-Profile domain for the current user will be used to
enrich information sent to Interac to complete the transfer.
Different errors can arise when completing an incoming transfer. For details see [ErrorCodes](#thebaasco.core-transfer.interac.v1.IncomingTransferErrorCodes)


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| interac_payment_number | [string](#string) |  | The payment number, as provided by Interac, for which the deposit is being completed |
| target_account_id | [string](#string) |  | Unique identifier that represents the target account ID: - Current customer needs to be the owner or authorized user of the target account. - Target account must be a deposit account. |
| recipient_memo | [string](#string) |  | An optional memo provided by the recipient. This memo will be sent to the sender |






<a name="thebaasco.tenant.coretransfer.interac.v1.CompleteIncomingTransferResponse"></a>

#### CompleteIncomingTransferResponse
CompleteIncomingTransferResponse is a wrapper model used to contain the result of a complete incoming transfer operation


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| interac_payment_number | [string](#string) |  | The payment number, as provided by Interac, for which the transfer is being completed |
| transaction_id | [string](#string) |  | Unique identifier that represents this single financial transaction (operation). This ID is stable across all domains for a given transaction. |
| lifecycle_transaction_id | [string](#string) |  | Unique identifier that represents the overarching transaction, across all phases of it's lifecycle (e.g. authorization, settlement, reversal, ...). |






<a name="thebaasco.tenant.coretransfer.interac.v1.DeclineIncomingTransferRequest"></a>

#### DeclineIncomingTransferRequest
DeclineIncomingTransferRequest is a message used to decline the deposit of an inbound Interac transfer.
Performing this operation will generate an asynchronous response of type DeclineIncomingTransferResponse, on the response topic.
Different errors can arise when declining an incoming transfer. For details see [ErrorCodes](#thebaasco.core-transfer.interac.v1.IncomingTransferErrorCodes)


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| interac_payment_number | [string](#string) |  | The payment number, as provided by Interac, for which the transfer is being declined |
| recipient_memo | [string](#string) |  | An optional memo provided by the recipient. This memo will be sent to the sender |






<a name="thebaasco.tenant.coretransfer.interac.v1.DeclineIncomingTransferResponse"></a>

#### DeclineIncomingTransferResponse
DeclineIncomingTransferResponse is a wrapper model used to contain the result of a decline incoming transfer operation


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| interac_payment_number | [string](#string) |  | The payment number, as provided by Interac, for which the transfer is being declined |






<a name="thebaasco.tenant.coretransfer.interac.v1.GetIncomingTransferDetailsRequest"></a>

#### GetIncomingTransferDetailsRequest
GetIncomingTransferDetailsRequest is a message passed as argument to the GetIncomingTransferDetails operation.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| interac_payment_number | [string](#string) |  | The payment number, as provided by Interac, for which to return the details of the transfer |






<a name="thebaasco.tenant.coretransfer.interac.v1.GetIncomingTransferDetailsResponse"></a>

#### GetIncomingTransferDetailsResponse
GetIncomingTransferDetailsResponse is a message returned in response to the GetIncomingTransferDetails operation.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| transfer_details | [IncomingTransferDetails](#thebaasco.tenant.coretransfer.interac.v1.IncomingTransferDetails) |  | Details of the current state of an incoming transfer |






<a name="thebaasco.tenant.coretransfer.interac.v1.IncomingTransferDetails"></a>

#### IncomingTransferDetails
IncomingTransferDetails message contains details about a given inbound Interac transfer.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| interac_payment_number | [string](#string) |  | The payment number, as provided by Interac, which the details of the transfer will be returned for |
| transfer_status | [IncomingTransferStatus](#thebaasco.tenant.coretransfer.interac.v1.IncomingTransferStatus) |  | The current status of the transfer |
| transfer_amount | [thebaasco.types.Amount](#thebaasco.types.Amount) |  | The amount of the transfer. The amount should be always positive. |
| expiry_date | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  | The UTC timestamp when this transfer will expire. |
| initiator_name | [string](#string) |  | The name of who initiated the transfer |
| authentication_required | [bool](#bool) |  | Indicates if this transfer requires that the recipient provide a security answer before depositing the transfer into the account |
| security_question | [string](#string) |  | If authentication_required is true, the field contains the security question that the user must provide the answer to in order to deposit the transfer |
| sender_memo | [string](#string) |  | An optional memo provided by the sender |
| hash_salt | [string](#string) |  | The salt which will be appended in Authentication. Will be provided only if authentication_required = true. |
| hash_algorithm | [HashAlgorithm](#thebaasco.tenant.coretransfer.interac.v1.HashAlgorithm) |  | The algorithm which must be used when hashing the security answer for Authentication. Will be provided only if authentication_required = true. |


 <!-- end messages -->



<a name="coretransfer_v1_routinginformation.proto"></a>

## coretransfer_v1_routinginformation.proto



### Services

<a name="thebaasco.tenant.coretransfer.v1.Routing"></a>

#### Routing
Routing service provides ability to retrieve the AccountRouting information mapped to an account

##### GetAccountRouting

> **rpc** GetAccountRouting([GetAccountRoutingRequest](#thebaasco.tenant.coretransfer.v1.GetAccountRoutingRequest))
    [AccountRouting](#thebaasco.tenant.coretransfer.v1.AccountRouting)

GetAccountRouting retrieves the AccountRouting information linked to the account specified. If no AccountRouting is linked to the account, then this operation will return an
error with code NOT_FOUND.


 <!-- end services -->




### API-specific types

<a name="thebaasco.tenant.coretransfer.v1.AccountRouting"></a>

#### AccountRouting
AccountRouting message contains the routing details that is mapped to an account. It corresponds to the Canadian bank routing information, as well as account number.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| transit_number | [string](#string) |  | Transit number is a 5 digits number representing the "branch" for traditional banks. In our case, since we are a fully digital financial institution, this number is arbitrary and does not represent branches, or any other partitioning. |
| institution_number | [string](#string) |  | Institution number is a 3 digits number representing the financial institution. |
| account_number | [string](#string) |  | Account number is a 12 digits number that uniquely represents the account for the associated transit and institution numbers. The 12th digit is a checksum number, following the Luhn algorithm, that can be used to validate during data entry if a number was typed properly. The checksum must be calculated by concatenating all digits of the AccountRouting in the TRANSIT-INSTITUTION-ACCOUNT order i.e. for account routing 11111-222-333333333334, the check digit 4 must be calculated using number 1111122233333333333. |






<a name="thebaasco.tenant.coretransfer.v1.AccountRoutingLinkedEvent"></a>

#### AccountRoutingLinkedEvent
AccountRoutingLinkedEvent is an event raised when AccountRouting information is linked to an Account.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| event_id | [string](#string) |  | The unique ID of this event. Can be used by consumers to determine if an event has already been processed or not. This ID will always be a valid UUID. |
| account_id | [string](#string) |  | The ID of the Account on which the AccountRouting information was linked. This will be a valid UUID. |
| account_routing | [AccountRouting](#thebaasco.tenant.coretransfer.v1.AccountRouting) |  | The AccountRouting information that was linked to the Account. Refer to AccountRouting message for details. |
| tenant_id | [int32](#int32) |  | The tenant ID, to identify the tenant on which the operation occurred. This can be used when processing events at the global scope to differentiate between tenants. |
| account_routing_linked_time | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  | The UTC timestamp at which the AccountRouting information was linked to the Account. |






<a name="thebaasco.tenant.coretransfer.v1.CreateAccountRoutingRequest"></a>

#### CreateAccountRoutingRequest
CreateAccountRoutingRequest is a message used to perform linking of the provided Account to new AccountRouting information. 
Performing this operation will generate an asynchronous response of type CreateAccountRoutingResponse, on the response topic.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| account_id | [string](#string) |  | The ID of the Account for which to link new AccountRouting information. This must be a valid UUID, and user must have proper access to the Account. |






<a name="thebaasco.tenant.coretransfer.v1.CreateAccountRoutingResponse"></a>

#### CreateAccountRoutingResponse
CreateAccountRoutingResponse is the response to a CreateAccountRoutingRequest. It provides the created AccountRouting information for the requested Account.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| account_id | [string](#string) |  | The ID of the Account on which the AccountRouting information was linked. This will be a valid UUID. |
| account_routing | [AccountRouting](#thebaasco.tenant.coretransfer.v1.AccountRouting) |  | The AccountRouting information that was linked to the Account. Refer to AccountRouting message for details. |






<a name="thebaasco.tenant.coretransfer.v1.GetAccountRoutingRequest"></a>

#### GetAccountRoutingRequest
GetAccountRoutingRequest is a message passed as parameter to the GetAccountRouting operation.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| account_id | [string](#string) |  | The ID of the Account for which to retrieve AccountRouting information. This must be a valid UUID, and user must have proper access to the Account. |





 <!-- end messages -->



<a name="coretransfer_v1_transaction.proto"></a>

## coretransfer_v1_transaction.proto



### Services

<a name="thebaasco.tenant.coretransfer.v1.Transfers"></a>

#### Transfers
Transfers service exposes generic synchronous endpoints related to the retrieval of transfers data
The same contract will be implemented by subdomains that perform different transfer types
e.g. Interac E-transfers, Internal transfers, International transfers etc.
In all cases, the concrete service must be called with a valid user authentication
Unauthenticated calls will generate an error with status code UNAUTHENTICATED

##### GetTransferDetails

> **rpc** GetTransferDetails([GetTransferDetailsRequest](#thebaasco.tenant.coretransfer.v1.GetTransferDetailsRequest))
    [TransferTransactionDetails](#thebaasco.tenant.coretransfer.v1.TransferTransactionDetails)

GetTransferDetails returns the latest state of a transfer by a given transaction ID.

Errors:
TransferErrorCodes.TRANS_INVALID_TRANSACTION - If a transaction ID is provided that is not found or resolves to an unauthorized account
GenericErrorCodes.GEN_INTERNAL_ERROR - If an unexpected error occurs while processing the request. The error will include a RefNumber which Finaptic can use to investigate the cause.


 <!-- end services -->



### Enums

<a name="thebaasco.tenant.coretransfer.v1.CustomerRole"></a>

#### CustomerRole
CustomerRole enumeration indicates the role played by the customer.

| Name | Number | Description |
| ---- | ------ | ----------- |
| CUSTOMER_ROLE_UNSPECIFIED | 0 | CUSTOMER_ROLE_UNSPECIFIED should never be used directly. It is returned when a value has not been specified, and is present by convention, for error-detection purposes. |
| RECIPIENT | 1 | RECIPIENT indicates that the customer is the recipient of the transfer operation. |
| INITIATOR | 2 | INITIATOR indicates that the customer is the initiator of the transfer operation. |



<a name="thebaasco.tenant.coretransfer.v1.TransferErrorCodes"></a>

#### TransferErrorCodes
A Domain-specific list of error codes (For Transfers/Interac) documenting the error situations that could arrive while consuming synchronous or asynchronous APIs
In all cases these error codes will be communicated to the consumer in a structured manner within an instance of [AppError](../Global-types/errordetails/#apperror)
that will be included within the error status response: status.Status.Details

| Name | Number | Description |
| ---- | ------ | ----------- |
| TRANS_ERROR_CODE_UNSPECIFIED | 0 | UNSPECIFIED should never be used directly. It is returned when a value has not been specified, and is present by convention, for error-detection purposes. |
| TRANS_INVALID_TRANSACTION | 1 | TRANS_INVALID_TRANSACTION - Invalid Transaction. gRPC Code=InvalidArgument. Returned when a transaction ID passed as argument for a given operation does not resolve to an existing transaction or the account associated with the transaction is not authorized for the caller (e.g. the customer does not own the account) |



<a name="thebaasco.tenant.coretransfer.v1.TransferStatus"></a>

#### TransferStatus
TransferStatus enumeration indicates the processing status of a transaction.

| Name | Number | Description |
| ---- | ------ | ----------- |
| TRANSFER_STATUS_UNSPECIFIED | 0 | TRANSFER_STATUS_UNSPECIFIED should never be used directly. It is returned when a value has not been specified, and is present by convention, for error-detection purposes. |
| DECLINED | 1 | DECLINED indicates that the transfer has been declined and rollback has been performed. When transfer_type is INTERNAL_ACCOUNT_TO_ACCOUNT, this could be due to insufficient funds to initiate the transfer. |
| COMPLETED | 2 | COMPLETED indicates that the transfer has been successfully completed. |
| PENDING | 3 | PENDING indicates that the transfer is in an intermediate stage before completion. depending on the type of transfer, for instance, it is used for two-steps transfers to indicate the the initial step(authorization) has taken place but the second step(completion) has not finished yet. |



<a name="thebaasco.tenant.coretransfer.v1.TransferType"></a>

#### TransferType
TransferType enumeration indicates the mechanism of transfer used for a transaction.

| Name | Number | Description |
| ---- | ------ | ----------- |
| TRANSFER_TYPE_UNSPECIFIED | 0 | TRANSFER_TYPE_UNSPECIFIED should never be used directly. It is returned when a value has not been specified, and is present by convention, for error-detection purposes. |
| INTERNAL_ACCOUNT_TO_ACCOUNT | 1 | INTERNAL_ACCOUNT_TO_ACCOUNT indicates that the transfer was an Account to Account transfer that did not transit through external systems ( stayed within the institution ). |
| INTERAC_AUTODEPOSIT | 2 | INTERAC_AUTODEPOSIT indicates that the transfer came through Interac systems, and used the Interac Autodeposit mechanism. |


 <!-- end enums -->


### API-specific types

<a name="thebaasco.tenant.coretransfer.v1.Counterparty"></a>

#### Counterparty
Counterparty contains details about the person/entity on the other side of the transfer.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| legal_name | [string](#string) |  | Legal name of the person/entity on the other side of the transfer. |






<a name="thebaasco.tenant.coretransfer.v1.GetTransferDetailsRequest"></a>

#### GetTransferDetailsRequest
GetTransferDetailsRequest is a message passed as argument to the GetTransferDetails operation.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| transaction_id | [string](#string) |  | The ID of the transaction to get details for. Must be a valid UUID. Must be an existing transaction and the associated account must be owned by or authorized for the user In the case of Interac transfers that happen in two steps (authorization+confirmation), this field could be the transaction ID generated for one of those. Meaning that passing the transaction ID of the authorization step or the confirmation step, will return the corresponding transfer. |






<a name="thebaasco.tenant.coretransfer.v1.InitiateTransferRequest"></a>

#### InitiateTransferRequest
InitiateTransferRequest create a transfer fund request between accounts owned by current customer


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| source_account_id | [string](#string) |  | Unique identifier that represents the source account ID: - Current customer needs to be owner or authorized user of the source account. - Source account needs to have sufficient available balance and actual balance to be able to initiate the transfer. |
| destination_account_id | [string](#string) |  | Unique identifier that represents the destination account ID: - Current customer needs to be owner or authorized user of the destination account. |
| fund_transfer | [thebaasco.types.Amount](#thebaasco.types.Amount) |  | The amount of the fund transfer. The amount should be always positive. |






<a name="thebaasco.tenant.coretransfer.v1.InitiateTransferResponse"></a>

#### InitiateTransferResponse
InitiateTransferResponse is the response to InitiateTransferRequest. It contains the details of the transfer.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| details | [TransferTransactionDetails](#thebaasco.tenant.coretransfer.v1.TransferTransactionDetails) |  | Details of the transaction with its status (whether the transaction is COMPLETED or DECLINED. See TransferTransactionDetails for more details |
| reason | [string](#string) |  | Empty when status is COMPLETED. Reason why transfer is declined. |






<a name="thebaasco.tenant.coretransfer.v1.TransferTransactionDetails"></a>

#### TransferTransactionDetails
TransferTransactionDetails message contains details about a given transaction, relevant and specific to the Core-Transfer domain.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| transaction_id | [string](#string) |  | Unique identifier that represents this single financial transaction (operation). This ID is stable across all domains for a given transaction. |
| lifecycle_transaction_id | [string](#string) |  | Unique identifier that represents the overarching transaction, across all phases of it's lifecycle (e.g. authorization, settlement, reversal, ...). Note: there might be multiple unique "transaction_id" for a single "lifecycle_transaction_id". |
| status | [TransferStatus](#thebaasco.tenant.coretransfer.v1.TransferStatus) |  | The current status of the transfer. See TransferStatus enumeration for details on possible statuses. |
| customer_role | [CustomerRole](#thebaasco.tenant.coretransfer.v1.CustomerRole) |  | The role played by our customer in this transaction. See CustomerRole for details on possible roles. When transfer_type is INTERNAL_ACCOUNT_TO_ACCOUNT, this will always be INITATOR. |
| transfer_type | [TransferType](#thebaasco.tenant.coretransfer.v1.TransferType) |  | The type of transfer for this transaction. See TransferType for details on possible types. |
| counterparty | [Counterparty](#thebaasco.tenant.coretransfer.v1.Counterparty) |  | When transfer_type is INTERAC_AUTODEPOSIT, this will contain the details about the person/entity on the other side of this transfer. When transfer_type is INTERNAL_ACCOUNT_TO_ACCOUNT, this will contain the details of the owner of the destination account. |
| clearing_system_ref_number | [string](#string) |  | Unique identifier assigned to the transaction by the external payment/transfer clearing system When transfer_type is INTERAC_AUTODEPOSIT, it will be set to interac payment number. When transfer_type is INTERNAL_ACCOUNT_TO_ACCOUNT, it will be set to the transaction ID. |
| remittance | [RemittanceInformation](#thebaasco.tenant.coretransfer.v1.RemittanceInformation) |  | Includes remittance data from this transfer if it contains any |






<a name="thebaasco.tenant.coretransfer.v1.TransferTransactionDetailsCreatedEvent"></a>

#### TransferTransactionDetailsCreatedEvent
TransferTransactionDetailsCreatedEvent event is raised by the Core-Transfer domain when transactions are created and Core-Transfer specific details are available.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| event_id | [string](#string) |  | The unique event ID for this transaction. Can be used by consumers to determine if an event has already been processed or not. This ID will always be a valid UUID. |
| details | [TransferTransactionDetails](#thebaasco.tenant.coretransfer.v1.TransferTransactionDetails) |  | The transaction details for this event. |
| tenant_id | [int32](#int32) |  | The tenant ID, to identify the tenant on which the operation occurred. This can be used when processing events at the global scope to differentiate between tenants. |
| event_time | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  | The UTC timestamp when this event was issued. |





 <!-- end messages -->



## Shared types
<a name="thebaasco.types.Amount"></a>
### Amount
[Global types / Amount](../Global-types/amount/#amount_1) 

<a name="thebaasco.types.AppError"></a>
### AppError
[Global types / AppError](../Global-types/errordetails/#apperror)

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

