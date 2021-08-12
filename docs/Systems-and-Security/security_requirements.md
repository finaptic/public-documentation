# Security Requirements for Partners

Security is a shared responsibility. While Finaptic provides a secure environment and APIs, it is important to ensure that the client applications and partner environments follow some guidelines, to ensure the entire experience is protected.

## Scope

This document establishes requirements for Finaptic partners, for systems and services which are integrated with Finaptic. 

Example systems that are covered:

* A mobile app using the Finaptic SDK
* A web application using Finaptic APIs
* Systems which impact the security of other systems that are in scope

## Updates
Finaptic may periodically update these Security Requirements by posting a new version. It is understood that as threat evolves and systems become more complex, security controls must keep up. These requirements are good security practices, and in most case should not generate a significant increase to Partner's obligations. Partners who wish to discuss these measures or alternate measures can do so in private with the Finaptic Security and Legal teams.

## Information Security Policy and Incident Notification

1. Partners must have an information security program, as well as policies and standards.
2. Partners must have an incident management process, and technical means of logging and monitoring activity to discover such incidents. Logs should be kept for a minimum of a year and be accessible for querying at all times.
3. Partners must notify Finaptic of security incidents within sevent-two hours of becoming aware of it.

### Secure development

1. A software development life cycle (SDLC) process must be in place. 
2. Source code must be stored in a source version control system.
3. Authentication to this system, if exposed to the Internet, must require Multi-Factor Authentication (MFA).
4. Code reviews must be performed on code.
5. Vulnerabilities in code and libraries must be managed.

Recommendations for additional security can be found in our [partner app accreditation](partner_app_accreditation.md) document.

## Vulnerability Management 

### Disclosure Policy

1. Partners must have a disclosure policy covering systems and services using Finaptic technology that is similar to [Finaptic's](vulnerability_disclosure.md) which:
    * Allows anyone to report vulnerabilities
    * Provides timelines for expected resolution
2. Partners must report any vulnerability found in their applications or systems when they could put Finaptic data, systems or customers at risk.

### Fixing Issues

Partner will fix all critical and high severity vulnerabilities that could affect the security of Finaptic Data and functionality, of which Partner becomes aware, within 14 days of becoming aware of the vulnerability. If Partner cannot fix the vulnerability within 14 days, Partner will promptly inform Finaptic, including all details of the risk to Finaptic arising from Partner’s inability to fix the vulnerability, so a solution can be designed to mitigate the risk.


## Application and Network Penetration Testing.
1. Partner shall, at least once per year, perform a suite of independent third-party tests. These tests will be performed upon: (i) the applications leveraging Finaptic technology; (ii) all aspects of Partner’s internet-facing perimeter related to the applications and services; and (iii) Partner’s internal corporate network and internal systems which could impact the security of the applications and services. Partner will supply Finaptic with details of all third-party tests from the previous year, including names of third-party testers and number of person hours used.
2. Partner shall, upon Finaptic's request and under suitable non-disclosure obligations, share with Finaptic: (i) confirmation that the tests required by this Section 1.3 were performed; and (ii) the third party tests results from Sections 1.3(a)(i) and (ii) above.

## Technical Security Requirements
### Mobile and Web Applications
Mobile applications leveraging the Finaptic SDK must include controls to protect end customers and their information.

A [mobile and web application security standard](partner_app_accreditation.md) is provided to this effect, and is kept up to date as the threat landscape changes.