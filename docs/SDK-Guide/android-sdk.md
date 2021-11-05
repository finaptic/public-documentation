# Finaptic Android SDK

The Finaptic Android SDK provides a way to consume Finaptic APIs from your Android application. It is written in Kotlin.
 
Note that the Finaptic APIs are strictly accessed through the SDK because it hides the underlying implementation that could change in the future.

Android API version 26 or newer is required.

## Sample application

A sample application demonstrates what is explained in the document. Make sure you can run it using your own SDK key.
 
Here are the steps to run the sample app:
 
1. Open the file MainActivity.kt and replace the SDK key with the one provided to you. The key is configured in the FinapticSDK.init(...) call.
2. Run the sample app on a device or simulator.
3. Press the button at the bottom of the first screen to execute the onboarding flow.
4. Check the logs in Logcat (filter with "Finaptic"). You will see the sample app logs as well as the SDK logs.
5. Let the whole flow execute. It can take more than one minute. In a sandbox environment, you may experience delays when using a client for the first time or after a small inactivity period. It is due to the backend services cold start.
6. If an error arises, a dialog will be presented with the error. The flow couldn't complete properly and the full exception stack will be logged in Logcat. In this case, make sure the SDK key is the right one and retry.
7. If the flow completes without error (no more requests are executing), you are good to integrate the SDK in your own app.
 
##### ID and face capture with Acuant
Please take a look at AcuantSample on how to use Acuant for document and face capture.
You can read their official documentation [here](https://github.com/Acuant/AndroidSDKV11).
 
## Installation
 
1. Copy Finaptic SDK **contract** and **implementation** aar files to `app/libs`.
   
2. Require the following permissions in your `AndroidManifest.xml` file.

        <manifest xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:tools="http://schemas.android.com/tools">

        <uses-permission android:name="android.permission.INTERNET" />
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />

        <!-- Acuant -->
        <uses-permission android:name="android.permission.CAMERA" />
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
        <uses-feature android:name="android.hardware.camera" />
        <uses-feature android:name="android.hardware.camera.autofocus" />
        <meta-data
            android:name="com.google.android.gms.vision.DEPENDENCIES"
            android:value="barcode,face"
            tools:replace="android:value"/>

        </manifest>

 
3. Make sure you have these flags in your `gradle.properties`.
   
        android.enableJetifier=true
        android.useAndroidX=true

 
4. Add the Finaptic SDK repositories in `build.gradle`.
   
        allprojects {
            repositories {
                google()
                mavenCentral()
                jcenter()

                // SDK libs path
                flatDir {
                    dirs 'libs'
                }

                // Acuant
                maven { url 'https://raw.githubusercontent.com/Acuant/AndroidSdkMaven/main/maven/' }
                maven { url 'https://raw.githubusercontent.com/iProov/android/master/maven/' }
            }
        }

 
5. Update `app/build.gradle` and sync.
      - Add the Manifest Placeholders for the Auth0 Domain and the Auth0 Scheme properties.
        
               android {
                   defaultConfig {
                       // Auth0 domain and scheme provided by Finaptic
                       manifestPlaceholders = [auth0Domain: "", auth0Scheme: "demo"]
                   }
               }

 
      - Target Java 8 byte code for Android and Kotlin plugins respectively.

               android {
                   compileOptions {
                       sourceCompatibility JavaVersion.VERSION_1_8
                       targetCompatibility JavaVersion.VERSION_1_8
                   }
                   kotlinOptions {
                       jvmTarget = '1.8'
                   }
               }

 
      - Add the Finaptic SDK dependencies.
          Because the SDK is provided as aar files instead of maven-like dependencies, you must also add the direct SDK dependencies.

               dependencies {
                   // Finaptic SDK in libs directory
                   implementation(name:'finaptic-sdk-contract', ext:'aar')
                   implementation(name:'finaptic-sdk-implementation', ext:'aar')

                   // Finaptic SDK dependencies
                   implementation 'io.grpc:grpc-okhttp:1.37.0'
                   implementation 'io.grpc:grpc-stub:1.37.0'
                   implementation("io.grpc:grpc-kotlin-stub-lite:1.0.0")
                   implementation platform('com.google.firebase:firebase-bom:27.0.0')
                   implementation 'com.google.firebase:firebase-analytics-ktx'
                   implementation 'com.google.firebase:firebase-auth-ktx'
                   implementation('com.google.firebase:firebase-firestore-ktx')
                   implementation 'org.jetbrains.kotlinx:kotlinx-coroutines-play-services:1.4.1'
                   implementation 'com.auth0.android:auth0:2.2.0'
                   
                   // Acuant dependencies
                   implementation 'com.acuant:acuantcommon:11.4.12'
                   implementation 'com.acuant:acuantcamera:11.4.12'
                   implementation 'com.acuant:acuantimagepreparation:11.4.12'
                   implementation 'com.acuant:acuantdocumentprocessing:11.4.12'
                   implementation 'com.acuant:acuantfacematch:11.4.12'
                   implementation 'com.acuant:acuantfacecapture:11.4.12'
                   implementation 'com.acuant:acuantpassiveliveness:11.4.12'
               }

 
## Initialization
 
The SDK must be initialized before you can make any call. The FinapticSDK.init function must be invoked as a coroutine. The first parameter is a Context (ex: Activity, ApplicationContext).
 
```
GlobalScope.launch {
   // Initialize the SDK with the key provided by Finaptic
   sdk = FinapticSDK.init(this@MainActivity, "ewogICJzeW5jT25ib2...")
}
```
The init function can be called multiple times, but a single instance will be returned (the first one created). This means you can't change the context or SDK key to get another instance. The context is used for initialization only and won't be retained by the SDK. It is recommended to initialize the SDK once and store the returned instance in your own variable or container.
 
You can now start to use the SDK through multiple clients.
 
## OnboardingClient

Onboarding flow consist of initiate, update, validate and finalize calls.
 
1. Initiate application
      - [initiateApplication](#initiateapplication)
2. Update information
      - [updatePersonalDetails](#updatepersonaldetails)
      - [updateContactDetails](#updatecontactdetails)
      - [updateAccountDetails](#updateaccountdetails)
      - [updateAddresses](#updateaddresses)
      - [updateConsents](#updateconsents)
      - [updateCustomerResidency](#updatecustomerresidency)
      - [updateDisclosures](#updatedisclosures)
      - [updateEmployment](#updateemployment)
      - [updateCommunicationPreferences](#updatecommunicationpreferences)
3. Validate application
      - [validateDocuments](#validatedocuments)
      - [validateSelfie](#validateselfie)
      - [validateApplication](#validateapplication)
      - [acceptApplication](#acceptapplication)
4. Finalize application
      - [finalizeApplicationCreateCustomer](#finalizeapplicationcreatecustomer)
      - [finalizeApplicationCreateProduct](#finalizeapplicationcreateproduct)
 
### initiateApplication
 
This is the only call that must be executed prior any other one when processing an onboarding application.
 
```
val response = sdk.onboardingClient.initiateApplication(
   InitiateApplicationRequest(
       productId = "prepaid_account"
   )
)

if (response.basicDetails.requestStatus != "OK") {
    // Could not process the request properly. You can retry the request.
}
val applicationId = response.basicDetails.applicationId
```
 
The returned application ID is required in every other request.
### updatePersonalDetails
```
val response = sdk.onboardingClient.updatePersonalDetails(
   UpdatePersonalDetailsRequest(
       onboardingApplicationId = applicationId,
       customerPersonalDetails = PersonalDetails(
           firstName = "John",
           lastName = "Miller",
           dateOfBirth = LocalDate.of(1941, 5, 24)
       )
   )
)

if (response.basicDetails.requestStatus != "OK") {
    // Could not process the request properly. You can retry the request.
}
```
 
### updateContactDetails
 
```
val response = sdk.onboardingClient.updateContactDetails(
   UpdateContactDetailsRequest(
       onboardingApplicationId = applicationId,
       customerContactDetails = ContactDetails(
           customerEmail = "john.miller@mail.com",
           phoneNumber = "+15141234567"
       )
   )
)

if (response.basicDetails.requestStatus != "OK") {
    // Could not process the request properly. You can retry the request.
}
```
 
### updateAccountDetails

Fetch account purpose and source of funds values from [reference data client](#referencedataclient).

- `dataType = ReferenceDataTypes.ACCOUNT_PURPOSE`
- `dataType = ReferenceDataTypes.SOURCE_OF_FUNDS`
 
```
val response = sdk.onboardingClient.updateAccountDetails(
   UpdateAccountDetailsRequest(
       onboardingApplicationId = applicationId,
       accountDetails = AccountUsageDetails(
           accountPurpose = accountPurposeCode,
           primarySourceOfFunds = listOf(sourceOfFundsCode),
           authorizedThirdPartyUsage = authorizedThirdPartyUsage
       )
   )
)

if (response.basicDetails.requestStatus != "OK") {
    // Could not process the request properly. You can retry the request.
}
```
 
### updateAddresses
You need to use [AddressClient](#addressclient) to get the user's address.
 
```
val response = sdk.onboardingClient.updateAddresses(
   UpdateAddressesRequest(
       onboardingApplicationId = applicationId,
       addresses = listOf(address)
   )
)

if (response.basicDetails.requestStatus != "OK") {
    // Could not process the request properly. You can retry the request.
}
```
 
### updateConsents
Fetch consent values from [reference data client](#referencedataclient) with `dataType = ReferenceDataTypes.CONSENT_TYPE`.
 
```
val response = sdk.onboardingClient.updateConsents(
   UpdateConsentsRequest(
       onboardingApplicationId = applicationId,
       consents = listOf(
           Consent(
               consentType = consentType.code,
               documentNameAndVersion = "https://must-be-a-well-formed-url.com/document/1.0",
               consentTimestamp = Instant.now(),
               viewTimestamp = Instant.now()
           )
       )
   )
)

if (response.basicDetails.requestStatus != "OK") {
    // Could not process the request properly. You can retry the request.
}
```
 
### updateCustomerResidency
 
```
val response = sdk.onboardingClient.updateCustomerResidency(
   UpdateCustomerResidencyRequest(
       onboardingApplicationId = applicationId,
       residency = CustomerResidency(
           isCanadianTaxResident = true,
           citizenship = "CA"
       )
   )
)

if (response.basicDetails.requestStatus != "OK") {
    // Could not process the request properly. You can retry the request.
}
```
 
### updateDisclosures
Fetch disclosure type values from [reference data client](#referencedataclient) with `dataType = ReferenceDataTypes.DISCLOSURE_TYPE`.
 
```
val response = sdk.onboardingClient.updateDisclosures(
   UpdateDisclosuresRequest(
       onboardingApplicationId = applicationId,
       disclosures = listOf(
           Disclosure(
               disclosureType = disclosureType.code,
               documentNameAndVersion = "https://must-be-a-well-formed-url.com/document/1.0",
               timestamp = Instant.now()
           )
       )
   )
)

if (response.basicDetails.requestStatus != "OK") {
    // Could not process the request properly. You can retry the request.
}
```
 
### updateEmployment

Fetch employment status from [reference data client](#referencedataclient).

- `dataType = ReferenceDataTypes.OCCUPATION`
- `dataType = ReferenceDataTypes.EMPLOYMENT_STATUS`
 
```
val response = sdk.onboardingClient.updateEmployment(
   UpdateEmploymentRequest(
       onboardingApplicationId = applicationId,
       employmentInfo = Employment(
           employmentStatus = employmentStatus.code,
           employerName = "Finaptic",
           occupation = occupation.code,
           industry = "Finances",
           workAddress = address,
           workPhoneNumber = "+14507654321",
           startDate = LocalDate.of(2019, 1, 1),
           endDate = LocalDate.of(2020, 12, 31)
       )
   )
)

if (response.basicDetails.requestStatus != "OK") {
    // Could not process the request properly. You can retry the request.
}
```
 
### updateCommunicationPreferences
Fetch communication preferences from [reference data client](#referencedataclient) with `dataType = ReferenceDataTypes.COMMUNICATION_PREFERENCE`.
```
val response = sdk.onboardingClient.updateCommunicationPreferences(
   UpdateCommunicationPreferencesRequest(
       onboardingApplicationId = applicationId,
       customerCommunicationPref = communicationPreferences.code
   )
)

if (response.basicDetails.requestStatus != "OK") {
    // Could not process the request properly. You can retry the request.
}
```

### createFileUploadURL
Create upload url for document front, back and selfie.

```
val fileUploadURL = sdk.onboardingClient.createFileUploadURL(
    CreateFileUploadUrlRequest(
        onboardingApplicationId = applicationId
    )
)
```
 
### validateDocuments
Fetch document types from [reference data client](#referencedataclient) with `dataType = ReferenceDataTypes.DOCUMENT_TYPE`.
 
```
val response = sdk.onboardingClient.validateDocuments(
   ValidateDocumentsRequest(
       onboardingApplicationId = applicationId,
       documentDetails = listOf(
           DocumentInfo(
               documentType = documentType.code,
               documentCountry = "CA",
               frontImageFileName = idFrontFileUpload.filename,
               backImageFileName = idBackFileUpload.filename
           )
       )
   )
)

if (response.basicDetails.requestStatus != "OK") {
    // Could not process the request properly. You can retry the request.
}

// The following check is only for validateSelfie, validateDocuments and validateApplication
if (response.basicDetails.onboardingApplicationStatus == ApplicationStatus.dataCollection ||
    response.basicDetails.onboardingApplicationStatus == ApplicationStatus.kycFailed ||
    response.basicDetails.onboardingApplicationStatus == ApplicationStatus.kycRejected ) {
    // The user was not approved during validation. You may ask the user to check his info, retake the images and update/validate again.
}
```
 
### validateSelfie
```
val response = sdk.onboardingClient.validateSelfie(
   ValidateSelfieRequest(
       onboardingApplicationId = applicationId,
       fileName = fileUpload.filename
   )
)

if (response.basicDetails.requestStatus != "OK") {
    // Could not process the request properly. You can retry the request.
}

// The following check is only for validateSelfie, validateDocuments and validateApplication
if (response.basicDetails.onboardingApplicationStatus == ApplicationStatus.dataCollection ||
    response.basicDetails.onboardingApplicationStatus == ApplicationStatus.kycFailed ||
    response.basicDetails.onboardingApplicationStatus == ApplicationStatus.kycRejected ) {
    // The user was not approved during validation. You may ask the user to check his info, retake the images and update/validate again.
}
```
 
### validateApplication
 
```
val response = sdk.onboardingClient.validateApplication(
   ValidateApplicationRequest(
       onboardingApplicationId = applicationId
   )
)

if (response.basicDetails.requestStatus != "OK") {
    // Could not process the request properly. You can retry the request.
}

// The following check is only for validateSelfie, validateDocuments and validateApplication
if (response.basicDetails.onboardingApplicationStatus == ApplicationStatus.dataCollection ||
    response.basicDetails.onboardingApplicationStatus == ApplicationStatus.kycFailed ||
    response.basicDetails.onboardingApplicationStatus == ApplicationStatus.kycRejected ) {
    // The user was not approved during validation. You may ask the user to check his info, retake the images and update/validate again.
}
```
 
### acceptApplication
 
```
val response = sdk.onboardingClient.acceptApplication(
   AcceptApplicationRequest(
       onboardingApplicationId = applicationId
   )
)

if (response.basicDetails.requestStatus != "OK") {
    // Could not process the request properly. You can retry the request.
}
```
 
### finalizeApplicationCreateCustomer
The user needs to be [authenticated](#authenticationclient) before you create a customer.
 
```
val response = sdk.onboardingClient.finalizeApplicationCreateCustomer(
   FinalizeApplicationCreateCustomerRequest(
       onboardingApplicationId = applicationId
   )
)

if (response.basicDetails.requestStatus != "OK") {
    // Could not process the request properly. You can retry the request.
}
```
 
### finalizeApplicationCreateProduct
 
```
val response = sdk.onboardingClient.finalizeApplicationCreateProduct(
   FinalizeApplicationCreateProductRequest(
       onboardingApplicationId = applicationId
   )
)

if (response.basicDetails.requestStatus != "OK") {
    // Could not process the request properly. You can retry the request.
}
```
 
## ReferenceDataClient

ReferenceDataTypes:

- ADDRESS_TYPE
- CONSENT_TYPE
- DISCLOSURE_TYPE
- DOCUMENT_TYPE
- COMMUNICATION_PREFERENCE
- ACCOUNT_PURPOSE
- SOURCE_OF_FUNDS
- EMPLOYMENT_STATUS
- ONBOARDING_STATUS
- COUNTRY
- OCCUPATION
 
```
val accountPurposes = sdk.referenceDataClient
   .getReferenceDataByType(
       GetReferenceDataByTypeRequest(
           dataType = ReferenceDataTypes.ACCOUNT_PURPOSE,
           locale = "EN_CA"
       )
   ).data
```
 
## AddressClient

Address client provides a list of suggested addresses. Make sure to use debouncing or throttling when calling this function on user input.
 
```
val suggestionsResponse = sdk.addressClient.getSuggestions(
   AddressSuggestionsRequest(
       onboardingApplicationId = applicationId,
       pattern = "123",
       addressType = AddressClient.HOME_ADDRESS_TYPE
   )
)
```
## AuthenticationClient
### signUp
```
val credential = Credential(
               email = "sample@finaptic.com",
               password = "Password1234!@#$",
               username =  "Username"
       )
sdk.authenticationClient.signUp(SignUpRequest(credential))
```
 
### signIn
```
val signInResult = sdk.authenticationClient.signIn(SignInRequest(credential))
```
 
### currentAuthState
If the accessToken has expired, the client automatically uses the refreshToken and renews the credentials for you. Returns null if the user is unauthenticated.
```
val currentState = sdk.authenticationClient.currentAuthState()
```
 
### refreshAuthState
Returns null if the user is unauthenticated.
```
val currentState = sdk.authenticationClient.refreshAuthState()
```
 
### signOut
```
sdk.authenticationClient.signOut()
```
 
## CoreBankingClient

### getAccountDetails
```
val request = GetAccountDetailsRequest(accountId)
val response = sdk.coreBankingClient.getAccountDetails(request)

val status = response.status
val accruedInterest = response.accruedInterest
```

## CoreCardClient
 
### getCard

```
val request = GetCardRequest(cardId = cardId)
val response = sdk.coreCardClient.getCard(request)
```

### listCards
```
val request = ListCardsRequest(
           pageSize = 10,
           pageToken = null
        )
val response = sdk.coreCardClient.listCards(request)
   
val cards = response.cards
val nextPageToken = response.nextPageToken
```

### createCardSDKSignOnToken
```
val request = CreateCardSDKSignOnTokenRequest(cardId)
val response = sdk.coreCardClient.createCardSDKSignOnToken(request)

val token = response.token
```


## Exceptions

Every callable method from the SDK potentially throws a FinapticException, which is a regular Java/Kotlin exception with an extra "code" attribute. 

This try-catch block can surround any SDK call.
```
try {
    val initiateApplicationResponse = sdk.onboardingClient.initiateApplication(
        InitiateApplicationRequest(
            productId = "prepaid_account"
        )
    )
    val applicationId = initiateApplicationResponse.basicDetails.applicationId
} catch (e: FinapticException) {
    when(e.code) {
        FinapticExceptionCode.UNAVAILABLE ->
            Log.e(TAG, "Check your internet connection and try again", e)
        FinapticExceptionCode.DEADLINE_EXCEEDED ->
            Log.e(TAG, "Operation couldn't complete in the default timeout. Try again.", e)
        else -> throw e
    }
}
```
