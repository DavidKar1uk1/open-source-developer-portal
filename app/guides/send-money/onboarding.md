---
layout: twoColumn
section: guides
guide:
    name: send-money
    step: '1'
type: guide
title:  Onboarding a customer with Dwolla's API
description: End users create their accounts entirely within your application Dwolla will securely store this sensitive information.
---
# Step 1: Create recipients using the Dwolla API

## Choose the Customer Type for your Recipient

Before your end user can receive funds to their connected bank account, they must be created as a Customer via the Dwolla API. The ability to send funds to end users is very flexible in that all Customer types can be used to leverage this funds flow. To learn more about the different types of Customers and the capabilities of each, [check out our developer resource article.](https://developers.dwolla.com/resources/account-types.html)

## Step 1A. Create the Customer

While you can use any Customer type for this funds flow, we will be creating a `receive-only` user in this guide, as it offers a lightweight onboarding experience for users. Just as the name implies, receive-only users are only eligible to receive funds into their attached bank account.

#### Request Parameters - Receive-only User

| Parameter     | Required? | Type   | Description              |
|---------------|-----------|--------|--------------------------|
| firstName     | yes       | string | Customer's first name    |
| lastName      | yes       | string | Customer's last name     |
| email         | yes       | string | Customer's email address |
| type          | yes       | string | Value of `receive-only`  |
| businessName  | no        | string | Customer's registered business name (optional if not a business entity) |
| ipAddress     | no        | string | Customer's IP address    |

```raw
POST https://api-sandbox.dwolla.com/customers
Content-Type: application/vnd.dwolla.v1.hal+json
Accept: application/vnd.dwolla.v1.hal+json
Authorization: Bearer 0Sn0W6kzNicvoWhDbQcVSKLRUpGjIdlPSEYyrHqrDDoRnQwE7Q
{
  "firstName": "Jane",
  "lastName": "Merchant",
  "email": "jmerchant@nomail.net",
  "type": "receive-only",
  "ipAddress": "99.99.99.99"
}

HTTP/1.1 201 Created
Location: https://api-sandbox.dwolla.com/customers/c7f300c0-f1ef-4151-9bbe-005005aa3747
```

```ruby
request_body = {
  :firstName => 'Jane',
  :lastName => 'Merchant',
  :email => 'jmerchant@nomail.net',
  :type => 'receive-only',
  :ipAddress => '99.99.99.99'
}

# Using DwollaV2 - https://github.com/Dwolla/dwolla-v2-ruby (Recommended)
customer = app_token.post "customers", request_body
customer.response_headers[:location] # => "https://api-sandbox.dwolla.com/customers/c7f300c0-f1ef-4151-9bbe-005005aa3747"
```

```javascript
var requestBody = {
  firstName: 'Jane',
  lastName: 'Merchant',
  email: 'jmerchant@nomail.net',
  type: 'receive-only',
  ipAddress: '99.99.99.99'
};

appToken
  .post('customers', requestBody)
  .then(function(res) {
    res.headers.get('location'); // => 'https://api-sandbox.dwolla.com/customers/c7f300c0-f1ef-4151-9bbe-005005aa3747'
  });
```

```python
request_body = {
  'firstName': 'Jane',
  'lastName': 'Merchant',
  'email': 'jmerchant@nomail.net',
  'type': 'receive-only',
  'ipAddress': '99.99.99.99'
}

# Using dwollav2 - https://github.com/Dwolla/dwolla-v2-python (Recommended)
customer = app_token.post('customers', request_body)
customer.headers['location'] # => 'https://api-sandbox.dwolla.com/customers/c7f300c0-f1ef-4151-9bbe-005005aa3747'
```

```php
<?php
$customersApi = new DwollaSwagger\CustomersApi($apiClient);

$customer = $customersApi->create([
  'firstName' => 'Jane',
  'lastName' => 'Merchant',
  'email' => 'jmerchant@nomail.net',
  'type' => 'receive-only'
  'ipAddress' => '99.99.99.99'
]);

print($customer); # => "https://api-sandbox.dwolla.com/customers/c7f300c0-f1ef-4151-9bbe-005005aa3747"
?>
```

<ol class = "alerts">
    <li class="alert icon-alert-alert">
      Provide the IP address of the end-user accessing your application as the `ipAddress` parameter. This enhances fraud detection and tracking.
    </li>
</ol>

When the Customer is successfully created on your application, you will receive a `201` HTTP response with an empty response body. You can reference the Location header to retrieve a link that represents the created Customer resource. We recommend storing the full URL for future use, as it will be needed for actions such as attaching a bank or correlating webhooks that are triggered for the user in the Dwolla system.

## Step 1B. Handle Webhooks

If you have an active [webhook subscription](/guides/webhooks/create-subscription.html), you will receive the `customer_created` webhook immediately after the resource has been created.

<nav class="pager-nav">
    <a href="./">Back: Overview</a>
    <a href="add-funding-source.html">Next: Add funding source</a>
</nav>
