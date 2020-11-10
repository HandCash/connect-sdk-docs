---
title: "Installation"
date: 2018-12-27T11:02:05+06:00
icon: "ti-panel"
description: "Integrate Connect with your app"
type : "docs"
weight: 1
---

#### Registration
To get started with Connect, you will need to register your app through the [HandCash developer dashboard](https://handcash-connect-dashboard.web.app/#/).

<br/>

#### Permissions

During the registration, you will be asked to select permissions that will be required by your app. Below are all the available permissions:

{{% tabs %}}
    {{% tab "Pay" %}}
>Grants access to make payments on the user's behalf
    {{% /tab %}}
    {{% tab "Public Profile" %}}
>Grants access to the user's public profile: 'handle', 'displayName' and 'avatarUrl'.            
    {{%/ tab %}}
    {{% tab "Private Profile" %}}
>Grants access to the user's private profile: 'email' and 'phoneNumber'.            
    {{%/ tab %}}
    {{% tab "Friends" %}}
>Grants access to view the user's friends list.           
    {{%/ tab %}}
    {{% tab "Decrypt & Encrypt" %}}
>Grants access to the encryption/decryption keypair.            
    {{%/ tab %}}
{{% /tabs %}}


{{< notice note >}}
 `PublicProfile` is enabled by default for all applications.
{{</ notice >}}

#### Install & Initialize the SDK

Once registered, install the SDK module to your project:
 {{% tabs %}}
   {{% tab "NodeJS" %}}
```bash
npm install --save @handcash/handcash-connect-beta
```
  {{% /tab %}}
{{% /tabs %}}


Then, import the module:
 {{% tabs %}}
   {{% tab "NodeJS" %}}
```javascript
const { HandCashCloudAccount } = require('@handcash/handcash-connect-beta');
```
  {{% /tab %}}
{{% /tabs %}}



{{< notice note >}}
To interact with Connect, you will need to create an instance of the `HandCashCloudAccount`. This must be initialized using an **authToken**, which represents the authorization granted by the user to your application.
{{</ notice >}}

Learn how to obtain the user's **authToken** in the next section.


