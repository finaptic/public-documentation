# Client Profile API (Draft)
<a name="top"></a>

**This is a draft API specification that is subject to change**

<a name="clientprofile_v1_individualparty.proto"></a>

## clientprofile_v1_individualparty.proto



### Services

<a name="thebaasco.tenant.clientprofile.v1.ClientProfileService"></a>

#### ClientProfileService


##### GetIndividualParty

> **rpc** GetIndividualParty([GetIndividualPartyRequest](#thebaasco.tenant.clientprofile.v1.GetIndividualPartyRequest))
[IndividualParty](#thebaasco.tenant.clientprofile.v1.IndividualParty)




 <!-- end services -->




### API-specific types

<a name="thebaasco.tenant.clientprofile.v1.Address"></a>

#### Address



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| type | [string](#string) |  | Required, HOME or WORK. Will be validated based on reference data values |
| organization | [string](#string) |  | NR max-len=300) |
| line_one | [string](#string) |  |  |
| line_two | [string](#string) |  |  |
| line_three | [string](#string) |  |  |
| line_four | [string](#string) |  |  |
| line_five | [string](#string) |  |  |
| line_six | [string](#string) |  |  |
| line_seven | [string](#string) |  |  |
| line_eight | [string](#string) |  |  |
| locality | [string](#string) |  |  |
| dependent_locality | [string](#string) |  |  |
| double_dependent_locality | [string](#string) |  |  |
| administrative_area | [string](#string) |  |  |
| sub_administrative_area | [string](#string) |  |  |
| area | [string](#string) |  |  |
| postal_code | [string](#string) |  |  |
| country | [string](#string) |  |  |






<a name="thebaasco.tenant.clientprofile.v1.ContactDetails"></a>

#### ContactDetails



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| phone_number | [string](#string) |  |  |
| customer_email | [string](#string) |  |  |
| customer_communication_language | [string](#string) |  |  |
| customer_communication_pref | [string](#string) |  |  |






<a name="thebaasco.tenant.clientprofile.v1.CreateIndividualPartyRequest"></a>

#### CreateIndividualPartyRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| individual_party | [IndividualParty](#thebaasco.tenant.clientprofile.v1.IndividualParty) |  |  |






<a name="thebaasco.tenant.clientprofile.v1.CreateIndividualPartyResponse"></a>

#### CreateIndividualPartyResponse



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| individual_party | [IndividualParty](#thebaasco.tenant.clientprofile.v1.IndividualParty) |  |  |






<a name="thebaasco.tenant.clientprofile.v1.CustomerResidency"></a>

#### CustomerResidency



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| is_canadian_tax_resident | [bool](#bool) |  | Are you resident of Canada? |
| other_residences | [OtherResidence](#thebaasco.tenant.clientprofile.v1.OtherResidence) | repeated |  |
| sin_token | [string](#string) |  | [(validate.rules).string = { min_len: 9, max_len:9, pattern:"^\\d*$"}]; // NR, ?How to deal with # patterns dependent on country |
| citizenship | [string](#string) |  |  |






<a name="thebaasco.tenant.clientprofile.v1.Employment"></a>

#### Employment



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| employment_status | [string](#string) |  |  |
| employer_name | [string](#string) |  |  |
| industry | [string](#string) |  |  |
| occupation | [string](#string) |  |  |
| start_date | [google.type.Date](#google.type.Date) |  | not required, format Month/year |
| end_date | [google.type.Date](#google.type.Date) |  | not required, format Month/year |
| work_address | [Address](#thebaasco.tenant.clientprofile.v1.Address) |  | Required for 0-2 status |
| work_phone_number | [string](#string) |  |  |






<a name="thebaasco.tenant.clientprofile.v1.GetIndividualPartyRequest"></a>

#### GetIndividualPartyRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| individual_party_id | [string](#string) |  |  |






<a name="thebaasco.tenant.clientprofile.v1.IndividualParty"></a>

#### IndividualParty
This structure must be filled with data from onboarding, previous version was draft.
Please update


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| first_name | [string](#string) |  |  |
| middle_name | [string](#string) |  |  |
| last_name | [string](#string) |  |  |
| date_of_birth | [google.type.Date](#google.type.Date) |  |  |
| customer_contact_details | [ContactDetails](#thebaasco.tenant.clientprofile.v1.ContactDetails) |  |  |
| customer_employment | [Employment](#thebaasco.tenant.clientprofile.v1.Employment) | repeated |  |
| customer_residency | [CustomerResidency](#thebaasco.tenant.clientprofile.v1.CustomerResidency) |  |  |
| customer_residence_address | [Address](#thebaasco.tenant.clientprofile.v1.Address) | repeated |  |
| individual_party_id | [string](#string) |  | Output only |
| kycDone | [bool](#bool) |  | This boolean field is used to indicate if the customer has been validated through the onboarding KYC process |






<a name="thebaasco.tenant.clientprofile.v1.IndividualPartyCreatedEvent"></a>

#### IndividualPartyCreatedEvent



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| event_id | [string](#string) |  |  |
| individual_party | [IndividualParty](#thebaasco.tenant.clientprofile.v1.IndividualParty) |  |  |
| tenant_id | [int32](#int32) |  |  |
| individual_party_created_time | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  |  |






<a name="thebaasco.tenant.clientprofile.v1.OtherResidence"></a>

#### OtherResidence



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| country | [string](#string) |  |  |
| tin_token | [string](#string) |  | [(validate.rules).string = {ignore_empty: true,max_len: 50}]; // max length 50 |
| citizenship | [string](#string) |  |  |





 <!-- end messages -->


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

