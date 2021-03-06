# Distribution API
<a name="top"></a>



<a name="notifications_v1.proto"></a>

## notifications_v1.proto



### Services

<a name="thebaasco.tenant.distribution.v1.PushNotifications"></a>

#### PushNotifications
PushNotifications service exposes operations for push notification of a customer.

##### RegisterChannel

> **rpc** RegisterChannel([RegisterPushNotificationChannelRequest](#thebaasco.tenant.distribution.v1.RegisterPushNotificationChannelRequest))
    [.google.protobuf.Empty](#google.protobuf.Empty)

RegisterChannel registers a push notification channel for the specified device.


 <!-- end services -->




### API-specific types

<a name="thebaasco.tenant.distribution.v1.RegisterPushNotificationChannelRequest"></a>

#### RegisterPushNotificationChannelRequest
RegisterPushNotificationChannelRequest is a message passed as parameter to the RegisterChannel operation.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| device_id | [string](#string) |  | The device ID for which to register the push notification channel. This can be a unique ID provided by the device vendor or an anonymous sandboxed ID that uniquely represents the device. |
| registration_id | [string](#string) |  | The registration ID for the respective device. This is the registration token generated by the application client. See Firebase documentation for more information (https://firebase.google.com/docs/cloud-messaging/android/client#retrieve-the-current-registration-token). and size recommendations from here https://stackoverflow.com/questions/39959417/what-is-the-maximum-length-of-an-fcm-registration-id-token |





 <!-- end messages -->


## Scalar Value Types

| .proto Type | Notes | C++ | Java | Python | Go | C# | PHP | Ruby |
| ----------- | ----- | --- | ---- | ------ | -- | -- | --- | ---- |
| <a name="double" /> double |  | double | double | float | float64 | double | float | Float |
| <a name="float" /> float |  | float | float | float | float32 | float | float | Float |
| <a name="int32" /> int32 | Uses variable-length encoding. Inefficient for encoding negative numbers ??? if your field is likely to have negative values, use sint32 instead. | int32 | int | int | int32 | int | integer | Bignum or Fixnum (as required) |
| <a name="int64" /> int64 | Uses variable-length encoding. Inefficient for encoding negative numbers ??? if your field is likely to have negative values, use sint64 instead. | int64 | long | int/long | int64 | long | integer/string | Bignum |
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

