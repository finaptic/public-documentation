# Partner Application Accreditation Process and App Security Checklist

## Summary

Partners that leverage our SDK or APIs in their own applications must build those applications securely.

While Finaptic handles security of the APIs themselves, the client application must be designed securely, and the implementation of it must be tested.

This document describes the steps in the accredidation process.


### Process Overview

| Step | Accreditation Step                                                                                             | Timeline |
|-| -------------------------------------------------------------------------------------------------------------- | -------- |
| 1| Finaptic and Partner to review that SDLC (Software Development Life Cycle) process must be confirmed to meet the requirements described in this document. Action items for review will be reviewed again later. |  Kickoff week        |
| 2| Partner must document all external dependencies (software/libraries and vendors) and their purpose, and communicate the list to Finaptic as early as possible. | Kickoff + 1 month         |
| 3| Partner must ensure the application has the required controls, listed in this document, implemented.           | Before security testing          |
| 4| A third party must perform a "penetration test" of the application before go-live. Finaptic will review the scope as well as the results                | Go Live - One Month         |
| 5| Functional testing results are reviewed by Finaptic               | Go Live - One Week         |
| 6| Findings deemed critical in the testing must be fixed before go-live.                                          | Before go-live         |




### Step 1: Software Development Lifecycle (SDLC)

We must ensure that all partners making custom applications (web or mobile) using Finaptic SDKs and APIs, have a sufficient level of security maturity when it comes to the SDLC.

This part can be reviewed early in the partner relationship, to identify potential issues and have time to perform the necessary improvements as soon as possible.

#### SDLC Requirements Checklist

| Requirement                                                                                                                                                                                                                                                 | Done |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---- |
| Partner must use a source version control system, such as GitHub.                                                                                                                                                                                           |      |
| Authentication to this system, if done over the Internet, must require Multi-Factor Authentication (MFA).                                                                                                                                                   |      |
| Code reviews must be enforced on all code. A single person must be unable to write code that will end up in production builds without being reviewed by someone else.                                                                                       |      |
| A vulnerability management process must be in place, ensuring all components of the application are patched/fixed in a timely manner.                                                                                                                       |      |
| The application must be reviewed/tested for security by a third party at least once a year and after significant changes. Testing can be of the penetration testing kind, code audits, or anywhere in between, with the entire mobile application in scope. |      |


#### Recommendations

* Code should be signed with a PGP key unique to the developer committing it.
* Static Application Security Testing (SAST) should be in place to scan code for common security vulnerabilities.
* The application should implement jailbreak/rooting detection and not be usable on devices detected as potentially compromised.


### Step 2: Document External Dependencies
External dependencies (libraries or services) have an important impact on the security of a mobile or web application.
Those dependencies must be inventoried, as well as their purpose.  For external dependencies specifically, please document what information is shared with the third parties as well. Once it is sent to Finaptic, the Finaptic security team will review it and see if it is necessary to discuss any of the dependencies further.

| Requirement                                                 | Done |
| ----------------------------------------------------------- | ---- |
| Inventory code dependencies (third party libraries, etc)    |      |
| Inventory external dependencies (third party services, etc) |      |
| Communicate inventories to Finaptic                        |      |


### Step 3: Application Security Controls

The current mobile app requirements and recommendations are valid for both iOS and Android, though the suggested implementations are only documented for iOS so far.

#### Requirements Checklist - Mobile application with Finaptic SDK

| Requirement                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | Done |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---- |
| Communication from the app must be done over encrypted protocols only. While Finaptic APIs are only accessible over HTTPS, other destinations being used over HTTP would put the integrity of communications at risk and could allow someone to exploit flaws in the application itself. If exceptions are needed, they must be documented, communicated to Finaptic and use `NSexceptionDomains` on iOS.                                                                        |      |
| Certificate authority pinning must be done to prevent breaking TLS communications. While pinning the certificates used by Finaptic is the safest, pinning the certificate authority, and allowing rotation in the future, is very secure and much easier to manage.                                                                                                                                                                                                               |      |
| Ensure that only strong TLS ciphers can be used in the application, by explicitly enabling `NSExceptionRequiresForwardSecrecy` and `NSRequiresCertificateTransparency` at least towards Finaptic APIs and authentication provider.                                                                                                                                                                                                                                                |      |
| If "long sessions" are provided, so the user does not have to enter credentials when opening the application, those sessions must be protected by biometrics. For iOS, this means ensuring `kSecAttrAccessibleWhenPasscodeSetThisDeviceOnly` is configured (a device without a passcode can't have biometric), and this ensures this credential won't be usable through keychain syncing. `kSecAccessControlBiometryAny` must also be configured for keychain stored credentials.  |      |
| Sessions not protected by biometrics should not last for more than 15 minutes of inactivity, and a `Log Out` button should be available to the end-user.                                                                                                                                                                                                                                                                                                                           |      |
| Sensitive data fields (passwords, PII entry) must be tagged in order to avoid 3rd party keyboards being usable on them.                                                                                                                                                                                                                                                                                                                                                            |      |
| The OS features to integrate screenshots of running applications in the application switchers must replace the screenshot of the application with a static image when the user is viewing financial information. This is to avoid screenshots with financial information being stored on the device, and potentially in backups.                                                                                                                                                   |      |
| All secrets and authentication related materials must be stored in the system secure storage, such as the keychain on iOS.                                                                                                                                                                                                                                                                                                                                                         |      |
| URL Schemes should never be used for any "write" action related to Finaptic APIs. For example, a URL scheme to trigger an Interac transfer is not acceptable, as URL schemes are insecure by design.                                                                                                                                                                                                                                                                              |      |
| External services, such as but not limited to analytics providers, must not receive PII, financial transactional data, or any other data which is managed by Finaptic, in order to avoid duplicating critical info that is and must be protected by Finaptic                                                                                                                                                                                                                       |      |

##### Recommendations

* Reject the use of 3rd party keyboards completely, using [UIApplicationKeyboardExtensionPointIdentifier](https://developer.apple.com/documentation/uikit/uiapplicationkeyboardextensionpointidentifier?language=objc)

#### Requirements Checklist - Web application with Finaptic APIs
TBD 


### Step 4: Security Testing (Penetration test, Code Audit, etc.)
Finaptic requires that partners test their applications which leverage Finaptic APIs or SDKs.

* Testing must be done **at least once a year** as well as when **major changes are performed.
* Testing must be done by a third party company that specializes in security testing, unless partner has an internal team dedicated to offensive security.
* The scope of the tests must be the entire mobile application. 
* The format of testing is up to the partner - very open testing techniques such as a code audit are acceptable, and completely advanced knowledge free penetration testing is acceptable as well. We recommend using many techniques over time.
* If the test is a penetration test, vulnerability re-testing must be included.
* Test results must be communicated to Finaptic, and remediation priority will be discussed between Finaptic and partner.



#### Mobile application with Finaptic SDK

If vulnerabilities are found on the SDK itself during the application test, Finaptic will resolve them based on risk and priority.

#### Web application with Finaptic APIs
Not supported at this time.

### Step 5: User Acceptance Testing (UAT) by Finaptic
One week before go-live, as well as before major changes, the application will be manually reviewed by Finaptic, focusing on:

* Functional testing
* Disaster recovery
* User consent
* Accessibility
* Legal

**This process will be detailed further in a future version of this document.**

### Step 6: Findings deemed critical in the testing must be fixed before go-live.
Findings discovered in Step 4 deemed to be critical to fix before go-live must now be fixed, and re-tested by the vendor used in Step 4.

#### Critical vulnerability

A critical vulnerability is defined as a vulnerability which could be used directly to impact the security or privacy of customers or Finaptic. The Finaptic security team reserves the right to flag any vulnerability as critical, based on our own threat modeling exercises, but will usually focus on these types of vulnerabilities:

1. Privacy/Confidentiality issues at the network level (ex: lack of proper HTTPS configuration)
2. Credential storage safety (ex: sessions stored in an unsafe manner)
3. Data leak caused by 3rd party tools (ex: PII sent to analytics service)

Less critical vulnerabilities also have to be fixed, but can be prioritized as per the partner's vulnerability management policy.

#### Process

* Re-testing of vulnerabilities is performed.
* Testing vendor provides documentation showing the issues were resolved.

### Step 7: Go live!

🥳