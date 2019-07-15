---
layout: twoColumn
section: Dwolla.js
type: article
title:  Add a Bank Account
weight: 0
description: Using dwolla.js, securely transmit sensitive data (bank account and routing number) to Dwolla without the data passing through your server.
---

# Add a bank funding source

## Dwolla.js to create a funding source

Using dwolla.js, securely transmit sensitive data (bank account and routing number) to Dwolla without the data passing through your server. This resource guide will walk you through the process of generating a funding sources token, collecting the user's bank account information, and calling a JavaScript function to send this data to Dwolla. When you collect and submit the user's bank account information, dwolla.js has built-in validation that will trigger an error if any of the required fields are invalid.

### dwolla.fundingSources.create()

Param | Type | Value
----------|-------------|-------------
funding-sources-token | string | A funding sources token generated on your server.
params | object | An object containing params to create a funding source. Contains keys: `routingNumber`, `accountNumber`, `type`, and `name`. See example below. <br> `routingNumber` represents a string value nine digit routing number. <br> `accountNumber` represents a string value account number. <br> `type` represents a string value of either `checking` or `savings`. <br> `name` represents a string value identifying name of the user's bank.  
callback | function | A callback function that handles the response from Dwolla.

#### Example

```javascriptnoselect
dwolla.fundingSources.create('1zN400zyPUobbdmeNfhTGH2Jh5JkFREJa9YBI8SLXp0ERXNTMT', {
  routingNumber: getVal('routingNumber'),
  accountNumber: getVal('accountNumber'),
  type: getVal('type'),
  name: getVal('name')
}, function(err, res) {
  console.log('Error: ' + JSON.stringify(err) + ' -- Response: ' + JSON.stringify(res));
});
```

### Step 1: Generate a funding sources token
Before calling a function within dwolla.js to add a new funding source, you need to generate a funding sources token. Your server initiates a POST request to Dwolla, specifying for which Dwolla account or Dwolla API Customer you want to add a bank account. Dwolla will respond with a funding sources `token` that expires in an hour. This token will be sent to the client and used to authenticate the HTTP request asking Dwolla to add a new funding source.

```raw
curl -X POST
\ -H "Content-Type: application/vnd.dwolla.v1.hal+json"
\ -H "Accept: application/vnd.dwolla.v1.hal+json"
\ -H "Authorization: Bearer qe634nV7dIYpYDf3VGZPciziPU2BCboUZ7G7EG8XEyGswKkBV5"
\ "https://api-sandbox.dwolla.com/customers/28138609-30FF-4607-B28C-4A3872F8FD4A/funding-sources-token"

HTTP/1.1 200 OK
{
  "_links": {
    "self": {
      "href": "https://api-sandbox.dwolla.com/customers/28138609-30ff-4607-b28c-4a3872f8fd4a/funding-sources-token"
    }
  },
  "token": "Z9BvpNuSrsI7Ke1mcGmTT0EpwW34GSmDaYP09frCpeWdq46JUg"
}
```
```ruby
customer_url = 'https://api-sandbox.dwolla.com/customers/AB443D36-3757-44C1-A1B4-29727FB3111C'

# Using DwollaV2 - https://github.com/Dwolla/dwolla-v2-ruby (Recommended)
customer = app_token.post "#{customer_url}/funding-sources-token"
customer.token # => "Z9BvpNuSrsI7Ke1mcGmTT0EpwW34GSmDaYP09frCpeWdq46JUg"
```
```javascript
// Using dwolla-v2 - https://github.com/Dwolla/dwolla-v2-node
var customerUrl = 'https://api-sandbox.dwolla.com/customers/28138609-30ff-4607-b28c-4a3872f8fd4a';

appToken
  .post(`${customerUrl}/funding-sources-token`)
  .then(function(res) {
    res.body.token; // => 'Z9BvpNuSrsI7Ke1mcGmTT0EpwW34GSmDaYP09frCpeWdq46JUg'
  });
```
```python
customer_url = 'https://api.dwolla.com/customers/28138609-30ff-4607-b28c-4a3872f8fd4a'
customers_api = dwollaswagger.CustomersApi(client)

token = customers_api.create_funding_sources_token_for_customer(customer_url)
print token['token'] # => 'Z9BvpNuSrsI7Ke1mcGmTT0EpwW34GSmDaYP09frCpeWdq46JUg'
```
```php
<?php
$customersApi = new DwollaSwagger\CustomersApi($apiClient);

$fsToken = $customersApi->createFundingSourcesTokenForCustomer("https://api-sandbox.dwolla.com/customers/28138609-30ff-4607-b28c-4a3872f8fd4a");
$fsToken->token; # => "Z9BvpNuSrsI7Ke1mcGmTT0EpwW34GSmDaYP09frCpeWdq46JUg"
?>
```

### Step 2: Add form to collect bank account information
You'll then add a form to the body of the page where you want to collect the user's bank account information.

```htmlnoselect
<form>
  <div>
    <label>Routing number</label>
    <input type="text" id="routingNumber" placeholder="273222226" />
  </div>
  <div>
    <label>Account number</label>
    <input type="text" id="accountNumber" placeholder="Account number" />
  </div>
  <div>
    <label>Bank account name</label>
    <input type="text" id="name" placeholder="Name" />
  </div>
  <div>
    <select name="type" id="type">
      <option value="checking">Checking</option>
      <option value="savings">Savings</option>
    </select>
  </div>
  <div>
    <input type="submit" value="Add Bank">
  </div>
</form>

<div id="logs">
</div>
```


### Step 3: Configure and call JavaScript function to create a new funding source
Assuming `dwolla.js` is already included and configured on your page, you will create the function that will call `dwolla.fundingSources.create()`. For this example, jQuery is used to call the function that creates a funding source when the user clicks the "Add Bank" button on your page. To test in the sandbox environment, use the `dwolla.configure()` helper function and pass in the value of `sandbox`.

In our example, `dwolla.fundingSources.create()` takes three arguments: a string value of the funding sources token, JavaScript object containing bank account information entered by the user, and a callback function that will handle any error or response.

```javascriptnoselect
$('form').on('submit', function() {
  dwolla.configure('sandbox');
  var token = 'Z9BvpNuSrsI7Ke1mcGmTT0EpwW34GSmDaYP09frCpeWdq46JUg';
  var bankInfo = {
    routingNumber: $('routingNumber').val(),
    accountNumber: $('accountNumber').val(),
    type: $('type').val(),
    name: $('name').val()
  }
  dwolla.fundingSources.create(token, bankInfo, callback);
  return false;
});

function callback(err, res) {
  var $div = $('<div />');
  var logValue = {
    error: err,
    response: res
  };
  $div.text(JSON.stringify(logValue));
  console.log(logValue);
  $('#logs').append($div);
}
```

### Step 4: Callback function - handling the error or response
The callback function (err, res) allows you to determine if there is an error with the request (e.g. `Routing number invalid.`) or if the response was successful. In the example above, we are displaying any error or response within a `logs` container on our page.

* If there is an error: Display the error to the user to have them correct any fields, and have them re-attempt to add their bank.
  * Example: `{"error":{"code":"ValidationError","message":"Validation error(s) present. See embedded errors list for more details.","_embedded":{"errors":[{"code":"Invalid","message":"Routing number invalid.","path":"/routingNumber"}]}}}`
* If successful: You will receive a JSON response that includes a link to the newly attached funding source.
  * Example:  `{"error":null,"response":{"_links":{"funding-source":{"href":"https://api-sandbox.dwolla.com/funding-sources/746d5c93-acb9-4826-a9c1-78ecf16401a6"}}}}`

* * *

#### View:

*   [Dwolla.js - Overview](/resources/dwolla-js.html)
*   [Add + verify a bank account (IAV)](/resources/dwolla-js/instant-account-verification.html)
*   [On-demand bank transfers](/resources/dwolla-js/on-demand-bank-transfers.html)
