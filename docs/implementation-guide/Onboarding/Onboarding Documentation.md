# Onboarding Functional Overview

Welcome to Finaptic's end-to-end Onboarding Service. We make it simple to build trust in a digital world and easy to welcome new customers. This documentation includes comprehensive information for building a compliant customer onboarding experience and integrating with Finaptic's Onboarding Service.

#### **This guide will**
1. Educate you on Finaptic's Onboarding Service
2. Guide you on frontend design
3. Arm you with an API Guide for effective integration

## **What is Onboarding?**
Onboarding is the process of welcoming and establishing a relationship with new customers including adding new products to existing customers. Steps are anchored to obtain a customer's consent to collect their personal information and verify their identity to provide banking products and services and establish a relationship.

## **About the Platform**
Finaptic's Onboarding Service processes high volumes of identity checks annually and inherits all scalability, reliability, and security features of the core platform.

#### **Three Pillars of Onboarding**
* Pillar 1 Customer Information
* Pillar 2 Digital ID Verification
* Pillar 3 Onboarding Agreements


## **Pillar 1 | Customer Information**
In order to satisfy regulatory obligations such as Know Your Customer (KYC) and Account Opening requirements there are 4 categories of information type that must be collected from the applicant. In total, there are about 15-17 data fields or questions to complete.


| PERSONAL                                                                                                          | TAX RESIDENCY                                                                                                                                                                                                                                                                       | PROFESSIONAL                                                                                                                                                 | ACCOUNT OPENING - PRODUCT DEPENDANT                     |
| ----------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------- |
| - Full Legal Name<br/>- Middle Name is Optional<br/>- Residential Address <br/>- Date of Birth<br/>- Phone Number | - Canadian Tax Residency Status<br/>- US Citizen<br/>- Another Country Tax Residency Status<br/>*If the applicant is a tax resident of Another Country (including the US), or a US Citizen, then the tax identification number for that country and Canadian SIN must be collected* | - Employment Status<br/>- Employment Occupation<br/>- Employer Name<br/>- Employer Address<br/>- Employer Phone Number<br/>- Employed Since Date is Optional | - Account Opening Purpose<br/>- Account Source of Funds |



## **Pillar 2 | Digital ID Verification**
This process verifies a person's identity by using computer technology. We screen for bad actors and potential device level frauds and validate the person's documentation and liveness.

#### **How does Finaptic's Onboarding Sevice help your business?**
Our approach helps prevent fraud, money laundering and terrorist funding. Using leading ID verification partner technologies that leverage artificial intelligence technology; providing document and biometric authentication with liveness tests to truly verify if the person applying for a financial product is who they say they are.

Working in real time, we will eliminate manual screening errors, speed up the document inspection process, and significantly reduce employee intervention, operating costs and customer inconvenience.

#### **Requirements for Digital ID Verification**
1. The identification document has to be authentic, valid, and original.
2. A valid (unexpired) identification document as listed in the "Accepted Forms of Identity Documentation for Digital Identification Verification" section.
3. A mobile device with a functional camera for a selfie.

#### **Digital ID Verification Decision Making**
Finaptic will render a decision of either Approved or Declined within 90 seconds from application submission.

During those 90 seconds, Finaptic will complete the following process to screen for bad actors and potential fraud with every applicant
1. Cross referenced against up-to-date Sanctions lists, Politically Exposed Persons, Watchlists
2. Validate for potential device & IP level fraud
3. Facial Recognition Match will include a liveness test. A liveness test ensures that there is a real person present instead of a photo, video playback or even a mask.

An applicant will be permitted up to 3 retries for common applicant error types (ex. name mismatch, incorrect date of birth, etc.).

An applicant will not be permitted to re-apply if the reason for a decline is Fraud or Sanctions related.

### **Visualizing Digital ID Verification**
![](images/idvflow.png)


### **Accepted forms of identity documentation for Digital ID Verification**
At this moment, Finaptic will accept select Valid (unexpired) Government issued identification.
Note, damaged and worn identification documents will not be accepted.

#### **Valid (unexpired) Government Issued Identification**
* Canadian Passport
* Canadian Permanent resident card Canada
* Canadian Citizenship card (issued prior to 2012) Canada

#### **Valid (unexpired) Canadian Government Driver's Licence Identification**
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

#### **Valid (unexpired) Canadian Provincial services cards**
* British Columbia Services Card British Columbia, Canada

#### **Valid (unexpired) Canadian Provincial or territorial identity cards**
* British Columbia Enhanced ID British Columbia, Canada
* Alberta Photo Identification Card Alberta, Canada
* Saskatchewan Non-driver photo ID Saskatchewan, Canada
* Manitoba Enhanced Identification Card Manitoba, Canada
* Ontario Photo Card Ontario, Canada
* New Brunswick Photo ID Card New Brunswick, Canada
* Nova Scotia Identification Card Nova Scotia, Canada
* Prince Edward Island Voluntary ID Prince Edward Island, Canada
* Newfoundland and Labrador Photo Identification Card Newfoundland and Labrador, * Canada
* Northwest Territories General Identification Card Northwest Territories, Canada
* Nunavut General Identification Card Nunavut, Canada

### **Onboarding a Minor**
Finaptic's Onboarding Service is ready to onboard minors and is subject to Finaptic ID Verification Policies listed below.

| AGE BRACKET          | FINAPTIC POLICY                                                                                                                                                                                                                                                                                               |
| -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Aged 0-11 years old  | Finaptic will not ID Verify a minor aged 0-11 years old
We will ID Verify Parent/Guardian<br>Details below:<br><br>- Parent/Guardian ID must be verified <br>- If Parent/Guardian is already verified and on-file then ID will not not be re-verified and Finaptic will capture parent/guardian acknowledgment |
| Aged 12-15 years old | Same as above                                                                                                                                                                                                                                                                                                 |
| Aged 16+             | Must ID Verify similar to an adult                                                                                                                                                                                                                                                                            |


## **Pillar 3 | Onboarding Agreements**
In addition to the agreements presented to your customer to upload your app, such as an End User License Agreement and Terms of Use, your customer must read and agree to a number of additional agreements before being able to proceed with the application. As part of Onboarding, these include the Electronic Communications Agreement and Privacy Policy. Documents that are presented later in the app, and that are related to your specific use case, including product disclosures, product applications and agreements are covered under the product sections of this guide.

### **The Agreements**
#### **Electronic Communications Agreement**
Your customer must consent to an Electronic Communications Agreement at the beginning of Onboarding so that financial products and services can be provided to them electronically. This includes their agreement to the delivery of certain regulatory information, which under federal regulations must be delivered in writing, to be delivered electronically to them. It will also allow your customer to enter into other agreements electronically through your app, rather than having to provide "wet signatures".

#### **Privacy Policy**
Your customer must also consent to the Privacy Policy at the beginning of Onboarding so that they provide permission for the collection, use and sharing of their personal information. The Privacy Policy must explain why their personal information is being collected, used and shared, and what will happen to their personal information when your relationship ends.

### **Making the agreement available within your app**
Customers must be able to easily access each agreement from your app. Best practice is to send the agreement to the customer's in app Message Centre noting their consent. You must also have the most recent version of the agreement displayed in the "Legal" section of the app or other obvious section that can be easily found.
