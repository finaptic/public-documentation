# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [clientprofile_v1_individualparty.proto](#clientprofile_v1_individualparty.proto)
    - [Address](#thebaasco.tenant.clientprofile.v1.Address)
    - [ContactDetails](#thebaasco.tenant.clientprofile.v1.ContactDetails)
    - [CreateIndividualPartyRequest](#thebaasco.tenant.clientprofile.v1.CreateIndividualPartyRequest)
    - [CreateIndividualPartyResponse](#thebaasco.tenant.clientprofile.v1.CreateIndividualPartyResponse)
    - [CustomerResidency](#thebaasco.tenant.clientprofile.v1.CustomerResidency)
    - [Employment](#thebaasco.tenant.clientprofile.v1.Employment)
    - [IndividualParty](#thebaasco.tenant.clientprofile.v1.IndividualParty)
    - [IndividualPartyCreatedEvent](#thebaasco.tenant.clientprofile.v1.IndividualPartyCreatedEvent)
    - [OtherResidence](#thebaasco.tenant.clientprofile.v1.OtherResidence)
  
- [Scalar Value Types](#scalar-value-types)



<a name="clientprofile_v1_individualparty.proto"></a>
<p align="right"><a href="#top">Top</a></p>

## clientprofile_v1_individualparty.proto



<a name="thebaasco.tenant.clientprofile.v1.Address"></a>

### Address



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| type | [string](#string) |  | Required, HOME or WORK. Will be validated based on reference data values |
| organization | [string](#string) |  | [(validate.rules).string = {ignore_empty: true, max_len: 300}]; //NR max-len=300) |
| line_one | [string](#string) |  | [(validate.rules).string = {ignore_empty: true,max_len: 300}];//Required max-len=300) |
| line_two | [string](#string) |  | [(validate.rules).string = {ignore_empty: true,max_len: 300}] ;//NR max-len=300) |
| line_three | [string](#string) |  | [(validate.rules).string = {ignore_empty: true,max_len: 300}];//NR max-len=300) |
| line_four | [string](#string) |  | [(validate.rules).string = {ignore_empty: true,max_len: 300}];//NR max-len=300) |
| line_five | [string](#string) |  | [(validate.rules).string = {ignore_empty: true,max_len: 300}];//NR max-len=300) |
| line_six | [string](#string) |  | [(validate.rules).string = {ignore_empty: true,max_len: 300}];//NR max-len=300) |
| line_seven | [string](#string) |  | [(validate.rules).string = {ignore_empty: true,max_len: 300}];//NR max-len=300) |
| line_eight | [string](#string) |  | [(validate.rules).string = {ignore_empty: true,max_len: 300}];//NR max-len=300) |
| locality | [string](#string) |  | [(validate.rules).string = {ignore_empty: true,max_len: 150}]; //NR max-len=150 (City) |
| dependent_locality | [string](#string) |  | [(validate.rules).string = {ignore_empty: true,max_len: 150}]; //NR max-len=150 |
| double_dependent_locality | [string](#string) |  | [(validate.rules).string = {ignore_empty: true,max_len: 150}]; //NR max-len=150 |
| administrative_area | [string](#string) |  | [(validate.rules).string = {ignore_empty: true,max_len: 150}]; //NR max-len=150 |
| sub_administrative_area | [string](#string) |  | [(validate.rules).string = {ignore_empty: true,max_len: 150}]; //NR max-len=150 |
| area | [string](#string) |  | [(validate.rules).string = {ignore_empty: true,max_len: 150}]; //Required max-len=150 ---- (State, municipality) |
| postal_code | [string](#string) |  | [(validate.rules).string = {ignore_empty: true,max_len: 150}]; //Required max-len=150 |
| country | [string](#string) |  | [(validate.rules).string = {ignore_empty: true,max_len: 50}]; // req |






<a name="thebaasco.tenant.clientprofile.v1.ContactDetails"></a>

### ContactDetails



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| phone_number | [string](#string) |  | [(validate.rules).string = { ignore_empty: true,pattern: &#39;\\&#43;(9[976]\\d|8[987530]\\d|6[987]\\d|5[90]\\d|42\\d|3[875]\\d|2[98654321]\\d|9[8543210]|8[6421]|6[6543210]|5[87654321]|4[987654310]|3[9643210]|2[70]|7|1)\\d{1,14}$&#39; }]; //Required, regex |
| customer_email | [string](#string) |  | required, should reject if Email&#39; field does not contain &#39;@&#39;, &#39;.com&#39;, &#39;.biz’, &#39;.ca&#39; or any other type of standard email format |






<a name="thebaasco.tenant.clientprofile.v1.CreateIndividualPartyRequest"></a>

### CreateIndividualPartyRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| individual_party | [IndividualParty](#thebaasco.tenant.clientprofile.v1.IndividualParty) |  |  |






<a name="thebaasco.tenant.clientprofile.v1.CreateIndividualPartyResponse"></a>

### CreateIndividualPartyResponse



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| individual_party | [IndividualParty](#thebaasco.tenant.clientprofile.v1.IndividualParty) |  |  |






<a name="thebaasco.tenant.clientprofile.v1.CustomerResidency"></a>

### CustomerResidency



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| is_canadian_tax_resident | [bool](#bool) |  | Are you resident of Canada? |
| other_residences | [OtherResidence](#thebaasco.tenant.clientprofile.v1.OtherResidence) | repeated | US PB-705 |
| sin_token | [string](#string) |  | [(validate.rules).string = { min_len: 9, max_len:9, pattern:&#34;^\\d*$&#34;}]; // NR, ?How to deal with # patterns dependent on country |
| citizenship | [string](#string) |  | US PB-703 - select |






<a name="thebaasco.tenant.clientprofile.v1.Employment"></a>

### Employment



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| employment_status | [string](#string) |  | required: &#34; Values defined here – https://thebaasco.atlassian.net/browse/PB-645&#34; |
| employer_name | [string](#string) |  | [(validate.rules).string = {ignore_empty: true,max_len: 50}]; // Required for 0-2 status , length 50, sql injection validation/policies |
| industry | [string](#string) |  | [(validate.rules).string = {ignore_empty: true,max_len: 50}]; // Required for 0-2 status |
| occupation | [string](#string) |  | [(validate.rules).string = {ignore_empty: true,max_len: 50}]; |
| start_date | [google.type.Date](#google.type.Date) |  | not required, format Month/year |
| end_date | [google.type.Date](#google.type.Date) |  | not required, format Month/year |
| work_address | [Address](#thebaasco.tenant.clientprofile.v1.Address) |  | Required for 0-2 status |
| work_phone_number | [string](#string) |  | [(validate.rules).string = { ignore_empty: true,pattern: &#39;\\&#43;(9[976]\\d|8[987530]\\d|6[987]\\d|5[90]\\d|42\\d|3[875]\\d|2[98654321]\\d|9[8543210]|8[6421]|6[6543210]|5[87654321]|4[987654310]|3[9643210]|2[70]|7|1)\\d{1,14}$&#39; }]; //Required for 0-2 |






<a name="thebaasco.tenant.clientprofile.v1.IndividualParty"></a>

### IndividualParty
This structure must be filled with data from onboarding, previous version was draft.
Please update


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| first_name | [string](#string) |  | [(validate.rules).string = {ignore_empty: true,max_len: 50}]; //required maxlen=50 AN characters, hyphen (-), apostrophe, (&#39;), French accents, numbers (0-9) |
| middle_name | [string](#string) |  | [(validate.rules).string = {ignore_empty: true,max_len: 50}]; // not required maxlen=50 AN characters, hyphen (-), apostrophe, (&#39;), French accents, numbers (0-9) |
| last_name | [string](#string) |  | [(validate.rules).string = {ignore_empty: true,max_len: 50}]; //required maxlen=50 AN characters, hyphen (-), apostrophe, (&#39;), French accents, numbers (0-9) |
| date_of_birth | [google.type.Date](#google.type.Date) |  |  |
| customer_contact_details | [ContactDetails](#thebaasco.tenant.clientprofile.v1.ContactDetails) |  |  |
| customer_employment | [Employment](#thebaasco.tenant.clientprofile.v1.Employment) | repeated | proto only - implementation hardcoded to one for MVP |
| customer_residency | [CustomerResidency](#thebaasco.tenant.clientprofile.v1.CustomerResidency) |  | Required |
| customer_residence_address | [Address](#thebaasco.tenant.clientprofile.v1.Address) | repeated |  |
| individual_party_id | [string](#string) |  | Output only |






<a name="thebaasco.tenant.clientprofile.v1.IndividualPartyCreatedEvent"></a>

### IndividualPartyCreatedEvent



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| event_id | [string](#string) |  |  |
| individual_party | [IndividualParty](#thebaasco.tenant.clientprofile.v1.IndividualParty) |  |  |
| tenant_id | [int32](#int32) |  |  |
| individual_party_created_time | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  |  |






<a name="thebaasco.tenant.clientprofile.v1.OtherResidence"></a>

### OtherResidence



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| country | [string](#string) |  |  |
| tin_token | [string](#string) |  | [(validate.rules).string = {ignore_empty: true,max_len: 50}]; // max length 50 |
| citizenship | [string](#string) |  | [(validate.rules).string = {ignore_empty: true,max_len: 50}]; // Required if us or canada |





 

 

 

 



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

