
# Auto-Deposit Overview

## Introduction

Finaptic offers its customers access to the Interac payments network. Multiple products are available for Interac, and are accessible via the [/Transfers](/../../API-Specifications/transfers/) endpoint. This endpopint gives Finaptic customers the ability to initiate Interac payment processes. 

## Guide Purpose

This guide will:

- Provide an overview on Interac services available to Finaptic customers.

- Provide an API overview for effective integration

---

## About the Product

#### What is Interac?

[Interac](https://www.interac.ca) is a Canadian payment network that provides interbank payment capabilities via a number of products. Finaptic customers are able to access this functionality, enabling their users to receive funds from other financial institutions.

Users perform a customer registration, in which their email address or telephone number are linked to the Finaptic deposit account of their choice. Users are then able to receive funds to their Finaptic account.

When funds have been receieved by the user, the deposit can exist in 3 states: Begin, Commited, Reversed.

All deposit states will appear in the transaction history. Transaction history can be accessed from the [Personal Financial Management](/../Implementation-Guide/Personal-Financial-Management/) domain. Additional details for a transaction can be retrieved from Finaptic's [Transfers](/../../API-Specifications/transfers/#autodeposit).

### API Overview

#### Registration
The registration request for Interac services is generated through the Interac API function.

This request is generated via the [CreateRegistrationRequest](/../../API-Specifications/transfers/#CreateRegistrationRequest) call.

4 data elements are required via the API call in order to generate this request:

* [account_id](/../../API-Specifications/transfers/#autodeposit) - Provided for the account that the customer wishes auto-deposit transactions to be routed to
* [account_alias](/../../API-Specifications/transfers/#autodeposit) - This is typically the user's email address, which can be used for both the account_alias field and the alias_email field
* [alias_email](/../../API-Specifications/transfers/#autodeposit)
* [alias_phone_number](/../../API-Specifications/transfers/#autodeposit)

All other details from the customer file that are required for registration will be retrieved by Finaptic during the registration process.

In the event that a user wants to change their registration to a different account, a new call with the new account ID must be made, at which point, the same process is to be followed.

#### Registration Errors

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


#### Deposits

Interac Auto-Deposit transactions can be retrieved from the [Transfers](/../../API-Specifications/transfers/) domain and from the [Personal Financial Management](/../../Personal-Financial-Management/) domain. The PFM domain contains basic summary information regarding the transaction, while the Transfers domain contains a complete data set which may or may not be required for the end user to identify the transaction.

Transactions will be routed to the account specified by the user in the registration call.

---

#### Visualizing Critical Experiences

##### Auto-Deposit Registration

Finaptic has provided the screen elements below for reference purposes. This process outlines a possible user flow representation of what your customer experience could resemble.

![Auto-Deposit Registration Example](Images/AutoDepositRegistrationAppExample.png)


##### Registration Confirmation

Once a registration request is submitted, Interac and Finaptic take care of the rest of the process.
Users will receive an email at the email address that they provided from Interac, which will ask them to verify that they have selected "NatCan Solutions" as their auto-deposit provider.

![Interac Verification Email](Images/InteracRegistrationVerify.png)

Once this verification step is complete, the registration process is also complete, which will result in a confirmation email being generated from Interac.

![Interac Confirmation Email](Images/InteracRegistrationConfirm.png)
---