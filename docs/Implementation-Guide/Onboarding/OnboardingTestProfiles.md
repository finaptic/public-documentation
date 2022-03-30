# Test profiles

## Guide Purpose

**This guide will**

1. Give you details on how to test onboarding applications that perform KYC checks

**Links to related Onboarding topics**

1. [Onboarding Overview](/../../Implementation-Guide/Onboarding/OnboardingDocumentation/)
2. [Onboarding Sequence Diagram](/../../Implementation-Guide/Onboarding/OnboardingSequenceDoc/)
3. [Finaptic's Authorized User Policies](/../../Implementation-Guide/Banking/Authorized-User/AuthorizedUserDocumentation/)
4. [Productless Onboarding Architecture](/../../Implementation-Guide/Onboarding/ProductlessOnboardingDoc/) to Onboard an Authorized User

## Profiles

In order to ease development and testing, we've provided test profiles at your disposal.

Note that :

- Any application that performs KYC and uses these names will trigger the rules regardless of the rest of the
  application.

- The application still needs to have a valid Canadian address (Verified against a 3rd party during
  the `updateAddresses` call)

- Age of the applicant must be equal or above the age of majority for the specified province of residency.

| Description                                                                            | Firstname | Middle            | Lastname | Returned in`FailureStatusCodes` after ValidateDocument/Selfie | Returned in`FailureStatusCodes` after ValidateDocument/Selfie |
|----------------------------------------------------------------------------------------|-----------|-------------------|----------|---------------------------------------------------------------|---------------------------------------------------------------|
| Customer that passes the KYC check succesfully                                         | John      | success           | Doe      | None                                                          | None                                                          |
| Customer that provided a DoB different than the one on the ID (Photo not clear enough) | John      | dob-match         | Doe      | `FAILURE_CODE_ID_DOB_MATCH`                                   | `FAILURE_CODE_ID_DOB_MATCH`                                   |
| Customer provided an ID that we were unable to verify                                  | John      | id-validation     | Doe      | `FAILURE_CODE_ID_VALIDATION`                                  | `FAILURE_CODE_ID_VALIDATION`                                  |
| Customer provided a Selfie that we were unable to verify                               | John      | selfie-validation | Doe      | `FAILURE_CODE_SELFIE_VALIDATION`                              | `FAILURE_CODE_SELFIE_VALIDATION`                              |
| Customer that provided an ID that is expired                                           | John      | id-expired        | Doe      | `FAILURE_CODE_ID_EXPIRED`                                     | `FAILURE_CODE_ID_EXPIRED`                                     |
| Customer that provided an ID that we were unable to validate (worn or damaged)         | John      | id-verification   | Doe      | `FAILURE_CODE_ID_VERIFICATION`                                | `FAILURE_CODE_ID_VERIFICATION`                                |
| Customer that provided a name that does not match the name on the ID                   | John      | name-match        | Doe      | `FAILURE_CODE_ID_NAME_MATCH`                                  | `FAILURE_CODE_ID_NAME_MATCH`                                  |
| Customer that provided a Selfie that does not match the photo on the ID                | John      | selfie-match      | Doe      | `FAILURE_CODE_ID_SELFIE_MATCH`                                | `FAILURE_CODE_ID_SELFIE_MATCH`                                |
| Customer that is rejected during KYC check because he is on a sanctioned list          | John      | sanctioned        | Doe      | None                                                          | `FAILURE_CODE_DEFAULT`                                        |
| Customer that is rejected during KYC with a general error message                      | John      | default           | Doe      | None                                                          | `FAILURE_CODE_DEFAULT`                                        |
