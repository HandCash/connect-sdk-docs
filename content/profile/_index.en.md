---
title: "Profile"
date: 2018-12-27T11:02:05+06:00
icon: "ti-user"
description: "View user profile details"
type : "docs"
weight: 3
---

#### User Public & Private Profile

HandCash Connect provides default access to the user's `public profile`. The user's `private profile` can also be accessed, given that those permissions are granted.

<br/>

The `profile.getCurrentProfile` function will return both objects if applicable, again depending on the permissions granted by the user. 

 {{% tabs %}}
   {{% tab "NodeJS" %}}

```javascript
const {HandCashConnect} = require('@handcash/handcash-connect');
const handCashConnect = new HandCashConnect('<app-id>'); 
const account = await handCashConnect.getAccountFromAuthToken(token);
const currentProfile = await account.profile.getCurrentProfile();
const {publicProfile, privateProfile} = await account.profile.getCurrentProfile();  
// Log the ouput
console.log(currentProfile);
console.log(publicProfile);
console.log(privateProfile);
```

**Output:**
```javascript
// Full profile
{
   publicProfile: {
      id: "5f15c31c3c177d003028eb97",
      handle: "stuk_91",
      paymail: "BrandonIAE@internal.handcash.io",
      displayName: "Steven Urban K.",
      avatarUrl: "https://handcash.io/avatar/7d399a0c-22cf-40cf-b162-f5511a4645db",
      localCurrencyCode: "USD",
   },
   privateProfile: { 
      phoneNumber: "+11234567891",
      email: "StevenUrban1234@gmail.com" 
   }
}
// Public profile 
{
   id: "5f15c31c3c177d003028eb97",
   handle: "stuk_91",
   paymail: "BrandonIAE@internal.handcash.io",
   displayName: "Steven Urban K.",
   avatarUrl: "https://handcash.io/avatar/7d399a0c-22cf-40cf-b162-f5511a4645db",
   localCurrencyCode: "USD",
},
// Private profile 
{ 
   phoneNumber: "+11234567891",
   email: "StevenUrban1234@gmail.com" 
}

```
   {{% /tab %}}
{{% /tabs %}}

#### Spendable Balance

You may fetch the user's spendable balance by using the `getSpendableBalance` function. 

 {{% tabs %}}
   {{% tab "NodeJS" %}}
```javascript
const {HandCashConnect} = require('@handcash/handcash-connect');
const handCashConnect = new HandCashConnect('<app-id>'); 
const account = await handCashConnect.getAccountFromAuthToken(token);
var balance = await account.wallet.getSpendableBalance();
console.log(balance);
```

**Output:**
```javascript
{
  spendableSatoshiBalance: 1260000,
  spendableFiatBalance: 2.96,
  currencyCode: 'CAD'
}
```
   {{% /tab %}}
{{% /tabs %}}


{{< notice note >}}
This feature requires the `Pay` permission. Otherwise you will receive an error.
{{</ notice >}}

You can also customize the currency conversion options by passing the currency code through the function (if not provided, it will return the user's prefered currency by default).
 {{% tabs %}}
   {{% tab "NodeJS" %}}
```javascript
var balance = await account.wallet.getSpendableBalance("USD");
console.log(balance);
```

**Output:**
```javascript
{
  spendableSatoshiBalance: 1260000,
  spendableFiatBalance: 2.03,
  currencyCode: 'USD'
}

```
   {{% /tab %}}
{{% /tabs %}}

#### Friends

Return a list of friends, each including their own public profile.


 {{% tabs %}}
   {{% tab "NodeJS" %}}


```javascript
const {HandCashConnect} = require('@handcash/handcash-connect');
const handCashConnect = new HandCashConnect('<app-id>'); 
const account = await handCashConnect.getAccountFromAuthToken(token);
const friends = await account.profile.getFriends();
console.log(friends);
```

**Output:**
```javascript
[
  {
    id: '5fa2d0ebab7c740c9e7b3ecb',
    handle: 'nosetwo',
    paymail: 'nosetwo@internal.handcash.io',
    displayName: 'Nose two',
    avatarUrl: 'https://res.cloudinary.com/hk7jbd3jh/image/upload/v1574787300/gntqxv6ed7sacwpfwumj.jpg',
    localCurrencyCode: 'EUR'
  },
  {
    id: '5f64dfbd7549610022d2861b',
    handle: 'rjseibane',
    paymail: 'rjseibane@internal.handcash.io',
    displayName: 'Rafa JS',
    avatarUrl: 'https://res.cloudinary.com/hk7jbd3jh/image/upload/v1584356800/hprcfwdasenpnrqei3uz.jpg',
    localCurrencyCode: 'USD'
  }
]
```
   {{% /tab %}}
{{% /tabs %}}


{{< notice note >}}
This feature requires the `Friends` permission. Otherwise you will receive an error.
{{</ notice >}}