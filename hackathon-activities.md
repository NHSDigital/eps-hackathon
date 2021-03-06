# EPS API Hackathon Activities

## Introduction
For the hackathon we have organised some introductory tasks to start developers off interacting with the EPS API. These are only suggestions and do not stand as an exhaustive guide; for details on the API itself refer to the API specification found [here](https://digital.nhs.uk/developer/api-catalogue/electronic-prescription-service-fhir).

We understand that some suppliers may only be interested in Prescribing actions or Dispensing actions respectively. As such, activities are separated into these two relevant areas. 

Additionally, there is an introduction to our preliminary Tracking API, with one endpoint available. One of our goals is to receive feedback on how this API could be developed further. 

### Task
Integrating with EPS API requires the sourcing of relevant information necessary for prescribing and dispensing messages. Suppliers often have different sources and workflows for obtaining the information necessary to form a valid message.

Messages contain the following elements, please consider how you would populate them. Suggestions are provided:

* MessageHeader - Message type and status information - [Guide](https://simplifier.net/guide/NHSDigital/NHSDigital-MessageHeader)
* MedicationRequest - Drug information including snomed/dm+d codes - [snomed](https://termbrowser.nhs.uk/?) / [dm+d](https://applications.nhsbsa.nhs.uk/DMDBrowser/DMDBrowser.do)
* Patient - Patient information including name and address - [PDS](https://digital.nhs.uk/developer/api-catalogue/personal-demographics-service-fhir) 
* Practitioner - Prescriber information e.g. DIN number, GMC number etc. [ODS](https://odsportal.digital.nhs.uk/Organisation/Search) / [SDS LDAP](https://digital.nhs.uk/developer/api-catalogue/spine-directory-service-ldap) (Prefer ODS over SDS).
* Organization - Organization information e.g. ODS code, telephone etc. [ODS](https://odsportal.digital.nhs.uk/Organisation/Search)
* PractitionerRole - Practitioner + Organization information - use above for reference.

## Prescribing
### Task 1
We have provided some pre-signed prescriptions, ask an EPS development team member for a set of prescriptions to use. 

Use the [`/$process-message`](https://digital.nhs.uk/developer/api-catalogue/electronic-prescription-service-fhir#api-Prescribing-send-prescription-order-message) endpoint to send these examples to Spine. 

After sending the prescription, track it using the Tracker endpoint detailed in the last section of this document. 

As well as tracking the prescription, you can send a cancellation message using the [`/$process-message#prescription-order-update`](https://digital.nhs.uk/developer/api-catalogue/electronic-prescription-service-fhir#api-Prescribing-send-prescription-order-update-message) endpoint. Use the tracker again to check the status has changed on the prescription.

### Task 2
To complete the prescribing workflow, you must sign new prescriptions in order to make them legally valid. We recommend using the [Signing Service API](https://digital.nhs.uk/developer/api-catalogue/signing-service) for this.

Information on integrating with the Signing Service API can be found [here](https://digital.nhs.uk/developer/api-catalogue/signing-service). Contact someone from the EPS development team to receive credentials for this.

With Signing Service credentials you will be able to [`POST /SignatureRequest`](https://digital.nhs.uk/developer/api-catalogue/signing-service#api-SignatureRequest-upload-payload), [`GET /SignatureResponse`](https://digital.nhs.uk/developer/api-catalogue/signing-service#api-SignatureResponse-retrieve-signature),  along with the [`/$prepare`](https://digital.nhs.uk/developer/api-catalogue/electronic-prescription-service-fhir#api-Prescribing-prepare-prescription) endpoint to complete the whole Prescribing flow.

## Dispensing
### Task 1
Release a non-nominated prescription, using a prescription ID, call the [`/Task/$release`](https://digital.nhs.uk/developer/api-catalogue/electronic-prescription-service-fhir#api-Dispensing-release) endpoint.
### Task 2
Release a nominated pharmacy prescription, using an ODS code for the nominated pharmacy, call the [`/Task/$release`](https://digital.nhs.uk/developer/api-catalogue/electronic-prescription-service-fhir#api-Dispensing-release) endpoint.
### Task 3
Create a non-nominated and nominated prescription dispense-notification message, call the [`/$process-message#dispense-notification`](https://digital.nhs.uk/developer/api-catalogue/electronic-prescription-service-fhir#api-Dispensing-send-dispense-notification-message) endpoint.
### Task 4
Query a prescription status using the Tracker API endpoint (section 3)
### Task 5
Claim for non-nominated and nominated prescriptions, call the [`/Claim`](https://digital.nhs.uk/developer/api-catalogue/electronic-prescription-service-fhir#api-Dispensing-send-dispense-claim-message) endpoint.
### Task 6
Withdraw a previous dispense notification from (e.g. from Task 4), call the `/Task#withdraw` endpoint.

## Tracking
The prescription tracking API is a work in progress and as yet only a query by ID is available for use: call `/Task` with query parameter `identifier`.

Example: `GET https://int.api.service.nhs.uk/FHIR/R4/Task?identifier=CDF34E-A99968-4FF3BQ`

### Task 1
We would appreciate feedback on how to further develop this Tracker API. A brief overview of our planned queries are outlined here:
* Query on Prescription ID (available) - returns a single prescription
* Query on Dispensing Organization - returns a set of many prescriptions
* Query on Patient - returns a set of many prescriptions
* Query on Business Status - returns a set of many prescriptions
* Query on Prescription Status - returns a set of many prescriptions

Questions for Supplier Feedback:
1. Which of these endpoints would be most useful for you and your users?
2. Are there any other ways of querying EPS that you think would be useful?
