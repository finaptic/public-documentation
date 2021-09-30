# Onboarding API

## API Specific Types

<a name="thebaasco.tenant.onboarding.v1.AcceptApplicationRequest"></a>

### AcceptApplicationRequest

AcceptApplicationRequest is used to perform the asynchronous
operation of accepting the onboarding application received from the
customer. Performing this operation will generate an asynchronous
response type of AcceptApplicationResponse on the response topic.

| Field                     | Type              | Label | Description                                       |
| ------------------------- | ----------------- | ----- | ------------------------------------------------- |
| onboarding_application_id | [string](#string) |       | The UUID of the onboarding application to accept. |

<a name="thebaasco.tenant.onboarding.v1.AcceptApplicationResponse"></a>

### AcceptApplicationResponse

AcceptApplicationResponse is the response message for AcceptApplicationRequest.
See AcceptApplicationRequest for details.

| Field         | Type                                                                                                     | Label | Description                                                                                       |
| ------------- | -------------------------------------------------------------------------------------------------------- | ----- | ------------------------------------------------------------------------------------------------- |
| basic_details | [OnboardingOperationResponseDetails](#thebaasco.tenant.onboarding.v1.OnboardingOperationResponseDetails) |       | The basic details of the onboarding application response. See OnboardingOperationResponseDetails. |

<a name="thebaasco.tenant.onboarding.v1.AccountUsageDetails"></a>

### AccountUsageDetails

AccountUsageDetails is a message for providing the source of funds and purpose for applying
for an Account.

| Field                   | Type              | Label    | Description                                                                                                                                                                                                                                                                                                      |
| ----------------------- | ----------------- | -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| account_purpose         | [string](#string) |          | The intended purpose of the Account. This value is required. The value stored here is one of the code values retrieved from the ReferenceDataService with data_type set to 'ACCOUNT_PURPOSE'. e.g. 'ACCOUNT_PURPOSE_EVERYDAY_TRANSACTIONS' or 'ACCOUNT_PURPOSE_TRAVELLING'                                       |
| primary_source_of_funds | [string](#string) | repeated | A collection of the primary sources of funds for the Account. At least one value is required. The values stored here are a collection of the code values retrieved from the ReferenceDataService with data_type set to 'SOURCE_OF_FUNDS' e.g. 'PRIMARY_SOURCE_OF_FUNDS_LOANS' or 'PRIMARY_SOURCE_OF_FUNDS_GIFTS' |

<a name="thebaasco.tenant.onboarding.v1.Address"></a>

### Address

Address is a message for providing a given Address for a customer.

| Field                     | Type              | Label | Description                                                                                                                                                                                                                                                      |
| ------------------------- | ----------------- | ----- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| type                      | [string](#string) |       | The type of Address being represented. Must be either 'HOME' or 'WORK' (required). This field is validated based on the message this Address is used in. See OnboardingApplication and Employment for required values.                                           |
| organization              | [string](#string) |       | The organization name associated with this Address. It is optional and has a maximum of 300 characters.                                                                                                                                                          |
| line_1                    | [string](#string) |       | The first line of the Address. This value is required and has a maximum of 300 characters.                                                                                                                                                                       |
| line_2                    | [string](#string) |       | The second line of the Address. This value is optional, and has a maximum of 300 characters.                                                                                                                                                                     |
| line_3                    | [string](#string) |       | The third line of the Address. This value is optional, and has a maximum of 300 characters.                                                                                                                                                                      |
| line_4                    | [string](#string) |       | The fourth line of the Address. This value is optional, and has a maximum of 300 characters.                                                                                                                                                                     |
| line_5                    | [string](#string) |       | The fifth line of the Address. This value is optional, and has a maximum of 300 characters.                                                                                                                                                                      |
| line_6                    | [string](#string) |       | The sixth line of the Address. This value is optional, and has a maximum of 300 characters.                                                                                                                                                                      |
| line_7                    | [string](#string) |       | The seventh line of the Address. This value is optional, and has a maximum of 300 characters.                                                                                                                                                                    |
| line_8                    | [string](#string) |       | The eighth line of the Address. This value is optional, and has a maximum of 300 characters.                                                                                                                                                                     |
| locality                  | [string](#string) |       | The local sub administrative area such as the municipality or community name. In Canadian and US addresses, this corresponds to the city or town name. This value is optional and has a maximum of 150 characters. e.g. Toronto, Orlando.                        |
| dependent_locality        | [string](#string) |       | The municipality or community name of the major postal region address. For Canadian and US addresses, this can remain empty. This value is optional and has a maximum of 150 characters.                                                                         |
| double_dependent_locality | [string](#string) |       | The municipality or community name of the local postal region address. For Canadian and US addresses, this can remain empty. This value is optional and has a maximum of 150 characters.                                                                         |
| administrative_area       | [string](#string) |       | The name of the first level administrative area. This value is optional and has a maximum of 150 characters. e.g. For US this would be 'State' and Canada this would be 'Province'.                                                                              |
| sub_administrative_area   | [string](#string) |       | The name of the second level administrative area. This value is optional and has a maximum of 150 characters. e.g. For a city use 'City', county use 'County', etc.                                                                                              |
| area                      | [string](#string) |       | The name of country first level administrative subdivision area in the address. For Canada, this is name of the province and for the US this is the name of the State. This field is required and has a maximum of 150 characters. e.g. 'Ontario' or 'New York'. |
| postal_code               | [string](#string) |       | The local postal code for routing mail to the address and should contain the ZIP code for US addresses. This field is required and has a maximum of 150 characters.                                                                                              |
| country                   | [string](#string) |       | The name of the country for the Address. This value is required and has a maximum of 50 characters.                                                                                                                                                              |

<a name="thebaasco.tenant.onboarding.v1.Consent"></a>

### Consent

Consent is a message indicating the receipt of a requested customer Consent.

| Field                     | Type                                                    | Label | Description                                                                                                                                                                                                                                   |
| ------------------------- | ------------------------------------------------------- | ----- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| consent_type              | [string](#string)                                       |       | The type of Consent received from the customer. This value is required. The value stored here is one of the code values retrieved from the ReferenceDataService with data_type set to 'CONSENT_TYPE'. e.g. ‘CONSENT_TYPE_TERMS_OF_USE_OF_APP’ |
| consent_timestamp         | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |       | The UTC timestamp when Consent was received from the customer.                                                                                                                                                                                |
| document_name_and_version | [string](#string)                                       |       | The name and version of the document in URL format that the customer consented to.                                                                                                                                                            |
| view_timestamp            | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |       | The UTC timestamp when the customer viewed the document.                                                                                                                                                                                      |

<a name="thebaasco.tenant.onboarding.v1.ContactDetails"></a>

### ContactDetails

ContactDetails is a message for providing the contact details for a customer.

| Field          | Type              | Label | Description                                                                                    |
| -------------- | ----------------- | ----- | ---------------------------------------------------------------------------------------------- |
| phone_number   | [string](#string) |       | The contact phone number. This value is optional and must be in a valid phone number format.   |
| customer_email | [string](#string) |       | The contact email address. This value is required and must be in a valid email address format. |

<a name="thebaasco.tenant.onboarding.v1.CreateFileUploadUrlRequest"></a>

### CreateFileUploadUrlRequest

CreateFileUploadUrlRequest is the parameter to pass when calling CreateFileUploadURL.

| Field                     | Type              | Label | Description                                                                        |
| ------------------------- | ----------------- | ----- | ---------------------------------------------------------------------------------- |
| onboarding_application_id | [string](#string) |       | The UUID representing the onboarding application that requested a file upload URL. |

<a name="thebaasco.tenant.onboarding.v1.CreateFileUploadUrlResponse"></a>

### CreateFileUploadUrlResponse

CreateFileUploadUrlResponse is the response returned by the CreateFileUploadURL

| Field      | Type              | Label | Description                                |
| ---------- | ----------------- | ----- | ------------------------------------------ |
| filename   | [string](#string) |       | The generated name for the file to upload. |
| upload_url | [string](#string) |       | The URL to use to upload the file.         |

<a name="thebaasco.tenant.onboarding.v1.CustomerResidency"></a>

### CustomerResidency

CustomerResidency contains the residency status details of the customer.

| Field                    | Type                                                             | Label    | Description                                                                                                            |
| ------------------------ | ---------------------------------------------------------------- | -------- | ---------------------------------------------------------------------------------------------------------------------- |
| is_canadian_tax_resident | [bool](#bool)                                                    |          | The Canadian residency status. This is true if the customer is a Canadian resident, false otherwise.                   |
| other_residences         | [OtherResidence](#thebaasco.tenant.onboarding.v1.OtherResidence) | repeated | A collection of other countries where the customer has residence. At least one is required if not a Canadian resident. |
| sin_token                | [string](#string)                                                |          | A tokenized UUID representation of the customer SIN number.                                                            |
| citizenship              | [string](#string)                                                |          | The primary or current citizenship of the customer. The format is an ISO 3166 Country alpha-2 code (e.g. US, CA, FR).  |

<a name="thebaasco.tenant.onboarding.v1.Disclosure"></a>

### Disclosure

Disclosure is a message indicating customer receipt of a documented Disclosure.

| Field                     | Type                                                    | Label | Description                                                                                                                                                                                                                                                              |
| ------------------------- | ------------------------------------------------------- | ----- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| disclosure_type           | [string](#string)                                       |       | The type of Disclosure sent to the customer. This value is required. The value stored here is one of the code values retrieved from the ReferenceDataService with data_type set to 'DISCLOSURE_TYPE'. e.g. 'DISCLOSURE_TYPE_DISCLOSURES_DURING_ISSUANCE_OF_PREPAID_CARD' |
| timestamp                 | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |       | The UTC timestamp when the customer viewed the document.                                                                                                                                                                                                                 |
| document_name_and_version | [string](#string)                                       |       | Name and version of the disclosure document in URL format that the customer viewed.                                                                                                                                                                                      |

<a name="thebaasco.tenant.onboarding.v1.DocumentInfo"></a>

### DocumentInfo

DocumentInfo is a message containing the details of an onboarding document
collected from the customer to prove identity.

| Field                 | Type              | Label | Description                                                                                                                                                                                                                                                     |
| --------------------- | ----------------- | ----- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| document_type         | [string](#string) |       | The type of document used for identification. The value stored here is one of the code values retrieved from the ReferenceDataService with data_type set to 'DOCUMENT_TYPE'. e.g. 'DOCUMENT_TYPE_CANADIAN_DRIVERS_LICENSE' or 'DOCUMENT_TYPE_CANADIAN_PASSPORT' |
| document_country      | [string](#string) |       | The country the document was issued for.                                                                                                                                                                                                                        |
| front_image_file_name | [string](#string) |       | The cloud storage file nme of the image taken of the front of the document. (required, file size &lt;4MB)                                                                                                                                                       |
| back_image_file_name  | [string](#string) |       | The cloud storage file nme of the image taken of the back of the document. (file size &lt;4MB)                                                                                                                                                                  |

<a name="thebaasco.tenant.onboarding.v1.Employment"></a>

### Employment

Employment defines the details of Employment for the customer being onboarded.

| Field             | Type                                               | Label | Description                                                                                                                                                                                                                                                                                                      |
| ----------------- | -------------------------------------------------- | ----- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| employment_status | [string](#string)                                  |       | The status of Employment for this entry. The value stored here is one of the code values retrieved from the ReferenceDataService with data_type set to 'EMPLOYMENT_STATUS'. e.g. EMPLOYMENT_STATUS_EMPLOYED or EMPLOYMENT_STATUS_RETIRED.                                                                        |
| employer_name     | [string](#string)                                  |       | The full employer name. The value is required and has a maximum of 50 characters.                                                                                                                                                                                                                                |
| industry          | [string](#string)                                  |       | The industry code the Employment is in. This value is required and has a maximum of 50 characters. The value stored here is one of the code values retrieved from the ReferenceDataService with data_type set to 'INDUSTRY'. e.g. '236110' for Residential Construction.                                         |
| occupation        | [string](#string)                                  |       | The roles and duties the customer performs for the employer. This value is optional and has a maximum of 50 characters. The value stored here is one of the code values retrieved from the ReferenceDataService with data_type set to 'OCCUPATION'. e.g. 'OCCUPATION_8255_CONTRACTOR_AND_SUPERVISOR_LANDSCAPING' |
| startDate         | [google.type.Date](#google.type.Date)              |       | The month and year the customer started this position.                                                                                                                                                                                                                                                           |
| endDate           | [google.type.Date](#google.type.Date)              |       | The month and year the customer no longer held this position.                                                                                                                                                                                                                                                    |
| work_address      | [Address](#thebaasco.tenant.onboarding.v1.Address) |       | The Address of the employer. The 'type' field must be set to 'WORK'. Required.                                                                                                                                                                                                                                   |
| work_phone_number | [string](#string)                                  |       | The phone number of the employer. Required.                                                                                                                                                                                                                                                                      |

<a name="thebaasco.tenant.onboarding.v1.FinalizeApplicationCreateCustomerRequest"></a>

### FinalizeApplicationCreateCustomerRequest

FinalizeApplicationCreateCustomerRequest is used to perform the asynchronous
operation of creating the customer for the onboarding application. Performing
this operation will generate an asynchronous response type of
FinalizeApplicationCreateCustomerResponse on the response topic.

| Field                     | Type              | Label | Description                                                      |
| ------------------------- | ----------------- | ----- | ---------------------------------------------------------------- |
| onboarding_application_id | [string](#string) |       | The UUID of the onboarding application to create a customer for. |

<a name="thebaasco.tenant.onboarding.v1.FinalizeApplicationCreateCustomerResponse"></a>

### FinalizeApplicationCreateCustomerResponse

FinalizeApplicationCreateCustomerResponse is the response message for
FinalizeApplicationCreateCustomerRequest.
See FinalizeApplicationCreateCustomerRequest for details.

| Field         | Type                                                                                                     | Label | Description                                                                                       |
| ------------- | -------------------------------------------------------------------------------------------------------- | ----- | ------------------------------------------------------------------------------------------------- |
| basic_details | [OnboardingOperationResponseDetails](#thebaasco.tenant.onboarding.v1.OnboardingOperationResponseDetails) |       | The basic details of the onboarding application response. See OnboardingOperationResponseDetails. |
| customer_id   | [string](#string)                                                                                        |       | The UUID representing the newly created customer.                                                 |

<a name="thebaasco.tenant.onboarding.v1.FinalizeApplicationCreateProductRequest"></a>

### FinalizeApplicationCreateProductRequest

FinalizeApplicationCreateProductRequest is used to perform the asynchronous
operation of creating the product for the onboarding application. Performing
this operation will generate an asynchronous response type of
FinalizeApplicationCreateProductResponse on the response topic.

| Field                     | Type              | Label | Description                                                       |
| ------------------------- | ----------------- | ----- | ----------------------------------------------------------------- |
| onboarding_application_id | [string](#string) |       | The UUID of the onboarding application to create the product for. |

<a name="thebaasco.tenant.onboarding.v1.FinalizeApplicationCreateProductResponse"></a>

### FinalizeApplicationCreateProductResponse

FinalizeApplicationCreateProductResponse is the response message for
FinalizeApplicationCreateProductRequest.
See FinalizeApplicationCreateProductRequest for details.

| Field         | Type                                                                                                     | Label    | Description                                                                                       |
| ------------- | -------------------------------------------------------------------------------------------------------- | -------- | ------------------------------------------------------------------------------------------------- |
| basic_details | [OnboardingOperationResponseDetails](#thebaasco.tenant.onboarding.v1.OnboardingOperationResponseDetails) |          | The basic details of the onboarding application response. See OnboardingOperationResponseDetails. |
| products      | [OnboardedProduct](#thebaasco.tenant.onboarding.v1.OnboardedProduct)                                     | repeated | The collection of products created. See OnboardedProduct.                                         |

<a name="thebaasco.tenant.onboarding.v1.GetApplicationStatusRequest"></a>

### GetApplicationStatusRequest

GetApplicationStatusRequest is used to retrieve the current status of
the onboarding application.

| Field          | Type              | Label | Description                                                  |
| -------------- | ----------------- | ----- | ------------------------------------------------------------ |
| application_id | [string](#string) |       | The UUID of the onboarding application to get the status of. |

<a name="thebaasco.tenant.onboarding.v1.GetApplicationStatusResponse"></a>

### GetApplicationStatusResponse

GetApplicationStatusResponse is the response message for
GetApplicationStatusRequest. See GetApplicationStatusRequest for details.

| Field         | Type                                                                                                     | Label | Description                                                                                       |
| ------------- | -------------------------------------------------------------------------------------------------------- | ----- | ------------------------------------------------------------------------------------------------- |
| basic_details | [OnboardingOperationResponseDetails](#thebaasco.tenant.onboarding.v1.OnboardingOperationResponseDetails) |       | The basic details of the onboarding application response. See OnboardingOperationResponseDetails. |

<a name="thebaasco.tenant.onboarding.v1.InitiateApplicationRequest"></a>

### InitiateApplicationRequest

InitiateApplicationRequest is used to perform the asynchronous
operation of starting the onboarding application.
Performing this operation will generate an asynchronous response
type of InitiateApplicationResponse on the response topic.

| Field       | Type              | Label | Description                                            |
| ----------- | ----------------- | ----- | ------------------------------------------------------ |
| customer_id | [string](#string) |       | The UUID representing the customer.                    |
| bundle_id   | [string](#string) |       | The unique identifier of the bundle being applied for. |

<a name="thebaasco.tenant.onboarding.v1.InitiateApplicationResponse"></a>

### InitiateApplicationResponse

InitiateApplicationResponse is the response message for
InitiateApplicationRequest.

| Field         | Type                                                                                                     | Label | Description                                                                                       |
| ------------- | -------------------------------------------------------------------------------------------------------- | ----- | ------------------------------------------------------------------------------------------------- |
| basic_details | [OnboardingOperationResponseDetails](#thebaasco.tenant.onboarding.v1.OnboardingOperationResponseDetails) |       | The basic details of the onboarding application response. See OnboardingOperationResponseDetails. |

<a name="thebaasco.tenant.onboarding.v1.OnboardedProduct"></a>

### OnboardedProduct

OnboardedProduct is the message representing a single product in the
onboarding application.

| Field         | Type              | Label | Description                                                    |
| ------------- | ----------------- | ----- | -------------------------------------------------------------- |
| product_id    | [string](#string) |       | The UUID of the onboarded product.                             |
| source_domain | [string](#string) |       | The source domain of the onboarded product (eg: core-banking). |

<a name="thebaasco.tenant.onboarding.v1.OnboardingApplication"></a>

### OnboardingApplication

OnboardingApplication is the message containing all of the onboarding details
collected from a customer required to apply for a product.

| Field                             | Type                                                                       | Label    | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| --------------------------------- | -------------------------------------------------------------------------- | -------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| application_id                    | [string](#string)                                                          |          | The UUID representing a given application.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| customer_id                       | [string](#string)                                                          |          | The UUID representing the onboarding customer.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| bundle_id                         | [string](#string)                                                          |          | The unique identifier of the bundled products being applied for.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| status                            | [string](#string)                                                          |          | The application status. The value here is one of the following. 'UNDEFINED' is reserved for error detection purposes. It should not be used directly. 'New Application' describes an Application that has just been created. 'Data Collection' describes an Application that has successfully performed an Update call or failed a ValidateSelfieRequest or a ValidateDocumentsRequest. 'KYC In Progress' describes an Application that has successfully performed a ValidateSelfieRequest or ValidateDocumentsRequest. 'KYC Success' describes an Application that has successfully performed KYC checks as a result of a ValidateApplicationRequest. 'KYC Failed' describes an Application that has failed KYC as a result of a ValidateApplicationRequest. 'KYC Rejected' describes an Application that has no remaining KYC attempts. 'Application Accepted' describes an application that has successfully completed an AcceptApplicationRequest. 'Application Processed' describes an application that has been successfully completed. 'Application Cancelled' describes an application that has been cancelled by the customer. |
| customer_personal_details         | [PersonalDetails](#thebaasco.tenant.onboarding.v1.PersonalDetails)         |          | The personal details about the customer. See PersonalDetails.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| customer_contact_details          | [ContactDetails](#thebaasco.tenant.onboarding.v1.ContactDetails)           |          | The contact details about the customer. See ContactDetails.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| account_details                   | [AccountUsageDetails](#thebaasco.tenant.onboarding.v1.AccountUsageDetails) |          | The Account usage intentions of the customer. See AccountUsageDetails.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| customer_employment               | [Employment](#thebaasco.tenant.onboarding.v1.Employment)                   | repeated | The employment history for the customer. See Employment.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| customer_residency                | [CustomerResidency](#thebaasco.tenant.onboarding.v1.CustomerResidency)     |          | The residency status of the customer. See CustomerResidency.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| consents                          | [Consent](#thebaasco.tenant.onboarding.v1.Consent)                         | repeated | The collection of consents received from the customer. See Consent.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| disclosures                       | [Disclosure](#thebaasco.tenant.onboarding.v1.Disclosure)                   | repeated | The collection of disclosures received from the customer. See Disclosure.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| customer_residence_address        | [Address](#thebaasco.tenant.onboarding.v1.Address)                         | repeated | The collection of resident addresses owned/occupied by the customer. See Address.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| customer_identification_documents | [DocumentInfo](#thebaasco.tenant.onboarding.v1.DocumentInfo)               | repeated | The collection of documents collected to identify the customer in the KYC process.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| customer_communication_pref       | [string](#string)                                                          |          | The preferred communication method chosen by the customer. The value stored here is one of the code values retrieved from the ReferenceDataService with data_type set to 'COMMUNICATION_PREFERENCE'. e.g. 'COMMUNICATION_PREFERENCES_MOBILE' or 'COMMUNICATION_PREFERENCES_EMAIL'                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |

<a name="thebaasco.tenant.onboarding.v1.OnboardingApplicationCustomerOnboardedEvent"></a>

### OnboardingApplicationCustomerOnboardedEvent

OnboardingApplicationCustomerOnboardedEvent is an event raised when a
customer is created.

| Field          | Type                                                    | Label | Description                                                                                                                                                  |
| -------------- | ------------------------------------------------------- | ----- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| event_id       | [string](#string)                                       |       | The unique ID of this event. Can be used by consumers to determine if an event has already been processed or not. This ID will always be a valid UUID.       |
| application_id | [string](#string)                                       |       | The UUID of the onboarding application connected to this event.                                                                                              |
| tenant_id      | [int32](#int32)                                         |       | The ID to identify the tenant on which the operation occurred. This can be used when processing events at the global scope to differentiate between tenants. |
| event_time     | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |       | The UTC timestamp when this event was issued.                                                                                                                |

<a name="thebaasco.tenant.onboarding.v1.OnboardingApplicationDataConfirmedEvent"></a>

### OnboardingApplicationDataConfirmedEvent

OnboardingApplicationDataConfirmedEvent is the async event message sent when
a new customer is onboarded. As part of PB-3330, this event will also occur
when onboarding a new product for an existing customer.

| Field       | Type                                                                           | Label | Description                                                                                                                                                  |
| ----------- | ------------------------------------------------------------------------------ | ----- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| event_id    | [string](#string)                                                              |       | The unique ID of this event. Can be used by consumers to determine if an event has already been processed or not. This ID will always be a valid UUID.       |
| application | [OnboardingApplication](#thebaasco.tenant.onboarding.v1.OnboardingApplication) |       | The complete onboarding application for the onboarded customer. See OnboardingApplication.                                                                   |
| tenant_id   | [int32](#int32)                                                                |       | The ID to identify the tenant on which the operation occurred. This can be used when processing events at the global scope to differentiate between tenants. |
| event_time  | [google.protobuf.Timestamp](#google.protobuf.Timestamp)                        |       | The UTC timestamp when this event was issued.                                                                                                                |

<a name="thebaasco.tenant.onboarding.v1.OnboardingApplicationProductOnboardedEvent"></a>

### OnboardingApplicationProductOnboardedEvent

OnboardingApplicationProductOnboardedEvent is the async event message sent
when a product is created with the FinalizeApplicationCreateProductRequest.

| Field          | Type                                                                 | Label    | Description                                                                                                                                                  |
| -------------- | -------------------------------------------------------------------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| event_id       | [string](#string)                                                    |          | The unique ID of this event. Can be used by consumers to determine if an event has already been processed or not. This ID will always be a valid UUID.       |
| application_id | [string](#string)                                                    |          | The UUID of the onboarding application connected to this event.                                                                                              |
| products       | [OnboardedProduct](#thebaasco.tenant.onboarding.v1.OnboardedProduct) | repeated | The collection of products onboarded. See OnboardedProduct.                                                                                                  |
| tenant_id      | [int32](#int32)                                                      |          | The ID to identify the tenant on which the operation occurred. This can be used when processing events at the global scope to differentiate between tenants. |
| event_time     | [google.protobuf.Timestamp](#google.protobuf.Timestamp)              |          | The UTC timestamp when this event was issued.                                                                                                                |

<a name="thebaasco.tenant.onboarding.v1.OnboardingOperationResponseDetails"></a>

### OnboardingOperationResponseDetails

OnboardingOperationResponseDetails are the common response fields for an
onboarding application create or update message.

| Field                         | Type              | Label | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| ----------------------------- | ----------------- | ----- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| application_id                | [string](#string) |       | The UUID of the application the response is for.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| onboarding_application_status | [string](#string) |       | The updated application status. The value here is one of the following. 'UNDEFINED' is reserved for error detection purposes. It should not be used directly. 'New Application' describes an Application that has just been created. 'Data Collection' describes an Application that has successfully performed an Update call or failed a ValidateSelfieRequest or a ValidateDocumentsRequest. 'KYC In Progress' describes an Application that has successfully performed a ValidateSelfieRequest or ValidateDocumentsRequest. 'KYC Success' describes an Application that has successfully performed KYC checks as a result of a ValidateApplicationRequest. 'KYC Failed' describes an Application that has failed KYC as a result of a ValidateApplicationRequest. 'KYC Rejected' describes an Application that has no remaining KYC attempts. 'Application Accepted' describes an application that has successfully completed an AcceptApplicationRequest. 'Application Processed' describes an application that has been successfully completed. 'Application Cancelled' describes an application that has been cancelled by the customer. |
| request_status                | [string](#string) |       | The success status of the request.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| request_status_reason         | [string](#string) |       | The detailed reason for the request status.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| is_retry_operation_restricted | [bool](#bool)     |       | This is for internal use only. Do not use.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| remaining_allowed_retries     | [int64](#int64)   |       | The remaining KYC retries allowed.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |

<a name="thebaasco.tenant.onboarding.v1.OtherResidence"></a>

### OtherResidence

OtherResidence is a message for providing alternate countries and places of residence.

| Field                   | Type              | Label | Description                                                                                        |
| ----------------------- | ----------------- | ----- | -------------------------------------------------------------------------------------------------- |
| other_residence_country | [string](#string) |       | The name of the other country of residence.                                                        |
| tin_token               | [string](#string) |       | A tokenized UUID representation of the unique TIN for the country of residence. Optional.          |
| citizenship             | [string](#string) |       | The citizenship status in the country of residence. Required if residence country is US or Canada. |

<a name="thebaasco.tenant.onboarding.v1.PersonalDetails"></a>

### PersonalDetails

PersonalDetails defines a message containing the customer's personal details.

| Field         | Type                                  | Label | Description                                                                                                                                |
| ------------- | ------------------------------------- | ----- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| first_name    | [string](#string)                     |       | The customer first name. This value is required and has a maximum of 50 characters. Accepted characters: -, ', French accents, 0-9, space. |
| middle_name   | [string](#string)                     |       | The customer middle name. This value is optional and has a maximum of 50 characters. Accepted characters: -,', French accents, 0-9, space. |
| last_name     | [string](#string)                     |       | The customer last name. This value is required and has a maximum of 50 characters. Accepted characters: -, ', French accents, 0-9, space.  |
| date_of_birth | [google.type.Date](#google.type.Date) |       | The customer's date of birth.                                                                                                              |

<a name="thebaasco.tenant.onboarding.v1.UpdateAccountDetailsRequest"></a>

### UpdateAccountDetailsRequest

UpdateAccountDetailsRequest is used to perform the asynchronous
operation of updating the customer account details in the onboarding application.
Performing this operation will generate an asynchronous response
type of UpdateAccountDetailsResponse on the response topic.

| Field                     | Type                                                                       | Label | Description                                                 |
| ------------------------- | -------------------------------------------------------------------------- | ----- | ----------------------------------------------------------- |
| onboarding_application_id | [string](#string)                                                          |       | The UUID of the onboarding application to update.           |
| account_details           | [AccountUsageDetails](#thebaasco.tenant.onboarding.v1.AccountUsageDetails) |       | The updated account usage details. See AccountUsageDetails. |

<a name="thebaasco.tenant.onboarding.v1.UpdateAccountDetailsResponse"></a>

### UpdateAccountDetailsResponse

UpdateAccountDetailsResponse is the response message for UpdateAccountDetailsRequest.
See UpdateAccountDetailsRequest for details.

| Field         | Type                                                                                                     | Label | Description                                                                                       |
| ------------- | -------------------------------------------------------------------------------------------------------- | ----- | ------------------------------------------------------------------------------------------------- |
| basic_details | [OnboardingOperationResponseDetails](#thebaasco.tenant.onboarding.v1.OnboardingOperationResponseDetails) |       | The basic details of the onboarding application response. See OnboardingOperationResponseDetails. |

<a name="thebaasco.tenant.onboarding.v1.UpdateAddressesRequest"></a>

### UpdateAddressesRequest

UpdateAddressesRequest is used to perform the asynchronous
operation of updating the addresses occupied by the customer in the
onboarding application. Performing this operation will generate an asynchronous
response type of UpdateAddressesResponse on the response topic.

| Field                     | Type                                               | Label    | Description                                                        |
| ------------------------- | -------------------------------------------------- | -------- | ------------------------------------------------------------------ |
| onboarding_application_id | [string](#string)                                  |          | The UUID of the onboarding application to update.                  |
| addresses                 | [Address](#thebaasco.tenant.onboarding.v1.Address) | repeated | The collection of addresses occupied by the customer. See Address. |

<a name="thebaasco.tenant.onboarding.v1.UpdateAddressesResponse"></a>

### UpdateAddressesResponse

UpdateAddressesResponse is the response message for UpdateAddressesRequest.
See UpdateAddressesRequest for details.

| Field         | Type                                                                                                     | Label | Description                                                                                       |
| ------------- | -------------------------------------------------------------------------------------------------------- | ----- | ------------------------------------------------------------------------------------------------- |
| basic_details | [OnboardingOperationResponseDetails](#thebaasco.tenant.onboarding.v1.OnboardingOperationResponseDetails) |       | The basic details of the onboarding application response. See OnboardingOperationResponseDetails. |

<a name="thebaasco.tenant.onboarding.v1.UpdateCommunicationPreferencesRequest"></a>

### UpdateCommunicationPreferencesRequest

UpdateCommunicationPreferencesRequest is used to perform the asynchronous
operation of updating the communication preferences of the customer in the
onboarding application. Performing this operation will generate an asynchronous
response type of UpdateCommunicationPreferencesResponse on the response topic.

| Field                       | Type              | Label | Description                                                       |
| --------------------------- | ----------------- | ----- | ----------------------------------------------------------------- |
| onboarding_application_id   | [string](#string) |       | The UUID of the onboarding application to update.                 |
| customer_communication_pref | [string](#string) |       | The customer communication preference. See OnboardingApplication. |

<a name="thebaasco.tenant.onboarding.v1.UpdateCommunicationPreferencesResponse"></a>

### UpdateCommunicationPreferencesResponse

UpdateCommunicationPreferencesResponse is the response message for
UpdateCommunicationPreferencesRequest.
See UpdateCommunicationPreferencesRequest for details.

| Field         | Type                                                                                                     | Label | Description                                                                                       |
| ------------- | -------------------------------------------------------------------------------------------------------- | ----- | ------------------------------------------------------------------------------------------------- |
| basic_details | [OnboardingOperationResponseDetails](#thebaasco.tenant.onboarding.v1.OnboardingOperationResponseDetails) |       | The basic details of the onboarding application response. See OnboardingOperationResponseDetails. |

<a name="thebaasco.tenant.onboarding.v1.UpdateConsentsRequest"></a>

### UpdateConsentsRequest

UpdateConsentsRequest is used to perform the asynchronous
operation of updating the consents received from the customer for the
onboarding application. Performing this operation will generate an asynchronous
response type of UpdateConsentsResponse on the response topic.

| Field                     | Type                                               | Label    | Description                                       |
| ------------------------- | -------------------------------------------------- | -------- | ------------------------------------------------- |
| onboarding_application_id | [string](#string)                                  |          | The UUID of the onboarding application to update. |
| consents                  | [Consent](#thebaasco.tenant.onboarding.v1.Consent) | repeated | The updated collection of consents. See Consent.  |

<a name="thebaasco.tenant.onboarding.v1.UpdateConsentsResponse"></a>

### UpdateConsentsResponse

UpdateConsentsResponse is the response message for UpdateConsentsRequest.
See UpdateConsentsRequest for details.

| Field         | Type                                                                                                     | Label | Description                                                                                       |
| ------------- | -------------------------------------------------------------------------------------------------------- | ----- | ------------------------------------------------------------------------------------------------- |
| basic_details | [OnboardingOperationResponseDetails](#thebaasco.tenant.onboarding.v1.OnboardingOperationResponseDetails) |       | The basic details of the onboarding application response. See OnboardingOperationResponseDetails. |

<a name="thebaasco.tenant.onboarding.v1.UpdateContactDetailsRequest"></a>

### UpdateContactDetailsRequest

UpdateContactDetailsRequest is used to perform the asynchronous
operation of updating the customer contact details in the onboarding application.
Performing this operation will generate an asynchronous response
type of UpdateContactDetailsResponse on the response topic.

| Field                     | Type                                                             | Label | Description                                       |
| ------------------------- | ---------------------------------------------------------------- | ----- | ------------------------------------------------- |
| onboarding_application_id | [string](#string)                                                |       | The UUID of the onboarding application to update. |
| customer_contact_details  | [ContactDetails](#thebaasco.tenant.onboarding.v1.ContactDetails) |       | The updated contact details. See ContactDetails.  |

<a name="thebaasco.tenant.onboarding.v1.UpdateContactDetailsResponse"></a>

### UpdateContactDetailsResponse

UpdateContactDetailsResponse is the response message for UpdateContactDetailsRequest.
See UpdateContactDetailsRequest for details.

| Field         | Type                                                                                                     | Label | Description                                                                                       |
| ------------- | -------------------------------------------------------------------------------------------------------- | ----- | ------------------------------------------------------------------------------------------------- |
| basic_details | [OnboardingOperationResponseDetails](#thebaasco.tenant.onboarding.v1.OnboardingOperationResponseDetails) |       | The basic details of the onboarding application response. See OnboardingOperationResponseDetails. |

<a name="thebaasco.tenant.onboarding.v1.UpdateCustomerResidencyRequest"></a>

### UpdateCustomerResidencyRequest

UpdateCustomerResidencyRequest is used to perform the asynchronous
operation of updating the customer residency status in the onboarding application.
Performing this operation will generate an asynchronous response
type of UpdateCustomerResidencyResponse on the response topic.

| Field                     | Type                                                                   | Label | Description                                           |
| ------------------------- | ---------------------------------------------------------------------- | ----- | ----------------------------------------------------- |
| onboarding_application_id | [string](#string)                                                      |       | The UUID of the onboarding application to update.     |
| residency                 | [CustomerResidency](#thebaasco.tenant.onboarding.v1.CustomerResidency) |       | The customer residency status. See CustomerResidency. |

<a name="thebaasco.tenant.onboarding.v1.UpdateCustomerResidencyResponse"></a>

### UpdateCustomerResidencyResponse

UpdateCustomerResidencyResponse is the response message for
UpdateCustomerResidencyRequest. See UpdateCustomerResidencyRequest for details.

| Field         | Type                                                                                                     | Label | Description                                                                                       |
| ------------- | -------------------------------------------------------------------------------------------------------- | ----- | ------------------------------------------------------------------------------------------------- |
| basic_details | [OnboardingOperationResponseDetails](#thebaasco.tenant.onboarding.v1.OnboardingOperationResponseDetails) |       | The basic details of the onboarding application response. See OnboardingOperationResponseDetails. |

<a name="thebaasco.tenant.onboarding.v1.UpdateDisclosuresRequest"></a>

### UpdateDisclosuresRequest

UpdateDisclosuresRequest is used to perform the asynchronous
operation of updating the disclosures received by the customer in the
onboarding application. Performing this operation will generate an asynchronous
response type of UpdateDisclosuresResponse on the response topic.

| Field                     | Type                                                     | Label    | Description                                            |
| ------------------------- | -------------------------------------------------------- | -------- | ------------------------------------------------------ |
| onboarding_application_id | [string](#string)                                        |          | The UUID of the onboarding application to update.      |
| disclosures               | [Disclosure](#thebaasco.tenant.onboarding.v1.Disclosure) | repeated | The updated collection of disclosures. See Disclosure. |

<a name="thebaasco.tenant.onboarding.v1.UpdateDisclosuresResponse"></a>

### UpdateDisclosuresResponse

UpdateDisclosuresResponse is the response message for UpdateDisclosuresRequest.
See UpdateDisclosuresRequest for details.

| Field         | Type                                                                                                     | Label | Description                                                                                       |
| ------------- | -------------------------------------------------------------------------------------------------------- | ----- | ------------------------------------------------------------------------------------------------- |
| basic_details | [OnboardingOperationResponseDetails](#thebaasco.tenant.onboarding.v1.OnboardingOperationResponseDetails) |       | The basic details of the onboarding application response. See OnboardingOperationResponseDetails. |

<a name="thebaasco.tenant.onboarding.v1.UpdateEmploymentRequest"></a>

### UpdateEmploymentRequest

UpdateEmploymentRequest is used to perform the asynchronous
operation of updating the customer employment information in the
onboarding application. Performing this operation will generate an
asynchronous response type of UpdateEmploymentResponse on the response topic.

| Field                     | Type                                                     | Label | Description                                         |
| ------------------------- | -------------------------------------------------------- | ----- | --------------------------------------------------- |
| onboarding_application_id | [string](#string)                                        |       | The UUID of the onboarding application to update.   |
| employment_info           | [Employment](#thebaasco.tenant.onboarding.v1.Employment) |       | The updated employment information. See Employment. |

<a name="thebaasco.tenant.onboarding.v1.UpdateEmploymentResponse"></a>

### UpdateEmploymentResponse

UpdateEmploymentResponse is the response message for UpdateEmploymentRequest.
See UpdateEmploymentRequest for details.

| Field         | Type                                                                                                     | Label | Description                                                                                       |
| ------------- | -------------------------------------------------------------------------------------------------------- | ----- | ------------------------------------------------------------------------------------------------- |
| basic_details | [OnboardingOperationResponseDetails](#thebaasco.tenant.onboarding.v1.OnboardingOperationResponseDetails) |       | The basic details of the onboarding application response. See OnboardingOperationResponseDetails. |

<a name="thebaasco.tenant.onboarding.v1.UpdatePersonalDetailsRequest"></a>

### UpdatePersonalDetailsRequest

UpdatePersonalDetailsRequest is used to perform the asynchronous
operation of updating the customer personal details in the onboarding application.
Performing this operation will generate an asynchronous response
type of UpdatePersonalDetailsResponse on the response topic.

| Field                     | Type                                                               | Label | Description                                       |
| ------------------------- | ------------------------------------------------------------------ | ----- | ------------------------------------------------- |
| onboarding_application_id | [string](#string)                                                  |       | The UUID of the onboarding application to update. |
| customer_personal_details | [PersonalDetails](#thebaasco.tenant.onboarding.v1.PersonalDetails) |       | The updated personal details.                     |

<a name="thebaasco.tenant.onboarding.v1.UpdatePersonalDetailsResponse"></a>

### UpdatePersonalDetailsResponse

UpdatePersonalDetailsResponse is the response message for UpdatePersonalDetailsRequest.
See UpdatePersonalDetailsRequest for details.

| Field         | Type                                                                                                     | Label | Description                                                                                       |
| ------------- | -------------------------------------------------------------------------------------------------------- | ----- | ------------------------------------------------------------------------------------------------- |
| basic_details | [OnboardingOperationResponseDetails](#thebaasco.tenant.onboarding.v1.OnboardingOperationResponseDetails) |       | The basic details of the onboarding application response. See OnboardingOperationResponseDetails. |

<a name="thebaasco.tenant.onboarding.v1.ValidateApplicationRequest"></a>

### ValidateApplicationRequest

ValidateApplicationRequest is used to perform the asynchronous
operation of validating the entire onboarding application and all documents
received from the customer. Performing this operation will generate an
asynchronous response type of ValidateApplicationResponse on the response topic.

| Field                     | Type              | Label | Description                                         |
| ------------------------- | ----------------- | ----- | --------------------------------------------------- |
| onboarding_application_id | [string](#string) |       | The UUID of the onboarding application to validate. |

<a name="thebaasco.tenant.onboarding.v1.ValidateApplicationResponse"></a>

### ValidateApplicationResponse

ValidateApplicationResponse is the response message for
ValidateApplicationRequest. See ValidateApplicationRequest for details.

| Field         | Type                                                                                                     | Label | Description                                                                                       |
| ------------- | -------------------------------------------------------------------------------------------------------- | ----- | ------------------------------------------------------------------------------------------------- |
| basic_details | [OnboardingOperationResponseDetails](#thebaasco.tenant.onboarding.v1.OnboardingOperationResponseDetails) |       | The basic details of the onboarding application response. See OnboardingOperationResponseDetails. |

<a name="thebaasco.tenant.onboarding.v1.ValidateDocumentsRequest"></a>

### ValidateDocumentsRequest

ValidateDocumentsRequest is used to perform the asynchronous
operation of validating the identification documents received from the
customer in the onboarding application. Performing this operation will
generate an asynchronous response type of ValidateDocumentsResponse on the
response topic.

| Field                     | Type                                                         | Label    | Description                                                |
| ------------------------- | ------------------------------------------------------------ | -------- | ---------------------------------------------------------- |
| onboarding_application_id | [string](#string)                                            |          | The UUID of the onboarding application to validate.        |
| document_details          | [DocumentInfo](#thebaasco.tenant.onboarding.v1.DocumentInfo) | repeated | The collection of documents to validate. See DocumentInfo. |

<a name="thebaasco.tenant.onboarding.v1.ValidateDocumentsResponse"></a>

### ValidateDocumentsResponse

ValidateDocumentsResponse is the response message for ValidateDocumentsRequest.
See ValidateDocumentsRequest for details.

| Field         | Type                                                                                                     | Label | Description                                                                                       |
| ------------- | -------------------------------------------------------------------------------------------------------- | ----- | ------------------------------------------------------------------------------------------------- |
| basic_details | [OnboardingOperationResponseDetails](#thebaasco.tenant.onboarding.v1.OnboardingOperationResponseDetails) |       | The basic details of the onboarding application response. See OnboardingOperationResponseDetails. |

<a name="thebaasco.tenant.onboarding.v1.ValidateSelfieRequest"></a>

### ValidateSelfieRequest

ValidateSelfieRequest is used to perform the asynchronous
operation of validating the selfie pictures received from the
customer in the onboarding application. Performing this operation will
generate an asynchronous response type of ValidateSelfieResponse on the
response topic.

| Field                     | Type              | Label | Description                                                         |
| ------------------------- | ----------------- | ----- | ------------------------------------------------------------------- |
| onboarding_application_id | [string](#string) |       | The UUID of the onboarding application to validate.                 |
| file_name                 | [string](#string) |       | The name of the selfie file to validate. See OnboardingApplication. |

<a name="thebaasco.tenant.onboarding.v1.ValidateSelfieResponse"></a>

### ValidateSelfieResponse

ValidateSelfieResponse is the response message for ValidateSelfieRequest.
See ValidateSelfieRequest for details.

| Field         | Type                                                                                                     | Label | Description                                                                                       |
| ------------- | -------------------------------------------------------------------------------------------------------- | ----- | ------------------------------------------------------------------------------------------------- |
| basic_details | [OnboardingOperationResponseDetails](#thebaasco.tenant.onboarding.v1.OnboardingOperationResponseDetails) |       | The basic details of the onboarding application response. See OnboardingOperationResponseDetails. |

<a name="thebaasco.tenant.onboarding.v1.Onboarding"></a>

### Onboarding

Onboarding provides a way to upload documents and check the status of an
onboarding application.

| Method Name          | Request Type                                                                               | Response Type                                                                                | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| -------------------- | ------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| GetApplicationStatus | [GetApplicationStatusRequest](#thebaasco.tenant.onboarding.v1.GetApplicationStatusRequest) | [GetApplicationStatusResponse](#thebaasco.tenant.onboarding.v1.GetApplicationStatusResponse) | GetApplicationStatus gets the current status of a given onboarding application. It provides a way to handle non-authenticated' interrupted sessions. See GetApplicationStatusRequest for details.                                                                                                                                                                                                                                                                     |
| CreateFileUploadURL  | [CreateFileUploadUrlRequest](#thebaasco.tenant.onboarding.v1.CreateFileUploadUrlRequest)   | [CreateFileUploadUrlResponse](#thebaasco.tenant.onboarding.v1.CreateFileUploadUrlResponse)   | CreateFileUploadUrl is used as an initial step for the upload of a customer KYC document. <br/>1. The UI will call this operation to generate upload-ready url for each file needed. <br/>2. The UI will perform the upload of each file. <br/>3. The UI will call UploadValidateDocument including file_name of each document along with its corresponding meta-data The filename will be used to match the document's meta-data with the uploaded content in step 2 |

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