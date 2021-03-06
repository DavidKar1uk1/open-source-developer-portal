---
layout: twoColumn
section: resources
type: article
title:  Customer types
description: Use this page to understand the different features, customer types and ACH transfer processes.
---
# Customer types

A Dwolla API customer is created programmatically by your CIP verified Dwolla Master Account via the [Create a Customer](https://docs.dwolla.com/#create-a-customer) endpoint. All of the Customer's required information will be handled through the API and the Customer will interact directly with your application to manage their account. As a developer, you will want to create a Customer that best suits the business case of your application. Below is a very high level detailing of the Customer types available with Dwolla.

> **Note**: Regardless of which Customer type(s) you choose to create for your application, keep in mind that all users onboarded as Customers must be US persons of age 18 or older.

| **Customer Type** | CIP Verification | Dwolla `balance` | Transaction Limit | Transact with |
|---------------------------|----------------------|------------------------|------------------------|--------------------|
| **Personal verified Customer** | Required | Yes | $5,000 per **transfer** | All Customer types |
| **Business verified Customer** | Required | Yes | $10,000 per **transfer** | All Customer type |
| **Unverified Customer** | Optional | No | $5,000 per **week** | Verified Customers, Dwolla Master Account |
| **Receive-only User** | Optional | No | N/A | Verified Customers, Dwolla Master Account |

As you decide what Customer types to create for your application, a good thing to keep in mind is [Customer Identification Program (CIP) Verification](https://www.dwolla.com/updates/guide-to-cip-customer-identification-program-dwolla-payments-api/). Remember that regardless of your application type, a transfer between two parties requires that at least one party must be CIP verified. It is your decision about which party completes this process based on your business model. Your own Dwolla Master Account can count as a verified party. You may also consider having both parties complete CIP verification, as we also require CIP verification in order for a customer to hold funds in the Dwolla network in the form of a balance.

## Verified Customer

Verified Customers are defined by their ability to both send and receive money. They can also interact with any customer type and hold a `balance` funding source within the Dwolla network. Think of the Dwolla `balance` as a wallet which a Customer can hold, send or receive funds to within the Dwolla network.

There are two types of verified Customer types your Customer can sign up as: `Personal` or `Business`.

### Personal verified Customer

Personal verified Customers have the advantage of fitting all funds flows, as they can both send and receive funds. This Customer type can also hold a balance. The individual will need to go through CIP verification prior to being able to send funds to or from their bank. CIP verification involves passing Customer data to verify them, including their name, date of birth, and last four digits of their social security number. For more information about verifying this Customer type in our [Customer verification article](/resources/personal-verified-customer/create-personal-verified-customers.html).

With a per-transaction limit of $5,000, this Customer type is able to interact with Dwolla and your application seamlessly.

### Business verified Customer

Business verified Customers are unique in their sign up flow, as they need multiple parties to be verified. These will include:

* The business (required)
* The controller (conditionally required)
* The beneficial owner (conditionally required)

Business verified Customers will need an Account Admin to signup the company during the onboarding process. This Account Admin is not identity verified. To become fully verified, a controller and/or a beneficial owner may need to be identity verified.  A controller is any natural individual who holds significant responsibilities to control, manage, or direct a company or other corporate entity (i.e. CEO, CFO, General Partner, President, etc). A company may have more than one controller, but only one controller’s information must be collected. A beneficial owner is any natural person who, directly or indirectly, owns 25% or more of the equity interests of the company.

The Controller will need to provide information to be fully identity verified. This includes their last four SSN and date of birth for identity verification purposes. For certain business types, a business’ EIN will also need to be provided as part of the CIP process. For more information about adding a controller, check out our [business verified Customer creation article](https://developers.dwolla.com/resources/business-verified-customer/create-business-verified-customers.html).

Certain business types may also need to add and certify beneficial ownership. You can find more information about adding beneficial owners in our [developer resource article](https://developers.dwolla.com/resources/business-verified-customer/adding-beneficial-owners.html). To learn more about certifying beneficial ownership, [check out our certification article](https://developers.dwolla.com/resources/business-verified-customer/handling-beneficial-owner-certification.html).

For a full series of articles that goes in depth on business verified Customers, take a look at our [Customer verification article](https://developers.dwolla.com/resources/business-verified-customer.html). This series of articles also goes into detail on the identity verification process for controllers and beneficial owners.

## Unverified Customer

An unverified Customer type requires a minimal amount of information : `firstName`, `lastName` and `email`. While Customer creation and onboarding is simple compared to a verified Customer, there are some things to consider.

Unverified Customers have a default sending transaction limit of $5,000 per week. A week is defined as Monday to Sunday UTC time. If you have an unverified Customer looking to send more than $5,000 in a week, you may want to explore [upgrading them to a verifed Customer type](https://docs.dwolla.com/#update-a-customer).

As this Customer is not CIP verified, they will only be able to transact with verified Customers or your Dwolla Master Account.

## Receive-only User

Receive-only Users are restricted to payouts only funds flow. This user type maintains limited functionality in the API and is only eligible to receive transfers to an attached bank account. This user type can only interact with verified Customers and a Dwolla Master Account.

<ol class="alerts">
   <li class="alert icon-alert-info">
       Receive-only Users cannot send funds back. If you need this user to send funds back for any reason, you may need to resolve this outside of the Dwolla network.
   </li>
</ol>
