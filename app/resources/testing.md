---
layout: twoColumn
section: resources
type: article
title:  Test in the Sandbox
description: The Sandbox environment allows you to test the Dwolla API without any real-world impact, meaning that no real, personal identifying information or financial data is used.
---

# Testing in the Sandbox

## Overview

The Sandbox environment allows you to test the Dwolla API without any real-world impact, meaning that no real, personal identifying information or financial data is used and bank transfer processing does not run as it would in Production. Since dummy data is utilized in the Sandbox environment, Dwolla provides a way to simulate various test cases by providing either sentinel values that can be used when making API requests or buttons that trigger certain actions (e.g. simulate bank transfer processing).

<ol class="alerts">
  <li class="alert icon-alert-info">If you are new to application development with Dwolla, we recommend you review our <a href="/guides/sandbox-setup">Sandbox Setup Guide</a>.
  </li>
</ol>

* * *

# Testing Customers

### Manage Customers
The [Sandbox Dashboard](https://dashboard-sandbox.dwolla.com) allows you to manage Customers, as well as transfers associated with the Customers that belong to your Sandbox account. Once your application has [created its Customers](https://docs.dwolla.com/#create-a-customer), you can access the [Sandbox Dashboard](https://dashboard-sandbox.dwolla.com) to validate that the request was recorded properly in our test environment.

#### `Verified Customers`

##### **Simulate identity verification statuses**

There are various reasons a Verified Customer may have a status other than `verified` after the initial Customer creation. You will want your app to be prepared to handle these alternative statuses.

In production, Dwolla will place the Verified Customer in either the `retry`, `kba`, `document`, `verified`, or `suspended` state of verification after an initial identity verification check.

**For personal verified Customers**, reference the resource article on [customer verification](https://developers.dwolla.com/resources/personal-verified-customer/handling-verification-statuses-personal.html) for more information on handling identity verification for Verified Customers. To simulate the various statuses in the Sandbox, submit either `verified`, `retry`, `kba`, `document`, or `suspended` in the **firstName** parameter in order to [create a new Verified Customer](https://docs.dwolla.com/#request-parameters---verified-customer) with that status.

**For business verified Customers**, reference the resource article on [customer verification](https://developers.dwolla.com/resources/business-verified-customer/create-business-verified-customers.html) article that goes over information on properly verifying your Controllers and Beneficial Owners. To simulate the various statuses in the Sandbox, submit either `verified`, `retry`, `document`, or `suspended` in the **controller firstName** parameter in order to [create a new Verified Customer](https://docs.dwolla.com/#request-parameters---verified-customer) with that status.

##### **Simulate KBA verified and failed events**

If a Personal Verified Customer isn’t systematically identity-verified after their second attempt to retry their information, the Customer may be placed in a `kba` status and will be required to successfully answer at least three out of four knowledge based authentication(KBA) questions in order to pass verification. 

To simulate the `customer_kba_verification_passed` event as the result of KBA success in Sandbox, answer all four questions with either “None of the above” or “I have never been associated with this vehicle”. As a result, the Customer will be placed in a verified status and the `customer_verified` event is triggered.

To simulate the `customer_kba_verification_failed` event as the result of KBA failure in Sandbox, answer the questions with any answer choices other than “None of the above” or “I have never been associated with this vehicle”. As a result, the Customer will be placed in a document status and the `customer_verification_document_needed` event is triggered.

##### **Simulate document upload approved and failed events**

If a Verified Customer isn't systematically identity-verified, the Customer may be placed in a `document` status and will require an identifying document to be uploaded and reviewed. Reference the [customer verification](https://developers.dwolla.com/resources/customer-verification.html) resource article for acceptable forms of identifying documents for `Verified Customers`.

Since the document review process requires interaction from Dwolla, sample test documents can be uploaded in the Sandbox environment to simulate the `customer_verification_document_approved` and `customer_verification_document_failed` events. Right click on an image below and select “Save image as...”. **Note:** When saving an image, make sure to keep the size, format, and name of the image the same.

##### **Sample document approved image**
![Image of document approved](https://cdn.dwolla.com/images/sandbox-test-images/test-document-upload-success.png "document success image")

##### **Sample document failed image**
![Image of document failed](https://cdn.dwolla.com/images/sandbox-test-images/test-document-upload-fail.png "document failed image")

* * *

# Testing Funding Sources

### Test bank account numbers

Dwolla requires a valid U.S. routing number and a random account number between 4-17 digits to add a bank account. For testing purposes, you can use the routing number `222222226` or refer to the list of routing numbers from the [Federal Reserve Bank Services](https://www.frbservices.org/EPaymentsDirectory/agreement.html) website.

## Simulate IAV success and error scenarios in Sandbox

Dwolla's Sandbox environment uses a [fake service](https://www.google.com/url?q=https://www.dwolla.com/updates/fake-it-as-you-make-it-why-fake-services-are-awesome-for-developers/) which allows you to simulate the end-user experience for adding and verifying a bank account within the IAV flow. The default "success" scenario is a happy path flow that includes: a single set of MFA questions, presentation of two valid accounts and one account without a routing number. Any text can be entered in the input fields to allow you to proceed through the IAV flow.

To help you test error scenarios, a flag and an option can be used in the first field (usually username or ID) for the bank you are trying to authenticate. The remaining input fields in the flow can be any string of text, as we allow you to proceed through the flow regardless of the information entered. Use the following flag and option values to test various error scenarios:

|     Flag      |   Option     | User-facing message |
|------------- |----------------- |----------------|
| -e  | InvalidLogin | Please make sure your login or security information is correct. |
| -e  | AccountNotFound | Sorry, we’re unable to find your {BANK NAME} account. Please try again or use a different account. |
| -e  | UnsupportedSite | Sorry, that financial institution is not supported. If possible, please choose a different one or an alternative method for connecting your financial institution. |
| -e  | AlreadyLoggedIn | If you're currently logged in to your {BANK NAME} account, please log out and try again. |
| -e  | VisitSite | We’re unable to process your information because the {BANK NAME} site is currently requiring additional action from you. Please resolve this, then try again. |
| -e  | UnavailableSite | Sorry, there seem to be some technical difficulties while attempting to process your information. Please try again later. |

##### Example IAV error scenario

![Screenshot of IAV user facing error message](/images/IAVErrorMessage.png "IAV user facing error")

### Test micro-deposit verification

If your application leverages the micro-deposit method of bank verification, Dwolla will transfer two deposits of less than $0.10 to your customer's linked bank or credit union account after calling the API to initiate micro-deposits. Since the Sandbox environment doesn't replicate any bank transfer processes, any two amounts **below** $`0.10` will allow you to verify the funding source immediately.

### Test micro-deposit failed verification attempts

When verifying a funding source using the micro-deposit method of bank verification, users are allowed **three attempts** to correctly input the two posted micro-deposit amounts. If the user fails to verify the two posted amounts on the third attempt, an event will be triggered and the funding source will not be verified using those micro-deposit amounts. To simulate the `microdeposits_maxattempts` or `customer_microdeposits_maxattempts` events in the Sandbox, use the amounts `0.09` and `0.09` when calling the API to verify micro-deposits. Reference the [micro-deposit verification resource article](https://developers.dwolla.com/resources/funding-source-verification/micro-deposit-verification.html) for more information on handling failed verification attempts.

* * *

# Testing Transfers

The Sandbox environment does not replicate any bank transfer processes, so a pending transfer will not clear or fail automatically after a few business days as it would in production. The transfer will simply remain in the pending state indefinitely.

### Simulate bank transfer processing

There are two options available for processing or failing bank transfers in the Sandbox environment.

* **Option 1:** your application will call the "sandbox-simulations" endpoint (referenced below) which will process or fail the last 500 bank transfers that occurred on the authorized application or Sandbox account.
* **Option 2:** you'll use the "Process bank transfers button" in the Sandbox Dashboard, which will process or fail the last 500 bank transfers that occurred on your Sandbox account or any API `Customers` you manage.

##### Sandbox simulations request and response

```noselect
POST https://api-sandbox.dwolla.com/sandbox-simulations
Accept: application/vnd.dwolla.v1.hal+json
Content-Type: application/vnd.dwolla.v1.hal+json
Authorization: Bearer {Your access token}

...

{
  "_links": {
    "self": {
      "href": "https://api-sandbox.dwolla.com/sandbox-simulations",
      "type": "application/vnd.dwolla.v1.hal+json",
      "resource-type": "sandbox-simulation"
    }
  },
  "total": 8
}
```

**Note:** If a bank-to-bank transaction is initiated between two users, you'll want to simulate bank transfer processing twice in order to process both sides of the transaction (debit and credit). Processing for bank transfers will also include initiated micro-deposits. If your application is [subscribed to webhooks](https://docs.dwolla.com/#webhook-subscriptions), notifications will be sent, including all transfer or micro-deposit related events, letting your application know that transfers have processed or failed.

##### **Process bank transfers button**

A “Process bank transfers” button is available in the Sandbox [Dwolla Dashboard and Admin](https://dashboard-sandbox.dwolla.com). This button performs the same function as the "sandbox-simulations" endpoint (mentioned above) and allows you to simulate bank transfer processing in the Sandbox. Once the button is clicked, Dwolla will process or fail (see below for how-to trigger ACH failures) the last 500 bank transfers that occurred on your Sandbox account or the API Customer accounts you manage.

![Screenshot of process bank transfers button](/images/process-bank-transfers.png "Process bank transfers button")

### Test bank transfer failures

Transfers to or from a bank account can fail for a number of reasons (e.g. insufficient funds, invalid account number, etc. ). When a bank transfer fails, the associated financial institution that rejected the transaction assigns an ACH return code and a transfer failure event is then triggered in Dwolla. Dwolla allows you to trigger various bank transfer failures by specifying an “R” code in the funding source `name` parameter when creating or [updating a funding source](https://docs.dwolla.com/#update-a-funding-source) for a Dwolla Account or API Customer. When a [transfer is initiated](https://docs.dwolla.com/#initiate-a-transfer) using a funding source that has an “R” code assigned to its name, a transfer failure event will trigger and the status will update to failed when you simulate bank transfer processing (as mentioned above).

Dwolla allows you to pass in a few different sentinel values that are used to test different bank transfer failure scenarios. The list of available sentinel values cover the most common uses cases where ACH return codes can be triggered in production. Reference the list of codes in the following table:

| Return code | Description |
|-------------|-------------|
| R01 | Insufficient Funds: This value is used to simulate funds failing from the source bank account (ACH debit). |
| R03 | No Account/Unable to Locate Account: This value is primarily used to simulate funds failing to the destination bank account (ACH credit). The funding source will be automatically removed from Dwolla when this return code is triggered. |
| R01-late | This value is used to simulate funds failing from the source bank account post-settlement. Note: You must click “Process bank transfers” twice in order to test this scenario. |
| R03-late | This value is primarily used to simulate funds failing to the destination bank post-settlement. The funding source will be automatically removed from Dwolla when this return code is triggered. Note: You must click “Process bank transfers” twice in order to test this scenario. |

For more information on bank transfer failures, and a list of common return codes and actions, refer to our [developer resource article](https://developers.dwolla.com/resources/bank-transfer-workflow/transfer-failures.html).

##### Example of using a sentinel value for testing bank transfer failures

This example assumes that a funding source has already been attached to an account. Once the funding source `name` has been updated to reflect the ACH failure scenario you want to test, then you can [initiate a transfer](https://docs.dwolla.com/#initiate-a-transfer) to or from that funding source via the API.

```raw
POST https://api-sandbox.dwolla.com/funding-sources/692486f8-29f6-4516-a6a5-c69fd2ce854c
Accept: application/vnd.dwolla.v1.hal+json
Content-Type: application/vnd.dwolla.v1.hal+json
Authorization: Bearer pBA9fVDBEyYZCEsLf/wKehyh1RTpzjUj5KzIRfDi0wKTii7DqY

...

{
  "name": "R03"
}
```
```ruby
funding_source_url = 'https://api-sandbox.dwolla.com/funding-sources/692486f8-29f6-4516-a6a5-c69fd2ce854c'
request_body = {
      "name" => "R03",
}

# Using DwollaV2 - https://github.com/Dwolla/dwolla-v2-ruby (Recommended)
funding_source = app_token.post "#{funding_source_url}", request_body
funding_source.name # => "R03"
```
```php
/**
 *  No example for this language yet. Coming soon.
 **/
```
```python
funding_source_url = 'https://api-sandbox.dwolla.com/funding-sources/692486f8-29f6-4516-a6a5-c69fd2ce854c'
request_body = {
  'name': 'R03'
}

# Using dwollav2 - https://github.com/Dwolla/dwolla-v2-python (Recommended)
funding_source = app_token.post(funding_source_url, request_body)
funding_source.body['name'] # => 'R03'
```
```javascript
var fundingSourceUrl = 'https://api-sandbox.dwolla.com/funding-sources/692486f8-29f6-4516-a6a5-c69fd2ce854c';
var requestBody = {
  name: "R03"
};

appToken
  .post(fundingSourceUrl, requestBody)
  .then(res => res.body.name); // => "R03"
```
