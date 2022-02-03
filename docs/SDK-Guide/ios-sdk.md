# Finaptic iOS SDK
 
The Finaptic iOS SDK provides a way to consume Finaptic APIs from your iOS application. It is written in Swift.
Note that the Finaptic APIs are strictly accessed through the SDK because it hides the underlying implementation that could change in the future.
 
###### Prerequisites
- iOS version 13.0 or later
- Xcode 12.4
 
## Sample application
 
A sample application demonstrates what is explained in the document. Make sure you can run it using your own SDK key.
Here are the steps to run the sample app:

1. Open the file ViewController.swift and replace the SDK key with the one provided to you.
2. Run the sample app on a device or simulator.
3. Press **Execute Onboarding** button at the bottom to execute the onboarding flow.
4. Open the xcode console. You will see the sample app logs as well as the SDK logs.
5. Let the whole flow execute. It can take more than one minute. In a sandbox environment, you may experience delays when using a client for the first time or after a small inactivity period. It is due to the backend services cold start.
6. If an error arises, it will be printed in the console. In this case, make sure the SDK key is the right one and retry.
7. If the flow completes without error (no more requests are executing), you are good to integrate the SDK in your own app.
##### ID and face capture with Acuant
Please take a look at AcuantSample on how to use Acuant for document and face capture.
You can read their official documentation [here](https://github.com/Acuant/iOSSDKV11).
## Installation


1. Create a Podfile.
     - Open a terminal window, and `cd` into your project directory
     - Run `pod init && pod install`
     - Open {ProjectName}.xcworkspace
   
2. Copy **Finaptic.xcframework**, **MCPSDK.xcframework**, **TrustKit.xcframework** and **gRPC-Swift.podspec** files to the root folder of your project.
 
3. Drag **Finaptic.xcframework**, **MCPSDK.xcframework** and **TrustKit.xcframework** to your project's **{ProjectName}/Frameworks** group and select **Add to targets**.
   
4. Select **General** tab of your **Target** and under ***Frameworks, Libraries, and Embedded Content*** set **Embed Without Signing** for **MCPSDK.xcframework** and **TrustKit.xcframework**.
 
5. Add Finaptic dependencies to your **Podfile**.
 
        # Finaptic dependencies (statically linked)
        pod 'gRPC-Swift', :podspec => 'gRPC-Swift.podspec'
        pod 'Firebase/Core', '~> 8.3'
        pod 'Firebase/Auth', '~> 8.3'
        pod 'Firebase/Firestore', '~> 8.3'
        pod 'Firebase/RemoteConfig', '~> 8.3'
        pod 'Auth0', '~> 1.34.0'
        pod 'AWSCore', '~> 2.26'
        pod 'AWSConnect', '~> 2.26'
        pod 'AWSConnectParticipant', '~> 2.26'
        pod 'Starscream', '~> 3.1'
        
        # Acuant dependencies
        pod 'AcuantiOSSDKV11/AcuantCommon', '11.5.1'
        pod 'AcuantiOSSDKV11/AcuantCamera', '11.5.1'
        pod 'AcuantiOSSDKV11/AcuantFaceCapture', '11.5.1'
        pod 'AcuantiOSSDKV11/AcuantImagePreparation', '11.5.1'
        pod 'AcuantiOSSDKV11/AcuantDocumentProcessing', '11.5.1'


6. Add the following hooks to your **Podfile**.
 
        pre_install do |installer|
            static_frameworks = [
            'Auth0',
            'BoringSSL-GRPC',
            'CGRPCZlib',
            'CNIOAtomics',
            'CNIOBoringSSL',
            'CNIOBoringSSLShims',
            'CNIODarwin',
            'CNIOHTTPParser',
            'CNIOLinux',
            'CNIOWindows',
            'Firebase',
            'FirebaseABTesting',
            'FirebaseAnalytics',
            'FirebaseAuth',
            'FirebaseCore',
            'FirebaseCoreDiagnostics',
            'FirebaseFirestore',
            'FirebaseInstallations',
            'FirebaseRemoteConfig',
            'GTMSessionFetcher',
            'GoogleAppMeasurement',
            'GoogleDataTransport',
            'GoogleUtilities',
            'JWTDecode',
            'Logging',
            'PromisesObjC',
            'SimpleKeychain',
            'SwiftNIO',
            'SwiftNIOConcurrencyHelpers',
            'SwiftNIOExtras',
            'SwiftNIOFoundationCompat',
            'SwiftNIOHPACK',
            'SwiftNIOHTTP1',
            'SwiftNIOHTTP2',
            'SwiftNIOSSL',
            'SwiftNIOTLS',
            'SwiftNIOTransportServices',
            'SwiftProtobuf',
            'abseil',
            'gRPC-C++',
            'gRPC-Core',
            'gRPC-Swift',
            'leveldb-library',
            'nanopb']
            installer.pod_targets.each do |pod|
                if static_frameworks.include?(pod.name)
                    def pod.static_framework?;
                        true
                    end
                end
            end
        end

        post_install do |installer|
        installer.pods_project.targets.each do |target|
            if ['gRPC-Swift', 'SwiftNIO', 'SwiftNIOConcurrencyHelpers', 'SwiftNIOExtras', 'SwiftNIOFoundationCompat', 'SwiftNIOHPACK', 'SwiftNIOHTTP1', 'SwiftNIOHTTP2', 'SwiftNIOSSL', 'SwiftNIOTLS', 'SwiftNIOTransportServices'].include? target.name
            target.build_configurations.each do |config|
                config.build_settings['BUILD_LIBRARY_FOR_DISTRIBUTION'] = 'NO'
            end
            else if ['Auth0', 'SwiftProtobuf', 'AcuantiOSSDKV11', 'Socket.IO-Client-Swift', 'Starscream'].include? target.name
                target.build_configurations.each do |config|
                config.build_settings['BUILD_LIBRARY_FOR_DISTRIBUTION'] = 'YES'
                end
              end
            end
          end
        end




 1. Run `pod install` to downloads and install the pods.
 
## Initialization
The SDK must be initialized before you can make any call. The FinapticSDK.init method must be invoked.
```
import Finaptic

...

FinapticSDK.initialize(sdkKey: "") { result in
   switch result {
   case .success(let sdk):
       print("FinapticSDK initialized")
      
   case .failure(let error):
       print("Initialization failed: \(error)")
   }
}
```
It is recommended to initialize the SDK once and store the returned instance in your own variable or container.
You can now start to use the SDK through multiple clients.
## OnboardingClient
 
Onboarding flow consists of initiate, update, validate and finalize calls.

1. Initiate application
     - [initiateApplication](#initiateapplication)
2. Get information
      - [getConsentElectronicCommunication](#getconsentelectroniccommunication)
      - [getConsentPrivacyPolicy](#getconsentprivacypolicy)
      - [getConsentTermsOfUse](#getconsenttermsofuse)
      - [getConsentProductAgreement](#getconsentproductagreement)
3. Update information
     - [updatePersonalDetails](#updatepersonaldetails)
     - [updateContactDetails](#updatecontactdetails)
     - [updateAccountDetails](#updateaccountdetails)
     - [updateAddresses](#updateaddresses)
     - [updateConsents](#updateconsents)
     - [updateCustomerResidency](#updatecustomerresidency)
     - [updateDisclosures](#updatedisclosures)
     - [updateEmployment](#updateemployment)
     - [updateCommunicationPreferences](#updatecommunicationpreferences)
4. Validate application
     - [validateDocuments](#validatedocuments)
     - [validateSelfie](#validateselfie)
     - [validateApplication](#validateapplication)
     - [acceptApplication](#acceptapplication)
5. Finalize application
     - [finalizeApplicationCreateCustomer](#finalizeapplicationcreatecustomer)
     - [finalizeApplicationCreateProduct](#finalizeapplicationcreateproduct)
     
#### OnboardingOperationResponseDetails

OnboardingOperationResponseDetails are the common response fields for an onboarding application create or update message.

- ***applicationId***: The UUID of the application the response is for.
- ***onboardingApplicationStatus***: The updated application status. The value here is one of the following:
    - **"UNDEFINED"** is reserved for error detection purposes. It should not be used directly.
    - **"New Application"** describes an Application that has just been created.
    - **"Data Collection** describes an Application that has successfully performed an Update call or failed a ValidateSelfieRequest or a ValidateDocumentsRequest.
    - **"KYC In Progress"** describes an Application that has successfully performed a ValidateSelfieRequest or ValidateDocumentsRequest.
    - **"KYC Success"** describes an Application that has successfully performed KYC checks as a result of a ValidateApplicationRequest.
    - **"KYC Failed"** describes an Application that has failed KYC as a result of a ValidateApplicationRequest.
    - **"KYC Rejected"** describes an Application that has no remaining KYC attempts.
    - **"Application Accepted"** describes an application that has successfully completed an AcceptApplicationRequest.
    - **"Application Processed"** describes an application that has been successfully completed.
    - **"Application Cancelled"** describes an application that has been cancelled by the customer.
    - **"Application Rejected"** describes an application that has been rejected and cannot be continued. This is used  when a more descriptive rejection status is not available.
- ***requestStatus***: The success status of the request.
- ***requestStatusReason***: The detailed reason for the request status. *Deprecated*: Use ***requestReasonCodes*** instead.
- ***remainingAllowedRetries***: The remaining KYC retries allowed.
- ***failureStatusCodes***: The list of codes explaining the reason of the request outcome. This list will only be populated when request status is **"KYC Failed"**, **"KYC Rejected"**, or **"Application Rejected"**. The list will otherwise be empty. The value stored here is one of the code values retrieved from the [ReferenceDataClient](#referencedataclient) with ***dataType*** set to ***ReferenceDataTypes.failureCode***.
- ***verificationNeeded***: The list of verifications that will be needed be performed in order to complete the application. See [OnboardingVerification](#onboardingverification).
- ***consents***: The list of Consents required as part of this onboarding application. See [TermsAndConditions](#termsandconditions).
- ***disclosures***: The list of Consents required as part of this onboarding application. See [TermsAndConditions](#termsandconditions).

#### OnboardingVerification

Verification needed for an onboarding application

- ***name***: The name of the OnboardingVerification.
    - **"KYC"** means this application will perform KYC validation during the validate phase (ValidateDocumentsRequest, ValidateSelfieRequest and ValidateApplicationRequest) before being able to complete onboarding application.
    - **"ProfileCreation"** means that this application requires input of basic customer information, without doing full KYC, before being able to complete onboarding application.
    - **"AccountUsage"** means that this application needs to collect Account Usage information using UpdateAccountDetailsRequest before being able to complete onboarding application.
- ***requiredUpdates***: The list of all Update request that are required as part of this verification (eg : [UpdateAccountDetailsRequest, UpdateConsentsRequest]).

#### TermsAndConditions

TermsAndConditions is either a consent or a disclosure.

- ***documentType***: The type of the terms and conditions. The value stored here is one of the code values retrieved from the [ReferenceDataClient](#referencedataclient) with ***dataType*** set to **ReferenceDataTypes.consentType** or **ReferenceDataTypes.disclosureType**.
- ***documentNameAndVersion***: The URL of the versioned terms and conditions document the customer needs to review and accept.
- ***neededBeforeValidation***: Whether or not the terms and conditions need to be accepted before calling any of the validate requests.

### initiateApplication
This is the only call that must be executed prior any other one when processing an onboarding application.
```
let request = InitiateApplicationRequest(customerId: nil, productId: "prepaidV1")
sdk.onboardingClient.initiateApplication(request: request) { result in
   switch result {
   case .success(let response):
       if (response.basicDetails.requestStatus != "OK") {
           // Could not process the request properly. You can retry the request.
       }
       let applicationId = response.basicDetails.applicationId
  
        print("Verification needed for an onboarding application:")
        response.basicDetails.verificationNeeded.forEach { print($0) }

   case .failure(let error):
       print("Initiate application error: \(error)")
   }
}
```

#### InitiateApplicationRequest
- ***bundleId***: The unique identifier of the bundle being applied for.
    - **"despositV1"**: Deposit account without card.
    - **"prepaidV1"**: Account and prepaid-card.
    - **"creditcardV1"**: Account with credit card.
    - **"productlessV1"**: Onboard a customer and **do not** validate his identity.
    - **"kycV1"**: Onboard a customer, but **do validate** his identity.

#### InitiateApplicationResponse
- ***basicDetails***: The basic details of the onboarding application response. See [OnboardingOperationResponseDetails](#onboardingoperationresponsedetails).


 The returned application CONSENT TYPE and DOCUMENT NAME AND VERSION are required in consent update request.
### getConsentElectronicCommunication

```
    let response = sdk.onboardingClient.getConsentElectronicCommunication()
```
### getConsentTermsOfUse

```
    let response = sdk.onboardingClient.getConsentTermsOfUse()
```
### getConsentProductAgreement

```
    let response = sdk.onboardingClient.getConsentProductAgreement()
```
### getConsentPrivacyPolicy

```
    let response = sdk.onboardingClient.getConsentPrivacyPolicy()
```

The returned application ID is required in every other request.
### updatePersonalDetails
```
let request = UpdatePersonalDetailsRequest(
   onboardingApplicationId: applicationId,
   customerPersonalDetails: PersonalDetails(
       firstName: "COMPANY",
       middleName: nil,
       lastName: "SAMPLE",
       dateOfBirth: Calendar.current.date(from: DateComponents(year: 2000, month: 3, day: 6))!)
)
sdk.onboardingClient.updatePersonalDetails(request: request) { result in
   switch result {
   case .success(let response):
       if (response.basicDetails.requestStatus != "OK") {
           // Could not process the request properly. You can retry the request.
       }
  
   case .failure(let error):
       print("Update personal details error: \(error)")
   }
}
```

#### UpdatePersonalDetailsRequest

Used to update the customer personal details in the onboarding application.

 - ***onboardingApplicationId***: The UUID of the onboarding application to update.
 - ***customerPersonalDetails***: See [personal details](#personaldetails).

#### UpdatePersonalDetailsResponse
- ***basicDetails***: The basic details of the onboarding application response. See [OnboardingOperationResponseDetails](#onboardingoperationresponsedetails).

#### PersonalDetails
- ***firstName***: The customer first name. This value is required and has a maximum of 50 characters. Accepted characters: -, ', French accents, 0-9.
- ***middleName***: The customer middle name. This value is optional and has a maximum of 50 characters. Accepted characters: -, ', French accents, 0-9.
- ***lastName***: The customer last name. This value is required and has a maximum of 50 characters. Accepted characters: -, ', French accents, 0-9.
- ***dateOfBirth***: The customer's date of birth.

### updateContactDetails
```
let contactPhoneNumber = PhoneNumberType(smsCompatible: true, number: "+15141234567")
let request = UpdateContactDetailsRequest(
   onboardingApplicationId: applicationId,
   customerContactDetails: ContactDetails(
       ontactPhoneNumber: contactPhoneNumber,
       customerEmail: "john.miller@mail.com")
)
sdk.onboardingClient.updateContactDetails(request: request) { result in
   switch result {
   case .success(let response):
       if (response.basicDetails.requestStatus != "OK") {
           // Could not process the request properly. You can retry the request.
       }
  
   case .failure(let error):
       print("Update contact details error: \(error)")
   }
}
```

#### UpdateContactDetailsRequest

Used to update the customer contact details in the onboarding application.

- ***onboardingApplicationId***: The UUID of the onboarding application to update.
- ***customerContactDetails***: See [contact details](#contactdetails).

#### UpdateContactDetailsResponse
- ***basicDetails***: The basic details of the onboarding application response. See [OnboardingOperationResponseDetails](#onboardingoperationresponsedetails).

#### ContactDetails
- ***contactPhoneNumber***: The contact phone number. This value is optional and must be in a valid phone number format.
- ***customerEmail***: The contact email address. This value is required and must be in a valid email address format.

### updateAccountDetails

Fetch account purpose and source of funds values from [reference data client](#referencedataclient).

- `dataType: ReferenceDataTypes.accountPurpose`
- `dataType: ReferenceDataTypes.sourceOfFunds`
  
```
let request = UpdateAccountDetailsRequest(
   onboardingApplicationId: applicationId,
   accountDetails: AccountUsageDetails(
       accountPurpose: accountPurpose.code,
       primarySourceOfFunds: [sourceOfFunds.code]),
        authorizedThirdPartyUsage = authorizedThirdPartyUsage
)
sdk.onboardingClient.updateAccountDetails(request: request) { result in
   switch result {
   case .success(let response):
       if (response.basicDetails.requestStatus != "OK") {
           // Could not process the request properly. You can retry the request.
       }
  
   case .failure(let error):
       print("Update account details error: \(error)")
   }
}
```

#### UpdateAccountDetailsRequest

Used to update the customer account details in the onboarding application.

- ***onboardingApplicationId***: The UUID of the onboarding application to update.
- ***accountDetails***: See [account usage details](#accountusagedetails).

#### UpdateAccountDetailsResponse
- ***basicDetails***: The basic details of the onboarding application response. See [OnboardingOperationResponseDetails](#onboardingoperationresponsedetails).

#### AccountUsageDetails

Providing the source of funds and purpose for applying for an Account.

- ***accountPurpose***: The intended purpose of the Account. This value is required. The value stored here is one of the code values retrieved from the [ReferenceDataClient](#referencedataclient) with ***dataType*** set to ***ReferenceDataTypes.accountPurpose***.
- ***primarySourceOfFunds***: The values stored here are a collection of the code values retrieved from the [ReferenceDataClient](#referencedataclient) with ***dataType*** set to ***ReferenceDataTypes.sourceOfFunds***.
- ***authorizedThirdPartyUsage***: Indicates wether or not the account will be used on behalf of third parties not otherwise identified as owners or authorized users. **True** means that account will be used on behalf of third parties while **false** means that account will not be used on behalf of third parties.

### updateAddresses
You need to use [AddressClient](#addressclient) to get the user's address.
```
let request = UpdateAddressesRequest(
   onboardingApplicationId: applicationId,
   addresses: [address]
)
sdk.onboardingClient.updateAddresses(request: request) { result in
   switch result {
   case .success(let response):
       if (response.basicDetails.requestStatus != "OK") {
           // Could not process the request properly. You can retry the request.
       }
  
   case .failure(let error):
       print("Update addresses error: \(error)")
   }
}
```

#### UpdateAddressesRequest

Used to update the addresses occupied by the customer in the onboarding application.

- ***onboardingApplicationId***: The UUID of the onboarding application to update.
- ***addresses***: Collection of addresses occupied by the customer. See [Address](#address).

#### UpdateAddressesResponse
- ***basicDetails***: The basic details of the onboarding application response. See [OnboardingOperationResponseDetails](#onboardingoperationresponsedetails).

#### Address
- ***type***: The type of Address being represented. Must be either **"HOME"** or **"WORK"** (required).
- ***organization***: The first line of the Address. This value is required and has a maximum of 300 characters.
- ***line1***: The first line of the Address. This value is required and has a maximum of 300 characters.
- ***line2***: The second line of the Address. This value is optional, and has a maximum of 300 characters.
- ***line3***: The third line of the Address. This value is optional, and has a maximum of 300 characters.
- ***line4***: The fourth line of the Address. This value is optional, and has a maximum of 300 characters.
- ***line5***: The fifth line of the Address. This value is optional, and has a maximum of 300 characters.
- ***line6***: The sixth line of the Address. This value is optional, and has a maximum of 300 characters.
- ***line7***: The seventh line of the Address. This value is optional, and has a maximum of 300 characters.
- ***line8***: The eighth line of the Address. This value is optional, and has a maximum of 300 characters.
- ***locality***: The local sub administrative area such as the municipality or community name. In Canadian and US addresses, this corresponds to the city or town name. This value is optional and has a maximum of 150 characters. e.g. Toronto, Orlando.
- ***dependentLocality***: The municipality or community name of the major postal region address. For Canadian and US addresses, this can remain empty. This value is optional and has a maximum of 150 characters. 
- ***doubleDependentLocality***: The municipality or community name of the local postal region address. For Canadian and US addresses, this can remain empty. This value is optional and has a maximum of 150 characters.
- ***administrativeArea***: The name of the first level administrative area. This value is optional and has a maximum of 150 characters. e.g. For US this would be 'State' and Canada this would be 'Province'
- ***subAdministrativeArea***: The name of the second level administrative area. This value is optional and has a maximum of 150 characters. e.g. For a city use 'City', county use 'County', etc.
- ***area***: The name of country first level administrative subdivision area in the address. For Canada, this is name of the province and for the US this is the name of the State. This field is required and has
- ***postalCode***: The local postal code for routing mail to the address and should contain the ZIP code for US addresses. This field is required and has a maximum of 150 characters.
- ***country***: The name of the country for the Address. This value is required and has a maximum of 50 characters.

### updateConsents
Fetch consent values from [reference data client](#referencedataclient) with `dataType: ReferenceDataTypes.consentType`.
```
let request = UpdateConsentsRequest(
   onboardingApplicationId: applicationId,
   consents: [
       Consent(consentType: consent.code,
               consentTimestamp: Date(),
               documentNameAndVersion: "https://must-be-a-well-formed-url.com/document/1.0",
               viewTimestamp: Date())
   ]
)
sdk.onboardingClient.updateConsents(request: request) { result in
   switch result {
   case .success(let response):
       if (response.basicDetails.requestStatus != "OK") {
           // Could not process the request properly. You can retry the request.
       }
  
   case .failure(let error):
       print("Update consents error: \(error)")
   }
}
```

#### UpdateConsentsRequest

Used to update the consents received from the customer for the onboarding application.

- ***onboardingApplicationId***: The UUID of the onboarding application to update.
- ***consents***: Collection of consents. See [Consent](#consent).

#### UpdateConsentsResponse
- ***basicDetails***: The basic details of the onboarding application response. See [OnboardingOperationResponseDetails](#onboardingoperationresponsedetails).

#### Consent

Consent is indicating the receipt of a requested customer Consent.

- ***consentType***: The type of Consent received from the customer. This value is required. The value stored here is one of the code values retrieved from the [ReferenceDataClient](#referencedataclient) with ***dataType*** set to **ReferenceDataTypes.consentType**.
- ***documentNameAndVersion***: The name and version of the document in URL format that the customer consented to.
- ***viewTimestamp***: The UTC timestamp when the customer viewed the document.

### updateCustomerResidency
```
let request = UpdateCustomerResidencyRequest(
   onboardingApplicationId: applicationId,
   residency: CustomerResidency(
       isCanadianTaxResident: true,
       otherResidences: nil,
       sinToken: nil,
       citizenship: "CA")
)
sdk.onboardingClient.updateCustomerResidency(request: request) { result in
   switch result {
   case .success(let response):
       if (response.basicDetails.requestStatus != "OK") {
           // Could not process the request properly. You can retry the request.
       }
  
   case .failure(let error):
       print("Update customer residency error: \(error)")
   }
}
```

#### UpdateCustomerResidencyRequest

Used to update the customer residency status in the onboarding application.

- ***onboardingApplicationId***: The UUID of the onboarding application to update.
- ***residency***: The customer residency status. See [CustomerResidency](#customerresidency).

#### UpdateCustomerResidencyResponse
- ***basicDetails***: The basic details of the onboarding application response. See [OnboardingOperationResponseDetails](#onboardingoperationresponsedetails).

#### CustomerResidency

Contains the residency status details of the customer.

- ***isCanadianTaxResident***: The Canadian residency status. This is true if the customer is a Canadian resident, false otherwise.
- ***otherResidences***: A collection of other countries where the customer has residence. At least one is required if not a Canadian resident. See [OtherResidence](#otherResidence).
- ***sinToken***: A tokenized UUID representation of the customer SIN number.
- ***citizenship***: The primary or current citizenship of the customer. The format is an ISO 3166 Country alpha-2 code (e.g. US, CA, FR).

#### OtherResidence

Alternate countries and places of residence.

- ***otherResidenceCountry***: The name of the other country of residence.
- ***tinToken***: A tokenized UUID representation of the unique TIN for the country of residence. Optional.
- ***citizenship***: The citizenship status in the country of residence. Required if residence country is US or Canada.

### updateDisclosures
Fetch disclosure type values from [reference data client](#referencedataclient) with `dataType: ReferenceDataTypes.disclosureType`.
```
let request = UpdateDisclosuresRequest(
   onboardingApplicationId: applicationId,
   disclosures: [
       Disclosure(
           disclosureType: disclosure.code,
           timestamp: Date(),
           documentNameAndVersion: "https://must-be-a-well-formed-url.com/document/1.0")
   ]
)
sdk.onboardingClient.updateDisclosures(request: request) { result in
   switch result {
   case .success(let response):
       if (response.basicDetails.requestStatus != "OK") {
           // Could not process the request properly. You can retry the request.
       }
  
   case .failure(let error):
       print("Update disclosures error: \(error)")
   }
}
```

#### UpdateDisclosuresRequest

Used to update the disclosures received by the customer in the onboarding application

- ***onboardingApplicationId***: The UUID of the onboarding application to update.
- ***disclosures***: Collection of disclosures. See [Disclosure](#disclosure).

#### UpdateDisclosuresResponse
- ***basicDetails***: The basic details of the onboarding application response. See [OnboardingOperationResponseDetails](#onboardingoperationresponsedetails).

#### Disclosure

Customer receipt of a documented Disclosure.

- ***disclosureType***: The type of Disclosure sent to the customer. This value is required. The value stored here is one of the code values retrieved from the [ReferenceDataClient](#referencedataclient) with ***dataType*** set to **ReferenceDataTypes.disclosureType**.
- ***timestamp***: The UTC timestamp when the customer viewed the document.
- ***documentNameAndVersion***: Name and version of the disclosure document in URL format that the customer viewed.

### updateEmployment
Fetch employment status from [reference data client](#referencedataclient).

- `dataType: ReferenceDataTypes.occupation`
- `dataType: ReferenceDataTypes.industry`
- `dataType: ReferenceDataTypes.employmentStatus`
  
```
let phoneNumber = PhoneNumberType(smsCompatible: true, number: "+15141234567")
let request = UpdateEmploymentRequest(
   onboardingApplicationId: applicationId,
   employmentInfo: Employment(
       employmentStatus: employment.code,
       employerName: "Finaptic",
       industry: industry.code,
       occupation: occupation.code,
       startDate: Calendar.current.date(from: DateComponents(year: 2019, month: 1, day: 1)),
       endDate: Calendar.current.date(from: DateComponents(year: 2020, month: 12, day: 31)),
       workAddress: address,
       phoneNumber: phoneNumber)
)
sdk.onboardingClient.updateEmployment(request: request) { result in
   switch result {
   case .success(let response):
       if (response.basicDetails.requestStatus != "OK") {
           // Could not process the request properly. You can retry the request.
       }
 
   case .failure(let error):
       print("Update employment error: \(error)")
   }
}
```

#### UpdateEmploymentRequest

Used to update the customer employment information in the onboarding application.

- ***onboardingApplicationId***: The UUID of the onboarding application to update.
- ***employmentInfo***: Employment information. See [Employment](#employment).

#### UpdateEmploymentResponse
- ***basicDetails***: The basic details of the onboarding application response. See [OnboardingOperationResponseDetails](#onboardingoperationresponsedetails).

#### Employment

Employment defines the details of Employment for the customer being onboarded.

- ***employmentStatus***: The status of Employment. The value is retrieved from the [ReferenceDataClient](#referencedataclient) with ***dataType*** set to **ReferenceDataTypes.employmentStatus**.
- ***employerName***: The full employer name. The value is required and has a maximum of 50 characters.
- ***industry***: The industry code the Employment is in. This value is required and has a maximum of 50 characters. The value is retrieved from the /[ReferenceDataClient](#referencedataclient) with ***dataType*** set to **ReferenceDataTypes.industry**.
- ***occupation***: The roles and duties the customer performs for the employer. This value is optional and has a maximum of 50 characters. The value is retrieved from the [ReferenceDataClient](#referencedataclient) with ***dataType*** set to **ReferenceDataTypes.occupation**. If the employment status is 'EMPLOYMENT_STATUS_UNEMPLOYED', 'EMPLOYMENT_STATUS_RETIRED', 'EMPLOYMENT_STATUS_STUDENT_UNEMPLOYED', or 'EMPLOYMENT_STATUS_HOMEMAKER' then the Occupation field must be left empty.
- ***startDate***: The month and year the customer started this position.
- ***endDate***: The month and year the customer no longer held this position.
- ***workAddress***: The 'type' field must be set to 'WORK'. Required. See [Address](#address).
- ***phoneNumber***: The phone number of the employer. Required.

### updateCommunicationPreferences
Fetch communication preferences from [reference data client](#referencedataclient) with `dataType: ReferenceDataTypes.communicationPreference`.
```
let request = UpdateCommunicationPreferencesRequest(
           onboardingApplicationId: applicationId,
           customerCommunicationPref: communicationPreference.code
)
sdk.onboardingClient.updateCommunicationPreferences(request: request) { result in
   switch result {
   case .success(let response):
       if (response.basicDetails.requestStatus != "OK") {
           // Could not process the request properly. You can retry the request.
       }
 
   case .failure(let error):
       print("Update communication preferences error: \(error)")
   }
}
```

#### UpdateCommunicationPreferencesRequest

Used to update the communication preferences of the customer in the onboarding application.

- ***onboardingApplicationId***: The UUID of the onboarding application to update.
- ***customerCommunicationPref***: The customer communication preference. The value is retrieved from the [ReferenceDataClient](#referencedataclient) with ***dataType*** set to **ReferenceDataTypes.communicationPreference**.
- ***customerCommunicationLanguage***: The customer communication language. The value is retrieved from the [ReferenceDataClient](#referencedataclient) with ***dataType*** set to **ReferenceDataTypes.language**.

#### UpdateCommunicationPreferencesResponse
- ***basicDetails***: The basic details of the onboarding application response. See [OnboardingOperationResponseDetails](#onboardingoperationresponsedetails).

### createFileUploadURL
Create upload url for document front, back and selfie.
 
```
let request = CreateFileUploadUrlRequest(onboardingApplicationId: applicationId)
sdk.onboardingClient.createFileUploadURL(request: request) { result in
   guard let self = self else { return }
   switch result {
   case .success(let response):
       print("File upload url: \(result.uploadUrl)")
 
   case .failure(let error):
       print("Create file upload url for front error: \(error)")
   }
}
```

#### CreateFileUploadUrlRequest
- ***onboardingApplicationId***: The UUID representing the onboarding application that requested a file upload URL.

#### CreateFileUploadUrlResponse
- ***filename***: The generated name for the file to upload. This value must be used for [ValidateDocumentsRequest](#validatedocumentsrequest) or [ValidateSelfieRequest](#validateselfierequest).
- ***uploadUrl***: The URL to use to upload the file.

### validateDocuments
Fetch document types from [reference data client](#referencedataclient) with `dataType: ReferenceDataTypes.documentType`.
```
let request = ValidateDocumentsRequest(
   onboardingApplicationId: applicationId,
   documentDetails: [
       DocumentInfo(
           documentType: documentType.code,
           documentCountry: "CA",
           frontImageFileName: idFrontFileUpload.filename,
           backImageFileName: idBackFileUpload.filename,
           
            // The state issuing the document used to identify the applicant.
            // documentState must be set when document type is driver license.
            // The value stored here is one of the code values retrieved from the
            // ReferenceDataClient with dataType set to ReferenceDataTypes.state concatenate with country code retrieved from dataType set to ReferenceDataTypes.country.
            // e.g. 'STATE_CA' will retrieve all states from Canada ('BC', 'AB', 'QC'...)
            documentState = documentStateType.code)
   ]
)
sdk.onboardingClient.validateDocuments(request: request) { result in
   switch result {
   case .success(let response):
       if (response.basicDetails.requestStatus != "OK") {
           // Could not process the request properly. You can retry the request.
       }
 
       // The following check is only for validateSelfie, validateDocuments and validateApplication
       if (response.basicDetails.onboardingApplicationStatus == ApplicationStatus.dataCollection ||
           response.basicDetails.onboardingApplicationStatus == ApplicationStatus.kycFailed ||
           response.basicDetails.onboardingApplicationStatus == ApplicationStatus.kycRejected) {

           // The user was not approved during validation. 
           // You may ask the user to check his info, retake the images and update/validate again.
                      
           let failureStatusCodes = details.failureStatusCodes
           if (!failureStatusCodes.isEmpty) {
                // The list of codes explaining the reason of the request outcome.
                // This list will only be populated when request status is "KYC Failed", "KYC Rejected", or "Application Rejected".
                // The list will otherwise be empty. The value stored here is one of the code values retrieved from the
                // ReferenceDataClient with dataType set to ReferenceDataClient.failureCode.

                // You can't retry after getting one of the following error codes:
                // FAILURE_CODE_DEFAULT
                // FAILURE_CODE_AGE_OF_MAJORITY

                print("failureStatusCodes: \(failureStatusCodes)")
           }
       }
 
   case .failure(let error):
       print("Validate documents error: \(error)")
   }
}
```

#### ValidateDocumentsRequest

Used to perform operation of validating the identification documents received from the customer in the onboarding application.

- ***onboardingApplicationId***: The UUID of the onboarding application to validate.
- ***documentDetails***: Collection of documents to validate. See [DocumentInfo](#documentinfo).

#### ValidateDocumentsResponse
- ***basicDetails***: The basic details of the onboarding application response. See [OnboardingOperationResponseDetails](#onboardingoperationresponsedetails).

For error handling, you need to check ***failureStatusCodes*** if the ***requestStatus*** is **"KYC Failed"**, **"KYC Rejected"**, or **"Application Rejected"**. The values stored here is one of the code values retrieved from the [ReferenceDataClient](#referencedataclient) with ***dataType*** set to ***ReferenceDataTypes.failureCode***.

You can't retry after getting one of the following error codes:

- **"FAILURE_CODE_DEFAULT"**
- **"FAILURE_CODE_AGE_OF_MAJORITY"**

#### DocumentInfo

Details of an onboarding document collected from the customer to prove identity.

- ***documentType***: The type of document used for identification. The value here is one of the code values retrieved from the [ReferenceDataClient](#referencedataclient) with ***dataType*** set to ***ReferenceDataTypes.documentType***. e.g. **"DOCUMENT_TYPE_CANADIAN_DRIVERS_LICENSE"** or **"DOCUMENT_TYPE_CANADIAN_PASSPORT"**.
- ***documentCountry***: The country the document was issued for.
- ***frontImageFileName***: The file name of the uploaded image of the front of the document. (required, file size <4MB)
- ***backImageFileName***: The file name of the uploaded image of the back of the document. (required, file size <4MB)
- ***documentState***: The state issuing the document used to identify the applicant. The value here is one of the code values retrieved from the [ReferenceDataClient](#referencedataclient) with ***dataType*** set to **"STATE_"** concatenate with country code retrieved from ***dataType*** set to ***ReferenceDataTypes.country***. e.g. 'STATE_CA' will retrieve all states from Canada ('BC', 'AB', 'QC'...)

### validateSelfie
```
let request = ValidateSelfieRequest(
   onboardingApplicationId: applicationId,
   fileName: selfieFileUpload.filename
)
sdk.onboardingClient.validateSelfie(request: request) { result in
   switch result {
   case .success(let response):
       if (response.basicDetails.requestStatus != "OK") {
           // Could not process the request properly. You can retry the request.
       }
 
       // The following check is only for validateSelfie, validateDocuments and validateApplication
       if (response.basicDetails.onboardingApplicationStatus == ApplicationStatus.dataCollection ||
           response.basicDetails.onboardingApplicationStatus == ApplicationStatus.kycFailed ||
           response.basicDetails.onboardingApplicationStatus == ApplicationStatus.kycRejected) {

           // The user was not approved during validation. 
           // You may ask the user to check his info, retake the images and update/validate again.
                      
           let failureStatusCodes = details.failureStatusCodes
           if (!failureStatusCodes.isEmpty) {
                // The list of codes explaining the reason of the request outcome.
                // This list will only be populated when request status is "KYC Failed", "KYC Rejected", or "Application Rejected".
                // The list will otherwise be empty. The value stored here is one of the code values retrieved from the
                // ReferenceDataClient with dataType set to ReferenceDataClient.failureCode.

                // You can't retry after getting one of the following error codes:
                // FAILURE_CODE_DEFAULT
                // FAILURE_CODE_AGE_OF_MAJORITY

                print("failureStatusCodes: \(failureStatusCodes)")
           }
       }
 
   case .failure(let error):
       print("Validate selfie error: \(error)")
   }
}
```

#### ValidateSelfieRequest

Used to perform operation of validating the selfie pictures received from the customer in the onboarding application.

- ***onboardingApplicationId***: The UUID of the onboarding application to validate.
- ***fileName***: The name of the selfie file to validate. (required, file size <4MB)

#### ValidateSelfieResponse
- ***basicDetails***: The basic details of the onboarding application response. See [OnboardingOperationResponseDetails](#onboardingoperationresponsedetails).

For error handling, you need to check ***failureStatusCodes*** if the ***requestStatus*** is **"KYC Failed"**, **"KYC Rejected"**, or **"Application Rejected"**. The values stored here is one of the code values retrieved from the [ReferenceDataClient](#referencedataclient) with ***dataType*** set to ***ReferenceDataTypes.failureCode***.

You can't retry after getting one of the following error codes:

- **"FAILURE_CODE_DEFAULT"**
- **"FAILURE_CODE_AGE_OF_MAJORITY"**

### validateApplication
```
let request = ValidateApplicationRequest(onboardingApplicationId: applicationId)
sdk.onboardingClient.validateApplication(request: request) { result in
   switch result {
   case .success(let response):
       if (response.basicDetails.requestStatus != "OK") {
           // Could not process the request properly. You can retry the request.
       }
 
       // The following check is only for validateSelfie, validateDocuments and validateApplication
       if (response.basicDetails.onboardingApplicationStatus == ApplicationStatus.dataCollection ||
           response.basicDetails.onboardingApplicationStatus == ApplicationStatus.kycFailed ||
           response.basicDetails.onboardingApplicationStatus == ApplicationStatus.kycRejected) {

           // The user was not approved during validation. 
           // You may ask the user to check his info, retake the images and update/validate again.
                      
           let failureStatusCodes = details.failureStatusCodes
           if (!failureStatusCodes.isEmpty) {
                // The list of codes explaining the reason of the request outcome.
                // This list will only be populated when request status is "KYC Failed", "KYC Rejected", or "Application Rejected".
                // The list will otherwise be empty. The value stored here is one of the code values retrieved from the
                // ReferenceDataClient with dataType set to ReferenceDataClient.failureCode.

                // You can't retry after getting one of the following error codes:
                // FAILURE_CODE_DEFAULT
                // FAILURE_CODE_AGE_OF_MAJORITY

                print("failureStatusCodes: \(failureStatusCodes)")
           }
       }
 
   case .failure(let error):
       print("Validate application error: \(error)")
   }
}
```

#### ValidateApplicationRequest

Used to perform operation of validating the entire onboarding application and all documents received from the customer.

- ***onboardingApplicationId***: The UUID of the onboarding application to validate.

#### ValidateApplicationResponse
- ***basicDetails***: The basic details of the onboarding application response. See [OnboardingOperationResponseDetails](#onboardingoperationresponsedetails).

For error handling, you need to check ***failureStatusCodes*** if the ***requestStatus*** is **"KYC Failed"**, **"KYC Rejected"**, or **"Application Rejected"**. The values stored here is one of the code values retrieved from the [ReferenceDataClient](#referencedataclient) with ***dataType*** set to ***ReferenceDataTypes.failureCode***.

You can't retry after getting one of the following error codes:

- **"FAILURE_CODE_DEFAULT"**
- **"FAILURE_CODE_AGE_OF_MAJORITY"**

### acceptApplication
```
let request = AcceptApplicationRequest(onboardingApplicationId: applicationId)
sdk.onboardingClient.acceptApplication(request: request) { result in
   switch result {
   case .success(let response):
       if (response.basicDetails.requestStatus != "OK") {
           // Could not process the request properly. You can retry the request.
       }
 
   case .failure(let error):
       print("Accept application error: \(error)")
   }
}
```

#### AcceptApplicationRequest

Used to perform operation of accepting the onboarding application received from the customer.

- ***onboardingApplicationId***: The UUID of the onboarding application to accept.

#### AcceptApplicationResponse
- ***basicDetails***: The basic details of the onboarding application response. See [OnboardingOperationResponseDetails](#onboardingoperationresponsedetails).

### finalizeApplicationCreateCustomer
The user needs to be [authenticated](#authenticationclient) before you create a customer.
```
let request = FinalizeApplicationCreateCustomerRequest(onboardingApplicationId: applicationId)
sdk.onboardingClient.finalizeApplicationCreateCustomer(request: request) { result in
   switch result {
   case .success(let response):
       if (response.basicDetails.requestStatus != "OK") {
           // Could not process the request properly. You can retry the request.
       }
 
   case .failure(let error):
       print("Finalize application create customer error: \(error)")
   }
}
```

#### FinalizeApplicationCreateCustomerRequest

Used to perform operation of creating the customer for the onboarding application.

- ***onboardingApplicationId***: The UUID of the onboarding application to create a customer for.

#### FinalizeApplicationCreateCustomerResponse
- ***basicDetails***: The basic details of the onboarding application response. See [OnboardingOperationResponseDetails](#onboardingoperationresponsedetails).
- ***customerId***: The UUID representing the newly created customer.

### finalizeApplicationCreateProduct
```
let request = FinalizeApplicationCreateProductRequest(onboardingApplicationId: applicationId)
sdk.onboardingClient.finalizeApplicationCreateProduct(request: request) { result in
   switch result {
   case .success(let response):
       if (response.basicDetails.requestStatus != "OK") {
           // Could not process the request properly. You can retry the request.
       }
      
   case .failure(let error):
       print("Finalize application create product error: \(error)")
   }
}
```

#### FinalizeApplicationCreateProductRequest

Used to perform operation of creating the product for the onboarding application.

- ***onboardingApplicationId***: The UUID of the onboarding application to create the product for.

#### FinalizeApplicationCreateProductResponse
- ***basicDetails***: The basic details of the onboarding application response. See [OnboardingOperationResponseDetails](#onboardingoperationresponsedetails).
- ***products***: Collection of products created. See [OnboardedProduct](#onboardedproduct).

#### OnboardedProduct
- ***productId***: The UUID of the onboarded product.
- ***sourceDomain***: The source domain of the onboarded product.

## Authorized Users 

### InviteAuthorizedUser
```
let request = InviteAuthorizedUserRequest(
                    invitedCustomerIdentifier: "test@finaptic.com",
                    accountId: account.id)
sdk.onboardingClient.inviteAuthorizedUser(request: request) { result in
    switch result {
    case .success(let response):
        print("Invite authorized user success with id \(response.invitation.invitationId)")
    
    case .failure(let error):
        print("Invite authorized user error: \(error)")
    }
}
```

#### InviteAuthorizedUserRequest

Used to perform operation of inviting a user to join an account as an authorized user.

- ***invitedCustomerIdentifier***: The identifier to be used to lookup for the invited customer. This can either the username, or the email address of the customer to invite. If the identifier does not map to any known user, the operation will succeed, if this occurs the invitation will expire automatically after a period of time, without having been accepted or declined.
- ***accountId***: The ID of the account on which an authorized user is to be added. The customer issuing the request must be an Owner for the account.
- ***invitationMetadata***: Optional. Additional metadata to attach to the invitation, to be consumed by the invited user before accepting the invitation.

#### InviteAuthorizedUserResponse
- ***invitation***: The resulting invitation created by the InviteAuthorizedUserRequest. See [AuthorizedUserInvitation](#authorizeduserinvitation) for details.

Errors:

- If the customer mapping to this identifier is already an Owner or an Authorized User for the account, then the operation will fail with a **FinapticErrorCode.alreadyExists** code,  and provide a message indicating the reason for the conflict. 
- If there is already an invitation that was issued for this account, for the invited user, the operation will fail with an **FinapticErrorCode.alreadyExists** code, and provide a message indicating the reason for the conflict.
- If the customer issuing the request isn't an Owner, then the operation will fail with a **FinapticErrorCode.permissionDenied** code, and provide a message indicating the reason. 
- If there is already an invitation that was issued for this account, for the invited customer, the operation will fail with a **FinapticErrorCode.alreadyExists** code, and provide a message indicating the reason for the conflict.

#### AuthorizedUserInvitation
- ***invitationId***: The unique identifier of the invitation. This value is required to Accept, Decline, or Confirm invitations.
- ***issuerCustomerId***: The customer ID of the person that issued the invitation.
- ***invitedCustomerId***: The customer ID of the person that was invited to join the account as an authorized user.
- ***accountId***: The account ID for which an invitation was sent.
- ***status***: The current status of the invitation. Refer to [AuthorizedUserInvitationStatus](#authorizeduserinvitationstatus-enum) enumeration for details on statuses and meaning.
- ***creationTime***: The time at which the invitation was issued.
- ***expirationTime***: Time at which it will no longer be possible for the invited customer to Accept the invitation.
- ***invitationMetadata***: Additional metadata that has been attached to the invitation. This will contain the metadata specified by the last operation on the invitation.


#### AuthorizedUserInvitationStatus enum
- ***pending***: Represents an invitation that is waiting for an accept or decline response from the invited user.
- ***accepted***: Represents an invitation that was accepted by the invited user, and pending completion by the issuer of the invitation.
- ***declined***: Represents an invitation that was declined by the invited user. A declined invitation cannot be completed.
- ***completed***: Represents an invitation that was successfully completed by the issuer of the invitation. In this status, the invited customer has successfully joined the account as an Authorized User.
- ***expired***: Represents an invitation that wasn't accepted or declined within the allotted time. An expired invitation cannot be accepted or declined.

### AcceptAuthorizedUserInvitation
```
let request = AcceptAuthorizedUserInvitationRequest(
                    invitationId: invitation.invitationId)
sdk.onboardingClient.acceptAuthorizedUserInvitation(request: request) { result in
    switch result {
    case .success(_):
        print("Accepted authorized user invitation with id \(invitation.invitationId)")
    
    case .failure(let error):
        print("Accept authorized user invitation error: \(error)")
    }
}
```

#### AcceptAuthorizedUserInvitationRequest

Used to perform operation of accepting an invitation issued to the user to join an account as an authorized user. 

- ***invitationId***: The ID of the invitation to accept. This must represent a valid invitation that was issued to the current user.
- ***invitationMetadata***: Optional. Additional metadata to attach to the response to the invitation, for the consumption by inviter before confirming the invitation.

#### AcceptAuthorizedUserInvitationResponse
*Response intentionally left without fields.*

Errors:

- If the current user is not the user the invitation was issued to, then the operation will fail with a **FinapticErrorCode.permissionDenied** code, and provide a message indicating the reason for the conflict.
- If the invitation is not in the Pending status, the operation will fail with a **FinapticErrorCode.failedPrecondition** code, and provide a message indicating that status was invalid. 
- If the invitation has expired, the operation will also fail with a **FinapticErrorCode.failedPrecondition** code, and provide a message indicating that invitation has expired.

### DeclineAuthorizedUserInvitation
```
let request = DeclineAuthorizedUserInvitationRequest(
                    invitationId: invitation.invitationId)
sdk.onboardingClient.declineAuthorizedUserInvitation(request: request) { result in
    switch result {
    case .success(_):
        print("Declined authorized user invitation with id \(invitation.invitationId)")
    
    case .failure(let error):
        print("Decline authorized user invitation error: \(error)")
    }
}
```

#### DeclineAuthorizedUserInvitationRequest
- ***invitationId***: The ID of the invitation to decline. This must represent a valid invitation that was issued to the current user. 
- ***invitationMetadata***: Optional. Additional metadata to attach to the response to the invitation, for the consumption by inviter when reviewing invitation status.

#### DeclineAuthorizedUserInvitationResponse
*Response intentionally left without fields.*

Errors:

- If the current user is not the user the invitation was issued to, then the operation will fail with a **FinapticErrorCode.permissionDenied** code, and provide a message indicating the reason for the conflict.
- If the invitation is not in the Pending status, the operation will fail with a **FinapticErrorCode.failedPrecondition** code, and provide a message indicating that status was invalid.
- If the invitation has expired, the operation will also fail with a **FinapticErrorCode.failedPrecondition** code, and provide a message indicating that invitation has expired.

### ConfirmAuthorizedUserInvitation
```
let request = ConfirmAuthorizedUserInvitationRequest(
    invitationId: invitation.invitationId)
sdk.onboardingClient.confirmAuthorizedUserInvitation(request: request) { result in
    switch result {
    case .success(_):
        print("Confirm authorized user invitation with id \(invitation.invitationId)")
    
    case .failure(let error):
        print("Confirm authorized user invitation error: \(error)")
    }
}
```

#### ConfirmAuthorizedUserInvitationRequest

Used to perform operation of confirming an invitation that was accepted by the invited customer. 

- ***invitationId***: The ID of the invitation to confirm.

#### ConfirmAuthorizedUserInvitationResponse
*Response intentionally left without fields.*

Errors:

- If the current user is not the issuer of the invitation, then the operation will fail with a **FinapticErrorCode.permissionDenied**  code, and provide a message indicating the reason for the conflict.
- If the invitation is not in the Accepted status, the operation will fail with a **FinapticErrorCode.failedPrecondition**  code, and provide a message indicating that status was invalid.

### RemoveAuthorizedUser
```
let request = RemoveAuthorizedUserRequest(accountId: accountId,
                                          authorizedUser: authorizedUserId)
self.sdk.onboardingClient.removeAuthorizedUser(request: request) { result in
    switch result {
    case .success(let response):
        print("Removed authorized user. \(response.authorizedUsers)")
    
    case .failure(let error):
        print("Remove authorized user failed with error: \(error)")
    }
}
```

A user can remove an authorized user only if they have completed the step up authentication with a scope set to **"remove:authorized_user"**.
```
let request = SignInRequest(credential: credential, scope: "remove:authorized_user")
sdk.authenticationClient.signIn(request: request) { result in
    switch result {
    case .success(_):
        print("You may call removeAuthorizedUser")
    
    case .failure(let error):
        print("Sign in error: \(error)")
    }
}
```

#### RemoveAuthorizedUserRequest

Used to remove an authorized user from the provided Account.

- ***accountId***: The ID of the Account where the authorized is removed from. 
- ***authorizedUser***:  The ID of the authorized user to add to the Account.

#### RemoveAuthorizedUserResponse
- ***authorizedUsers***: The resulting list of authorized users of the Account.

### GetInvitations

Returns the list of invitations for the current user. This will include invitations issued by the current user, as well as invitations where the current user is the invited customer. This operation always return all invitations, including invitations that are pending, expired, have been accepted, declined, require confirmation or that have already been completed.
    
```
sdk.onboardingClient.getInvitations(request: GetAuthorizedUserInvitationsRequest()) { result in
    switch result {
    case .success(let response):
        if (response.invitations.isEmpty) {
            print("No invitations")
        } else {
            response.invitations.forEach { print("Invitation with \($0.status) status and id \($0.invitationId)") }
        }
        completion(response.invitations)
    
    case .failure(let error):
        print("Get invitations error: \(error)")
    }
}
```

#### GetAuthorizedUserInvitationsRequest
*At this moment, there are no configurable options for this request.*

#### GetAuthorizedUserInvitationsResponse
- ***invitations***: The list of invitations for the current user. See [AuthorizedUserInvitation](#authorizeduserinvitation)

## ReferenceDataClient
 
ReferenceDataTypes:

- addressType
- consentType
- disclosureType
- documentType
- communicationPreference
- accountPurpose
- sourceOfFunds
- employmentStatus
- onboardingStatus
- country
- state
- occupation
- industry
- language
- failureCode
  
```
let request = GetReferenceDataByTypeRequest(dataType: ReferenceDataTypes.accountPurpose,
                                               locale: "EN_CA")
sdk.referenceDataClient.regetReferenceDataByType(request: request) { result in
   switch result {
   case .success(let response):
       print(response.data)
  
   case .failure(let error):
       print("Reference data error: \(error)")
   }
}
```
## AddressClient
 
Address client provides a list of suggested addresses. Make sure to use debouncing or throttling when calling this function on user input.
```
let request = AddressSuggestionsRequest(
   onboardingApplicationId: onboardingApplicationId,
   pattern: "123",
   addressType: AddressTypes.homeAddressType
)
sdk.addressClient.getSuggestions(request: request) { result in
   switch result {
   case .success(let response):
       print(response.suggestions.first)
  
   case .failure(let error):
       print("Address suggestions error: \(error)")
   }
}
```
 
## AuthenticationClient
### signUp
```
let credential = Credential(
           email: "sample@finaptic.com",
           password: "Password1234!@#$",
           username: "Username"
       )
 
let request = SignUpRequest(credential: credential)
sdk.authenticationClient.signUp(request: request) { result in
   switch result {
   case .success(let response):
       print(response)
  
   case .failure(let error):
       print("Sign up error: \(error)")
   }
}
```

Possible error codes:

- ***signupCredentialError***
      - If the password used doesn't comply with the password policy for the connection.
      - The user your are attempting to sign up is invalid.
      - The chosen password is too weak
- ***signupError***: 
      - The chosen password is based on user information. eg. password contains email, username contains emails etc.
      - The user you are attempting to sign up has already signed up.
      - The username you are attempting to sign up with is already in use.

The ***message*** property of **FinapticError** will give more information about the error.

#### Credential
- ***email***: Used to signUp or signIn the user. (requred for signUp)
- ***username***: Username can be up to 15 characters, contain alphanumeric characters and the following characters: '_', '+', '-', '.', '!', '#', '$', ''', '^', '`', '~' and '@'. Can be used to sign up and sign in the user. It might be optional.
- ***password***: Password must have at least 12 characters, 1 lowercase, 1 uppercase, 1 number and should not contain common words or email parts.

### signIn
```
let credential = Credential(
           email: "sample@finaptic.com",
           password: "Password1234!@#$",
           username: "Username"
       )
 
let request = SignInRequest(credential: credential)
sdk.authenticationClient.signIn(request: request) { result in
   switch result {
   case .success(let response):
       print(response)
  
   case .failure(let error):
       print("Sign up error: \(error)")
   }
}
```

Possible error codes:

- ***signinError***
      - The password provided for sign up/update has already been used (reported when password history feature is enabled).
      - The account is blocked due to too many attempts to sign in.
      - If the password has been leaked and a different one needs to be used.
- ***signinInvalidPassword*** 
      - The username and/or password used for authentication are invalid.
      - The password provided does not match the connection's strength requirements.
- ***mfaError***: 
      - The multi-factor authentication (MFA) code provided by the user is invalid/expired.
      - The administrator has required multi-factor authentication, but the user has not enrolled.
      - The user must provide the multi-factor authentication code to authenticate.

The ***message*** property of **FinapticError** will give more information about the error.

### currentAuthState
If the accessToken has expired, the client automatically uses the refreshToken and renews the credentials for you. Returns **nil** if the user is **unauthenticated**.
```
sdk.authenticationClient.currentAuthState() { authState in
   print("Current auth state: \(String(describing: authState))")
}
```
### refreshAuthState
Returns **nil** if the user is **unauthenticated**.
```
sdk.authenticationClient.refreshAuthState() { result in
   switch result {
   case .success(let authState):
       print("Refreshed auth state: \(String(describing: authState))")
      
   case .failure(let error):
       print("Refresh auth state error: \(error)")
   }
}
```
### signOut
```
sdk.authenticationClient.signOut() { error in
   if let error = error {
       print("Sign out error: \(error)")
       return
   }
   print("Signed out")
}
```
### resetPassword

If the call is successful and the email is registered, the user receives a password reset email.

```
let request = ResetRequest(email: "sample@finaptic.com")
sdk.authenticationClient.resetPassword(request: request) { error in
    if let error = error {
        switch error.code {
        case .invalidArgument: 
            print("Valid email is required.")
            
        case .unknown: 
            print("Check your internet connection and try again.")
            
        default: 
            print("Reset Password error: \(error.msg)")
        }
        return
    }

    print("Reset Password success")
}
```

### getUserInfo

On success, **getUserInfo** will return **AuthUserInfo**.
```
authenticationClient.getUserInfo { result in
    switch result {
    case .failure(let error):
        print("Get user info error: \(error)")
        
    case .success(let authUserInfo):
        print("Get user info success: \(authUserInfo))
    }
}
```

#### AuthUserInfo
- ***username***
- ***email***
- ***isEmailVerified***

## CoreBankingClient

### getAccountDetails
```
let request = GetAccountDetailsRequest(accountId: accountId)
sdk.coreBankingClient.getAccountDetails(request: request) { result in
   switch result {
   case .success(let response):
       print(response.status)
       print(response.accruedInterest)
  
   case .failure(let error):
       print("Get account details error: \(error)")
   }
}
```

## CoreCardClient
 
### getCard
```
let request = GetCardRequest(cardId: cardId)
sdk.coreCardClient.getCard(request: request) { result in
   switch result {
   case .success(let response):
       print(response.description)
  
   case .failure(let error):
       print("Get card error: \(error)")
   }
}
```
  
### listCards
```
let request = ListCardsRequest(
           pageSize: 10,
           pageToken: nil
        )
sdk.coreCardClient.listCards(request: request) { result in
   switch result {
   case .success(let response):
       print(response.cards.count)
       print(response.nextPageToken)
  
   case .failure(let error):
       print("List cards error: \(error)")
   }
}
```

### createCardSDKSignOnToken
```
let request = CreateCardSDKSignOnTokenRequest(cardId: cardId)
sdk.coreCardClient.createCardSDKSignOnToken(request: request) { result in
   switch result {
   case .success(let response):
       print(response.token)
  
   case .failure(let error):
       print("Create card SDK sign on token error: \(error)")
   }
}
```

## HelpCenter

The help center adds the ability to connect a Finaptic Customer Service agent for L2+ escalations. We provide APIs to integrate existing customer service chat / threaded conversations into your client application. The expectation is that a new chat should be triggered by an L1 customer support agent, while customers should be able to resume existing threads to provide additional information or view responses from Finaptic.

- Start customer support chat
- List chat threads
- Get a chat thread's history

### startChat

Create ***StartChatRequest***.

- ***displayName*** can be username. You can get it from AuthenticationClient's [getUserInfo](#getuserinfo).
- ***subject*** provided by the customer when starting a new chat or from [ChatThread](#chatthread).
- ***contactCaseId*** is used to resume a thread. Use ***nil*** to start a new one.

```
let request = StartChatRequest(displayName: displayName,
                                subject: subject,
                                contactCaseId: nil)
```

Calling **startChat** will try to connect and return [ChatClient](#chatclient).
```
sdk.helpCenter.startChat(request: request) { result in
    switch result {
    case .failure(let error):
        print("Start chat error: \(error)")

    case .success(let chatClient):
        print("Start chat success")
        self.chatClient = chatClient
    }
}
```

### listChatThreads

**Note:** This method can be called after [Customer is created](#finalizeapplicationcreatecustomer).


Create ***ListChatThreadsRequest***.

- ***nextToken*** is provided by [ListChatThreadsResponse](#listchatthreadsresponse) and can be used to get next page. 

```
let request = ListChatThreadsRequest(nextToken: nil)
```

On success, **listChatThreads** will return [ListChatThreadsResponse](#listchatthreadsresponse).
```
sdk.helpCenter.listChatThreads(request: request) { result in
    switch result {
    case .failure(let error):
        print("List chat threads failed: \(error)")

    case .success(let response):
        print("List chat threads success: \(response.items.count)")
    }
}
```

#### ListChatThreadsResponse

- ***nextToken*** can be used to fetch next page.
- ***items*** is an array of [ChatThread](#chatthread)

#### ChatThread

- ***id***
- ***lastUpdated*** is a Date.
- ***status*** is [ChatThreadStatus](#chatthreadstatus-enum).
- ***subject*** provided by the customer when starting a new chat.
- ***createdAt*** is a Date.
- ***customerId***

#### ChatThreadStatus enum

- **waitingForAgent**
- **waitingForCustomer** when an agent has responded and needs more information from the customer. Waiting for customer should indicate there is an unread message.
- **resolved**
  
### listChatHistory

**Note:** This method can be called after [Customer is created](#finalizeapplicationcreatecustomer).


Create ***ListChatHistoryRequest***.

- ***contactId*** is the **id** of [ChatThread](#chatthread).
- ***nextToken*** is provided by [ListChatHistoryResponse](#listchathistoryresponse) and can be used to get next page. 


```
let request = ListChatHistoryRequest(contactId: contactId, nextToken: nil)
```


On success, **listChatHistory** will return [ListChatHistoryResponse](#listchathistoryresponse).
```
sdk.helpCenter.listChatHistory(request: request) { result in
    switch result {
    case .failure(let error):
        print("List chat history failed: \(error)")

    case .success(let response):
        print("List chat history success: \(response.items.count)")
    }
}
```


#### ListChatHistoryResponse

- ***nextToken*** used to fetch next page.
- ***items*** is an array of [ChatMessage](#chatmessage)


## ChatClient

### sendMessage
```
let request = SendMessageRequest(messageContent: "Text message")
            
chatClient.sendMessage(request: request) { error in
    guard error == nil else {
        print("Send chat message failed: \(error!)")
        return
    }

    print("Chat message sent.")
}
```

### messages

Message history in **descending** order. It's an array of [ChatMessage](#chatmessage).

### receiveMessage

Subscribing to **receiveMessage** publisher will deliver sequence of [ChatMessage](#chatmessage) over time.

```
private var subscriptions = Set<AnyCancellable>()

private func subscribeToMessages() {
    chatClient
        .receiveMessage()
        .sink { message in

            print("On new message: \(String(describing: message))")

        }.store(in: &subscriptions)
}
```

### receiveEvent

Subscribing to **receiveEvent** publisher will deliver sequence of [ChatEvent](#chatevent) over time.

```
private var subscriptions = Set<AnyCancellable>()

private func subscribeToEvents() {
    chatClient
        .receiveEvent()
        .sink { event in

            print("On new event: \(String(describing: event))")

        }.store(in: &subscriptions)
}
```

### connectionStatus

Subscribing to **connectionStatus** publisher will deliver sequence of [ChatConnectionStatus](#chatconnectionstatus-enum) over time.

```
private var subscriptions = Set<AnyCancellable>()

private func subscribeToConnectionStatusChanges() {
    chatClient
        .connectionStatus()
        .sink { connectionStatus in

            print("Connection status changed: \(connectionStatus)")

        }.store(in: &subscriptions)
}
```

You can get the current status at any time by calling connectionStatus().**value**.
```
let connectionStatus = chatClient.connectionStatus().value
```

### stopContact

Stops the chat, but threads are not deleted. They will show up in the help center listChatHistory API call after ~3 minutes.

```
chatClient.stopContact { error in
    guard error == nil else {
        print("Stop contact error: \(error!)")
        return
    }
    
    print("Stop contract success")
}
```

#### ChatConnectionStatus enum

- ***connecting*** when ChatClient is connecting or reconnecing after connection loss.
- ***connected***
- ***ended*** when [stopContact](#stopcontact) is called or agent closes the case.

#### ChatMessage

- ***id***
- ***participantRole*** is [ParticipantRole](#participantrole-enum) enum.
- ***displayName***
- ***content*** is the text message.
- ***timestamp*** is a Date.
  
#### ParticipantRole enum

- ***customer***
- ***systemMessage***
- ***agent***
  
#### ChatEvent

- ***id***
- **type** is [ChatEventType](#chateventtype-enum) enum.
- **timestamp** is a Date.
- **displayName**

#### ChatEventType enum

- ***joined*** when agent joined the chat.
- ***typing*** when agent is typing.


## DistributionClient

Distribution client exposes operations for push notification.

### registerChannel

Registers a push notification channel for the specified device.

```
let request = RegisterPushNotificationChannelRequest(deviceId: deviceId,
                                                     registrationId: registrationId)
    
sdk.distributionClient.registerChannel(request: request) { result in
    switch result {
    case .failure(let error):
        print("Register channel error: \(errror)")
        
    case .success():
        print("Register channel success")
    }
}
```

Errors
- ***FinapticErrorCode.transInvalidTransaction***: If a transaction ID is provided that is not found or resolves to an unauthorized account
- ***FinapticErrorCode.internalError***: If an unexpected error occurs while processing the request. The error will include a RefNumber which Finaptic can use to investigate the cause.

#### RegisterPushNotificationChannelRequest

- ***deviceId*** for which to register the push notification channel. This can be a unique ID provided by the device vendor or an anonymous sandboxed ID that uniquely represents the device.
- ***registrationId*** for the respective device. This is the registration token generated by the application client. See [Firebase documentation for more information](https://firebase.google.com/docs/cloud-messaging/ios/client#fetching-the-current-registration-token).

#### TransactionCreatedEvent

For each Transaction, a push notification will be sent with the following data. You can call [getTransaction](#gettransaction) to get more details.

```
{
    "eventType": "TransactionCreatedEvent",
    "transactionId": "00c660a0-c03d-401d-bf77-99df3e6a157f"
    "amount": "12.30",
    "availableBalance": "120.00",
    "actualBalance": "100.00",
    "transactionTime": "2021-12-17 12:34:56.793213 +0000 UTC",
}
```

## InteracClient

InteracClient exposes methods used to register and list/retrieve Auto Deposit Registrations.

### createRegistration

Once a registration request is submitted, users will receive an email at the email address that they provided from Interac, which will ask them to confirm their auto-deposit registration.

```
let registration = Registration(accountId: account.id, aliasEmail: "test@finaptic.com")
let request = CreateRegistrationRequest(registration: registration)
sdk.interacClient.createRegistration(request: request) { result in
    switch result {
    case .success(let response):
        print("Create autodeposit registration success with email \(response.registration.aliasEmail)")
    
    case .failure(let error):
        switch error.code {
        case .unavailable:
            print("Check your internet connection and try again", error)
        case .deadlineExceeded:
            print("Operation couldn't complete in the default timeout. Try again.", error)
            
        case .transInteracInvalidAccount:
            print("Invalid account", error)
        case .transInteracAliasInUse:
            print("Autodeposit alias in use", error)
        case .transInteracRegCountExceeded:
            print("Maximum number of account-alias registrations threshold exceeded", error)
        case .transInteracInvMobNumber:
            print("Invalid mobile phone number", error)
        case .transInteracInvMobAreacode:
            print("Invalid mobile phone area code", error)
        
        default:
            print("Create autodeposit registration error: \(error)")
        }
    }
}
```

Possible error codes:

- ***transInteracInvalidAccount***: Invalid account.
- ***transInteracAliasInUse***: Autodeposit alias in use.
- ***transInteracRegCountExceeded***: Maximum number of account-alias registrations threshold exceeded.
- ***transInteracInvMobNumber***: Invalid mobile phone number.
- ***transInteracInvMobAreacode***: Invalid mobile phone area code.

#### CreateRegistrationRequest
- ***registration***: See [Registration](#registration).

#### CreateRegistrationResponse
- ***registration***: See [Registration](#registration).

#### Registration
- ***registrationId***: Output only.
- ***expiryDate***: Output only.
- ***accountId***: Provided for the account that the customer wishes auto-deposit transactions to be routed to.
- ***aliasEmail***: Email for autodeposit registation.
- ***aliasPhoneNumber***: Optional. See [PhoneNumber](#phonenumber).

#### PhoneNumber
- ***phoneNumber***: Valid phone number.

### listAutodepositRegistrations

```
let request = ListAutodepositRegistrationsRequest(accountId: account.id)
sdk.interacClient.listAutodepositRegistrations(request: request) { result in
    switch result {
    case .success(let response):
        response.registrations.forEach { print("Autodeposit registration for  \($0.aliasEmail)") }

    
    case .failure(let error):
        print("List autodeposit registrations error: \(error)")
    }
}
```

#### ListAutodepositRegistrationsRequest
- ***accountId***: The Account ID for the Registrations that are to be listed.

#### ListAutodepositRegistrationsResponse
- ***registrations***: The requested list of [Registrations](#registration).

### updateRegistration

When updating the account alias registration, minimal changes are allowed, notably it is meant to change the target account linked to the alias. Notice that it is not possible to change the account alias handle itself (e.g. email/phone number). If more important changes are needed, the customer could always use the delete registration service and start a new account alias from scratch after that.

```
let request =  UpdateRegistrationRequest(registrationId: registrationId, accountId: accountId)
sdk.interacClient.updateRegistration(request: request) { result in
    switch result {
    case .success(let response):
        print("Update autodeposit registration success: \(response.registration)")
    
    case .failure(let error):
        switch error.code {
        case .unavailable:
            print("Check your internet connection and try again", error)
        case .deadlineExceeded:
            print("Operation couldn't complete in the default timeout. Try again.", error)
            
        case .transInteracInvalidAccount:
            print("Invalid account", error)
        case .unauthorized:
            print("Registration ID passed as argument is not owned by the current user.", error)
        
        default:
            print("Update autodeposit registration error: \(error)")
        }
        
    }
}
```

#### UpdateRegistrationRequest

Used to initiate the update of an existing Interac account alias registration.

- ***registrationId***: The ID of the Registration to update.
- ***accountId***: The ID of the Account.

#### UpdateRegistrationResponse
- ***registration***: See [Registration](#registration).

Errors:

- **FinapticErrorCode.invalidArgument**: if the account ID passed as argument is not owned or authorized for the current user.
- **FinapticErrorCode.unauthorized**: if the registration ID passed as argument is not owned by the current user.
- **FinapticErrorCode.internal**: if there is an unexpected error while processing the request.

### deleteRegistration

Deleting an account alias registration will invalidate/inactivate the link between an account alias handle (email/phone) and an account, so that the account will no longer receive deposits automatically when sent using the given alias. At that moment, the customer is free to use the same alias handle to create a new registration pointing to the same or a different account.

```
let request =  DeleteRegistrationRequest(registrationId: registrationId)
sdk.interacClient.deleteRegistration(request: request) { result in
    switch result {
    case .success(let response):
        print("Delete autodeposit registration success: \(response.registration)")
    
    case .failure(let error):
        switch error.code {
        case .unavailable:
            print("Check your internet connection and try again", error)
        case .deadlineExceeded:
            print("Operation couldn't complete in the default timeout. Try again.", error)
            
        case .unauthorized:
            print("Registration ID passed as argument is not owned by the current user.", error)
        
        default:
            print("Delete autodeposit registration error: \(error)")
        }
    }
}
```

#### DeleteRegistrationRequest

Used to initiate the deletion/deactivation of an existing Interac account alias registration.

- ***registrationId***: The ID of the Registration to delete.

#### UpdateRegistrationResponse
- ***registration***: See [Registration](#registration).

Errors:

- **FinapticErrorCode.unauthorized**: if the registration ID passed as argument is not owned by the current user.
- **FinapticErrorCode.internal**: if there is an unexpected error while processing the request.

## CoreTransactionClient

### listAccounts

Lists all accounts on which the current user is the owner, or an authorized user. Uses pagination to allow listing accounts in smaller, easy to manage, chunks.

```
let request = ListAccountsRequest(pageSize: 2, pageToken: nil)
sdk.coreTransactionClient.listAccounts(request: request) { result in
    switch result {
    case .success(let response):
        print("Accounts: \(response.accounts.count)")

    case .failure(let error):
        print("List accounts error: \(error)")
    }
}
```

#### ListAccountsRequest
- ***pageSize***:  The maximum number of Accounts to retrieve as part of this request. This value needs to be superior, or equal to 1.
- ***pageToken***:  When retrieving a page other than the initial page, the value for the ***pageToken*** must be specified, and taken from the previous page's ***nextPageToken*** value (see [ListAccountsResponse](#listaccountsresponse)). When no value is specified, then the initial page will be returned.

#### ListAccountsResponse
- ***accounts***:  The requested list of Accounts.
- ***nextPageToken***:  The token to be used to retrieve the next page of data. Must be passed in the ***pageToken*** field of the next request (see [ListAccountsRequest](#listaccountsrequest)). When no value is returned in this field, then no further data exists to be listed.

### getAccount

```
let request = GetAccountRequest(accountId: accountId)
sdk.coreTransactionClient.getAccount(request: request) { result in
    switch result {
    case .success(let account):
        print("Get account success: \(account)")
    
    case .failure(let error):
        print("Get account error: \(error)")
    }
}
```

#### GetAccountRequest
- ***accountId***: The ID of the Account to retrieve.

### listAuthorizedUsers

Lists all authorized users of an account.

```
let request = ListAuthorizedUsersRequest(accountId: accountId)
sdk.coreTransactionClient.listAuthorizedUsers(request: request) { result in
    switch result {
    case .success(let response):
        print("List authorized users success")
        for authorizedUserId in response.authorizedUsers {
            print("Authorized user with id \(authorizedUserId)")
        }
    
    case .failure(let error):
        print("List authorized users error: \(error)")
    }
}
```

#### ListAuthorizedUsersRequest
- ***accountId***:  The ID of the Account for which the authorized users are to be listed.

#### ListAuthorizedUsersResponse
- ***authorizedUsers***: The requested list of authorized users ids.

### listOwners

Lists all owners of an account if the caller as the access rights.

```
let request = ListOwnersRequest(accountId: accountId)
sdk.coreTransactionClient.listOwners(request: request) { result in
    switch result {
    case .success(let response):
        print("List owners success")
        for ownerId in response.owners {
            print("Owner with id \(ownerId)")
        }

    case .failure(let error):
        print("List owners error: \(error)")
    }
}
```

### listTransactions

Lists Transactions associated to the Account specified using pagination. Transactions are sorted in descending order of Transaction time.

```
let request = ListTransactionsRequest(accountId: accountId, pageSize: 2)
sdk.coreTransactionClient.listTransactions(request: request) { result in
    switch result {
    case .success(let response):
        print("List transactions success")
        for transaction in response.transactions {
            print("Transaction with id \(transaction.transactionId)")
        }
    
    case .failure(let error):
        print("List transactions error: \(error)")
    }
}
```

#### ListTransactionsRequest
- ***accountId***: The ID of the Account to list Transactions for.
- ***from***: The UTC timestamp from which to list Transactions.
- ***to***: The UTC timestamp until which to list Transactions.
- ***pageSize***: The maximum number of Transactions to retrieve as part of this request. This value needs to be superior, or equal to 1.
- ***pageToken***: When retrieving page other than the initial page, the value for the ***pageToken*** must be specified, and taken from the previous page's ***nextPageToken*** value (see [ListTransactionsResponse](#listtransactionsresponse)). When no value is specified, then the initial page will be returned.

##### ListTransactionsResponse
- ***transactions***: The requested list of Transactions.
- ***nextPageToken***: The token to be used to retrieve the next page of data. Must be passed in the ***pageToken*** field of the next request (see [ListTransactionsRequest](#listtransactionsrequest)). When no value is returned in this field, then no further data exists to be listed.

## CoreTransferClient

Exposes endpoints related to the retrieval of transfers data.

### getTransferDetails

Returns the latest state of a transfer by a given transaction ID. See [TransferTransactionDetails](#transfertransactiondetails) for more details.

```
let request = GetTransferDetailsRequest(transactionId: transaction.transactionId)
sdk.coreTransferClient.getTransferDetails(request: request) { result in
    switch result {
    case .failure(let error):
        print("Get TransferTransactionDetails error: \(error)")
        
    case .success(let transferTransactionDetails):
        print("Get TransferTransactionDetails success: \(transferTransactionDetails)")
    }
}
```

#### GetTransferDetailsRequest
- ***transactionId***: The ID of the transaction to get details for.

### initiateTransfer

Create a transfer fund request between accounts owned by current customer.

```
let amount = Amount(amount: Decimal(string: "0.01")!, currency: "CAD")
let request = InitiateTransferRequest(sourceAccountId: sourceAccount.id,
                                        destinationAccountId: destinationAccount.id,
                                        fundTransfer: amount)
sdk.coreTransferClient.initiateTransfer(request: request) { result in
    switch result {
    case .failure(let error):
        print("Initiate transfer error: \(error)")
        
    case .success(let response):
        guard response.reason.isEmpty else {
            print("Initiate transfer failed with reason: \(response.reason)")
            return
        }
        let transfer = response.details
        print("Initiate transfer success: \(transfer)")
    }
}
```

#### InitiateTransferRequest
- ***sourceAccountId***: Represents the source account ID. Current customer needs to be owner or authorized user of the source account. Source account needs to have sufficient available balance and actual balance to be able to initiate the transfer.
- ***destinationAccountId***: Represents the destination account ID. Current customer needs to be owner or authorized user of the destination account.
- ***fundTransfer***: The amount of the fund transfer. The amount should be always positive.

#### InitiateTransferResponse
- ***details***: Details of the transaction with its status (whether the transaction is [TransferStatus.completed](#transferstatus-enum) or [TransferStatus.declined](#transferstatus-enum)). See [TransferTransactionDetails](#transfertransactiondetails) for more details.
- ***reason***: Empty when status is [TransferStatus.completed](#transferstatus-enum). Reason why transfer is declined.

#### TransferTransactionDetails
- ***transactionId***: Represents this single financial transaction (operation).
- ***lifecycleTransactionId***: Represents the overarching transaction, across all phases of it's lifecycle (e.g. authorization, settlement, reversal, ...).
- ***status***: Status of the transfer. See [TransferStatus](#transferstatus-enum) enumeration for details on possible statuses.
- ***customerRole***: The role played by our customer in this transaction. See [CustomerRole](#customerole-enum) for details on possible roles.
- ***transferType***: The type of transfer for this transaction. See [TransferType](#transfertype-enum) for details on possible types.
- ***counterparty***: When ***transferType*** is **interacAutodeposit**, this will contain the details about the person/entity on the other side of this transfer. When ***transferType*** is **internalAccountToAccount**, this will contain the details of the owner of the destination account.
- ***clearingSystemRefNumber***: Unique identifier assigned to the transaction by the external payment/transfer clearing system.
- ***remittance***: Includes [remittance data](#structured-and-unstructured-remittance-data) from this transfer if it contains any.


#### TransferStatus enum
- ***unspecified***: It is returned when a value has not been specified, and is present by convention, for error-detection purposes. 
- ***declined***: Indicates that the transfer has been declined and rollback has been performed.
  When transferType is ***internalAccountToAccount***, this could be due to insufficient funds to initiate the transfer.
- ***completed***: Indicates that the transfer has been successfully completed.
- ***pending***: Indicates that the transfer is in an intermediate stage before completion.

#### CustomerRole enum
- ***unspecified***: It is returned when a value has not been specified, and is present by convention, for error-detection purposes.
- ***recipient***: Indicates that the customer is the recipient of the transfer operation.
- ***initiator***: Indicates that the customer is the initiator of the transfer operation.

#### TransferType enum
- ***unspecified***: It is returned when a value has not been specified, and is present by convention, for error-detection purposes. 
- ***internalAccountToAccount***: Indicates that the transfer was an Account to Account transfer that did not transit through external systems ( stayed within the institution ).
- ***interacAutodeposit***: Indicates that the transfer came through Interac systems, and used the Interac Autodeposit mechanism.
  

### Structured and Unstructured Remittance Data

***Remittance*** includes additional information provided by the sender giving context for a transfer for instance, a transfer may be sent to a business by one of its clients in order to pay for a bill, the remittance data will include a reference to that bill payment. Remittance data could be **unstructured** or **structured** or both.


#### Unstructured

The unstructured remittance data can be used for free-form text such as in a memo field.

User Interface and Data Mapping Example:

| Label | Data element |
|:----- |:------------:|
| Notes | data |

![authuser-icons.png](images/unstructured.jpg)

#### Structured

Structured remittance data can be sent in up to 5 blocks for each transaction, with each block representing a document, such as a purchase order.

The structured remittance data is organized into the following groupings of information:

- ***Referred Document Information***: Information about the 'invoice'. Not necessarily an invoice, but some document that outlines the context and reason for the payment.
- ***Referred Document Amount***: The amount being paid in the current transfer.
- ***Creditor Reference Information***: Additional reference info provided by the creditor.
- ***Invoicer***: Contains information about the payee, such as contact information.
- ***Invoicee***: Contains information about the payor, such as contact information.
- ***Additional remittance information***: A free form text field for additional remittance information.

#### User Interface and Data Mapping Examples:


##### Payor Details

***invoicee*** contains information about the payor.

| Label | Data element |
|:----- |:------------:|
| Name | invoicee.name |
| **Address** |  |
| Address Type | invoicee.postalAddress.addressType.[code](#addresstype2code-enum) |
| Department | invoicee.postalAddress.department |
| Sub-department | invoicee.postalAddress.subDepartment |
| Street | invoicee.postalAddress.streetName |
| Street Number | invoicee.postalAddress.buildingNumber |
| Postal Code | invoicee.postalAddress.postCode |
| City | invoicee.postalAddress.townName |
| Province | invoicee.postalAddress.countrySubDivision |
| Country | invoicee.postalAddress.country |
| Address | invoicee.postalAddress.addressLine |
| **Identification** | |
| Payor ID | invoicee.identification.organisationIdentification.other[n].identification |
| ID type | invoicee.identification.organisationIdentification.other[n].schemeName.[code](#externalorganisationidentification1code-enum) |
| **Contact Details** | |
| Name | invoicee.contactDetails. name |
| Phone | invoicee.contactDetails.phoneNumber |
| Mobile | invoicee.contactDetails.mobileNumber |
| Fax | invoicee.contactDetails.faxNumber |
| Email Address | invoicee.contactDetails.emailAddress |

![authuser-icons.png](images/payor.jpg)

##### Payee Details

***invoicer*** contains information about the payee.

| Label | Data element |
|:----- |:------------:|
| Name | invoicer.name |
| **Address** |  |
| Address Type | invoicer.postalAddress.addressType.[code](#addresstype2code-enum) |
| Department | invoicer.postalAddress.department |
| Sub-department | invoicer.postalAddress.subDepartment |
| Street | invoicer.postalAddress.streetName |
| Street Number | invoicer.postalAddress.buildingNumber |
| Postal Code | invoicer.postalAddress.postCode |
| City | invoicer.postalAddress.townName |
| Province | invoicer.postalAddress.countrySubDivision |
| Country | invoicer.postalAddress.country |
| Address | invoicer.postalAddress.addressLine |
| **Identification** | |
| Payee ID | invoicer.identification.organisationIdentification.other[n]. identification |
| ID type | invoicer.identification.organisationIdentification.other[n].schemeName.[code](#externalorganisationidentification1code-enum) |
| **Contact Details** | |
| Name | invoicer.contactDetails. name |
| Phone | invoicer.contactDetails. phoneNumber |
| Mobile | invoicer.contactDetails. mobileNumber |
| Fax | invoicer.contactDetails. faxNumber |
| Email Address | invoicer.contactDetails. emailAddress |

![authuser-icons.png](images/payee.jpg)

##### Document information

| Label | Data element |
|:----- |:------------:|
| Document Type | referredDocumentInformation[n].type.codeOrProprietary.[code](#documenttype6code-enum) |
| Purchase Order Number | referredDocumentInformation[n].number |
| Issue Date | referredDocumentInformation[n].relatedDate |
| Total Amount Due | referredDocumentAmount.duePayableAmount.amount<br/>referredDocumentAmount.duePayableAmount.currency |
| Discount Amount<br/>*(if creditDebitIndicator=DBIT)* | referredDocumentAmount.adjustmentAmountAndReason[n].amount.amount<br/>referredDocumentAmount.adjustmentAmountAndReason[n].amount.currency |
| Credit Amount<br/>*(if creditDebitIndicator=CRDT)* | referredDocumentAmount.adjustmentAmountAndReason[n].amount.amount<br/>referredDocumentAmount.adjustmentAmountAndReason[n].amount.currency |
| Reason for Adjustment | referredDocumentAmount.adjustmentAmountAndReason[n].reason |
| Additional Adjustment Information | referredDocumentAmount.adjustmentAmountAndReason[n].additionalInformation |
| Amount to be Paid | referredDocumentAmount.remittedAmount.amount<br/>referredDocumentAmount.remittedAmount.currency |
| Creditor Reference Type | creditorReferenceInformation.type.codeOrProprietary.[code](#documenttype3code-enum) |
| Creditor Reference | creditorReferenceInformation.reference |

![authuser-icons.png](images/document.jpg)

##### Additional remittance information

| Label | Data element |
|:----- |:------------:|
| Additional Details | additionalRemittanceInformation[n] |

![authuser-icons.png](images/additional_details.jpg)

#### AddressType2Code enum
- ***ADDR***: Postal Address
- ***PBOX***: POBox Address
- ***HOME***: Residential Address
- ***BIZZ***: Busines s Address
- ***MLTO***: MailTo Address
- ***DLVY***: Delivery To Address

#### ExternalOrganisationIdentification1Code enum
- ***BANK***: Bank ID
- ***CBID***: Central Bank ID
- ***CHID***: Clearing House ID
- ***CINC***: Corporation
- ***COID***: Country Code
- ***CUST***: Customer ID
- ***DUNS***: DUNS Number
- ***EMPL***: Employer ID
- ***GS1G***: Global Location Number
- ***SREN***: SIREN code
- ***SRET***: SIRET code
- ***TXID***: Tax ID

#### DocumentType6Code enum
- ***MSIN***: Metered Service Invoice
- ***CNFA***: Credit Note Related To Financial Adjustment
- ***DNFA***: Debit Note Related To Financial Adjustment
- ***CINV***: Commercial Invoice
- ***CREN***: Credit Note
- ***DEBN***: Debit Note
- ***HIRI***: Hire Invoice
- ***SBIN***: Self Billed Invoice
- ***CMCN***: Commercial Contract
- ***SOAC***: Statement Of Account
- ***DISP***: Dispatch Advice
- ***BOLD***: Bill Of Lading
- ***VCHR***: Voucher
- ***AROI***: Account Receivable Open Item
- ***TSUT***: Trade Services Utility Trans action
- ***PUOR***: Purchase Order

#### DocumentType3Code enum
- ***RADM***: Remittance Advice Message
- ***RPIN***: Related Payment Instructions
- ***FXDR***: Foreign Exchange Deal Reference
- ***DISP***: Dispatch Advice
- ***PUOR***: Purchase Order
- ***SCOR***: Structured Communication Reference

## Personal Finance Management

### getTransactions

Lists transactions for a given Account with pagination.

```
let request = GetTransactionsRequest(accountId: accountId, pageSize: 10, sortOrder: .descending)
sdk.personalFinanceManagementClient.getTransactions(request: request) { result in
    switch result {
    case .success(let response):
        print("Get transactions success")
        for transaction in response.transactions {
            print("Transaction with id \(transaction.transactionId)")
        }
    
    case .failure(let error):
        print("Get transactions error: \(error)")
    }
}
```

#### GetTransactionsRequest
- ***accountId***:  The Account ID for which Transactions are returned.
- ***pageSize***: The maximum number of Transactions to return. This value must be greater or equal to one.
- ***pageToken***: When retrieving page other than the initial page, the value for the page_token must be specified, and taken from the previous page's ***nextPageToken*** value (see [GetTransactionsResponse](#gettransactionsresponse)). When no value is specified, then the initial page will be returned.
- ***sortOrder***: The order the returned Transactions are sorted in. Transactions are sorted by the Transaction's ***transactionTime*** field. See [TransactionOrder](#transactionorder-enum).

#### GetTransactionsResponse
- ***previousPageToken***: The token to be used to retrieve the previous page of data. Must be passed in the **pageToken** field of the next request (see [GetTransactionsRequest]($gettransactionsrequest)). When no value is returned in this field, then there is no previous data to be listed.
- ***transactions***:  The requested list of [Transactions](#transaction).
- ***nextPageToken***: The token to be used to retrieve the next page of data. Must be passed in the ***pageToken*** field of the next request (see [GetTransactionsRequest]($gettransactionsrequest)). When no value is returned in this field, then no further data exists to be listed.
- ***sortOrder***: The order the Transactions are listed in. See [TransactionOrder](#transactionorder-enum).

### getTransaction
```
let request = GetTransactionRequest(transactionId: transactionId)
sdk.personalFinanceManagementClient.getTransaction(request: request) { result in
    switch result {
    case .success(let response):
        print("Get transaction success: \(response.transaction.transactionId)")
    
    case .failure(let error):
        print("Get transaction error: \(error)")
    }
}
```

#### GetTransactionRequest
- ***transactionId***: Transaction id.

#### GetTransactionResponse
- ***transaction***: See [Transactions](#transaction).
- 
#### TransactionOrder enum
- ***orderUnspecified***: Should never be used directly. It is returned when a value has not been specified, and is present by convention, for error-detection purposes. Trying to make a request with this type will result in an error.
- ***descending***: Represents newest first.
- ***ascending***: Represents oldest first.

#### Transaction
- ***transactionId***:  Unique identifier that represents this single financial transaction.
- ***lifecycleTransactionId***: Unique identifier that represents the overarching transaction, across all phases of it's lifecycle.
- ***amount***: The Amount of the Transaction.
- ***transactorName***: The name and location of the vendor where the payment was processed.
- ***balance***: The resulting Balance of the Account after the Transaction has occurred.
- ***transactionState***: TransactionState enumeration represents the state of a Transaction. See [TransactionState](#transactionstate-enum).
- ***transactionType***: The TransactionType of the Transaction. See [TransactionType](#transactiontype-enum).
- ***accountId***: ID of the Account on which the Transaction was authorised. This can also be utilised to fetch the Account.
- ***category***: A localized descriptor of the transaction's category (i.e. Groceries, Transport, etc.).
- ***transactionTime***:  The UTC timestamp when the Transaction occurred.
- ***accountDomain***: The name of the domain that can be queried for additional details regarding the Account of this transaction.
- ***initiatingDomain***: The name of the domain that can be queried for additional details regarding this Transaction.

#### TransactionState enum
- ***transactionStateUnspecified***: Reserved for error detection purposes. It should not be used directly.
- ***posted***: Represents a transaction that has been completed.
- ***pending***: Represents a transaction in an intermediate state, dependent on the transaction type.
- ***unknown***: Represents a transaction type where available information is insufficient to determine TransactionState.

#### TransactionType enum
- ***transactionTypeUnspecified***: Reserved for error detection purposes. It should not be used directly.
- ***authorized***: Represents a transaction type that is a purchase for which the merchant has received approval from the bank. An ***authorized*** transaction will always have state ***pending***.
- ***debit***: Represents a transaction type when there is an addition to the amount in the customer's account.
- ***refund***: Represents a transaction type when businesses and merchants issue a reversal of the transaction that had previously occurred (e.g. a previously credited transaction will be debited back into the customers account).
- ***declined***: Represents a transaction type when the transaction fails for any reason (e.g. fraud, operations).
- ***chargeback***: Represents a transaction type that is returned to a payment card after a customer successfully disputes an item on their account statement or transactions report. A chargeback may occur on debit cards (and the underlying bank account) or on credit cards.
- ***credit***: Represents a transaction type when there is a deduction to the amount in the customer's account.
- ***productNotDefined***: Represents a transaction state where available information is insufficient to determine TransactionType.

## Errors
 
Most callable methods from the SDK potentially can fail with a FinapticError, which contains "code", "message" and "cause" attributes.
 
```
sdk.onboardingClient.initiateApplication(request: request) { result in
           switch result {
   case .success(let response):
       ...
      
   case .failure(let error):
       switch error.code {
       case .unavailable:
           print("Check your internet connection and try again")
       case .deadlineExceeded:
           print("Operation couldn't complete in the default timeout. Try again.")
       default:
           print("Initiate application error: \(error)")
       }
   }
}
```
