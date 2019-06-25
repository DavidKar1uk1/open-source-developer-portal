---
layout: twoColumn
section: guides
type: guide
guide:
    name: me-to-me
    step: overview
title:  Transfer Money Between A Single User’s Accounts
description: Move funds between two different bank accounts belonging to a single verified Customer.
---
# Me-to-Me

## Overview

This guide is designed to get you up and running quickly through creating a bank to bank transfer between a verified Customer’s two bank accounts. In this guide we’ll cover the basics of integrating this payment flow by walking through the steps needed to onboard your end-user as a verified Customer and create the bank to bank transfer.

<img src="/images/funds_flow_me-to-me.gif" width="75%" height="75%">

In this quickstart guide, you’ll learn the key concepts involved with sending money between a Customer’s bank accounts.

1. Choosing a Customer type and creating a verified Customer record
2. Attaching a `source` and `destination` funding source (bank account) to the Customer record
3. Fetching the available funding sources
4. Sending funds from the Customer’s checking account to the Dwolla Balance
5. Sending funds from the Dwolla Balance to the Customer’s savings account

## Before you begin

We encourage you to create a sandbox account, if you haven’t already. This will allow you to follow along with the steps outlined in this guide. [Check out our Sandbox setup guide to learn more](https://developers.dwolla.com/guides/sandbox-setup/).

After creating a sandbox account, you’ll obtain your API Key and Secret, which are used to obtain an OAuth access token. An access token is required in order to authenticate against the Dwolla API. Learn more about how to [obtain an access token in our guide](https://developers.dwolla.com/guides/auth/).

Lastly, in this sandbox walkthrough, we recommend having an active webhook subscription. This will help notify your application of various events that occur within Dwolla. [Check out our guide to learn more](https://developers.dwolla.com/guides/webhooks/).

**Let’s get started!**

<nav class="pager-nav">
    <a href="" style="display:none;"></a>
    <a href="onboarding.html">Next: Customer onboarding</a>
</nav>
