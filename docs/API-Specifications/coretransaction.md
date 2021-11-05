# Core Transaction API
<a name="top"></a>



<a name="coretransaction_v1_accounts.proto"></a>

## coretransaction_v1_accounts.proto



### Services

<a name="thebaasco.tenant.coretransaction.v1.Accounts"></a>

#### Accounts
Accounts service exposes endpoints related to listing and retrieving Accounts.

##### ListAccounts

> **rpc** ListAccounts([ListAccountsRequest](#thebaasco.tenant.coretransaction.v1.ListAccountsRequest))
[ListAccountsResponse](#thebaasco.tenant.coretransaction.v1.ListAccountsResponse)

ListAccounts lists all accounts on which the current user is the owner, or an authorized user. This endpoint uses pagination to allow listing accounts in smaller, easy to manage, chunks.

##### GetAccount

> **rpc** GetAccount([GetAccountRequest](#thebaasco.tenant.coretransaction.v1.GetAccountRequest))
[Account](#thebaasco.tenant.coretransaction.v1.Account)

GetAccount retrieves an Account. The user must have proper access to the Account.

##### ListAuthorizedUsers

> **rpc** ListAuthorizedUsers([ListAuthorizedUsersRequest](#thebaasco.tenant.coretransaction.v1.ListAuthorizedUsersRequest))
[ListAuthorizedUsersResponse](#thebaasco.tenant.coretransaction.v1.ListAuthorizedUsersResponse)

ListAuthorizedUsers lists all authorized users of an account if the caller as the access rights.

##### ListOwners

> **rpc** ListOwners([ListOwnersRequest](#thebaasco.tenant.coretransaction.v1.ListOwnersRequest))
[ListOwnersResponse](#thebaasco.tenant.coretransaction.v1.ListOwnersResponse)

ListOwners lists all owners of an account if the caller as the access rights.


 <!-- end services -->




### API-specific types

<a name="thebaasco.tenant.coretransaction.v1.Account"></a>

#### Account
Account represents generic details about a given Account. This notion of Account is shared across all types of Accounts, from any domains ( e.g. Core-Banking, ... ).


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| id | [string](#string) |  | The unique ID of the Account. This ID can be used to retrieve the Account. This will always be a valid UUID. |
| balance | [thebaasco.types.Balance](#thebaasco.types.Balance) |  | The current balance of the Account. |
| name | [string](#string) |  | The display name of the Account. |
| source_domain | [string](#string) |  | The source domain which created the Account (eg: core-banking). |
| owners | [string](#string) | repeated | The owners of the Account. This will always be a list of valid UUID. |
| authorized_users | [string](#string) | repeated | The list authorized users of the Account. Each item in the list will be a valid UUID. |






<a name="thebaasco.tenant.coretransaction.v1.AddAuthorizedUserRequest"></a>

#### AddAuthorizedUserRequest
AddAuthorizedUserRequest is used to add an authorized user to the provided Account.
Performing this operation will generate an asynchronous response of type AddAuthorizedUserResponse, on the response topic.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| account_id | [string](#string) |  | The ID of the Account where the authorized is added. The value must be a valid UUID, and current user must be owner of the Account. |
| authorized_user | [string](#string) |  | The ID of the authorized user to add to the Account. The value must be a valid UUID, from an onboarded Customer. |






<a name="thebaasco.tenant.coretransaction.v1.AddAuthorizedUserResponse"></a>

#### AddAuthorizedUserResponse
AddAuthorizedUserResponse is the response to a AddAuthorizedUserRequest. It provides the Updated list of authorized users.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| authorized_users | [string](#string) | repeated | The resulting list of authorized users of the Account, including the newly added one. Each item in the list will be a valid UUID. |






<a name="thebaasco.tenant.coretransaction.v1.AddOwnerRequest"></a>

#### AddOwnerRequest
AddOwnerRequest is used to add an owner to the provided Account.
Performing this operation will generate an asynchronous response of type AddOwnerResponse, on the response topic.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| account_id | [string](#string) |  | The ID of the Account where the owner is added. The value must be a valid UUID, and current user must be owner of the Account. |
| owner | [string](#string) |  | The ID of the owner to add to the Account. The value must be a valid UUID, from an onboarded Customer. |






<a name="thebaasco.tenant.coretransaction.v1.AddOwnerResponse"></a>

#### AddOwnerResponse
AddOwnerResponse is the response to a AddOwnerRequest. It provides the Updated list of owners.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| owners | [string](#string) | repeated | The resulting list of owners of the Account, including the newly added one. Each item in the list will be a valid UUID. |






<a name="thebaasco.tenant.coretransaction.v1.AuthorizedUserAddedEvent"></a>

#### AuthorizedUserAddedEvent
AuthorizedUserAddedEvent is an event raised when adding an authorized user to an account.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| event_id | [string](#string) |  | The unique ID of this event. Can be used by consumers to determine if an event has already been processed or not. This ID will always be a valid UUID. |
| account_id | [string](#string) |  | The ID of the Account where the authorized user was added. This will be a valid UUID. |
| authorized_user_id | [string](#string) |  | The ID of the authorized user added to the Account. This will be a valid UUID. |
| tenant_id | [int32](#int32) |  | The tenant ID, to identify the tenant on which the operation occurred. This can be used when processing events at the global scope to differentiate between tenants. |
| event_time | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  | The UTC timestamp at which the authorized user was added to the Account. |






<a name="thebaasco.tenant.coretransaction.v1.AuthorizedUserRemovedEvent"></a>

#### AuthorizedUserRemovedEvent
AuthorizedUserRemovedEvent is an event raised when removing an authorized user from an account.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| event_id | [string](#string) |  | The unique ID of this event. Can be used by consumers to determine if an event has already been processed or not. This ID will always be a valid UUID. |
| account_id | [string](#string) |  | The ID of the Account where the authorized user was added. This will be a valid UUID. |
| authorized_user_id | [string](#string) |  | The ID of the authorized user added to the Account. This will be a valid UUID. |
| tenant_id | [int32](#int32) |  | The tenant ID, to identify the tenant on which the operation occurred. This can be used when processing events at the global scope to differentiate between tenants. |
| event_time | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  | The UTC timestamp at which the authorized user was added to the Account. |






<a name="thebaasco.tenant.coretransaction.v1.GetAccountRequest"></a>

#### GetAccountRequest
GetAccountRequest is a message passed as parameter to the GetAccount operation.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| account_id | [string](#string) |  | The ID of the Account to retrieve. The value must be a valid UUID, and current user must have proper access. |






<a name="thebaasco.tenant.coretransaction.v1.ListAccountsRequest"></a>

#### ListAccountsRequest
ListAccountsRequest is a message passed as parameter to the ListAccounts operation.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| page_size | [int32](#int32) |  | The maximum number of Accounts to retrieve as part of this request. This value needs to be superior, or equal to 1. |
| page_token | [string](#string) |  | When retrieving a page other than the initial page, the value for the page_token must be specified, and taken from the previous page's next_page_token value (see ListAccountsResponse). When no value is specified, then the initial page will be returned. |






<a name="thebaasco.tenant.coretransaction.v1.ListAccountsResponse"></a>

#### ListAccountsResponse
ListAccountsResponse is a message returned in response to the ListAccounts operation.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| accounts | [Account](#thebaasco.tenant.coretransaction.v1.Account) | repeated | The requested list of Accounts. |
| next_page_token | [string](#string) |  | The token to be used to retrieve the next page of data. Must be passed in the page_token field of the next request (see ListAccountsRequest). When no value is returned in this field, then no further data exists to be listed. |






<a name="thebaasco.tenant.coretransaction.v1.ListAuthorizedUsersRequest"></a>

#### ListAuthorizedUsersRequest
ListAuthorizedUsersRequest is a message passed as parameter to the ListAuthorizedUsers operation.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| account_id | [string](#string) |  | The ID of the Account for which the authorized users are to be listed. Must be a valid UUID. |






<a name="thebaasco.tenant.coretransaction.v1.ListAuthorizedUsersResponse"></a>

#### ListAuthorizedUsersResponse
ListAuthorizedUsersResponse is a message returned in response to the ListAuthorizedUsers operation.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| authorized_users | [string](#string) | repeated | The requested list of authorized users ids. Each item in the list will be a valid UUID. |






<a name="thebaasco.tenant.coretransaction.v1.ListOwnersRequest"></a>

#### ListOwnersRequest
ListOwnersRequest is a message passed as parameter to the ListOwners operation.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| account_id | [string](#string) |  | The ID of the Account for which the owners are to be listed. Must be a valid UUID. |






<a name="thebaasco.tenant.coretransaction.v1.ListOwnersResponse"></a>

#### ListOwnersResponse
ListOwnersResponse is a message returned in response to the ListOwners operation.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| owners | [string](#string) | repeated | The requested list of owners ids. Each item in the list will be a valid UUID. |






<a name="thebaasco.tenant.coretransaction.v1.OwnerAddedEvent"></a>

#### OwnerAddedEvent
OwnerAddedEvent is an event raised when adding an owner to an account.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| event_id | [string](#string) |  | The unique ID of this event. Can be used by consumers to determine if an event has already been processed or not. This ID will always be a valid UUID. |
| account_id | [string](#string) |  | The ID of the Account where the owner was added. This will be a valid UUID. |
| owner_id | [string](#string) |  | The ID of the owner added to the Account. This will be a valid UUID. |
| tenant_id | [int32](#int32) |  | The tenant ID, to identify the tenant on which the operation occurred. This can be used when processing events at the global scope to differentiate between tenants. |
| event_time | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  | The UTC timestamp at which the owner was added to the Account. |






<a name="thebaasco.tenant.coretransaction.v1.RemoveAuthorizedUserRequest"></a>

#### RemoveAuthorizedUserRequest
RemoveAuthorizedUserRequest is used to remove an authorized user from the provided Account.
Performing this operation will generate an asynchronous response of type RemoveAuthorizedUserResponse, on the response topic.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| account_id | [string](#string) |  | The ID of the Account where the authorized is removed from. The value must be a valid UUID, and current user must be owner of the Account. The current user must have remove:authorized_user scope |
| authorized_user | [string](#string) |  | The ID of the authorized user to add to the Account. The value must be a valid UUID, from an onboarded Customer. |






<a name="thebaasco.tenant.coretransaction.v1.RemoveAuthorizedUserResponse"></a>

#### RemoveAuthorizedUserResponse
RemoveAuthorizedUserResponse is the response to a RemoveAuthorizedUserRequest. It provides the Updated list of authorized users.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| authorized_users | [string](#string) | repeated | The resulting list of authorized users of the Account. Each item in the list will be a valid UUID. |





 <!-- end messages -->



<a name="coretransaction_v1_transactions.proto"></a>

## coretransaction_v1_transactions.proto



### Services

<a name="thebaasco.tenant.coretransaction.v1.Transactions"></a>

#### Transactions
Transactions service exposes operations related to listing and retrieving details for Transactions.

##### ListTransactions

> **rpc** ListTransactions([ListTransactionsRequest](#thebaasco.tenant.coretransaction.v1.ListTransactionsRequest))
[ListTransactionsResponse](#thebaasco.tenant.coretransaction.v1.ListTransactionsResponse)

ListTransactions lists Transactions associated to the Account specified using pagination. The current user must be an authorized user of the Account to invoke this endpoint.
Transactions are sorted in descending order of Transaction time. It is possible for this endpoint to return less than the number of items requested by the page_size field of
the ListTransactionsRequest parameters ( including returning a page containing 0 items ). It is important that consumers of this API use the next_page_token field of the ListTransactionsResponse object
to determine if more data is available or not, as such cases are not abnormal for this endpoint.


 <!-- end services -->




### API-specific types

<a name="thebaasco.tenant.coretransaction.v1.InternalAccountTransaction"></a>

#### InternalAccountTransaction
InternalAccountTransaction represents the elements common to any internal Transaction.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| transaction_id | [string](#string) |  | Unique identifier that represents this single financial transaction (operation). This ID is stable across all domains for a given transaction. |
| lifecycle_transaction_id | [string](#string) |  | Unique identifier that represents the overarching Transaction, across all phases of it's lifecycle (e.g. authorization, settlement, reversal, ...). Note: there might be multiple unique "transaction_id" for a single "lifecycle_transaction_id". |
| internal_account_id | [string](#string) |  | The UUID that correlates to the the Internal Account on which the Transaction occurred. |
| transaction_time | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  | The UTC timestamp at which the Transaction occurred. |
| amount | [thebaasco.types.Amount](#thebaasco.types.Amount) |  | The amount of the Transaction. If the amount is negative, then the transaction is a debit. If the amount if positive, the transaction is a credit. |
| initiating_domain | [string](#string) |  | The name of the domain that can be queried for additional details regarding this Transaction (e.g. core-card, core-transfer or core-banking) |






<a name="thebaasco.tenant.coretransaction.v1.InternalAccountTransactionCreatedEvent"></a>

#### InternalAccountTransactionCreatedEvent
InternalAccountTransactionCreatedEvent is an event raised when a Transaction occurs on a given Internal Account.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| event_id | [string](#string) |  | The unique ID of this event. Can be used by consumers to determine if an event has already been processed or not. This ID will always be a valid UUID. |
| transaction | [InternalAccountTransaction](#thebaasco.tenant.coretransaction.v1.InternalAccountTransaction) |  | The InternalAccountTransaction that caused the event. |
| tenant_id | [int32](#int32) |  | The tenant ID, to identify the tenant on which the operation occurred. This can be used when processing events at the global scope to differentiate between tenants. |
| event_time | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  | The UTC timestamp when this event was issued. |






<a name="thebaasco.tenant.coretransaction.v1.ListTransactionsRequest"></a>

#### ListTransactionsRequest
ListTransactionsRequest is a message passed as parameter to the ListTransactions operation. Refer to ListTransactions operation for more details.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| account_id | [string](#string) |  | The ID of the Account to list Transactions for. This value must be a valid UUID, and user must have proper access to the Account. |
| from | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  | The UTC timestamp from which to list Transactions. |
| to | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  | The UTC timestamp until which to list Transactions. |
| page_size | [int32](#int32) |  | The maximum number of Transactions to retrieve as part of this request. This value needs to be superior, or equal to 1. |
| page_token | [string](#string) |  | When retrieving page other than the initial page, the value for the page_token must be specified, and taken from the previous page's next_page_token value (see ListTransactionsResponse). When no value is specified, then the initial page will be returned. |






<a name="thebaasco.tenant.coretransaction.v1.ListTransactionsResponse"></a>

#### ListTransactionsResponse
ListTransactionsResponse is a message returned in response to the ListTransactions operation.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| transactions | [Transaction](#thebaasco.tenant.coretransaction.v1.Transaction) | repeated | The requested list of Transactions. |
| next_page_token | [string](#string) |  | The token to be used to retrieve the next page of data. Must be passed in the page_token field of the next request (see ListTransactionsRequest). When no value is returned in this field, then no further data exists to be listed. |






<a name="thebaasco.tenant.coretransaction.v1.Transaction"></a>

#### Transaction
Transaction represents the elements common to any Transaction.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| transaction_id | [string](#string) |  | Unique identifier that represents this single financial transaction (operation). This ID is stable across all domains for a given transaction. |
| lifecycle_transaction_id | [string](#string) |  | Unique identifier that represents the overarching Transaction, across all phases of it's lifecycle (e.g. authorization, settlement, reversal, ...). Note: there might be multiple unique "transaction_id" for a single "lifecycle_transaction_id". |
| account_id | [string](#string) |  | The ID of the Account on which the Transaction occurred. This will always be a valid UUID. |
| transaction_time | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  | The UTC timestamp at which the Transaction was inserted to the postings ledger |
| amount | [thebaasco.types.Amount](#thebaasco.types.Amount) |  | The amount of the Transaction. If the amount is negative, then the transaction is a debit. If the amount if positive, the transaction is a credit. |
| balance | [thebaasco.types.Balance](#thebaasco.types.Balance) |  | The resulting Balance of the Account after the Transaction has been processed. Warning: This value does not reflect the current Balance of the Account, just the balance following the transaction (since more recent Transactions might have occurred that also impacted the Account's balance) |
| account_domain | [string](#string) |  | The name of the domain that can be queried for additional details regarding the Account of this transaction (e.g. core-banking) |
| initiating_domain | [string](#string) |  | The name of the domain that can be queried for additional details regarding this Transaction (e.g. core-card, core-transfer or core-banking) |






<a name="thebaasco.tenant.coretransaction.v1.TransactionCreatedEvent"></a>

#### TransactionCreatedEvent
TransactionCreatedEvent is an event raised when a Transaction occurs on a given Account.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| event_id | [string](#string) |  | The unique ID of this event. Can be used by consumers to determine if an event has already been processed or not. This ID will always be a valid UUID. |
| transaction | [Transaction](#thebaasco.tenant.coretransaction.v1.Transaction) |  | The Transaction that caused the event. |
| tenant_id | [int32](#int32) |  | The tenant ID, to identify the tenant on which the operation occurred. This can be used when processing events at the global scope to differentiate between tenants. |
| event_time | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  | The UTC timestamp when this event was issued. |





 <!-- end messages -->



## Shared types

<a name="thebaasco.types.Amount"></a>
### Amount



[Global types / Amount](../Global-types/amount/#amount_1)


<a name="thebaasco.types.Balance"></a>

### Balance


[Global types / Balance](../Global-types/balance/#balance)

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

