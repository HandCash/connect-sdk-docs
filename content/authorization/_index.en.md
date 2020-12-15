---
title: "Authorization"
date: 2018-12-28T11:02:05+06:00
icon: "ti-link"
description: "Connect users to your app"
type : "docs"
weight: 2
---

#### Overview

Below are the steps each user will need to follow in order to connect with your app:

<br/>

![HandCash Authorization Flow](handcash-connect-auth-flow.png)


1. The user will click a **redirection url** from within your app, redirecting them to the HandCash web app, or the HandCash native app.
{{< notice tip >}}
To generate the **redirection url**, your **App ID** will be required. This will be provided to you when your app is registered to HandCash.
{{</ notice >}} 
2. The user will sign in to HandCash. If they don't have an account already, they will be prompted to register.
3. HandCash will ask the user if they would like to grant permissions to your app.
4. The user can **accept** or **decline** access to the your app.

<br/>

>Accept  →  Authorization Success URL. 
>
>Decline  →  Authorization Failed URL.

<br/>


5. HandCash will then redirect the user back to the your app.
{{< notice info >}}
While redirecting, an **authToken** query parameter will be added to the request: <br/> `<auth-success-url>?authToken=<token>`
{{</ notice >}} 


#### Your App Authorization

To connect with a user, generate a **redirection url** using the SDK:
 {{% tabs %}}
   {{% tab "NodeJS" %}}
```javascript
const {HandCashConnect} = require('@handcash/handcash-connect');
const handCashConnect = new HandCashConnect('<app-id>');

// Use this field to redirect the user to the HandCash authorization screen.
const redirectionLoginUrl = await handCashConnect.getRedirectionUrl();
```
   {{% /tab %}}
{{% /tabs %}}


Once the user selects **accept** or **decline**, they will be redirected back your app's **Authorization Success URL** or **Authorization Failed URL**.

<br/>

While redirecting, an **authToken** query parameter will be added to the request: 
>`<auth-success-url>?authToken=<token>`


{{< notice tip >}}
Any extra query parameters provided will be added to the `<auth-success-url>`. <br/> Ex: `<auth-success-url>?authToken=<token>?`
{{</ notice >}} 


At this point, you may use the **authToken** to view and spend on behalf of the user.

<br/>

#### Video Walkthrough

Below is a video walkthrough illustrating a demo implementation of the authentication process:

 {{% tabs %}}
   {{% tab "NodeJS" %}}
{{< youtube Zk9-hqQFzic >}}
   {{% /tab %}}
{{% /tabs %}}
