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
     
### initiateApplication
This is the only call that must be executed prior any other one when processing an onboarding application.
```
let request = InitiateApplicationRequest(customerId: nil, productId: "prepaid_card")
sdk.onboardingClient.initiateApplication(request: request) { result in
   switch result {
   case .success(let response):
       if (response.basicDetails.requestStatus != "OK") {
           // Could not process the request properly. You can retry the request.
       }
       let applicationId = response.basicDetails.applicationId
  
   case .failure(let error):
       print("Initiate application error: \(error)")
   }
}
```
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
### updateContactDetails
```
let request = UpdateContactDetailsRequest(
   onboardingApplicationId: applicationId,
   customerContactDetails: ContactDetails(
       phoneNumber: "+15141234567",
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
### updateEmployment
Fetch employment status from [reference data client](#referencedataclient).

- `dataType: ReferenceDataTypes.occupation`
- `dataType: ReferenceDataTypes.employmentStatus`
  
```
let request = UpdateEmploymentRequest(
   onboardingApplicationId: applicationId,
   employmentInfo: Employment(
       employmentStatus: employment.code,
       employerName: "Finaptic",
       industry: "Finances",
       occupation: occupation.code,
       startDate: Calendar.current.date(from: DateComponents(year: 2019, month: 1, day: 1)),
       endDate: Calendar.current.date(from: DateComponents(year: 2020, month: 12, day: 31)),
       workAddress: address,
       workPhoneNumber: "+14507654321")
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
           backImageFileName: idBackFileUpload.filename)
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
           // The user was not approved during validation. You may ask the user to check his info, retake the images and update/validate again.
       }
 
   case .failure(let error):
       print("Validate documents error: \(error)")
   }
}
```
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
           // The user was not approved during validation. You may ask the user to check his info, retake the images and update/validate again.
       }
 
   case .failure(let error):
       print("Validate selfie error: \(error)")
   }
}
```
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
           // The user was not approved during validation. You may ask the user to check his info, retake the images and update/validate again.
       }
 
   case .failure(let error):
       print("Validate application error: \(error)")
   }
}
```
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
- occupation
  
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
