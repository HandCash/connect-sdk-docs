---
title: "Installation"
date: 2018-12-27T11:02:05+06:00
icon: "ti-panel"
description: "Integrate Connect with your app"
type : "docs"
weight: 1
---

#### Registration

To get started with Connect, you will need to register your app through the HandCash [developer dashboard](https://dashboard.handcash.dev).


<hr>
<br>

#### Permissions

During the registration, you will be asked to select permissions that will be required by your app. Below are all the available permissions:

</br>

{{< permission label="Pay" description="Process user payments" img="credit-card-outline.png">}}

{{< permission label="Public Profile" description="View the user's public profile: 'handle', 'displayName', and 'avatarUrl'." img="account.png">}}

{{< permission label="Private Profile" description="Connect with the user's private profile: 'email' and 'phoneNumber'. " img="shield-account-variant.png">}}

{{< permission label="Friends" description="Fetch the user's friends list." img="account-multiple.png">}}

{{< permission label="Decrypt & Encrypt" description="Generate an encryption/decryption keypair." img="key.png">}}

{{< notice note >}}
 `PublicProfile` is enabled by default for all applications.
{{</ notice >}}


<hr>
<br>

#### Initialize the SDK
Install the SDK module to your project:

 {{< tabs >}}
   {{% tab "NodeJS" %}}
```bash
npm install --save @handcash/handcash-connect
```
  {{% /tab %}}
{{< /tabs >}}
<hr>
</br>

Import the module and initialize:

 {{< tabs >}}
   {{% tab "NodeJS" %}}
```javascript
const {HandCashConnect} = require('@handcash/handcash-connect');
const handCashConnect = new HandCashConnect('<app-id>');
```
  {{% /tab %}}
{{< /tabs >}}
<hr>
</br>

Generate the oauth url:

 {{< tabs >}}
   {{% tab "NodeJS" %}}
```javascript
const redirectionLoginUrl =  handCashConnect.getRedirectionUrl();
```
  {{% /tab %}}
{{< /tabs >}}
<hr>
</br>

Initialize the user's cloud account:

 {{< tabs >}}
   {{% tab "NodeJS" %}}
```javascript
const account = handCashConnect.getAccountFromAuthToken(token);
```
  {{% /tab %}}
{{< /tabs >}}

{{< notice note >}}
This must be initialized using the **authToken** from the earlier authorization. See our [authorization page](../authorization) for a full walkthrough.
{{</ notice >}}

<hr>
</br>

View user profile:

{{< tabs >}}
   {{% tab "NodeJS" %}}
```javascript
const { publicProfile } = await account.profile.getCurrentProfile();

console.log(`Hey ${publicProfile.handle}, welcome to HandCash Connect!`);
```
  {{% /tab %}}
{{< /tabs >}}

<hr>
</br>

Make a payment:
{{< tabs >}}

 {{% tab "NodeJS" %}}

  ```javascript
  const payParameters = {
      payments: [{ destination: 'nosetwo', currencyCode: 'DUR', sendAmount: 25 }]
  };    
    
  await account.wallet.pay(payParameters);
  ```

 {{% /tab %}}

{{< /tabs >}}



{{< notice tip >}}
[Explore our documentation](../) for more features!
{{</ notice >}}

</br>

In the next section, we'll be prompting the user for authorization.