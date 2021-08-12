# Cards Overview

## Introduction

Finaptic offers its customers access to create Cards that link to other products, and serve as an access vehicle via the Mastercard Network. End users are able to use the card to access funds in the funding account associated with the card. The Core Card API enables customers to build various experiences that provide their customers with virtual and physical cards.

## Guide Purpose

This guide will:

- Provide an overview on Card services available to Finaptic customers.

- Provide an API overview for effective integration

---

## About the Product

#### What are cards?

##### Virtual Cards

##### Physical Cards

### API Overview

#### API Name

#### Errors

A number of errors could occur in the registration process. These errors will be communicated either via our asynchronous API surface, or via the synchronous SDK capabilities which can be found in our [SDK Guide](/../../SDK-Guide/).

Error possibilities at time of registration are as follows:

| Code | Description                                                              |
|------|--------------------------------------------------------------------------|
| 301  | Customer does not exist                                                  |
| 379  | Invalid mobile phone area code                                           |
| 380  | Invalid mobile phone number                                              |
| 400  | Mutually exclusive options are specified in the request                  |
| 410  | Customer is disabled                                                     |
| 422  | Retail name missing                                                      |
| 451  | Customer not registered for transfer product, currency code combination  |
| 540  | Invalid bank account                                                     |
| 543  | Account-alias handle is already defined in e-Transfer system             |
| 545  | Duplicated participant account-alias reference number                    |
| 546  | Invalid account-alias registration notification preference               |
| 547  | Maximum number of account-alias registrations threshold exceeded         |
| 567  | Invalid UUID format                                                      |
| 591  | Sender Account Identifier and Service Type combination is not supported. |


---

#### Visualizing Critical Experiences

##### Virtual Card Display

Finaptic has provided the screen elements below for reference purposes. This process outlines a possible user flow representation of what your customer experience could resemble.

![Auto-Deposit Registration Example](Images/AutoDepositRegistrationAppExample.png)


##### 

Once a registration request is submitted, Interac and Finaptic take care of the rest of the process.
Users will receive an email at the email address that they provided from Interac, which will ask them to verify that they have selected "NatCan Solutions" as their auto-deposit provider.

![Interac Verification Email](Images/InteracRegistrationVerify.png)

Once this verification step is complete, the registration process is also complete, which will result in a confirmation email being generated from Interac.

![Interac Confirmation Email](Images/InteracRegistrationConfirm.png)
---