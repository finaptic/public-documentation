# Cross-domain error model
<a name="top"></a>



<a name="errordetails.proto"></a>

## errordetails.proto


### Enums

<a name="thebaasco.types.GenericErrorCodes"></a>

#### GenericErrorCodes
A list of error codes which could be returned from any API service. See documentation of the specific services for more details.
In all cases these error codes will be communicated to the consumer in a structured manner within an instance of AppError
that will be included within the error status response: status.Status.Details

| Name | Number | Description |
| ---- | ------ | ----------- |
| GEN_UNSPECIFIED | 0 | UNSPECIFIED should never be used directly. It is returned when a value has not been specified, and is present by convention, for error-detection purposes. |
| GEN_INTERNAL_ERROR | 1 | GEN_INTERNAL_ERROR - Internal Error. gRPC Code=Internal. Returned when an unexpected error occurs. This error will be accompanied by a RefNumber which can be shared with Finaptic to investigate the source of the error. |


 <!-- end enums -->


### API-specific types

<a name="thebaasco.types.AppError"></a>

#### AppError
AppError is used to enrich the gRPC error response returned by a service, so that the consumer
can have additional elements to use while handling errors at its side
It is meant to be added to the error details collection of proto messages that a status.Status can contain
For reference see: https://github.com/googleapis/googleapis/blob/master/google/rpc/status.proto#L46
Description as follows:
> A list of messages that carry the error details...
> repeated google.protobuf.Any details = 3;


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| error_code | [string](#string) |  | Used for services to provided a machine-readable coded error that consumers could use to for assertions, conditional processing and so on It's recommended that its value is taken from a well-documented enumeration from the service's API's surface |
| source | [string](#string) |  | Used to document the source domain/application from where an error originates |
| ref_number | [string](#string) |  | reference number provided along with an error so that the consumer can send it back while making support requests and then the value could be used to quickly locate the error in our logs/incident management system |
| field_violations | [FieldViolation](#thebaasco.types.FieldViolation) | repeated | In case the status response has code codes.InvalidArgument, an API implementor can add detailed information about the fields found at fault |






<a name="thebaasco.types.FieldViolation"></a>

#### FieldViolation
FieldViolation describes a violation found on a specific field from the request made to an API
It allows an API to provide structured error message for errors found while validating the request input


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| field | [string](#string) |  |  |
| description | [string](#string) |  |  |





 <!-- end messages -->