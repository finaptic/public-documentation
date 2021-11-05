# Onboarding API
<a name="top"></a>



<a name="authorizedusers_v1.proto"></a>

## authorizedusers_v1.proto



### Services

<a name="thebaasco.tenant.onboarding.v1.AuthorizedUsers"></a>

#### AuthorizedUsers
AuthorizedUsers service provides a way to retrieve information on invitations sent or received to join an account as an authorized user.

##### GetInvitations

> **rpc** GetInvitations([GetAuthorizedUserInvitationsRequest](#thebaasco.tenant.onboarding.v1.GetAuthorizedUserInvitationsRequest))
[GetAuthorizedUserInvitationsResponse](#thebaasco.tenant.onboarding.v1.GetAuthorizedUserInvitationsResponse)

GetInvitations returns the list of invitations for the current user. This will include invitations issued by the current user, as well as invitations
where the current user is the invited customer. This operation always return all invitations, including invitations that are pending, expired, have been accepted, declined, require confirmation or that have already been completed.
Refer to GetAuthorizedUserInvitationsRequest and GetAuthorizedUserInvitationsResponse for details.


 <!-- end services -->



### Enums

<a name="thebaasco.tenant.onboarding.v1.AuthorizedUserInvitationStatus"></a>

#### AuthorizedUserInvitationStatus
AuthorizedUserInvitationStatus represents the state of the invitation for a customer to join an account as an Authorized User.

| Name | Number | Description |
| ---- | ------ | ----------- |
| AUTHORIZED_USER_INVITATION_STATUS_UNSPECIFIED | 0 | AUTHORIZED_USER_INVITATION_STATUS_UNSPECIFIED is reserved for error detection purposes. It should not be used directly. |
| PENDING | 1 | PENDING represents an invitation that is waiting for an accept or decline response from the invited user. |
| ACCEPTED | 2 | ACCEPTED represents an invitation that was accepted by the invited user, and pending completion by the issuer of the invitation. |
| DECLINED | 3 | DECLINED represents an invitation that was declined by the invited user. A declined invitation cannot be completed. |
| COMPLETED | 4 | COMPLETED represents an invitation that was successfully completed by the issuer of the invitation. In this status, the invited customer has successfully joined the account as an Authorized User. |
| EXPIRED | 5 | EXPIRED represents an invitation that wasn't accepted or declined within the allotted time. An expired invitation cannot be accepted or declined. |


 <!-- end enums -->


### API-specific types

<a name="thebaasco.tenant.onboarding.v1.AcceptAuthorizedUserInvitationRequest"></a>

#### AcceptAuthorizedUserInvitationRequest
AcceptAuthorizedUserInvitationRequest is used to perform the asynchronous
operation of accepting an invitation issued to the user to join an account as an authorized user.
Performing this operation will generate an asynchronous
response type of AcceptAuthorizedUserInvitationResponse on the response topic.

Errors:
- If the current user is not the user the invitation was issued to, then the operation will fail with a PERMISSION_DENIED status code,
  and provide a message indicating the reason for the conflict.
- If the invitation is not in the Pending status, the operation will fail with a FAILED_PRECONDITION status code,
  and provide a message indicating that status was invalid.
- If the invitation has expired, the operation will also fail with a FAILED_PRECONDITION status code,
  and provide a message indicating that invitation has expired.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| invitation_id | [string](#string) |  | The ID of the invitation to accept. This must be a valid UUID, and represent a valid invitation that was issued to the current user. |






<a name="thebaasco.tenant.onboarding.v1.AcceptAuthorizedUserInvitationResponse"></a>

#### AcceptAuthorizedUserInvitationResponse
AcceptAuthorizedUserInvitationResponse is the response message for AcceptAuthorizedUserInvitationRequest.
See AcceptAuthorizedUserInvitationRequest for details. Message intentionally left without fields.






<a name="thebaasco.tenant.onboarding.v1.AuthorizedUserInvitation"></a>

#### AuthorizedUserInvitation
AuthorizedUserInvitation is the message that provides information on invitations issued, or received by users.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| invitation_id | [string](#string) |  | The unique identifier of the invitation. This value is required to Accept, Decline, or Confirm invitations. This will always be a valid UUID. |
| issuer_customer_id | [string](#string) |  | The customer ID of the person that issued the invitation. This will always be a valid UUID. |
| invited_customer_id | [string](#string) |  | The customer ID of the person that was invited to join the account as an authorized user. This will always be a valid UUID. In the scenario where the identifier passed in the InviteAuthorizedUserRequest does not map to an existing customer ID, the value of this field will be generated randomly, and verified to not exist in system, as to prevent inferring that the identifier was invalid. |
| account_id | [string](#string) |  | The account ID for which an invitation was sent. This will always be a valid UUID. |
| status | [AuthorizedUserInvitationStatus](#thebaasco.tenant.onboarding.v1.AuthorizedUserInvitationStatus) |  | The current status of the invitation. Refer to AuthorizedUserInvitationStatus enumeration for details on statuses and meaning. |
| creation_time | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  | The time at which the invitation was issued. |
| expiration_time | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  | ExpirationTime is the time at which it will no longer be possible for the invited customer to Accept the invitation. |






<a name="thebaasco.tenant.onboarding.v1.AuthorizedUserInvitationAcceptedOrDeclinedEvent"></a>

#### AuthorizedUserInvitationAcceptedOrDeclinedEvent
AuthorizedUserInvitationAcceptedOrDeclinedEvent is the async event message sent when
an authorized user invitation is accepted or declined by the invited customer.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| event_id | [string](#string) |  | The unique ID of this event. Can be used by consumers to determine if an event has already been processed or not. This ID will always be a valid UUID. |
| invitation | [AuthorizedUserInvitation](#thebaasco.tenant.onboarding.v1.AuthorizedUserInvitation) |  | The details of the invitation that has been accepted or declined. |
| tenant_id | [int32](#int32) |  | The ID to identify the tenant on which the operation occurred. This can be used when processing events at the global scope to differentiate between tenants. |
| event_time | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  | The UTC timestamp when this event was issued. |






<a name="thebaasco.tenant.onboarding.v1.AuthorizedUserInvitationConfirmedEvent"></a>

#### AuthorizedUserInvitationConfirmedEvent
AuthorizedUserInvitationConfirmedEvent is the async event message sent when
an authorized user invitation is confirmed by the issuer.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| event_id | [string](#string) |  | The unique ID of this event. Can be used by consumers to determine if an event has already been processed or not. This ID will always be a valid UUID. |
| invitation | [AuthorizedUserInvitation](#thebaasco.tenant.onboarding.v1.AuthorizedUserInvitation) |  | The details of the invitation that has been confirmed. |
| tenant_id | [int32](#int32) |  | The ID to identify the tenant on which the operation occurred. This can be used when processing events at the global scope to differentiate between tenants. |
| event_time | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  | The UTC timestamp when this event was issued. |






<a name="thebaasco.tenant.onboarding.v1.AuthorizedUserInvitationIssuedEvent"></a>

#### AuthorizedUserInvitationIssuedEvent
AuthorizedUserInvitationIssuedEvent is the async event message sent when
an authorized user invitation is issued to a customer.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| event_id | [string](#string) |  | The unique ID of this event. Can be used by consumers to determine if an event has already been processed or not. This ID will always be a valid UUID. |
| invitation | [AuthorizedUserInvitation](#thebaasco.tenant.onboarding.v1.AuthorizedUserInvitation) |  | The details of the invitation that has been issued. |
| invited_user_id_spoofed | [bool](#bool) |  | A flag indicating if the InvitedCustomerId was spoofed as to prevent attacks to determine who are users of our system. If the value is true, then the InvitedCustomerId as part of the Invitation field can be ignored. |
| tenant_id | [int32](#int32) |  | The ID to identify the tenant on which the operation occurred. This can be used when processing events at the global scope to differentiate between tenants. |
| event_time | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  | The UTC timestamp when this event was issued. |






<a name="thebaasco.tenant.onboarding.v1.ConfirmAuthorizedUserInvitationRequest"></a>

#### ConfirmAuthorizedUserInvitationRequest
ConfirmAuthorizedUserInvitationRequest is used to perform the asynchronous
operation of confirming an invitation that was accepted by the invited customer.
This operation will result in the invited customer obtaining access to the account as
an authorized user. Performing this operation will generate an asynchronous
response type of ConfirmAuthorizedUserInvitationResponse on the response topic.

Errors:
- If the current user is not the issuer of the invitation, then the operation will fail with a PERMISSION_DENIED status code,
  and provide a message indicating the reason for the conflict.
- If the invitation is not in the Accepted status, the operation will fail with a FAILED_PRECONDITION status code,
  and provide a message indicating that status was invalid.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| invitation_id | [string](#string) |  | The ID of the invitation to confirm. This must be a valid UUID, and represent a valid invitation that was issued by the current user. |






<a name="thebaasco.tenant.onboarding.v1.ConfirmAuthorizedUserInvitationResponse"></a>

#### ConfirmAuthorizedUserInvitationResponse
ConfirmAuthorizedUserInvitationResponse is the response message for ConfirmAuthorizedUserInvitationRequest.
See ConfirmAuthorizedUserInvitationRequest for details. Message intentionally left without fields.






<a name="thebaasco.tenant.onboarding.v1.DeclineAuthorizedUserInvitationRequest"></a>

#### DeclineAuthorizedUserInvitationRequest
DeclineAuthorizedUserInvitationRequest is used to perform the asynchronous
operation of declining an invitation issued to the user to join an account as an authorized user.
Performing this operation will generate an asynchronous
response type of DeclineAuthorizedUserInvitationResponse on the response topic.

Errors:
- If the current user is not the user the invitation was issued to, then the operation will fail with a PERMISSION_DENIED status code,
  and provide a message indicating the reason for the conflict.
- If the invitation is not in the Pending status, the operation will fail with a FAILED_PRECONDITION status code,
  and provide a message indicating that status was invalid.
- If the invitation has expired, the operation will also fail with a FAILED_PRECONDITION status code,
  and provide a message indicating that invitation has expired.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| invitation_id | [string](#string) |  | The ID of the invitation to decline. This must be a valid UUID, and represent a valid invitation that was issued to the current user. |






<a name="thebaasco.tenant.onboarding.v1.DeclineAuthorizedUserInvitationResponse"></a>

#### DeclineAuthorizedUserInvitationResponse
DeclineAuthorizedUserInvitationResponse is the response message for DeclineAuthorizedUserInvitationRequest.
See DeclineAuthorizedUserInvitationRequest for details. Message intentionally left without fields.






<a name="thebaasco.tenant.onboarding.v1.GetAuthorizedUserInvitationsRequest"></a>

#### GetAuthorizedUserInvitationsRequest
GetAuthorizedUserInvitationsRequest has been created to allow expanding the functionalities of the GetInvitations() operation in the future, without introducing breaking change.
At this moment, there are no configurable options for this operation.

Voluntarily left empty. This structure will be expanded as new features and capabilities are added to the GetInvitations() operation.






<a name="thebaasco.tenant.onboarding.v1.GetAuthorizedUserInvitationsResponse"></a>

#### GetAuthorizedUserInvitationsResponse
GetAuthorizedUserInvitationsResponse is the response for the GetInvitations() operation. It will contain all invitations for the current user, independently of their status. This will include
invitations for which the current user is the issuer, as well as invitations for which the current user is the invited customer. If there are no invitations for the current user, then the response
will contain zero items.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| invitations | [AuthorizedUserInvitation](#thebaasco.tenant.onboarding.v1.AuthorizedUserInvitation) | repeated | The list of invitations for the current user. |






<a name="thebaasco.tenant.onboarding.v1.InviteAuthorizedUserRequest"></a>

#### InviteAuthorizedUserRequest
InviteAuthorizedUserRequest is used to perform the asynchronous
operation of inviting a user to join an account as an authorized user. Performing this operation will generate an asynchronous
response type of InviteAuthorizedUserResponse on the response topic.

Errors:
- If the customer mapping to this identifier is already an Owner or an Authorized User for the account, then the operation will fail with a ALREADY_EXISTS status code,
  and provide a message indicating the reason for the conflict.
- If there is already an invitation that was issued for this account,
  for the invited user, the operation will fail with an ALREADY_EXISTS status code, and provide a message indicating the reason for the conflict.
- If the customer issuing the request isn't an Owner, then the operation will fail with a PERMISSION_DENIED status code,
  and provide a message indicating the reason.
- If there is already an invitation that was issued for this account, for the invited customer, the operation will fail with a ALREADY_EXISTS status code,
  and provide a message indicating the reason for the conflict.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| invited_customer_identifier | [string](#string) |  | The identifier to be used to lookup for the invited customer. This can either the username, or the email address of the customer to invite. If the identifier does not map to any known user, the operation will succeed, if this occurs the invitation will expire automatically after a period of time, without having been accepted or declined. |
| account_id | [string](#string) |  | The ID of the account on which an authorized user is to be added. This must be a valid UUID, and represent an existing account. In addition, the customer issuing the request must be an Owner for the account. |






<a name="thebaasco.tenant.onboarding.v1.InviteAuthorizedUserResponse"></a>

#### InviteAuthorizedUserResponse
InviteAuthorizedUserResponse is the response message for InviteAuthorizedUserRequest.
See InviteAuthorizedUserRequest for details.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| invitation | [AuthorizedUserInvitation](#thebaasco.tenant.onboarding.v1.AuthorizedUserInvitation) |  | The resulting invitation created by the InviteAuthorizedUserRequest. See AuthorizedUserInvitation for details. |





 <!-- end messages -->



<a name="onboarding_v1.proto"></a>

## onboarding_v1.proto



### Services

<a name="thebaasco.tenant.onboarding.v1.Onboarding"></a>

#### Onboarding
Onboarding provides a way to upload documents and check the status of an
onboarding application.

##### GetApplicationStatus

> **rpc** GetApplicationStatus([GetApplicationStatusRequest](#thebaasco.tenant.onboarding.v1.GetApplicationStatusRequest))
[GetApplicationStatusResponse](#thebaasco.tenant.onboarding.v1.GetApplicationStatusResponse)

GetApplicationStatus gets the current status of a given onboarding application.
It provides a way to handle non-authenticated users' interrupted sessions.
See GetApplicationStatusRequest for details.

##### CreateFileUploadURL

> **rpc** CreateFileUploadURL([CreateFileUploadUrlRequest](#thebaasco.tenant.onboarding.v1.CreateFileUploadUrlRequest))
[CreateFileUploadUrlResponse](#thebaasco.tenant.onboarding.v1.CreateFileUploadUrlResponse)

CreateFileUploadUrl is used as an initial step for the upload of a customer KYC document.
1. The UI will call this operation to generate upload-ready url for each file needed.
2. The UI will perform the upload of each file.
3. The UI will call UploadValidateDocument including file_name of each document
   along with its corresponding meta-data
   The filename will be used to match the document's meta-data with the uploaded
   content in step 2


 <!-- end services -->




### API-specific types

<a name="thebaasco.tenant.onboarding.v1.AcceptApplicationRequest"></a>

#### AcceptApplicationRequest
AcceptApplicationRequest is used to perform the asynchronous
operation of accepting the onboarding application received from the
customer. Performing this operation will generate an asynchronous
response type of AcceptApplicationResponse on the response topic.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| onboarding_application_id | [string](#string) |  | The UUID of the onboarding application to accept. |






<a name="thebaasco.tenant.onboarding.v1.AcceptApplicationResponse"></a>

#### AcceptApplicationResponse
AcceptApplicationResponse is the response message for AcceptApplicationRequest.
See AcceptApplicationRequest for details.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| basic_details | [OnboardingOperationResponseDetails](#thebaasco.tenant.onboarding.v1.OnboardingOperationResponseDetails) |  | The basic details of the onboarding application response. See OnboardingOperationResponseDetails. |






<a name="thebaasco.tenant.onboarding.v1.AccountUsageDetails"></a>

#### AccountUsageDetails
AccountUsageDetails is a message for providing the source of funds and purpose for applying
for an Account.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| account_purpose | [string](#string) |  | The intended purpose of the Account. This value is required. The value stored here is one of the code values retrieved from the ReferenceDataService with data_type set to 'ACCOUNT_PURPOSE'. e.g. 'ACCOUNT_PURPOSE_EVERYDAY_TRANSACTIONS' or 'ACCOUNT_PURPOSE_TRAVELLING' |
| primary_source_of_funds | [string](#string) | repeated | A collection of the primary sources of funds for the Account. At least one value is required. The values stored here are a collection of the code values retrieved from the ReferenceDataService with data_type set to 'SOURCE_OF_FUNDS'. e.g. 'PRIMARY_SOURCE_OF_FUNDS_LOANS' or 'PRIMARY_SOURCE_OF_FUNDS_GIFTS' |
| authorized_third_party_usage | [bool](#bool) |  | Indicates wether or not the account will be used on behalf of third parties not otherwise identified as owners or authorized users. True means that account will be used on behalf of third parties while false means that account will not be used on behalf of third parties. |






<a name="thebaasco.tenant.onboarding.v1.Address"></a>

#### Address
Address is a message for providing a given Address for a customer.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| type | [string](#string) |  | The type of Address being represented. Must be either 'HOME' or 'WORK' (required). This field is validated based on the message this Address is used in. See OnboardingApplication and Employment for required values. |
| organization | [string](#string) |  | The organization name associated with this Address. It is optional and has a maximum of 300 characters. |
| line_1 | [string](#string) |  | The first line of the Address. This value is required and has a maximum of 300 characters. |
| line_2 | [string](#string) |  | The second line of the Address. This value is optional, and has a maximum of 300 characters. |
| line_3 | [string](#string) |  | The third line of the Address. This value is optional, and has a maximum of 300 characters. |
| line_4 | [string](#string) |  | The fourth line of the Address. This value is optional, and has a maximum of 300 characters. |
| line_5 | [string](#string) |  | The fifth line of the Address. This value is optional, and has a maximum of 300 characters. |
| line_6 | [string](#string) |  | The sixth line of the Address. This value is optional, and has a maximum of 300 characters. |
| line_7 | [string](#string) |  | The seventh line of the Address. This value is optional, and has a maximum of 300 characters. |
| line_8 | [string](#string) |  | The eighth line of the Address. This value is optional, and has a maximum of 300 characters. |
| locality | [string](#string) |  | The local sub administrative area such as the municipality or community name. In Canadian and US addresses, this corresponds to the city or town name. This value is optional and has a maximum of 150 characters. e.g. Toronto, Orlando. |
| dependent_locality | [string](#string) |  | The municipality or community name of the major postal region address. For Canadian and US addresses, this can remain empty. This value is optional and has a maximum of 150 characters. |
| double_dependent_locality | [string](#string) |  | The municipality or community name of the local postal region address. For Canadian and US addresses, this can remain empty. This value is optional and has a maximum of 150 characters. |
| administrative_area | [string](#string) |  | The name of the first level administrative area. This value is optional and has a maximum of 150 characters. e.g. For US this would be 'State' and Canada this would be 'Province' |
| sub_administrative_area | [string](#string) |  | The name of the second level administrative area. This value is optional and has a maximum of 150 characters. e.g. For a city use 'City', county use 'County', etc. |
| area | [string](#string) |  | The name of country first level administrative subdivision area in the address. For Canada, this is name of the province and for the US this is the name of the State. This field is required and has a maximum of 150 characters. e.g. 'Ontario' or 'New York'. |
| postal_code | [string](#string) |  | The local postal code for routing mail to the address and should contain the ZIP code for US addresses. This field is required and has a maximum of 150 characters. |
| country | [string](#string) |  | The name of the country for the Address. This value is required and has a maximum of 50 characters. |






<a name="thebaasco.tenant.onboarding.v1.CommunicationPreferenceDetails"></a>

#### CommunicationPreferenceDetails
CommunicationDetails is a message for providing communication preference details for a customer.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| customer_communication_language | [string](#string) |  | The language in which the customer prefers receiving his communications. The value stored here is one of the values retrieved from the ReferenceDataService with data_type set to 'LANGUAGE'. e.g. 'LANGUAGE_FR_CA' |
| customer_communication_pref | [string](#string) |  | The preferred communication method chosen by the customer. The value stored here is one of the code values retrieved from the ReferenceDataService with data_type set to 'COMMUNICATION_PREFERENCE'. e.g. 'COMMUNICATION_PREFERENCES_MOBILE' or 'COMMUNICATION_PREFERENCES_EMAIL' |






<a name="thebaasco.tenant.onboarding.v1.Consent"></a>

#### Consent
Consent is a message indicating the receipt of a requested customer Consent.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| consent_type | [string](#string) |  | The type of Consent received from the customer. This value is required. The value stored here is one of the code values retrieved from the ReferenceDataService with data_type set to 'CONSENT_TYPE'. e.g. ‘CONSENT_TYPE_TERMS_OF_USE_OF_APP’ |
| consent_timestamp | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  | The UTC timestamp when Consent was received from the customer. |
| document_name_and_version | [string](#string) |  | The name and version of the document in URL format that the customer consented to. |
| view_timestamp | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  | The UTC timestamp when the customer viewed the document. |






<a name="thebaasco.tenant.onboarding.v1.ContactDetails"></a>

#### ContactDetails
ContactDetails is a message for providing the contact details for a customer.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| phone_number | [string](#string) |  | The contact phone number. This value is optional and must be in a valid phone number format. |
| customer_email | [string](#string) |  | The contact email address. This value is required and must be in a valid email address format. |






<a name="thebaasco.tenant.onboarding.v1.CreateFileUploadUrlRequest"></a>

#### CreateFileUploadUrlRequest
CreateFileUploadUrlRequest is the parameter to pass when calling CreateFileUploadURL.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| onboarding_application_id | [string](#string) |  | The UUID representing the onboarding application that requested a file upload URL. |






<a name="thebaasco.tenant.onboarding.v1.CreateFileUploadUrlResponse"></a>

#### CreateFileUploadUrlResponse
CreateFileUploadUrlResponse is the response returned by the CreateFileUploadURL


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| filename | [string](#string) |  | The generated name for the file to upload. |
| upload_url | [string](#string) |  | The URL to use to upload the file. |






<a name="thebaasco.tenant.onboarding.v1.CustomerResidency"></a>

#### CustomerResidency
CustomerResidency contains the residency status details of the customer.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| is_canadian_tax_resident | [bool](#bool) |  | The Canadian residency status. This is true if the customer is a Canadian resident, false otherwise. |
| other_residences | [OtherResidence](#thebaasco.tenant.onboarding.v1.OtherResidence) | repeated | A collection of other countries where the customer has residence. At least one is required if not a Canadian resident. |
| sin_token | [string](#string) |  | A tokenized UUID representation of the customer SIN number. |
| citizenship | [string](#string) |  | The primary or current citizenship of the customer. The format is an ISO 3166 Country alpha-2 code (e.g. US, CA, FR). |






<a name="thebaasco.tenant.onboarding.v1.Disclosure"></a>

#### Disclosure
Disclosure is a message indicating customer receipt of a documented Disclosure.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| disclosure_type | [string](#string) |  | The type of Disclosure sent to the customer. This value is required. The value stored here is one of the code values retrieved from the ReferenceDataService with data_type set to 'DISCLOSURE_TYPE'. e.g. 'DISCLOSURE_TYPE_DISCLOSURES_DURING_ISSUANCE_OF_PREPAID_CARD' |
| timestamp | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  | The UTC timestamp when the customer viewed the document. |
| document_name_and_version | [string](#string) |  | Name and version of the disclosure document in URL format that the customer viewed. |






<a name="thebaasco.tenant.onboarding.v1.DocumentInfo"></a>

#### DocumentInfo
DocumentInfo is a message containing the details of an onboarding document
collected from the customer to prove identity.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| document_type | [string](#string) |  | The type of document used for identification. The value stored here is one of the code values retrieved from the ReferenceDataService with data_type set to 'DOCUMENT_TYPE'. e.g. 'DOCUMENT_TYPE_CANADIAN_DRIVERS_LICENSE' or 'DOCUMENT_TYPE_CANADIAN_PASSPORT' |
| document_country | [string](#string) |  | The country the document was issued for. |
| front_image_file_name | [string](#string) |  | The cloud storage file nme of the image taken of the front of the document. (required, file size <4MB) |
| back_image_file_name | [string](#string) |  | The cloud storage file nme of the image taken of the back of the document. (file size <4MB) |






<a name="thebaasco.tenant.onboarding.v1.Employment"></a>

#### Employment
Employment defines the details of Employment for the customer being onboarded.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| employment_status | [string](#string) |  | The status of Employment for this entry. The value stored here is one of the code values retrieved from the ReferenceDataService with data_type set to 'EMPLOYMENT_STATUS'. e.g. EMPLOYMENT_STATUS_EMPLOYED or EMPLOYMENT_STATUS_RETIRED. |
| employer_name | [string](#string) |  | The full employer name. The value is required and has a maximum of 50 characters. |
| industry | [string](#string) |  | The industry code the Employment is in. This value is required and has a maximum of 50 characters. The value stored here is one of the code values retrieved from the ReferenceDataService with data_type set to 'INDUSTRY'. e.g. '236110' for Residential Construction. |
| occupation | [string](#string) |  | The roles and duties the customer performs for the employer. This value is optional and has a maximum of 50 characters. The value stored here is one of the code values retrieved from the ReferenceDataService with data_type set to 'OCCUPATION'. If the employment status is 'EMPLOYMENT_STATUS_UNEMPLOYED', 'EMPLOYMENT_STATUS_RETIRED', 'EMPLOYMENT_STATUS_STUDENT_UNEMPLOYED', or 'EMPLOYMENT_STATUS_HOMEMAKER' then the Occupation field must be left empty. e.g. 'OCCUPATION_8255_CONTRACTOR_AND_SUPERVISOR_LANDSCAPING' |
| startDate | [google.type.Date](#google.type.Date) |  | The month and year the customer started this position. |
| endDate | [google.type.Date](#google.type.Date) |  | The month and year the customer no longer held this position. |
| work_address | [Address](#thebaasco.tenant.onboarding.v1.Address) |  | The Address of the employer. The 'type' field must be set to 'WORK'. Required. |
| work_phone_number | [string](#string) |  | The phone number of the employer. Required. |






<a name="thebaasco.tenant.onboarding.v1.FinalizeApplicationCreateCustomerRequest"></a>

#### FinalizeApplicationCreateCustomerRequest
FinalizeApplicationCreateCustomerRequest is used to perform the asynchronous
operation of creating the customer for the onboarding application. Performing
this operation will generate an asynchronous response type of
FinalizeApplicationCreateCustomerResponse on the response topic.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| onboarding_application_id | [string](#string) |  | The UUID of the onboarding application to create a customer for. |






<a name="thebaasco.tenant.onboarding.v1.FinalizeApplicationCreateCustomerResponse"></a>

#### FinalizeApplicationCreateCustomerResponse
FinalizeApplicationCreateCustomerResponse is the response message for
FinalizeApplicationCreateCustomerRequest.
See FinalizeApplicationCreateCustomerRequest for details.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| basic_details | [OnboardingOperationResponseDetails](#thebaasco.tenant.onboarding.v1.OnboardingOperationResponseDetails) |  | The basic details of the onboarding application response. See OnboardingOperationResponseDetails. |
| customer_id | [string](#string) |  | The UUID representing the newly created customer. |






<a name="thebaasco.tenant.onboarding.v1.FinalizeApplicationCreateProductRequest"></a>

#### FinalizeApplicationCreateProductRequest
FinalizeApplicationCreateProductRequest is used to perform the asynchronous
operation of creating the product for the onboarding application. Performing
this operation will generate an asynchronous response type of
FinalizeApplicationCreateProductResponse on the response topic.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| onboarding_application_id | [string](#string) |  | The UUID of the onboarding application to create the product for. |






<a name="thebaasco.tenant.onboarding.v1.FinalizeApplicationCreateProductResponse"></a>

#### FinalizeApplicationCreateProductResponse
FinalizeApplicationCreateProductResponse is the response message for
FinalizeApplicationCreateProductRequest.
See FinalizeApplicationCreateProductRequest for details.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| basic_details | [OnboardingOperationResponseDetails](#thebaasco.tenant.onboarding.v1.OnboardingOperationResponseDetails) |  | The basic details of the onboarding application response. See OnboardingOperationResponseDetails. |
| products | [OnboardedProduct](#thebaasco.tenant.onboarding.v1.OnboardedProduct) | repeated | The collection of products created. See OnboardedProduct. list of products will be empty for onboarding application without product associated |






<a name="thebaasco.tenant.onboarding.v1.GetApplicationStatusRequest"></a>

#### GetApplicationStatusRequest
GetApplicationStatusRequest is used to retrieve the current status of
the onboarding application.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| application_id | [string](#string) |  | The UUID of the onboarding application to get the status of. |






<a name="thebaasco.tenant.onboarding.v1.GetApplicationStatusResponse"></a>

#### GetApplicationStatusResponse
GetApplicationStatusResponse is the response message for
GetApplicationStatusRequest. See GetApplicationStatusRequest for details.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| basic_details | [OnboardingOperationResponseDetails](#thebaasco.tenant.onboarding.v1.OnboardingOperationResponseDetails) |  | The basic details of the onboarding application response. See OnboardingOperationResponseDetails. |






<a name="thebaasco.tenant.onboarding.v1.IdentificationDocument"></a>

#### IdentificationDocument
IdentificationDocument contains information about the ID document provided during application process.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| id_number | [thebaasco.types.ProtectedString](#thebaasco.types.ProtectedString) |  | The number of the document used to identify the applicant, tokenized before being produced. |
| expiry_date | [thebaasco.types.ProtectedDate](#thebaasco.types.ProtectedDate) |  | The expiry date of the document used to identify the applicant, tokenized before being produced. If expiration date could not be retrieved from the provided ID document, this field will be absent. |
| document_type | [string](#string) |  | The type of document used for identification. The value stored here is one of the code values retrieved from the ReferenceDataService with data_type set to 'DOCUMENT_TYPE'. e.g. 'DOCUMENT_TYPE_CANADIAN_DRIVERS_LICENSE' or 'DOCUMENT_TYPE_CANADIAN_PASSPORT' |
| document_country | [string](#string) |  | The country issuing the document used to identify the applicant. The value stored here is one of the code values retrieved from the ReferenceDataService with data_type set to 'COUNTRY'. e.g. 'CA' |






<a name="thebaasco.tenant.onboarding.v1.InitiateApplicationRequest"></a>

#### InitiateApplicationRequest
InitiateApplicationRequest is used to perform the asynchronous
operation of starting the onboarding application.
Performing this operation will generate an asynchronous response
type of InitiateApplicationResponse on the response topic.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| bundle_id | [string](#string) |  | The unique identifier of the bundle being applied for. |






<a name="thebaasco.tenant.onboarding.v1.InitiateApplicationResponse"></a>

#### InitiateApplicationResponse
InitiateApplicationResponse is the response message for
InitiateApplicationRequest.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| basic_details | [OnboardingOperationResponseDetails](#thebaasco.tenant.onboarding.v1.OnboardingOperationResponseDetails) |  | The basic details of the onboarding application response. See OnboardingOperationResponseDetails. |
| is_kyc_needed | [bool](#bool) |  | If true validation of customer data (KYC) is needed as part of this onboarding application. If false, customer data has already been validated, and needs to be bypassed ( go straight to AcceptApplicationRequest). |






<a name="thebaasco.tenant.onboarding.v1.OnboardedProduct"></a>

#### OnboardedProduct
OnboardedProduct is the message representing a single product in the
onboarding application.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| product_id | [string](#string) |  | The UUID of the onboarded product. |
| source_domain | [string](#string) |  | The source domain of the onboarded product (eg: core-banking). |






<a name="thebaasco.tenant.onboarding.v1.OnboardingApplication"></a>

#### OnboardingApplication
OnboardingApplication is the message containing all of the onboarding details
collected from a customer required to apply for a product.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| application_id | [string](#string) |  | The UUID representing a given application. |
| customer_id | [string](#string) |  | The UUID representing the onboarding customer. |
| bundle_id | [string](#string) |  | The unique identifier of the bundled products being applied for. |
| status | [string](#string) |  | The application status. The value here is one of the following. "UNDEFINED" is reserved for error detection purposes. It should not be used directly. "New Application" describes an Application that has just been created. "Data Collection" describes an Application that has successfully performed an Update call or failed a ValidateSelfieRequest or a ValidateDocumentsRequest. "KYC In Progress" describes an Application that has successfully performed a ValidateSelfieRequest or ValidateDocumentsRequest. "KYC Success" describes an Application that has successfully performed KYC checks as a result of a ValidateApplicationRequest. "KYC Failed" describes an Application that has failed KYC as a result of a ValidateApplicationRequest. "KYC Rejected" describes an Application that has no remaining KYC attempts. "Application Accepted" describes an application that has successfully completed an AcceptApplicationRequest. "Application Processed" describes an application that has been successfully completed. "Application Cancelled" describes an application that has been cancelled by the customer. "Application Rejected" describes an application that has been rejected and cannot be continued. This is used when a more descriptive rejection status is not available. |
| customer_personal_details | [PersonalDetails](#thebaasco.tenant.onboarding.v1.PersonalDetails) |  | The personal details about the customer. See PersonalDetails. |
| customer_contact_details | [ContactDetails](#thebaasco.tenant.onboarding.v1.ContactDetails) |  | The contact details about the customer. See ContactDetails. |
| account_details | [AccountUsageDetails](#thebaasco.tenant.onboarding.v1.AccountUsageDetails) |  | The Account usage intentions of the customer. See AccountUsageDetails. |
| customer_employment | [Employment](#thebaasco.tenant.onboarding.v1.Employment) | repeated | The employment history for the customer. See Employment. |
| customer_residency | [CustomerResidency](#thebaasco.tenant.onboarding.v1.CustomerResidency) |  | The residency status of the customer. See CustomerResidency. |
| consents | [Consent](#thebaasco.tenant.onboarding.v1.Consent) | repeated | The collection of consents received from the customer. See Consent. |
| disclosures | [Disclosure](#thebaasco.tenant.onboarding.v1.Disclosure) | repeated | The collection of disclosures received from the customer. See Disclosure. |
| customer_residence_address | [Address](#thebaasco.tenant.onboarding.v1.Address) | repeated | The collection of resident addresses owned/occupied by the customer. See Address. |
| customer_identification_documents | [DocumentInfo](#thebaasco.tenant.onboarding.v1.DocumentInfo) | repeated | The collection of documents collected to identify the customer in the KYC process. |
| customer_communication_preference | [CommunicationPreferenceDetails](#thebaasco.tenant.onboarding.v1.CommunicationPreferenceDetails) |  | The communication preference details about the customer. See CommunicationPreferenceDetails. |






<a name="thebaasco.tenant.onboarding.v1.OnboardingApplicationCustomerOnboardedEvent"></a>

#### OnboardingApplicationCustomerOnboardedEvent
OnboardingApplicationCustomerOnboardedEvent is an event raised when a
customer is created.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| event_id | [string](#string) |  | The unique ID of this event. Can be used by consumers to determine if an event has already been processed or not. This ID will always be a valid UUID. |
| application_id | [string](#string) |  | The UUID of the onboarding application connected to this event. |
| tenant_id | [int32](#int32) |  | The ID to identify the tenant on which the operation occurred. This can be used when processing events at the global scope to differentiate between tenants. |
| event_time | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  | The UTC timestamp when this event was issued. |






<a name="thebaasco.tenant.onboarding.v1.OnboardingApplicationDataConfirmedEvent"></a>

#### OnboardingApplicationDataConfirmedEvent
OnboardingApplicationDataConfirmedEvent is the async event message sent when
a new customer is onboarded. As part of PB-3330, this event will also occur
when onboarding a new product for an existing customer.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| event_id | [string](#string) |  | The unique ID of this event. Can be used by consumers to determine if an event has already been processed or not. This ID will always be a valid UUID. |
| application | [OnboardingApplication](#thebaasco.tenant.onboarding.v1.OnboardingApplication) |  | The complete onboarding application for the onboarded customer. See OnboardingApplication. |
| tenant_id | [int32](#int32) |  | The ID to identify the tenant on which the operation occurred. This can be used when processing events at the global scope to differentiate between tenants. |
| event_time | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  | The UTC timestamp when this event was issued. |
| id_document | [IdentificationDocument](#thebaasco.tenant.onboarding.v1.IdentificationDocument) | repeated | The de-identified information about the provided ID document if the application has one |






<a name="thebaasco.tenant.onboarding.v1.OnboardingApplicationProductOnboardedEvent"></a>

#### OnboardingApplicationProductOnboardedEvent
OnboardingApplicationProductOnboardedEvent is the async event message sent
when a product is created with the FinalizeApplicationCreateProductRequest.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| event_id | [string](#string) |  | The unique ID of this event. Can be used by consumers to determine if an event has already been processed or not. This ID will always be a valid UUID. |
| application_id | [string](#string) |  | The UUID of the onboarding application connected to this event. |
| products | [OnboardedProduct](#thebaasco.tenant.onboarding.v1.OnboardedProduct) | repeated | The collection of products onboarded. See OnboardedProduct. |
| tenant_id | [int32](#int32) |  | The ID to identify the tenant on which the operation occurred. This can be used when processing events at the global scope to differentiate between tenants. |
| event_time | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  | The UTC timestamp when this event was issued. |






<a name="thebaasco.tenant.onboarding.v1.OnboardingOperationResponseDetails"></a>

#### OnboardingOperationResponseDetails
OnboardingOperationResponseDetails are the common response fields for an
onboarding application create or update message.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| application_id | [string](#string) |  | The UUID of the application the response is for. |
| onboarding_application_status | [string](#string) |  | The updated application status. The value here is one of the following. "UNDEFINED" is reserved for error detection purposes. It should not be used directly. "New Application" describes an Application that has just been created. "Data Collection" describes an Application that has successfully performed an Update call or failed a ValidateSelfieRequest or a ValidateDocumentsRequest. "KYC In Progress" describes an Application that has successfully performed a ValidateSelfieRequest or ValidateDocumentsRequest. "KYC Success" describes an Application that has successfully performed KYC checks as a result of a ValidateApplicationRequest. "KYC Failed" describes an Application that has failed KYC as a result of a ValidateApplicationRequest. "KYC Rejected" describes an Application that has no remaining KYC attempts. "Application Accepted" describes an application that has successfully completed an AcceptApplicationRequest. "Application Processed" describes an application that has been successfully completed. "Application Cancelled" describes an application that has been cancelled by the customer. "Application Rejected" describes an application that has been rejected and cannot be continued. This is used when a more descriptive rejection status is not available. |
| request_status | [string](#string) |  | The success status of the request. |
| request_status_reason | [string](#string) |  | **Deprecated.** The detailed reason for the request status. Deprecated: Use request_reason_codes instead. |
| is_retry_operation_restricted | [bool](#bool) |  | This is for internal use only. Do not use. |
| remaining_allowed_retries | [int64](#int64) |  | The remaining KYC retries allowed. |
| failure_status_codes | [string](#string) | repeated | The list of codes explaining the reason of the request outcome. This list will only be populated when request status is "KYC Failed", "KYC Rejected", or "Application Rejected". The list will otherwise be empty. The value stored here is one of the code values retrieved from the ReferenceDataService with data_type set to 'FAILURE_STATUS'. |






<a name="thebaasco.tenant.onboarding.v1.OtherResidence"></a>

#### OtherResidence
OtherResidence is a message for providing alternate countries and places of residence.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| other_residence_country | [string](#string) |  | The name of the other country of residence. |
| tin_token | [string](#string) |  | A tokenized UUID representation of the unique TIN for the country of residence. Optional. |
| citizenship | [string](#string) |  | The citizenship status in the country of residence. Required if residence country is US or Canada. |






<a name="thebaasco.tenant.onboarding.v1.PersonalDetails"></a>

#### PersonalDetails
PersonalDetails defines a message containing the customer's personal details.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| first_name | [string](#string) |  | The customer first name. This value is required and has a maximum of 50 characters. Accepted characters: -, ', French accents, 0-9. |
| middle_name | [string](#string) |  | The customer middle name. This value is optional and has a maximum of 50 characters. Accepted characters: -, ', French accents, 0-9. |
| last_name | [string](#string) |  | The customer last name. This value is required and has a maximum of 50 characters. Accepted characters: -, ', French accents, 0-9. |
| date_of_birth | [google.type.Date](#google.type.Date) |  | The customer's date of birth. |






<a name="thebaasco.tenant.onboarding.v1.UpdateAccountDetailsRequest"></a>

#### UpdateAccountDetailsRequest
UpdateAccountDetailsRequest is used to perform the asynchronous
operation of updating the customer account details in the onboarding application.
Performing this operation will generate an asynchronous response
type of UpdateAccountDetailsResponse on the response topic.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| onboarding_application_id | [string](#string) |  | The UUID of the onboarding application to update. |
| account_details | [AccountUsageDetails](#thebaasco.tenant.onboarding.v1.AccountUsageDetails) |  | The updated account usage details. See AccountUsageDetails. |






<a name="thebaasco.tenant.onboarding.v1.UpdateAccountDetailsResponse"></a>

#### UpdateAccountDetailsResponse
UpdateAccountDetailsResponse is the response message for UpdateAccountDetailsRequest.
See UpdateAccountDetailsRequest for details.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| basic_details | [OnboardingOperationResponseDetails](#thebaasco.tenant.onboarding.v1.OnboardingOperationResponseDetails) |  | The basic details of the onboarding application response. See OnboardingOperationResponseDetails. |






<a name="thebaasco.tenant.onboarding.v1.UpdateAddressesRequest"></a>

#### UpdateAddressesRequest
UpdateAddressesRequest is used to perform the asynchronous
operation of updating the addresses occupied by the customer in the
onboarding application. Performing this operation will generate an asynchronous
response type of UpdateAddressesResponse on the response topic.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| onboarding_application_id | [string](#string) |  | The UUID of the onboarding application to update. |
| addresses | [Address](#thebaasco.tenant.onboarding.v1.Address) | repeated | The collection of addresses occupied by the customer. See Address. |






<a name="thebaasco.tenant.onboarding.v1.UpdateAddressesResponse"></a>

#### UpdateAddressesResponse
UpdateAddressesResponse is the response message for UpdateAddressesRequest.
See UpdateAddressesRequest for details.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| basic_details | [OnboardingOperationResponseDetails](#thebaasco.tenant.onboarding.v1.OnboardingOperationResponseDetails) |  | The basic details of the onboarding application response. See OnboardingOperationResponseDetails. |






<a name="thebaasco.tenant.onboarding.v1.UpdateCommunicationPreferencesRequest"></a>

#### UpdateCommunicationPreferencesRequest
UpdateCommunicationPreferencesRequest is used to perform the asynchronous
operation of updating the communication preferences of the customer in the
onboarding application. Performing this operation will generate an asynchronous
response type of UpdateCommunicationPreferencesResponse on the response topic.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| onboarding_application_id | [string](#string) |  | The UUID of the onboarding application to update. |
| customer_communication_pref | [string](#string) |  | The customer communication preference. See CommunicationPrefenceDetails. |
| customer_communication_language | [string](#string) |  | The customer communication language. See CommunicationPrefenceDetails. |






<a name="thebaasco.tenant.onboarding.v1.UpdateCommunicationPreferencesResponse"></a>

#### UpdateCommunicationPreferencesResponse
UpdateCommunicationPreferencesResponse is the response message for
UpdateCommunicationPreferencesRequest.
See UpdateCommunicationPreferencesRequest for details.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| basic_details | [OnboardingOperationResponseDetails](#thebaasco.tenant.onboarding.v1.OnboardingOperationResponseDetails) |  | The basic details of the onboarding application response. See OnboardingOperationResponseDetails. |






<a name="thebaasco.tenant.onboarding.v1.UpdateConsentsRequest"></a>

#### UpdateConsentsRequest
UpdateConsentsRequest is used to perform the asynchronous
operation of updating the consents received from the customer for the
onboarding application. Performing this operation will generate an asynchronous
response type of UpdateConsentsResponse on the response topic.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| onboarding_application_id | [string](#string) |  | The UUID of the onboarding application to update. |
| consents | [Consent](#thebaasco.tenant.onboarding.v1.Consent) | repeated | The updated collection of consents. See Consent. |






<a name="thebaasco.tenant.onboarding.v1.UpdateConsentsResponse"></a>

#### UpdateConsentsResponse
UpdateConsentsResponse is the response message for UpdateConsentsRequest.
See UpdateConsentsRequest for details.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| basic_details | [OnboardingOperationResponseDetails](#thebaasco.tenant.onboarding.v1.OnboardingOperationResponseDetails) |  | The basic details of the onboarding application response. See OnboardingOperationResponseDetails. |






<a name="thebaasco.tenant.onboarding.v1.UpdateContactDetailsRequest"></a>

#### UpdateContactDetailsRequest
UpdateContactDetailsRequest is used to perform the asynchronous
operation of updating the customer contact details in the onboarding application.
Performing this operation will generate an asynchronous response
type of UpdateContactDetailsResponse on the response topic.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| onboarding_application_id | [string](#string) |  | The UUID of the onboarding application to update. |
| customer_contact_details | [ContactDetails](#thebaasco.tenant.onboarding.v1.ContactDetails) |  | The updated contact details. See ContactDetails. |






<a name="thebaasco.tenant.onboarding.v1.UpdateContactDetailsResponse"></a>

#### UpdateContactDetailsResponse
UpdateContactDetailsResponse is the response message for UpdateContactDetailsRequest.
See UpdateContactDetailsRequest for details.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| basic_details | [OnboardingOperationResponseDetails](#thebaasco.tenant.onboarding.v1.OnboardingOperationResponseDetails) |  | The basic details of the onboarding application response. See OnboardingOperationResponseDetails. |






<a name="thebaasco.tenant.onboarding.v1.UpdateCustomerResidencyRequest"></a>

#### UpdateCustomerResidencyRequest
UpdateCustomerResidencyRequest is used to perform the asynchronous
operation of updating the customer residency status in the onboarding application.
Performing this operation will generate an asynchronous response
type of UpdateCustomerResidencyResponse on the response topic.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| onboarding_application_id | [string](#string) |  | The UUID of the onboarding application to update. |
| residency | [CustomerResidency](#thebaasco.tenant.onboarding.v1.CustomerResidency) |  | The customer residency status. See CustomerResidency. |






<a name="thebaasco.tenant.onboarding.v1.UpdateCustomerResidencyResponse"></a>

#### UpdateCustomerResidencyResponse
UpdateCustomerResidencyResponse is the response message for
UpdateCustomerResidencyRequest. See UpdateCustomerResidencyRequest for details.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| basic_details | [OnboardingOperationResponseDetails](#thebaasco.tenant.onboarding.v1.OnboardingOperationResponseDetails) |  | The basic details of the onboarding application response. See OnboardingOperationResponseDetails. |






<a name="thebaasco.tenant.onboarding.v1.UpdateDisclosuresRequest"></a>

#### UpdateDisclosuresRequest
UpdateDisclosuresRequest is used to perform the asynchronous
operation of updating the disclosures received by the customer in the
onboarding application. Performing this operation will generate an asynchronous
response type of UpdateDisclosuresResponse on the response topic.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| onboarding_application_id | [string](#string) |  | The UUID of the onboarding application to update. |
| disclosures | [Disclosure](#thebaasco.tenant.onboarding.v1.Disclosure) | repeated | The updated collection of disclosures. See Disclosure. |






<a name="thebaasco.tenant.onboarding.v1.UpdateDisclosuresResponse"></a>

#### UpdateDisclosuresResponse
UpdateDisclosuresResponse is the response message for UpdateDisclosuresRequest.
See UpdateDisclosuresRequest for details.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| basic_details | [OnboardingOperationResponseDetails](#thebaasco.tenant.onboarding.v1.OnboardingOperationResponseDetails) |  | The basic details of the onboarding application response. See OnboardingOperationResponseDetails. |






<a name="thebaasco.tenant.onboarding.v1.UpdateEmploymentRequest"></a>

#### UpdateEmploymentRequest
UpdateEmploymentRequest is used to perform the asynchronous
operation of updating the customer employment information in the
onboarding application. Performing this operation will generate an
asynchronous response type of UpdateEmploymentResponse on the response topic.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| onboarding_application_id | [string](#string) |  | The UUID of the onboarding application to update. |
| employment_info | [Employment](#thebaasco.tenant.onboarding.v1.Employment) |  | The updated employment information. See Employment. |






<a name="thebaasco.tenant.onboarding.v1.UpdateEmploymentResponse"></a>

#### UpdateEmploymentResponse
UpdateEmploymentResponse is the response message for UpdateEmploymentRequest.
See UpdateEmploymentRequest for details.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| basic_details | [OnboardingOperationResponseDetails](#thebaasco.tenant.onboarding.v1.OnboardingOperationResponseDetails) |  | The basic details of the onboarding application response. See OnboardingOperationResponseDetails. |






<a name="thebaasco.tenant.onboarding.v1.UpdatePersonalDetailsRequest"></a>

#### UpdatePersonalDetailsRequest
UpdatePersonalDetailsRequest is used to perform the asynchronous
operation of updating the customer personal details in the onboarding application.
Performing this operation will generate an asynchronous response
type of UpdatePersonalDetailsResponse on the response topic.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| onboarding_application_id | [string](#string) |  | The UUID of the onboarding application to update. |
| customer_personal_details | [PersonalDetails](#thebaasco.tenant.onboarding.v1.PersonalDetails) |  | The updated personal details. |






<a name="thebaasco.tenant.onboarding.v1.UpdatePersonalDetailsResponse"></a>

#### UpdatePersonalDetailsResponse
UpdatePersonalDetailsResponse is the response message for UpdatePersonalDetailsRequest.
See UpdatePersonalDetailsRequest for details.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| basic_details | [OnboardingOperationResponseDetails](#thebaasco.tenant.onboarding.v1.OnboardingOperationResponseDetails) |  | The basic details of the onboarding application response. See OnboardingOperationResponseDetails. |






<a name="thebaasco.tenant.onboarding.v1.ValidateApplicationRequest"></a>

#### ValidateApplicationRequest
ValidateApplicationRequest is used to perform the asynchronous
operation of validating the entire onboarding application and all documents
received from the customer. Performing this operation will generate an
asynchronous response type of ValidateApplicationResponse on the response topic.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| onboarding_application_id | [string](#string) |  | The UUID of the onboarding application to validate. |






<a name="thebaasco.tenant.onboarding.v1.ValidateApplicationResponse"></a>

#### ValidateApplicationResponse
ValidateApplicationResponse is the response message for
ValidateApplicationRequest. See ValidateApplicationRequest for details.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| basic_details | [OnboardingOperationResponseDetails](#thebaasco.tenant.onboarding.v1.OnboardingOperationResponseDetails) |  | The basic details of the onboarding application response. See OnboardingOperationResponseDetails. |






<a name="thebaasco.tenant.onboarding.v1.ValidateDocumentsRequest"></a>

#### ValidateDocumentsRequest
ValidateDocumentsRequest is used to perform the asynchronous
operation of validating the identification documents received from the
customer in the onboarding application. Performing this operation will
generate an asynchronous response type of ValidateDocumentsResponse on the
response topic.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| onboarding_application_id | [string](#string) |  | The UUID of the onboarding application to validate. |
| document_details | [DocumentInfo](#thebaasco.tenant.onboarding.v1.DocumentInfo) | repeated | The collection of documents to validate. See DocumentInfo. |






<a name="thebaasco.tenant.onboarding.v1.ValidateDocumentsResponse"></a>

#### ValidateDocumentsResponse
ValidateDocumentsResponse is the response message for ValidateDocumentsRequest.
See ValidateDocumentsRequest for details.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| basic_details | [OnboardingOperationResponseDetails](#thebaasco.tenant.onboarding.v1.OnboardingOperationResponseDetails) |  | The basic details of the onboarding application response. See OnboardingOperationResponseDetails. |






<a name="thebaasco.tenant.onboarding.v1.ValidateSelfieRequest"></a>

#### ValidateSelfieRequest
ValidateSelfieRequest is used to perform the asynchronous
operation of validating the selfie pictures received from the
customer in the onboarding application. Performing this operation will
generate an asynchronous response type of ValidateSelfieResponse on the
response topic.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| onboarding_application_id | [string](#string) |  | The UUID of the onboarding application to validate. |
| file_name | [string](#string) |  | The name of the selfie file to validate. See OnboardingApplication. |






<a name="thebaasco.tenant.onboarding.v1.ValidateSelfieResponse"></a>

#### ValidateSelfieResponse
ValidateSelfieResponse is the response message for ValidateSelfieRequest.
See ValidateSelfieRequest for details.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| basic_details | [OnboardingOperationResponseDetails](#thebaasco.tenant.onboarding.v1.OnboardingOperationResponseDetails) |  | The basic details of the onboarding application response. See OnboardingOperationResponseDetails. |





 <!-- end messages -->



## Shared types

<a name="thebaasco.types.ProtectedDate"></a>

### ProtectedDate

[Global types / Protected Date](../Global-types/protectedtypes/#protecteddate)


<a name="thebaasco.types.ProtectedString"></a>

### ProtectedString

[Global types / Protected String](../Global-types/protectedtypes/#protectedstring)

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

