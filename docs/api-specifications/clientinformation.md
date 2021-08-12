# Client Profile API (Draft)

**This is a draft API specification that is subject to change**

## API Specific Types

<a name="thebaasco.tenant.clientprofile.v1.Address"></a>

### Address
Address is a message for providing an address for an Individual Party, either attached to their home or employment.
The model is extensable enough for international addresses.
Example valid Canadian domestic address:
```
{
  type = HOME
  line_1 = "3456 Richmond St.
  locality = "Montreal".                     // City or town
  area = "Quebec".                           // Provice in Canada, State in US
  postal_code = "T5D3H2"
  country = "Canada"
}
```


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| type | [string](#string) |  | The type of Address being represented. Must be either &#39;HOME&#39; or &#39;WORK&#39; (required). This field is validated based on the message this Address is used in. See IndividualParty and Employment for required values. |
| organization | [string](#string) |  | The organization name associated with this Address. It is optional and has a maximum of 300 characters. This is only applicable of the address is owned by a legal entity and not a natural person. |
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
| administrative_area | [string](#string) |  | The name of the first level administrative area. This value is optional and has a maximum of 150 characters. e.g. For US this would be &#39;State&#39; and Canada this would be &#39;Province&#39; |
| sub_administrative_area | [string](#string) |  | The name of the second level administrative area. This value is optional and has a maximum of 150 characters. e.g. For a city use &#39;City&#39;, county use &#39;County&#39;, etc. |
| area | [string](#string) |  | The name of country first level administrative subdivision area in the address. For Canada, this is name of the province and for the US this is the name of the State. This field is required and has a maximum of 150 characters. e.g. &#39;Ontario&#39; or &#39;New York&#39;. |
| postal_code | [string](#string) |  | The local postal code for routing mail to the address and should contain the ZIP code for US addresses. This field is required and has a maximum of 150 characters. |
| country | [string](#string) |  | The name of the country for the Address. This value is required and has a maximum of 50 characters. |
| creation_datetime | [google.type.DateTime](#google.type.DateTime) |  | Date Time the Address was created. |
| last_updated_datetime | [google.type.DateTime](#google.type.DateTime) |  | Date Time the Address was last updated. |






<a name="thebaasco.tenant.clientprofile.v1.ContactDetails"></a>

### ContactDetails
ContactDetails is a message for providing the contact details for a customer.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| phone_number | [string](#string) |  | The contact phone number. This value is optional and must be in a valid phone number format. |
| customer_email | [string](#string) |  | The contact email address. This value is required and must be in a valid email address format. |
| contact_details_id | [string](#string) |  | Unique identifier that represents this ContactDetails. Valid UUID. |
| creation_datetime | [google.type.DateTime](#google.type.DateTime) |  | Date Time the ContactDetails was created.


<a name="thebaasco.tenant.clientprofile.v1.CreateIndividualPartyContactDetailsRequest"></a>

### CreateIndividualPartyContactDetailsRequest
Contact Details APIs


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| individual_party_id | [string](#string) |  | Unique identifier that represents this IndividualParty. Valid UUID. |
| customer_contact_details | [ContactDetails](#thebaasco.tenant.clientprofile.v1.ContactDetails) |  | See ContactDetails message details. |






<a name="thebaasco.tenant.clientprofile.v1.CreateIndividualPartyIdentityDocumentDetailsRequest"></a>

### CreateIndividualPartyIdentityDocumentDetailsRequest
Identity Document Details APIs


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| individual_party_id | [string](#string) |  | Unique identifier that represents this IndividualParty. Valid UUID. |
| identity_document_details | [IdentityDocumentDetails](#thebaasco.tenant.clientprofile.v1.IdentityDocumentDetails) |  | See IdentityDocumentDetails message details. |






<a name="thebaasco.tenant.clientprofile.v1.CreateIndividualPartyOtherResidencyRequest"></a>

### CreateIndividualPartyOtherResidencyRequest
Other Residency APIs


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| individual_party_id | [string](#string) |  | Unique identifier that represents this IndividualParty. Valid UUID. |
| other_residence | [OtherResidence](#thebaasco.tenant.clientprofile.v1.OtherResidence) |  | See OtherResidence message details. |






<a name="thebaasco.tenant.clientprofile.v1.CreateIndividualPartyRelationshipRequest"></a>

### CreateIndividualPartyRelationshipRequest
CreateIndividualPartyRelationshipRequest is message used to perform the asynchronous operation of creating a new Relationship between Individual Parties. Performing this operation will generate an asynchronous response of type Relationship, on the response topic.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| relationship | [Relationship](#thebaasco.tenant.clientprofile.v1.Relationship) |  | A valid Relationship message. Refer to Relationship message for details. |






<a name="thebaasco.tenant.clientprofile.v1.CreateIndividualPartyRequest"></a>

### CreateIndividualPartyRequest
Individual Party API
Request to create a new IndividualParty


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| individual_party | [IndividualParty](#thebaasco.tenant.clientprofile.v1.IndividualParty) |  | See IndividualParty message details. |






<a name="thebaasco.tenant.clientprofile.v1.CreateIndividualPartyResponse"></a>

### CreateIndividualPartyResponse
Response message for the CreateIndividualPartyRequest


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| individual_party | [IndividualParty](#thebaasco.tenant.clientprofile.v1.IndividualParty) |  | See IndividualParty message details. |






<a name="thebaasco.tenant.clientprofile.v1.CustomerResidency"></a>

### CustomerResidency
CustomerResidency contains the residency status details of the customer.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| is_canadian_tax_resident | [bool](#bool) |  | The Canadian residency status. This is true if the customer is a Canadian resident, false otherwise. |
| sin_token | [string](#string) |  | A tokenized UUID representation of the customer SIN number. |
| citizenship | [string](#string) |  | The primary or current citizenship of the customer. The format is an ISO 3166 Country alpha-2 code (e.g. US, CA, FR). |
| last_updated_datetime | [google.type.DateTime](#google.type.DateTime) |  | Date Time the CustomerResidency was created. |






<a name="thebaasco.tenant.clientprofile.v1.DeleteIndividualPartyContactDetailsRequest"></a>

### DeleteIndividualPartyContactDetailsRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| individual_party_id | [string](#string) |  | Unique identifier that represents this IndividualParty. Valid UUID. |
| contact_details_id | [string](#string) |  | Unique identifier that represents this ContactDetails. Valid UUID. |






<a name="thebaasco.tenant.clientprofile.v1.DeleteIndividualPartyIdentityDocumentDetailsRequest"></a>

### DeleteIndividualPartyIdentityDocumentDetailsRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| individual_party_id | [string](#string) |  | Unique identifier that represents this IndividualParty. Valid UUID. |
| identity_document_details_id | [string](#string) |  | Unique identifier that represents this IdentityDocumentDetails. Valid UUID. |






<a name="thebaasco.tenant.clientprofile.v1.DeleteIndividualPartyOtherResidencyRequest"></a>

### DeleteIndividualPartyOtherResidencyRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| individual_party_id | [string](#string) |  | Unique identifier that represents this IndividualParty. Valid UUID. |
| other_residency_id | [string](#string) |  | Unique identifier that represents this OtherResidency. Valid UUID. |






<a name="thebaasco.tenant.clientprofile.v1.DeleteIndividualPartyRelationshipRequest"></a>

### DeleteIndividualPartyRelationshipRequest
DeleteIndividualPartyRelationshipRequest is message used to perform the asynchronous operation of deleting a Relationship between Individual Parties.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| relationship_id | [string](#string) |  | The Relationship ID for the Relationships to be deleted. Must be a valid UUID. |






<a name="thebaasco.tenant.clientprofile.v1.Employment"></a>

### Employment
Employment defines the details of Employment for an Individual Party.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| employment_status | [string](#string) |  | The status of Employment for this entry. The value stored here is one of the code values retrieved from the ReferenceDataService with data_type set to &#39;EMPLOYMENT_STATUS&#39;. e.g. EMPLOYMENT_STATUS_EMPLOYED or EMPLOYMENT_STATUS_RETIRED. |
| employer_name | [string](#string) |  | The full employer name. The value is required and has a maximum of 50 characters. |
| industry | [string](#string) |  | The industry code the Employment is in. This value is required and has a maximum of 50 characters. The value stored here is one of the code values retrieved from the ReferenceDataService with data_type set to &#39;INDUSTRY&#39;. e.g. &#39;236110&#39; for Residential Construction. |
| occupation | [string](#string) |  | The roles and duties the customer performs for the employer. This value is optional and has a maximum of 50 characters. The value stored here is one of the code values retrieved from the ReferenceDataService with data_type set to &#39;OCCUPATION&#39;. e.g. &#39;OCCUPATION_8255_CONTRACTOR_AND_SUPERVISOR_LANDSCAPING&#39; |
| startDate | [google.type.Date](#google.type.Date) |  | The month and year the customer started this position. |
| endDate | [google.type.Date](#google.type.Date) |  | The month and year the customer no longer held this position. |
| work_address | [Address](#thebaasco.tenant.clientprofile.v1.Address) |  | The Address of the employer. The &#39;type&#39; field must be set to &#39;WORK&#39;. Required. |
| work_phone_number | [string](#string) |  | The phone number of the employer. Required. |
| creation_datetime | [google.type.DateTime](#google.type.DateTime) |  | Date Time the Employment was created. |
| last_updated_datetime | [google.type.DateTime](#google.type.DateTime) |  | Date Time the Employment was last updated. |






<a name="thebaasco.tenant.clientprofile.v1.GetIndividualPartyRelationshipsRequest"></a>

### GetIndividualPartyRelationshipsRequest
GetIndividualPartyRelationshipsRequest is a message passed as parameter to the GetIndividualPartyRelationships operation.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| individual_party_id | [string](#string) |  | The Individual Party ID for which Relationships are returned. Must be a valid UUID. |
| page_size | [int32](#int32) |  | The maximum number of Relationships to return. This value must be greater or equal to one. |
| page_token | [string](#string) |  | When retrieving page other than the initial page, the value for the page_token must be specified, and taken from the previous page&#39;s next_page_token value (see GetTransactionsResponse). When no value is specified, then the initial page will be returned. |






<a name="thebaasco.tenant.clientprofile.v1.GetIndividualPartyRelationshipsResponse"></a>

### GetIndividualPartyRelationshipsResponse
GetIndividualPartyRelationshipsResponse is the response of a successful GetIndividualPartyRelationships operation.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| previous_page_token | [string](#string) |  | The token to be used to retrieve the previous page of data. Must be passed in the page_token field of the next request (see GetTransactionsRequest). When no value is returned in this field, then there is no previous data to be listed. |
| relationship | [Relationship](#thebaasco.tenant.clientprofile.v1.Relationship) | repeated | The requested list of Relationships. |
| next_page_token | [string](#string) |  | The token to be used to retrieve the next page of data. Must be passed in the page_token field of the next request (see GetTransactionsRequest). When no value is returned in this field, then no further data exists to be listed. |






<a name="thebaasco.tenant.clientprofile.v1.GetIndividualPartyRequest"></a>

### GetIndividualPartyRequest
Message to request an Individual Party by ID


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| individual_party_id | [string](#string) |  | Unique identifier that represents this IndividualParty. Valid UUID.

UUID of an Individual Party |






<a name="thebaasco.tenant.clientprofile.v1.IdentityDocumentDetails"></a>

### IdentityDocumentDetails
Encapsulation of an Identity document


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| id_type | [string](#string) |  | The type of document used for identification. The value stored here is one of the code values retrieved from the ReferenceDataService with data_type set to &#39;DOCUMENT_TYPE&#39;. e.g. &#39;DOCUMENT_TYPE_CANADIAN_DRIVERS_LICENSE&#39; or &#39;DOCUMENT_TYPE_CANADIAN_PASSPORT&#39; |
| id_number | [string](#string) |  | Alpha-numeric string that represents the Identity document number on the physical ID. Max Length 50 characters. |
| issuing_authority | [string](#string) |  | The issuer of the identity. I.e. The province for a driver&#39;s license, or the country for a passport. |
| additional_details | [string](#string) |  | Free form addtional details, maybe be required for certain types of IDs. |
| start_date | [google.type.Date](#google.type.Date) |  | The start date of the validity of the ID. |
| end_date | [google.type.Date](#google.type.Date) |  | The end date of the validity of the ID. |
| identity_document_id | [string](#string) |  | Unique identifier that represents this IdentityDocumentDetails. Valid UUID. |
| creation_datetime | [google.type.DateTime](#google.type.DateTime) |  | Date Time the IdentityDocumentDetails was created.

Output only |






<a name="thebaasco.tenant.clientprofile.v1.IndividualParty"></a>

### IndividualParty
The top level domain object that encapsulates a natural entity.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| first_name | [string](#string) |  | The customer&#39;s first name. This value is required and has a maximum of 50 characters. Accepted characters: -, &#39;, French accents, 0-9. |
| middle_name | [string](#string) |  | The customer&#39;s middle name. This value is optional and has a maximum of 50 characters. Accepted characters: -, &#39;, French accents, 0-9. |
| last_name | [string](#string) |  | The customer&#39;s last name. This value is required and has a maximum of 50 characters. Accepted characters: -, &#39;, French accents, 0-9. |
| date_of_birth | [google.type.Date](#google.type.Date) |  | The customer&#39;s date of birth. |
| customer_contact_details | [ContactDetails](#thebaasco.tenant.clientprofile.v1.ContactDetails) | repeated | The list of contact details for this Individual Party. See ContactDetails message. |
| customer_employment | [Employment](#thebaasco.tenant.clientprofile.v1.Employment) |  | Employment details for this Individual Party. See Employment message. |
| customer_residency | [CustomerResidency](#thebaasco.tenant.clientprofile.v1.CustomerResidency) |  | Customer Residency contains the residency status details of this Individual Party. See CustomerResidency message. |
| other_residences | [OtherResidence](#thebaasco.tenant.clientprofile.v1.OtherResidence) | repeated | A collection of other countries where the customer has residence. At least one is required if not a Canadian resident. |
| customer_residence_address | [Address](#thebaasco.tenant.clientprofile.v1.Address) |  | The physical address of this Individual Party&#39;s home. See Address Message. The address type should be set to HOME. |
| identity_document_details | [IdentityDocumentDetails](#thebaasco.tenant.clientprofile.v1.IdentityDocumentDetails) | repeated | Identity documentation that was provided by this Individual Party. See IdentityDocumentDetails message. |
| kycStatus | [bool](#bool) |  | Status flag to indidicate this Individual Party has completed the required Know-Your-Customer (KYC) process during onboarding. |
| individual_party_id | [string](#string) |  | Unique identifier that represents this IndividualParty. This ID is stable across all domains for a given IndividualParty. Valid UUID. |
| creation_datetime | [google.type.DateTime](#google.type.DateTime) |  | Date Time the IndividualParty was created. |
| last_updated_datetime | [google.type.DateTime](#google.type.DateTime) |  | Date Time the IndividualParty was last updated. |






<a name="thebaasco.tenant.clientprofile.v1.IndividualPartyAddressSetEvent"></a>

### IndividualPartyAddressSetEvent
Event emitted when an address is set on an individual party.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| event_id | [string](#string) |  | The unique ID of this event. Can be used by consumers to determine if an event has already been processed or not. This ID will always be a valid UUID. |
| address | [Address](#thebaasco.tenant.clientprofile.v1.Address) |  | Address message |
| tenant_id | [int32](#int32) |  | The tenant ID, to identify the tenant on which the operation occurred.

Internal use only. |
| event_time | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  | The UTC timestamp when this event was issued. |
| individual_party_id | [string](#string) |  | Unique identifier that represents this IndividualParty. Valid UUID.

UUID of an Individual Party |






<a name="thebaasco.tenant.clientprofile.v1.IndividualPartyContactDetailsCreatedEvent"></a>

### IndividualPartyContactDetailsCreatedEvent



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| event_id | [string](#string) |  | The unique ID of this event. Can be used by consumers to determine if an event has already been processed or not. This ID will always be a valid UUID. |
| contact_details | [ContactDetails](#thebaasco.tenant.clientprofile.v1.ContactDetails) |  | See ContactDetails message details. |
| tenant_id | [int32](#int32) |  | The tenant ID, to identify the tenant on which the operation occurred. |
| event_time | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  | The UTC timestamp when this event was issued. |
| individual_party_id | [string](#string) |  | Unique identifier that represents this IndividualParty. Valid UUID. |






<a name="thebaasco.tenant.clientprofile.v1.IndividualPartyContactDetailsDeletedEvent"></a>

### IndividualPartyContactDetailsDeletedEvent



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| event_id | [string](#string) |  | The unique ID of this event. Can be used by consumers to determine if an event has already been processed or not. This ID will always be a valid UUID. |
| contact_details | [ContactDetails](#thebaasco.tenant.clientprofile.v1.ContactDetails) |  | See ContactDetails message details. |
| tenant_id | [int32](#int32) |  | The tenant ID, to identify the tenant on which the operation occurred. |
| event_time | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  | The UTC timestamp when this event was issued. |
| individual_party_id | [string](#string) |  | Unique identifier that represents this IndividualParty. Valid UUID. |






<a name="thebaasco.tenant.clientprofile.v1.IndividualPartyCreatedEvent"></a>

### IndividualPartyCreatedEvent
IndividualPartyCreatedEvent is an event raised when an IndividualParty is created after they have been onboarded.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| event_id | [string](#string) |  | The unique ID of this event. Can be used by consumers to determine if an event has already been processed or not. This ID will always be a valid UUID. |
| individual_party | [IndividualParty](#thebaasco.tenant.clientprofile.v1.IndividualParty) |  | See IndividualParty message details. |
| tenant_id | [int32](#int32) |  | The tenant ID, to identify the tenant on which the operation occurred. |
| event_time | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  | The UTC timestamp when this event was issued. |






<a name="thebaasco.tenant.clientprofile.v1.IndividualPartyCustomerResidencySetEvent"></a>

### IndividualPartyCustomerResidencySetEvent
Event emitted when a primary customer residency is set on an individual party.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| event_id | [string](#string) |  | The unique ID of this event. Can be used by consumers to determine if an event has already been processed or not. This ID will always be a valid UUID. |
| customer_residency | [CustomerResidency](#thebaasco.tenant.clientprofile.v1.CustomerResidency) |  | See CustomerResidency message details. |
| tenant_id | [int32](#int32) |  | The tenant ID, to identify the tenant on which the operation occurred. |
| event_time | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  | The UTC timestamp when this event was issued. |
| individual_party_id | [string](#string) |  | Unique identifier that represents this IndividualParty. Valid UUID. |






<a name="thebaasco.tenant.clientprofile.v1.IndividualPartyEmploymentSetEvent"></a>

### IndividualPartyEmploymentSetEvent
Event emitted when an Employment is set on an individual party.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| event_id | [string](#string) |  | The unique ID of this event. Can be used by consumers to determine if an event has already been processed or not. This ID will always be a valid UUID. |
| employment | [Employment](#thebaasco.tenant.clientprofile.v1.Employment) |  |  |
| tenant_id | [int32](#int32) |  | The tenant ID, to identify the tenant on which the operation occurred. |
| event_time | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  | The UTC timestamp when this event was issued. |
| individual_party_id | [string](#string) |  | Unique identifier that represents this IndividualParty. Valid UUID. |






<a name="thebaasco.tenant.clientprofile.v1.IndividualPartyIdentityDocumentDetailsCreatedEvent"></a>

### IndividualPartyIdentityDocumentDetailsCreatedEvent



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| event_id | [string](#string) |  | The unique ID of this event. Can be used by consumers to determine if an event has already been processed or not. This ID will always be a valid UUID. |
| identity_document_details | [IdentityDocumentDetails](#thebaasco.tenant.clientprofile.v1.IdentityDocumentDetails) |  | See IdentityDocumentDetails message details. |
| tenant_id | [int32](#int32) |  | The tenant ID, to identify the tenant on which the operation occurred. |
| event_time | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  | The UTC timestamp when this event was issued. |
| individual_party_id | [string](#string) |  | Unique identifier that represents this IndividualParty. Valid UUID. |






<a name="thebaasco.tenant.clientprofile.v1.IndividualPartyIdentityDocumentDetailsDeletedEvent"></a>

### IndividualPartyIdentityDocumentDetailsDeletedEvent



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| event_id | [string](#string) |  | The unique ID of this event. Can be used by consumers to determine if an event has already been processed or not. This ID will always be a valid UUID. |
| identity_document_details | [IdentityDocumentDetails](#thebaasco.tenant.clientprofile.v1.IdentityDocumentDetails) |  | See IdentityDocumentDetails message details. |
| tenant_id | [int32](#int32) |  | The tenant ID, to identify the tenant on which the operation occurred. |
| event_time | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  | The UTC timestamp when this event was issued. |
| individual_party_id | [string](#string) |  | Unique identifier that represents this IndividualParty. Valid UUID. |






<a name="thebaasco.tenant.clientprofile.v1.IndividualPartyOtherResidencyCreatedEvent"></a>

### IndividualPartyOtherResidencyCreatedEvent



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| event_id | [string](#string) |  | The unique ID of this event. Can be used by consumers to determine if an event has already been processed or not. This ID will always be a valid UUID. |
| customer_residency | [OtherResidence](#thebaasco.tenant.clientprofile.v1.OtherResidence) |  | See OtherResidence message details. |
| tenant_id | [int32](#int32) |  | The tenant ID, to identify the tenant on which the operation occurred. |
| event_time | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  | The UTC timestamp when this event was issued. |
| individual_party_id | [string](#string) |  | Unique identifier that represents this IndividualParty. Valid UUID. |






<a name="thebaasco.tenant.clientprofile.v1.IndividualPartyOtherResidencyDeletedEvent"></a>

### IndividualPartyOtherResidencyDeletedEvent



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| event_id | [string](#string) |  | The unique ID of this event. Can be used by consumers to determine if an event has already been processed or not. This ID will always be a valid UUID. |
| customer_residency | [OtherResidence](#thebaasco.tenant.clientprofile.v1.OtherResidence) |  | See OtherResidence message details. |
| tenant_id | [int32](#int32) |  | The tenant ID, to identify the tenant on which the operation occurred. |
| event_time | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  | The UTC timestamp when this event was issued. |
| individual_party_id | [string](#string) |  | Unique identifier that represents this IndividualParty. Valid UUID. |






<a name="thebaasco.tenant.clientprofile.v1.IndividualPartyRelationshipCreatedEvent"></a>

### IndividualPartyRelationshipCreatedEvent
IndividualPartyRelationshipCreatedEvent is an event raised when a Relationship is successfuly created.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| event_id | [string](#string) |  | The unique ID of this event. Can be used by consumers to determine if an event has already been processed or not. This ID will always be a valid UUID. |
| relationship | [Relationship](#thebaasco.tenant.clientprofile.v1.Relationship) |  | A valid Relationship message. Refer to Relationship message for details. |
| tenant_id | [int32](#int32) |  | The tenant ID, to identify the tenant on which the operation occurred. |
| event_time | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  | The UTC timestamp when this event was issued. |






<a name="thebaasco.tenant.clientprofile.v1.IndividualPartyRelationshipDeletedEvent"></a>

### IndividualPartyRelationshipDeletedEvent
IndividualPartyRelationshipDeletedEvent is an event raised when a Relationship is successfuly deleted.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| event_id | [string](#string) |  | The unique ID of this event. Can be used by consumers to determine if an event has already been processed or not. This ID will always be a valid UUID. |
| relationship | [Relationship](#thebaasco.tenant.clientprofile.v1.Relationship) |  | A valid Relationship message. Refer to Relationship message for details. |
| tenant_id | [int32](#int32) |  | The tenant ID, to identify the tenant on which the operation occurred. |
| relationship_deleted_time | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  |  |






<a name="thebaasco.tenant.clientprofile.v1.OtherResidence"></a>

### OtherResidence
OtherResidence is a message for providing alternate countries and places of residence.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| other_residence_country | [string](#string) |  | The name of the other country of residence. The format is an ISO 3166 Country alpha-2 code (e.g. US, CA, FR). |
| tin_token | [string](#string) |  | A tokenized UUID representation of the unique TIN for the country of residence. Optional. |
| other_residency_id | [string](#string) |  | Unique identifier that represents this OtherResidence. Valid UUID.

Output only, UUID |
| creation_datetime | [google.type.DateTime](#google.type.DateTime) |  | Date Time the OtherResidence was created. |






<a name="thebaasco.tenant.clientprofile.v1.Relationship"></a>

### Relationship
Message defines the regulatory relationship between two individual parties.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| individual_party_id | [string](#string) |  | UUID of the primary Individual Party (owner) of the relationship |
| related_individual_party_id | [string](#string) |  | UUID of the related Individual Party |
| relationship_type | [RelationshipType](#thebaasco.tenant.clientprofile.v1.RelationshipType) |  | Relationship type between two Individual Parties. Based regulatory reporting types: Accountant,Agent,Legal counsel,Borrower,Broker,Customer,Employee,Friend,Relative,Other (specify) ) |
| other_relationship | [string](#string) |  | Other (specify) (required if Other type selected) |
| relationship_id | [string](#string) |  | Unique identifier that represents this Relationship. Valid UUID. |
| creation_datetime | [google.type.DateTime](#google.type.DateTime) |  | Date Time the Relationship was created. |






<a name="thebaasco.tenant.clientprofile.v1.SetIndividualPartyAddressRequest"></a>

### SetIndividualPartyAddressRequest
Address APIs
Async message to set the primary residence address for an individual party.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| individual_party_id | [string](#string) |  | Unique identifier that represents this IndividualParty. Valid UUID.

UUID of an Individual Party |
| customer_residence_address | [Address](#thebaasco.tenant.clientprofile.v1.Address) |  | See Address message details. |






<a name="thebaasco.tenant.clientprofile.v1.SetIndividualPartyCustomerResidencyRequest"></a>

### SetIndividualPartyCustomerResidencyRequest
Customer Residency API
Async message to set the primary customer residency for an individual party.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| individual_party_id | [string](#string) |  | Unique identifier that represents this IndividualParty. Valid UUID. |
| customer_residency | [CustomerResidency](#thebaasco.tenant.clientprofile.v1.CustomerResidency) |  | See CustomerResidency message details. |






<a name="thebaasco.tenant.clientprofile.v1.SetIndividualPartyEmploymentRequest"></a>

### SetIndividualPartyEmploymentRequest
Async message to set the Employment for an individual party.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| individual_party_id | [string](#string) |  | Unique identifier that represents this IndividualParty. Valid UUID. |
| employment | [Employment](#thebaasco.tenant.clientprofile.v1.Employment) |  | See Employment message details. |





 


<a name="thebaasco.tenant.clientprofile.v1.RelationshipType"></a>

### RelationshipType
Relationship Types are regulatory relationships between Individuals.

| Name | Number | Description |
| ---- | ------ | ----------- |
| ACCOUNTANT | 0 |  |
| AGENT | 1 |  |
| LEGAL_COUNSEL | 2 |  |
| BORROWER | 3 |  |
| BROKER | 4 |  |
| CUSTOMER | 5 |  |
| EMPLOYEE | 6 |  |
| FRIEND | 7 |  |
| RELATIVE | 8 |  |
| OTHER | 9 |  |


 

 


<a name="thebaasco.tenant.clientprofile.v1.IndividualParties"></a>

### IndividualParties
Service that returns an IndividualParty when given a GetIndividualPartyRequest

| Method Name | Request Type | Response Type | Description |
| ----------- | ------------ | ------------- | ------------|
| GetIndividualParty | [GetIndividualPartyRequest](#thebaasco.tenant.clientprofile.v1.GetIndividualPartyRequest) | [IndividualParty](#thebaasco.tenant.clientprofile.v1.IndividualParty) |  |


<a name="thebaasco.tenant.clientprofile.v1.Relationships"></a>

### Relationships
Relationships provides a way to retrieve relationships for an IndividualParty

| Method Name | Request Type | Response Type | Description |
| ----------- | ------------ | ------------- | ------------|
| GetIndividualPartyRelationships | [GetIndividualPartyRelationshipsRequest](#thebaasco.tenant.clientprofile.v1.GetIndividualPartyRelationshipsRequest) | [GetIndividualPartyRelationshipsResponse](#thebaasco.tenant.clientprofile.v1.GetIndividualPartyRelationshipsResponse) |  |

 



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
