# Client Profile Guide

The customer profile domain’s primary purpose is to be the canonical reference for customer demographics (IDs, addresses, contact information, date of birth, gender, etc.)  and linked entity information. In addition, the domain stores relationships between customers, customer stakeholders (accountants, friends, family, parent/child/dependant), customers and partners, and know-your-customer (KYC) status. 
The demographic information collected on a customer is thoroughly described in the [onboarding domain](../../../API-Specifications/onboarding/). 
In addition to those demographics, we are required to track relationships between individual parties as part of our AML and Risk compliance program. These relationship types are described as a part of the Fintrac STR batch [specification](https://www.fintrac-canafe.gc.ca/reporting-declaration/batch-lots/mod2-eng) and [guide](https://www.fintrac-canafe.gc.ca/guidance-directives/transaction-operation/Guide3/str-eng).
Finaptic recommends partners collect their own demographic information, with proper consents and disclosures to the customer, for the purposes of their own application, business, or CRM.

## Client Profile Field Change Guide

The APIs provided by the customer profile domain can be used to provide the functionality to your customers to update their demographic information when it is related to their financial products. For example, if they change primary residences, telephone numbers, or employment. Certain information on the customer profile can not be changed directly as it requires going through the KYC process, such as ID verification. A re-verification will have to be triggered through the onboarding APIs when an ID, Name or Date Of Birth needs to be updated.
| Field Name                          | Requires KYC / Re-onboarding steps to change field | Requires KYC / Re-onboarding steps to add an additional fields. (N/A means only one exists on a profile) |
| ----------------------------------- | -------------------------------------------------- | -------------------------------------------------------------------------------------------------------- |
| First Name, Middle Name, Last Name  | Yes                                                | N/A                                                                                                      |
| Date of Birth                       | Yes                                                | N/A                                                                                                      |
| Contact Details - Telelphone number | No                                                 | No                                                                                                       |
| Contact Details - Email Address     | Yes                                                | No                                                                                                       |
| Employment                          | No                                                 | N/A                                                                                                      |
| Customer Residency                  | Yes                                                | N/A                                                                                                      |
| Add/Remove Other Tax Residence      | No                                                 | No                                                                                                       |
| Address (Primary)                   | Yes                                                | N/A                                                                                                      |
| Identity Document Details           | Yes                                                | Yes                                                                                                      |
## Links

* [Customer Profile API Specification](../../../API-Specifications/clientinformation/)

* [Onboarding API Specification](../../API-Specifications/onboarding/)
* [Onboarding API Specification](../../../API-Specifications/onboarding/)

* Fintrac Suspicious Transaction report batch [specification](https://www.fintrac-canafe.gc.ca/reporting-declaration/batch-lots/mod2-eng) and [guide](https://www.fintrac-canafe.gc.ca/guidance-directives/transaction-operation/Guide3/str-eng).