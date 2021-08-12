# Core Banking API

## API Specific Types

<a name="thebaasco.tenant.corebanking.v1.AccountCreatedEvent"></a>

### AccountCreatedEvent

AccountCreatedEvent is an event that is raised when a Core-Banking Account is successfully created, following a CreateAccountRequest.

| Field                 | Type                                                              | Label | Description                                                                                                                                                  |
| --------------------- | ----------------------------------------------------------------- | ----- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| event_id              | [string](#string)                                                 |       | The unique ID of this event. Can be used by consumers to determine if an event has already been processed or not. This ID will always be a valid UUID.       |
| account_details       | [AccountDetails](#thebaasco.tenant.corebanking.v1.AccountDetails) |       | The details of the created Account.                                                                                                                          |
| tenant_id             | [int32](#int32)                                                   |       | The ID to identify the tenant on which the operation occurred. This can be used when processing events at the global scope to differentiate between tenants. |
| account_creation_time | [google.protobuf.Timestamp](#google.protobuf.Timestamp)           |       | The UTC timestamp at which the Account was created.                                                                                                          |

<a name="thebaasco.tenant.corebanking.v1.AccountDetails"></a>

### AccountDetails

AccountDetails represents the details of a Core-Banking Account.

| Field                 | Type                                                            | Label    | Description                                                                                                                                                             |
| --------------------- | --------------------------------------------------------------- | -------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| account_id            | [string](#string)                                               |          | The ID of the Account. This will always be a valid UUID. Output only.                                                                                                   |
| status                | [AccountStatus](#thebaasco.tenant.corebanking.v1.AccountStatus) |          | The status of the Account. See documentation on AccountStatus enum for details on enum values. Output only.                                                             |
| owner_client_id       | [string](#string)                                               |          | The primary holder of the Account. This value comes from the Client-Profile domain. This will always be a list of valid UUID. Output only.                              |
| authorized_client_ids | [string](#string)                                               | repeated | The list of authorized users for the Account. This will always be a list of valid UUIDs. Output only.                                                                   |
| accrued_interest      | [thebaasco.types.Amount](#thebaasco.types.Amount)               |          | The interest that is accumulated daily since last applied, but hasn&#39;t been applied to the balance yet. Interest is applied to Account balance monthly. Output only. |

<a name="thebaasco.tenant.corebanking.v1.AccountStatusChangedEvent"></a>

### AccountStatusChangedEvent

AccountStatusChangedEvent is an event raised when an Account changes status.

| Field                       | Type                                                              | Label | Description                                                                                                                                                  |
| --------------------------- | ----------------------------------------------------------------- | ----- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| event_id                    | [string](#string)                                                 |       | The unique ID of this event. Can be used by consumers to determine if an event has already been processed or not. This ID will always be a valid UUID.       |
| account_details             | [AccountDetails](#thebaasco.tenant.corebanking.v1.AccountDetails) |       | The updated details for the Account.                                                                                                                         |
| tenant_id                   | [int32](#int32)                                                   |       | The ID to identify the tenant on which the operation occurred. This can be used when processing events at the global scope to differentiate between tenants. |
| account_status_changed_time | [google.protobuf.Timestamp](#google.protobuf.Timestamp)           |       | The UTC timestamp at which the Account status changed.                                                                                                       |

<a name="thebaasco.tenant.corebanking.v1.ChangeAccountStatusRequest"></a>

### ChangeAccountStatusRequest

ChangeAccountStatusRequest is used to perform the asynchronous operation of changing an Account&#39;s status. Performing this operation will generate an asynchronous response of type ChangeAccountStatusResponse, on the response topic.
Possible state transitions:

- From &#34;ACCOUNT_STATUS_INACTIVE&#34; to &#34;ACCOUNT_STATUS_ACTIVE&#34;

| Field      | Type                                                            | Label | Description                                                                                                               |
| ---------- | --------------------------------------------------------------- | ----- | ------------------------------------------------------------------------------------------------------------------------- |
| account_id | [string](#string)                                               |       | The ID of the Account to update. The value must be a valid UUID, and current user must have proper access to the Account. |
| status     | [AccountStatus](#thebaasco.tenant.corebanking.v1.AccountStatus) |       | The target status of the Account. See documentation on AccountStatus enum for details on enum values.                     |

<a name="thebaasco.tenant.corebanking.v1.ChangeAccountStatusResponse"></a>

### ChangeAccountStatusResponse

ChangeAccountStatusResponse message is returned when issuing a CreateAccountRequest. It contains the AccountDetails for the Account.

| Field           | Type                                                              | Label | Description                          |
| --------------- | ----------------------------------------------------------------- | ----- | ------------------------------------ |
| account_details | [AccountDetails](#thebaasco.tenant.corebanking.v1.AccountDetails) |       | The updated details for the Account. |

<a name="thebaasco.tenant.corebanking.v1.CreateAccountRequest"></a>

### CreateAccountRequest

CreateAccountRequest is used to perform the asynchronous operation of creating a new Account. Performing this operation will generate an asynchronous response of type CreateAccountResponse, on the response topic.

| Field              | Type                                                                                                                        | Label    | Description                                                                                                                                                                                                                                                      |
| ------------------ | --------------------------------------------------------------------------------------------------------------------------- | -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| account_type       | [string](#string)                                                                                                           |          | Required. The type of Account to open is a string that is composed in conjunction between product catalog and requestor domain (e.g. &#34;current_account.v39&#34;) An unknown or invalid Account type will generate an error with status code INVALID_ARGUMENT. |
| account_parameters | [CreateAccountRequest.AccountParametersEntry](#thebaasco.tenant.corebanking.v1.CreateAccountRequest.AccountParametersEntry) | repeated | Optional. The parameters for the Account type. The possible parameters and format is defined by the banking layer&#39;s Smart Contract.                                                                                                                          |

<a name="thebaasco.tenant.corebanking.v1.CreateAccountRequest.AccountParametersEntry"></a>

### CreateAccountRequest.AccountParametersEntry

| Field | Type              | Label | Description |
| ----- | ----------------- | ----- | ----------- |
| key   | [string](#string) |       |             |
| value | [string](#string) |       |             |

<a name="thebaasco.tenant.corebanking.v1.CreateAccountResponse"></a>

### CreateAccountResponse

CreateAccountResponse is the response to CreateAccountRequest. It contains the AccountDetails for the created Account.

| Field           | Type                                                              | Label | Description                          |
| --------------- | ----------------------------------------------------------------- | ----- | ------------------------------------ |
| account_details | [AccountDetails](#thebaasco.tenant.corebanking.v1.AccountDetails) |       | The details for the created Account. |

<a name="thebaasco.tenant.corebanking.v1.GetAccountDetailsRequest"></a>

### GetAccountDetailsRequest

GetAccountDetailsRequest is passed as parameter to the GetAccountDetails operation, in order to determine which Account to retrieve the details from.

| Field      | Type              | Label | Description                                                                                                                                |
| ---------- | ----------------- | ----- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| account_id | [string](#string) |       | The ID of the Account to retrieve. The value must be a valid UUID, and current user must have proper access to the Account to retrieve it. |

<a name="thebaasco.tenant.corebanking.v1.AccountStatus"></a>

### AccountStatus

AccountStatus is an enumeration indicating the status of an Account. All accounts are created in the ACCOUNT_STATUS_INACTIVE state by default.

| Name                       | Number | Description                                                                                                                                                               |
| -------------------------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ACCOUNT_STATUS_UNSPECIFIED | 0      | ACCOUNT_STATUS_UNSPECIFIED should never be used directly. It is returned when a value has not been specified, and is present by convention, for error-detection purposes. |
| ACCOUNT_STATUS_INACTIVE    | 1      | ACCOUNT_STATUS_INACTIVE represents an Account that is inactive. It is the initial state after creation. An Account that is inactive cannot process any transactions.      |
| ACCOUNT_STATUS_ACTIVE      | 2      | ACCOUNT_STATUS_ACTIVE represents an Account that is active. In this state normal operations can be processed on the Account.                                              |

<a name="thebaasco.tenant.corebanking.v1.Accounts"></a>

### Accounts

Accounts service exposes specific operations related to checking and saving accounts.
To retrieve general information about an Account, such as its balance, check the Core-Transaction api documentation.

| Method Name       | Request Type                                                                          | Response Type                                                     | Description                                                                                                                                                                                                                                                                                           |
| ----------------- | ------------------------------------------------------------------------------------- | ----------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| GetAccountDetails | [GetAccountDetailsRequest](#thebaasco.tenant.corebanking.v1.GetAccountDetailsRequest) | [AccountDetails](#thebaasco.tenant.corebanking.v1.AccountDetails) | GetAccountDetails retrieves details about an Account. Only users associated with the Account (owner or authorized users) will be able to invoke this endpoint. To list Accounts, or to get generic information about a specific Account, such as its balance, see Core-Transaction api documentation. |

<a name="shared_types"></a>

## Shared Types

<a name="thebaasco.types.Amount"></a>

### Amount

| Field    | Type              | Label | Description                                   |
| -------- | ----------------- | ----- | --------------------------------------------- |
| amount   | [string](#string) |       | Only accept decimal representation of a value |
| currency | [string](#string) |       |                                               |

## Scalar Value Types

| .proto Type                    | Notes                                                                                                                                           | C++    | Java       | Python      | Go      | C#         | PHP            | Ruby                           |
| ------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------- | ------ | ---------- | ----------- | ------- | ---------- | -------------- | ------------------------------ |
| <a name="double" /> double     |                                                                                                                                                 | double | double     | float       | float64 | double     | float          | Float                          |
| <a name="float" /> float       |                                                                                                                                                 | float  | float      | float       | float32 | float      | float          | Float                          |
| <a name="int32" /> int32       | Uses variable-length encoding. Inefficient for encoding negative numbers – if your field is likely to have negative values, use sint32 instead. | int32  | int        | int         | int32   | int        | integer        | Bignum or Fixnum (as required) |
| <a name="int64" /> int64       | Uses variable-length encoding. Inefficient for encoding negative numbers – if your field is likely to have negative values, use sint64 instead. | int64  | long       | int/long    | int64   | long       | integer/string | Bignum                         |
| <a name="uint32" /> uint32     | Uses variable-length encoding.                                                                                                                  | uint32 | int        | int/long    | uint32  | uint       | integer        | Bignum or Fixnum (as required) |
| <a name="uint64" /> uint64     | Uses variable-length encoding.                                                                                                                  | uint64 | long       | int/long    | uint64  | ulong      | integer/string | Bignum or Fixnum (as required) |
| <a name="sint32" /> sint32     | Uses variable-length encoding. Signed int value. These more efficiently encode negative numbers than regular int32s.                            | int32  | int        | int         | int32   | int        | integer        | Bignum or Fixnum (as required) |
| <a name="sint64" /> sint64     | Uses variable-length encoding. Signed int value. These more efficiently encode negative numbers than regular int64s.                            | int64  | long       | int/long    | int64   | long       | integer/string | Bignum                         |
| <a name="fixed32" /> fixed32   | Always four bytes. More efficient than uint32 if values are often greater than 2^28.                                                            | uint32 | int        | int         | uint32  | uint       | integer        | Bignum or Fixnum (as required) |
| <a name="fixed64" /> fixed64   | Always eight bytes. More efficient than uint64 if values are often greater than 2^56.                                                           | uint64 | long       | int/long    | uint64  | ulong      | integer/string | Bignum                         |
| <a name="sfixed32" /> sfixed32 | Always four bytes.                                                                                                                              | int32  | int        | int         | int32   | int        | integer        | Bignum or Fixnum (as required) |
| <a name="sfixed64" /> sfixed64 | Always eight bytes.                                                                                                                             | int64  | long       | int/long    | int64   | long       | integer/string | Bignum                         |
| <a name="bool" /> bool         |                                                                                                                                                 | bool   | boolean    | boolean     | bool    | bool       | boolean        | TrueClass/FalseClass           |
| <a name="string" /> string     | A string must always contain UTF-8 encoded or 7-bit ASCII text.                                                                                 | string | String     | str/unicode | string  | string     | string         | String (UTF-8)                 |
| <a name="bytes" /> bytes       | May contain any arbitrary sequence of bytes.                                                                                                    | string | ByteString | str         | []byte  | ByteString | string         | String (ASCII-8BIT)            |