# Protocol Documentation
<a name="top"></a>


## onboarding_v1.proto


<a name="thebaasco.tenant.onboarding.v1.AcceptApplicationRequest"></a>

### AcceptApplicationRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| onboarding_application_id | [string](#string) |  |  |






<a name="thebaasco.tenant.onboarding.v1.AcceptApplicationResponse"></a>

### AcceptApplicationResponse



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| basic_details | [OnboardingOperationResponseDetails](#thebaasco.tenant.onboarding.v1.OnboardingOperationResponseDetails) |  |  |






<a name="thebaasco.tenant.onboarding.v1.AccountUsageDetails"></a>

### AccountUsageDetails



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| account_purpose | [string](#string) |  | Required, &#34;Accepted values are defined at https://thebaasco.atlassian.net/browse/PB-706&#34; |
| primary_source_of_funds | [string](#string) | repeated | required, Accepted Values are here – https://thebaasco.atlassian.net/browse/PB-707&#34; |






<a name="thebaasco.tenant.onboarding.v1.Address"></a>

### Address



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| type | [string](#string) |  | Required, HOME or WORK. Will be validated based on reference data values |
| organization | [string](#string) |  |  |
| line_1 | [string](#string) |  |  |
| line_2 | [string](#string) |  |  |
| line_3 | [string](#string) |  |  |
| line_4 | [string](#string) |  |  |
| line_5 | [string](#string) |  |  |
| line_6 | [string](#string) |  |  |
| line_7 | [string](#string) |  |  |
| line_8 | [string](#string) |  |  |
| locality | [string](#string) |  | City |
| dependent_locality | [string](#string) |  |  |
| double_dependent_locality | [string](#string) |  |  |
| administrative_area | [string](#string) |  |  |
| sub_administrative_area | [string](#string) |  |  |
| area | [string](#string) |  | State |
| postal_code | [string](#string) |  |  |
| country | [string](#string) |  |  |






<a name="thebaasco.tenant.onboarding.v1.Consent"></a>

### Consent



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| consent_type | [string](#string) |  | Required, &#34;values for each consent type here - https://thebaasco.atlassian.net/browse/PB-741&#34; |
| consent_timestamp | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  |  |
| document_name_and_version | [string](#string) |  | Required, URL, Somehow we must store the document version of each type of consent |
| view_timestamp | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  |  |






<a name="thebaasco.tenant.onboarding.v1.ContactDetails"></a>

### ContactDetails



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| phone_number | [string](#string) |  |  |
| customer_email | [string](#string) |  | required, should reject if Email&#39; field does not contain &#39;@&#39;, &#39;.com&#39;, &#39;.biz’, &#39;.ca&#39; or any other type of standard email format |






<a name="thebaasco.tenant.onboarding.v1.CustomerResidency"></a>

### CustomerResidency



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| is_canadian_tax_resident | [bool](#bool) |  | Are you resident of Canada? |
| other_residences | [OtherResidence](#thebaasco.tenant.onboarding.v1.OtherResidence) | repeated | US PB-705 |
| sin_token | [string](#string) |  | Allow empty and only store UUID token to avoid storing PII |
| citizenship | [string](#string) |  | US PB-703 - select |






<a name="thebaasco.tenant.onboarding.v1.Disclosure"></a>

### Disclosure



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| disclosure_type | [string](#string) |  | required, &#34;Enumerated values https://thebaasco.atlassian.net/browse/PB-745&#34; |
| timestamp | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  |  |
| document_name_and_version | [string](#string) |  |  |






<a name="thebaasco.tenant.onboarding.v1.DocumentInfo"></a>

### DocumentInfo
Request/Response Messages
******************************************


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| document_type | [string](#string) |  |  |
| document_country | [string](#string) |  |  |
| front_image_file_name | [string](#string) |  | Google cloud storage file name; max image size for Acuant has to be &lt;4MB. |
| back_image_file_name | [string](#string) |  | Google cloud storage file name; max image size for Acuant has to be &lt;4MB. |






<a name="thebaasco.tenant.onboarding.v1.Employment"></a>

### Employment



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| employment_status | [string](#string) |  | required: &#34; Values defined here – https://thebaasco.atlassian.net/browse/PB-645&#34; |
| employer_name | [string](#string) |  | Required for 0-2 status , length 50, sql injection validation/policies |
| industry | [string](#string) |  | Required for 0-2 status |
| occupation | [string](#string) |  |  |
| startDate | [google.type.Date](#google.type.Date) |  | format Month/year |
| endDate | [google.type.Date](#google.type.Date) |  | format Month/year |
| work_address | [Address](#thebaasco.tenant.onboarding.v1.Address) |  | Required for 0-2 status |
| work_phone_number | [string](#string) |  | Required for 0-2 |






<a name="thebaasco.tenant.onboarding.v1.FinalizeApplicationCreateCustomerRequest"></a>

### FinalizeApplicationCreateCustomerRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| onboarding_application_id | [string](#string) |  |  |






<a name="thebaasco.tenant.onboarding.v1.FinalizeApplicationCreateCustomerResponse"></a>

### FinalizeApplicationCreateCustomerResponse



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| basic_details | [OnboardingOperationResponseDetails](#thebaasco.tenant.onboarding.v1.OnboardingOperationResponseDetails) |  |  |
| customer_id | [string](#string) |  |  |






<a name="thebaasco.tenant.onboarding.v1.FinalizeApplicationCreateProductRequest"></a>

### FinalizeApplicationCreateProductRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| onboarding_application_id | [string](#string) |  |  |






<a name="thebaasco.tenant.onboarding.v1.FinalizeApplicationCreateProductResponse"></a>

### FinalizeApplicationCreateProductResponse



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| basic_details | [OnboardingOperationResponseDetails](#thebaasco.tenant.onboarding.v1.OnboardingOperationResponseDetails) |  |  |
| products | [OnboardedProduct](#thebaasco.tenant.onboarding.v1.OnboardedProduct) | repeated |  |






<a name="thebaasco.tenant.onboarding.v1.GetApplicationStatusRequest"></a>

### GetApplicationStatusRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| application_id | [string](#string) |  |  |






<a name="thebaasco.tenant.onboarding.v1.GetApplicationStatusResponse"></a>

### GetApplicationStatusResponse



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| basic_details | [OnboardingOperationResponseDetails](#thebaasco.tenant.onboarding.v1.OnboardingOperationResponseDetails) |  |  |






<a name="thebaasco.tenant.onboarding.v1.InitiateApplicationRequest"></a>

### InitiateApplicationRequest
InitiateApplication


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| customer_id | [string](#string) |  |  |
| bundle_id | [string](#string) |  |  |






<a name="thebaasco.tenant.onboarding.v1.InitiateApplicationResponse"></a>

### InitiateApplicationResponse



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| basic_details | [OnboardingOperationResponseDetails](#thebaasco.tenant.onboarding.v1.OnboardingOperationResponseDetails) |  |  |






<a name="thebaasco.tenant.onboarding.v1.OnboardedProduct"></a>

### OnboardedProduct



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| product_id | [string](#string) |  |  |
| source_domain | [string](#string) |  |  |






<a name="thebaasco.tenant.onboarding.v1.OnboardingApplication"></a>

### OnboardingApplication



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| application_id | [string](#string) |  |  |
| customer_id | [string](#string) |  |  |
| bundle_id | [string](#string) |  |  |
| status | [string](#string) |  |  |
| customer_personal_details | [PersonalDetails](#thebaasco.tenant.onboarding.v1.PersonalDetails) |  |  |
| customer_contact_details | [ContactDetails](#thebaasco.tenant.onboarding.v1.ContactDetails) |  |  |
| account_details | [AccountUsageDetails](#thebaasco.tenant.onboarding.v1.AccountUsageDetails) |  |  |
| customer_employment | [Employment](#thebaasco.tenant.onboarding.v1.Employment) | repeated |  |
| customer_residency | [CustomerResidency](#thebaasco.tenant.onboarding.v1.CustomerResidency) |  |  |
| consents | [Consent](#thebaasco.tenant.onboarding.v1.Consent) | repeated |  |
| disclosures | [Disclosure](#thebaasco.tenant.onboarding.v1.Disclosure) | repeated |  |
| customer_residence_address | [Address](#thebaasco.tenant.onboarding.v1.Address) | repeated |  |
| customer_identification_documents | [DocumentInfo](#thebaasco.tenant.onboarding.v1.DocumentInfo) | repeated |  |
| customer_communication_pref | [string](#string) |  |  |






<a name="thebaasco.tenant.onboarding.v1.OnboardingApplicationCustomerOnboardedEvent"></a>

### OnboardingApplicationCustomerOnboardedEvent



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| event_id | [string](#string) |  |  |
| application_id | [string](#string) |  |  |
| tenant_id | [int32](#int32) |  |  |
| event_time | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  |  |






<a name="thebaasco.tenant.onboarding.v1.OnboardingApplicationDataConfirmedEvent"></a>

### OnboardingApplicationDataConfirmedEvent
This event is emitted once a new customer is onboarded. As part of PB-3330, this
event will also be emitted when onboarding a new product for an existing customer


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| event_id | [string](#string) |  |  |
| application | [OnboardingApplication](#thebaasco.tenant.onboarding.v1.OnboardingApplication) |  |  |
| tenant_id | [int32](#int32) |  |  |
| event_time | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  |  |






<a name="thebaasco.tenant.onboarding.v1.OnboardingApplicationProductOnboardedEvent"></a>

### OnboardingApplicationProductOnboardedEvent



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| event_id | [string](#string) |  |  |
| application_id | [string](#string) |  |  |
| products | [OnboardedProduct](#thebaasco.tenant.onboarding.v1.OnboardedProduct) | repeated |  |
| tenant_id | [int32](#int32) |  |  |
| event_time | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  |  |






<a name="thebaasco.tenant.onboarding.v1.OnboardingOperationResponseDetails"></a>

### OnboardingOperationResponseDetails
Generic Create/Update message type


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| application_id | [string](#string) |  |  |
| onboarding_application_status | [string](#string) |  |  |
| request_status | [string](#string) |  |  |
| request_status_reason | [string](#string) |  |  |
| is_retry_operation_restricted | [bool](#bool) |  |  |
| remaining_allowed_retries | [int64](#int64) |  |  |






<a name="thebaasco.tenant.onboarding.v1.OtherResidence"></a>

### OtherResidence



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| other_residence_country | [string](#string) |  |  |
| tin_token | [string](#string) |  | Allow empty and only store UUID token to avoid storing PII |
| citizenship | [string](#string) |  | Required if us or canada |






<a name="thebaasco.tenant.onboarding.v1.PersonalDetails"></a>

### PersonalDetails



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| first_name | [string](#string) |  | required maxlen=50 AN characters, hyphen (-), apostrophe, (&#39;), French accents, numbers (0-9) |
| middle_name | [string](#string) |  | not required maxlen=50 AN characters, hyphen (-), apostrophe, (&#39;), French accents, numbers (0-9) |
| last_name | [string](#string) |  | required maxlen=50 AN characters, hyphen (-), apostrophe, (&#39;), French accents, numbers (0-9) |
| date_of_birth | [google.type.Date](#google.type.Date) |  |  |






<a name="thebaasco.tenant.onboarding.v1.UpdateAccountDetailsRequest"></a>

### UpdateAccountDetailsRequest
UpdateAccountDetails


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| onboarding_application_id | [string](#string) |  |  |
| account_details | [AccountUsageDetails](#thebaasco.tenant.onboarding.v1.AccountUsageDetails) |  |  |






<a name="thebaasco.tenant.onboarding.v1.UpdateAccountDetailsResponse"></a>

### UpdateAccountDetailsResponse



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| basic_details | [OnboardingOperationResponseDetails](#thebaasco.tenant.onboarding.v1.OnboardingOperationResponseDetails) |  |  |






<a name="thebaasco.tenant.onboarding.v1.UpdateAddressesRequest"></a>

### UpdateAddressesRequest
UpdateAddresses


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| onboarding_application_id | [string](#string) |  |  |
| addresses | [Address](#thebaasco.tenant.onboarding.v1.Address) | repeated |  |






<a name="thebaasco.tenant.onboarding.v1.UpdateAddressesResponse"></a>

### UpdateAddressesResponse



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| basic_details | [OnboardingOperationResponseDetails](#thebaasco.tenant.onboarding.v1.OnboardingOperationResponseDetails) |  |  |






<a name="thebaasco.tenant.onboarding.v1.UpdateCommunicationPreferencesRequest"></a>

### UpdateCommunicationPreferencesRequest
UpdateCommunicationPreferences


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| onboarding_application_id | [string](#string) |  |  |
| customer_communication_pref | [string](#string) |  |  |






<a name="thebaasco.tenant.onboarding.v1.UpdateCommunicationPreferencesResponse"></a>

### UpdateCommunicationPreferencesResponse



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| basic_details | [OnboardingOperationResponseDetails](#thebaasco.tenant.onboarding.v1.OnboardingOperationResponseDetails) |  |  |






<a name="thebaasco.tenant.onboarding.v1.UpdateConsentsRequest"></a>

### UpdateConsentsRequest
UpdateConsents


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| onboarding_application_id | [string](#string) |  |  |
| consents | [Consent](#thebaasco.tenant.onboarding.v1.Consent) | repeated |  |






<a name="thebaasco.tenant.onboarding.v1.UpdateConsentsResponse"></a>

### UpdateConsentsResponse



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| basic_details | [OnboardingOperationResponseDetails](#thebaasco.tenant.onboarding.v1.OnboardingOperationResponseDetails) |  |  |






<a name="thebaasco.tenant.onboarding.v1.UpdateContactDetailsRequest"></a>

### UpdateContactDetailsRequest
UpdateContactDetails


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| onboarding_application_id | [string](#string) |  |  |
| customer_contact_details | [ContactDetails](#thebaasco.tenant.onboarding.v1.ContactDetails) |  |  |






<a name="thebaasco.tenant.onboarding.v1.UpdateContactDetailsResponse"></a>

### UpdateContactDetailsResponse



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| basic_details | [OnboardingOperationResponseDetails](#thebaasco.tenant.onboarding.v1.OnboardingOperationResponseDetails) |  |  |






<a name="thebaasco.tenant.onboarding.v1.UpdateCustomerResidencyRequest"></a>

### UpdateCustomerResidencyRequest
UpdateCustomerResidency


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| onboarding_application_id | [string](#string) |  |  |
| residency | [CustomerResidency](#thebaasco.tenant.onboarding.v1.CustomerResidency) |  |  |






<a name="thebaasco.tenant.onboarding.v1.UpdateCustomerResidencyResponse"></a>

### UpdateCustomerResidencyResponse



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| basic_details | [OnboardingOperationResponseDetails](#thebaasco.tenant.onboarding.v1.OnboardingOperationResponseDetails) |  |  |






<a name="thebaasco.tenant.onboarding.v1.UpdateDisclosuresRequest"></a>

### UpdateDisclosuresRequest
UpdateDisclosures


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| onboarding_application_id | [string](#string) |  |  |
| disclosures | [Disclosure](#thebaasco.tenant.onboarding.v1.Disclosure) | repeated |  |






<a name="thebaasco.tenant.onboarding.v1.UpdateDisclosuresResponse"></a>

### UpdateDisclosuresResponse



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| basic_details | [OnboardingOperationResponseDetails](#thebaasco.tenant.onboarding.v1.OnboardingOperationResponseDetails) |  |  |






<a name="thebaasco.tenant.onboarding.v1.UpdateEmploymentRequest"></a>

### UpdateEmploymentRequest
UpdateEmployment


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| onboarding_application_id | [string](#string) |  |  |
| employment_info | [Employment](#thebaasco.tenant.onboarding.v1.Employment) |  |  |






<a name="thebaasco.tenant.onboarding.v1.UpdateEmploymentResponse"></a>

### UpdateEmploymentResponse



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| basic_details | [OnboardingOperationResponseDetails](#thebaasco.tenant.onboarding.v1.OnboardingOperationResponseDetails) |  |  |






<a name="thebaasco.tenant.onboarding.v1.UpdatePersonalDetailsRequest"></a>

### UpdatePersonalDetailsRequest
UpdatePersonalDetails


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| onboarding_application_id | [string](#string) |  |  |
| customer_personal_details | [PersonalDetails](#thebaasco.tenant.onboarding.v1.PersonalDetails) |  |  |






<a name="thebaasco.tenant.onboarding.v1.UpdatePersonalDetailsResponse"></a>

### UpdatePersonalDetailsResponse



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| basic_details | [OnboardingOperationResponseDetails](#thebaasco.tenant.onboarding.v1.OnboardingOperationResponseDetails) |  |  |






<a name="thebaasco.tenant.onboarding.v1.ValidateApplicationRequest"></a>

### ValidateApplicationRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| onboarding_application_id | [string](#string) |  |  |






<a name="thebaasco.tenant.onboarding.v1.ValidateApplicationResponse"></a>

### ValidateApplicationResponse



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| basic_details | [OnboardingOperationResponseDetails](#thebaasco.tenant.onboarding.v1.OnboardingOperationResponseDetails) |  |  |






<a name="thebaasco.tenant.onboarding.v1.ValidateDocumentsRequest"></a>

### ValidateDocumentsRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| onboarding_application_id | [string](#string) |  |  |
| document_details | [DocumentInfo](#thebaasco.tenant.onboarding.v1.DocumentInfo) | repeated |  |






<a name="thebaasco.tenant.onboarding.v1.ValidateDocumentsResponse"></a>

### ValidateDocumentsResponse



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| basic_details | [OnboardingOperationResponseDetails](#thebaasco.tenant.onboarding.v1.OnboardingOperationResponseDetails) |  |  |






<a name="thebaasco.tenant.onboarding.v1.ValidateSelfieRequest"></a>

### ValidateSelfieRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| onboarding_application_id | [string](#string) |  |  |
| file_name | [string](#string) |  |  |






<a name="thebaasco.tenant.onboarding.v1.ValidateSelfieResponse"></a>

### ValidateSelfieResponse



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| basic_details | [OnboardingOperationResponseDetails](#thebaasco.tenant.onboarding.v1.OnboardingOperationResponseDetails) |  |  |





 

 

 


<a name="thebaasco.tenant.onboarding.v1.Onboarding"></a>

### Onboarding
Service Operations

| Method Name | Request Type | Response Type | Description |
| ----------- | ------------ | ------------- | ------------|
| GetApplicationStatus | [GetApplicationStatusRequest](#thebaasco.tenant.onboarding.v1.GetApplicationStatusRequest) | [GetApplicationStatusResponse](#thebaasco.tenant.onboarding.v1.GetApplicationStatusResponse) | In order to support non-authenticated users&#39; interrupted sessions, this method will return app ID and status of the application |
| CreateFileUploadURL | [CreateFileUploadUrlRequest](#thebaasco.tenant.onboarding.v1.CreateFileUploadUrlRequest) | [CreateFileUploadUrlResponse](#thebaasco.tenant.onboarding.v1.CreateFileUploadUrlResponse) | CreateFileUploadUrl is used as an initial step for the upload of a customer KYC document 1. The UI will call this operation to generate upload-ready url for each file needed 2. The UI will perform the upload of each file 3. The UI will call UploadValidateDocument including file_name of each document along with its corresponding meta-data filename will be used to match the document&#39;s meta-data with the uploaded content in step 2 |

 



<a name="onboarding_v1_fileupload.proto"></a>
<p align="right"><a href="#top">Top</a></p>

## onboarding_v1_fileupload.proto



<a name="thebaasco.tenant.onboarding.v1.CreateFileUploadUrlRequest"></a>

### CreateFileUploadUrlRequest
CreateFileUploadUrlRequest is used to ask the server for a url to use for file upload
The urls are expected to be ready to use for file uploads, different mechanisms could be used to that end e.g. signed urls in GCS


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| onboarding_application_id | [string](#string) |  |  |






<a name="thebaasco.tenant.onboarding.v1.CreateFileUploadUrlResponse"></a>

### CreateFileUploadUrlResponse



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| filename | [string](#string) |  |  |
| upload_url | [string](#string) |  |  |





 

 

 

 



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

