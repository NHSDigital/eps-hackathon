# EPS API Hackathon Activities

## Introduction
For the hackathon we have organised some introductory tasks to start developers off interacting with the EPS API. These are only suggestions and do not stand as an exhaustive guide; for details on the API itself refer to the API specification found [here](https://digital.nhs.uk/developer/api-catalogue/electronic-prescription-service-fhir).

We understand that some suppliers may only be interested in Prescribing actions or Dispensing actions respectively. As such, activities are separated into these two relevant areas. 

Additionally, there is an introduction to our preliminary Tracking API, with one endpoint available. One of our goals is to receive feedback on how this API could be developed further. 

## Prescribing
### Task 1
We have provided some pre-signed prescriptions [here](/eps-hackathon/signed-prescriptions). 

You can use the `/$process-message` endpoint to send these examples to Spine. 

After sending the prescription, track it using the Tracker endpoint detailed in the last section of this document. 

As well as tracking the prescription, you can send a cancellation message using the `/$process-message#prescription-order-update` endpoint. Use the tracker again to check the status has changed on the prescription.

### Task 2
The complete prescribing workflow requires a further integration with the Signing Service API to sign new prescriptions in order to make them legally valid.

Information on integrating with the Signing Service can be found [here](https://digital.nhs.uk/developer/api-catalogue/signing-service). Contact someone from the EPS development team to receive credentials for this.

With Signing Service credentials you will be able to `POST /SignatureRequest`, `GET /SignatureResponse`,  along with the `/$prepare` endpoint to complete the whole Prescribing flow.

## Dispensing
### Task 1
Release a non-nominated prescription, using a prescription ID, call the `/Task/$release` endpoint.
### Task 2
Release a nominated pharmacy prescription, using an ODS code for the nominated pharmacy,call the `/Task/$release` endpoint.
### Task 3
Create a non-nominated and nominated prescription dispense-notification message, call the `/$process-message#dispense-notification` endpoint.
### Task 4
Query a prescription status using the Tracker API endpoint (section 3)
### Task 5
Claim for non-nominated and nominated prescriptions, call the `/Claim` endpoint.
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
1. Which of these endpoints would be most useful for you?
2. Are there any other ways of querying EPS that you think would be useful?
