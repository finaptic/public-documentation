# Onboarding Overview

Welcome to Finaptic's end-to-end Onboarding Service. We make it simple to build trust in a digital world and easy to welcome new customers. This documentation includes comprehensive information for building a compliant customer onboarding experience and integrating with Finaptic's Onboarding Service.

## Guide Purpose

**This guide will**

1. Educate you on Finaptic's Onboarding Service for a **`Primary Account Holder`**
2. Arm you with an API Guide for effective integration

**Links to related Onboarding topics**

1. [Onboarding Sequence Diagram](/../../Implementation-Guide/Onboarding/OnboardingSequenceDoc/)
2. [Finaptic's Authorized User Policies](/../../Implementation-Guide/Banking/AuthorizedUserDocumentation/)
3. [Productless Onboarding Architecture](/../../Implementation-Guide/Onboarding/ProductlessOnboardingDoc/) to Onboard an Authorized User

---

## **What is Onboarding?**

Onboarding is the process of welcoming and establishing a relationship with new customers including adding new products to existing customers. Steps are anchored to obtain a customer's consent to collect their personal information and verify their identity to provide banking products and services and establish a relationship.

## **About the Platform**

Finaptic's Onboarding Service processes high volumes of identity checks annually and inherits all scalability, reliability, and security features of the core platform.

#### **Three Pillars of Onboarding**

* Pillar 1 Customer Information
* Pillar 2 Digital ID Verification
* Pillar 3 Onboarding Agreements

---

## **Pillar 1 | Customer Information**

In order to satisfy regulatory obligations such as **Know Your Customer** (KYC) and Account Opening requirements there are 4 categories of information type that must be collected from the applicant. In total, there are about 15-24 data fields or questions to complete.

| CATEGORIES                               | RATIONALE                                                                  | FIELDS                                                                                                                                                                                                                                                                                                               | API CALLS                                                                                                                                                                                                           |
| ---------------------------------------- | -------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **PERSONAL**                             | Collect basic applicant information for communication & vetting purposes   | - Full Legal Name<br/>- Middle Name *(optional)*<br/>- Alias (optional)*<br/>- Residential Address <br/>- Date of Birth<br/>- Phone Number<br/>- Phone Number Type (Mobile or Landline) <br/>- Email Address<br/>- Language Preferences<br/>- Social Insurance Number<sup>1</sup> *(Interest bearing products only)* | [Personal Details](/../../API-Specifications/onboarding/#personaldetails)<br/>[Contact Details](/../../API-Specifications/onboarding/#contactdetails) <br/>[Address](/../../API-Specifications/onboarding/#address) |
| **TAX** **RESIDENCY<sup>2</sup>**        | Identifying tax residency during account opening to comply with regulation | - Canadian Tax Residency Status<br/>- US Citizen<br/>- Another Country Tax Residency Status<br/>*If the applicant is a tax resident of Another Country (including the US), or a US Citizen, then the tax identification number for that country and Canadian SIN must be collected*                                  | [Tax Residency](/../../API-Specifications/onboarding/#customerresidency)<br/>[Other Residency](/../../API-Specifications/onboarding/#customerresidency)                                                             |
| **EMPLOYMENT**                           | Support with Anti-Money Laundering filtering                               | - Employment Status<br/>- Employment Occupation<br/>- Industry<br/>- Employer Name<br/>- Employer Address<br/>- Employer Phone Number<br/>- Employed Since Date *(optional)*<br/>- Income *(optional)*                                                                                                               | [Employment](/../../API-Specifications/onboarding/#employment)                                                                                                                                                      |
| **ACCOUNT PURPOSE** *(Deposit Products)* | Support with Anti-Money Laundering filtering                               | - Account Opening Purpose<br/>- Account Source of Funds<br/>- Third Party Declaration                                                                                                                                                                                                                                | [Account Purpose](/../../API-Specifications/onboarding/#accountusagedetails)                                                                                                                                        |

<sup>1</sup> "Social Insurance Number" is currently not built and will be released later in 2022
<sup>2</sup> Note, at this time we do not support ID verification for Canadian Residents who are also Tax Residents of another country

#### 

#### Onboarding a Sole Proprietor

A Sole Proprietor is also governed by the same "Know Your Client" regulations; and what may appear as an outlier field like "Residential Address" is required. Below is a breakdown of fields required for a Sole Proprietor.

| CATEGORIES                               | RATIONALE                                                                  | FIELDS                                                                                                                                                                                                                                                                              | API CALLS                                                                                                                                                                                                           |
| ---------------------------------------- | -------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **PERSONAL**                             | Collect basic applicant information for communication & vetting purposes   | - Full Legal Name<br/>- Middle Name *(optional)*<br/>- Alias *(optional)*<br/>- Residential Address <br/>- Date of Birth<br/>- Phone Number<br/>- Phone Number Type (Mobile or Landline) <br/>- Email Address<br/>- Language Preferences                                            | [Personal Details](/../../API-Specifications/onboarding/#personaldetails)<br/>[Contact Details](/../../API-Specifications/onboarding/#contactdetails) <br/>[Address](/../../API-Specifications/onboarding/#address) |
| **TAX** **RESIDENCY<sup>2</sup>**        | Identifying tax residency during account opening to comply with regulation | - Canadian Tax Residency Status<br/>- US Citizen<br/>- Another Country Tax Residency Status<br/>*If the applicant is a tax resident of Another Country (including the US), or a US Citizen, then the tax identification number for that country and Canadian SIN must be collected* | [Tax Residency](/../../API-Specifications/onboarding/#customerresidency)<br/>[Other Residency](/../../API-Specifications/onboarding/#customerresidency)                                                             |
| **EMPLOYMENT**                           | Support with Anti-Money Laundering filtering                               | - Employment Status<br/>- Employment Occupation<br/>- Industry<br/>- Employer Name<br/>- Business Address<br/>- Employer Phone Number<br/>- Employed Since Date *(optional)*                                                                                                        | [Employment](/../../API-Specifications/onboarding/#employment)                                                                                                                                                      |
| **ACCOUNT PURPOSE** *(Deposit Products)* | Support with Anti-Money Laundering filtering                               | - Account Purpose<br/>- Account Source of Funds<br/>- Third Party Declaration                                                                                                                                                                                                       | [Account Purpose](/../../API-Specifications/onboarding/#accountusagedetails)                                                                                                                                        |

<sup>2</sup> Note, at this time we do not support ID verification for Canadian Residents who are also Tax Residents of another country



#### Reference data sets for select fields

| CATEGORIES                  | LIST OF OPTIONS PRESENTED                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| --------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **EMPLOYMENT STATUS**       | - Employed<br/>- Unemployed<br/>- Retired<br/>- Student-Unemployed<br/>- Student-Employed<br/>- Self-employed <br/>- Homemaker                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| **PRIMARY ACCOUNT USAGE**   | - Everyday transactions<br/>- Everyday Business transactions (for Sole Proprietors only)<br/>- Paying for school<br/>- Travelling<br/>- Buying property/Renovations<br/>- Buying a car<br/>- Savings<br/>- Other                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| **PRIMARY SOURCE OF FUNDS** | - Employment Income (Salary / Self-employment Income) <br/>- Sale of Assets/Capital Gains (House, Car, etc.) <br/>- Trust / Inheritance/ Family income <br/>- Loans <br/>- Government Payments / Social Assistance <br/>- Support Payments <br/>- Grants/Scholarships or Bursaries <br/>- Work-Related Retirement or Disability Pension <br/>- Rental Income <br/>- Investment Income - Savings (Dividends / Interest) <br/>- Royalty Income<br/>- Tips/Commissions<br/>- Gifts <br/>- Insurance Claims / Payments <br/>- Pension / Retirement Income <br/>- Windfall (Lottery winnings, casino, etc.)                                                                                                                                                                                                   |
| **OCCUPATION<br/>INDUSTRY** | The “Occupation” and “Industry” fields are required to comply with Anti-Money Laundering and Anti-Terrorist Financing regulations.<br/><br/>We use the National Occupational Classification (NOC) list for **Occupations** [National Occupational Classification - Canada.ca](https://noc.esdc.gc.ca/) and the North American Industry Classification System (Canada) (NAICS) for **Industries** [Industry classifications](https://www.statcan.gc.ca/eng/concepts/industry). <br/><br/>The NOC list may not include newer titles such as “influencer”, “brand ambassador”, or “social media guru”, however, the traditional classification of “publicist” or “professional in marketing” would be an equivalent or a close enough approximation that customers can select to identify their occupation. |

---

## **Pillar 2 | Digital ID Verification**

This process verifies a person's identity by using computer technology. We screen for bad actors and potential device level frauds and validate the person's documentation and liveness.

#### **How does Finaptic's Onboarding Sevice help your business?**

Our approach helps prevent fraud, money laundering and terrorist funding. Using leading ID verification partner technologies that leverage artificial intelligence technology; providing document and biometric authentication with liveness tests to truly verify if the person applying for a financial product is who they say they are.

Working in real time, we will eliminate manual screening errors, speed up the document inspection process, and significantly reduce employee intervention, operating costs and customer inconvenience.

#### **Requirements for Digital ID Verification**

1. The identification document has to be authentic, valid, and original.
2. A valid (unexpired) identification document as listed in the [Accepted Forms of Identity Documentation for Digital Identification Verification](/../../Implementation-Guide/onboarding/OnboardingDocumentation/#accepted-forms-of-identity-documentation-for-digital-id-verification) section.
3. A mobile device with a functional camera for a selfie.

#### **Digital ID Verification Decision Making**

Finaptic will render a decision of either Approved or Declined within 90 seconds from the point of application submission.

During those 90 seconds, Finaptic will complete the following process to screen for bad actors and potential fraud with every applicant:

1. Cross referenced against up-to-date Sanctions lists, Politically Exposed Persons, and other Watchlists
2. Validate for potential device & IP level fraud
3. Facial Recognition Match will include a liveness test. A liveness test ensures that there is a real person present instead of a photo, video playback or even a mask

An applicant will be permitted up to 3 retries for common applicant error types (ex. name mismatch, incorrect date of birth, etc.).

An applicant will not be permitted to re-apply if the reason for a decline is Fraud or Sanctions related.

### **Visualizing Digital ID Verification**

![](images/idvflow.png)

The [**DocumentInfo**](/../../API-Specifications/onboarding/#documentinfo) **API** is a message containing the details of an onboarding document collected from the customer to prove identity.

**TIP |**

As a Partner, you can re-brand the Digital ID Verification experience to reflect your brand's colour & tone; with the exeption of when your camera is attempting to take a picture. 

#### **Accepted forms of identity documentation for Digital ID Verification**

At this moment, Finaptic will accept select Valid (unexpired) Government issued identification.
Note, damaged and worn identification documents will not be accepted.

##### **Valid (unexpired) Government Issued Identification**

* Canadian Passport (front page only)
* Canadian Permanent Resident Card Canada (front & backpages)
* Canadian Citizenship card (issued prior to 2012) Canada (front & backpages)

##### **Valid (unexpired) Canadian Government Driver's Licence Identification**

* British Columbia Driver's License British Columbia, Canada
* Alberta Driver's License Alberta, Canada
* Ontario Driver's License Ontario, Canada
* Saskatchewan Driver's License Saskatchewan, Canada
* Manitoba Driver's License Manitoba, Canada
* Quebec Driver's License Quebec, Canada
* New Brunswick Driver's License New Brunswick, Canada
* Nova Scotia Driver's License Nova Scotia, Canada
* Prince Edward Island Driver's License Prince Edward Island, Canada
* Newfoundland and Labrador Driver's License Newfoundland & Labrador, Canada
* Yukon Driver's License Yukon, Canada
* Northwest Territories Driver's License Northwest Territories, Canada
* Nunavut Driver's License Nunavut, Canada

##### **Valid (unexpired) Canadian Provincial services cards**

* British Columbia Services Card British Columbia, Canada

##### **Valid (unexpired) Canadian Provincial or territorial identity cards**

* British Columbia Enhanced ID British Columbia, Canada
* Alberta Photo Identification Card Alberta, Canada
* Saskatchewan Non-driver photo ID Saskatchewan, Canada
* Manitoba Enhanced Identification Card Manitoba, Canada
* Ontario Photo Card Ontario, Canada
* New Brunswick Photo ID Card New Brunswick, Canada
* Nova Scotia Identification Card Nova Scotia, Canada
* Prince Edward Island Voluntary ID Prince Edward Island, Canada
* Newfoundland and Labrador Photo Identification Card Newfoundland and Labrador, Canada
* Northwest Territories General Identification Card Northwest Territories, Canada
* Nunavut General Identification Card Nunavut, Canada

### **Onboarding a Minor**

Finaptic's Onboarding Service is ready to onboard minors and is subject to Finaptic ID Verification Policies listed below.

| **Age Brackets**         | **FINTRAC Requirement**            | **Finaptic Technology Policy**                                                                                                                                                                                                               | Available |
| ------------------------ | ---------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------- |
| **0 - 11 years of age**  | ID Verification is <u>Optional</u> | Don't ID Verify Child <br>Must ID Verify Parent/Guardian<br><br><u>Details below</u>:<br>Parent/Guardian ID must be verified *(ID does not be re-verified if on file but parent/guardian should be authenticated or provide acknowledgment)* | Yes       |
| **12 - 15 years of age** | ID Verification is <u>Optional</u> | Same as above; *0-11 years of age*                                                                                                                                                                                                           | Yes       |
| **16+ years of age**     | ID Verification is <u>Required</u> | Must ID Verify like an Adult                                                                                                                                                                                                                 | No        |

---

### Data Retention Guidelines

**For how long are the Personal Identity and Selfie information stored?**

Our third party vendor, Acuant, which receives the selfie and and Personal ID is permitted to store the photo and ID solely for the purposes of verifying the identity of the applicant and verifying that the identification document is authentic, valid and current, and for providing us the information extracted from the personal identification. `Acuant expunges the selfie and the personal identification from their system within **24 hours**.` 

If the applicant is approved to open an account, Finaptic receives the following information regarding the identification document used to verify the applicant’s identification, which Finaptic will store for `7 years after the customer’s account is closed`; retained data points include

- Person’s Name

- Date on which the person’s identity was verified

- Type of document used (for example, driver's licence, passport, etc.)

- Unique identifying number of the document used

- Jurisdiction (province or state) and country of issue of the document

- Expiry date of the document, if available (if this information appears on the document or card, we must record it)

In case of a malfunction in the onboarding process or the user doesn’t complete the process, a garbage collection will delete picture within 24 hours.

`All retained data is stored in the Google Cloud Platform's GCS Encrypted buckets.`

---

## **Pillar 3 | Onboarding Agreements**

In addition to the agreements presented to your customer to upload your app, such as an End User License Agreement and Terms of Use, your customer must read and agree to a number of additional agreements before being able to proceed with the application. As part of Onboarding, these include the Electronic Communications Agreement and Privacy Policy. Documents that are presented later in the app, and that are related to your specific use case, including product disclosures, product applications and agreements are covered under the product sections of this guide.

There are 2 notable APIs to refer to:

1. The [**Consent**](/../../API-Specifications/onboarding/#consent) **API** is a message indicating the receipt of a requested customer Consent.

2. The [**Disclosure**](/../../API-Specifications/onboarding/#disclosure) **API** is a message indicating the receipt of a requested customer Disclosure.

### **The Agreements**

There are 4 primary agreement types with each tailored to the partner agreement.

1. [Finaptic Account Fee Guide and Agreement](/../../Implementation-Guide/Legal-And-Regulatory-Compliance/finaptic_account_fee_guide_and_agreement/)

2. [Finaptic App Terms of User](/../../Implementation-Guide/Legal-And-Regulatory-Compliance/finaptic_app_terms_of_use/)

3. [Finaptic Electronic Communications Agreement](/../../Implementation-Guide/Legal-And-Regulatory-Compliance/finaptic_electronic_communications_agreement/)

4. [Finaptic Privacy Policy](/../../Implementation-Guide/Legal-And-Regulatory-Compliance/finaptic_privacy_policy/)

### **Making the agreement available within your app; [SDK Guides](/../../sdk-guide/index/)**

Customers must be able to easily access each agreement from your app. Best practice is to send the agreement to the customer's in app Message Centre noting their consent. You must also have the most recent version of the agreement displayed in the "Legal" section of the app or other obvious section that can be easily found.
