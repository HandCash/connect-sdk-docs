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

 {{< tabs >}}
   {{% tab "NodeJS" %}}

```javascript
const {HandCashConnect} = require('@handcash/handcash-connect');
const handCashConnect = new HandCashConnect('<app-id>'); 

const account = handCashConnect.getAccountFromAuthToken(token);

// Fetch and log the entire profile
const currentProfile = await account.profile.getCurrentProfile();
console.log(currentProfile);

// Extract public or private profile from currentProfile;
const {publicProfile, privateProfile} = currentProfile;
console.log(publicProfile);
console.log(privateProfile);
```

{{< output >}}

```javascript
// Full profile
{
   publicProfile: {
      id: "5f15c31c3c177d003028eb97",
      handle: "stuk_91",
      paymail: "BrandonC@handcash.io",
      bitcoinUnit: "DUR",
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
   paymail: "BrandonC@handcash.io",
   bitcoinUnit: "DUR",
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
{{< /tabs >}}



{{< notice note >}}
The field `bitcoinUnit` will denote whether the user has duro-mode enabled or not. Possible values are `DUR` and `SAT`.<br>
**Read more about duro [here](../duro).**
{{</ notice >}}

<hr>
<br>



#### Fetch Public Profiles of other users

Public user data can also be fetched using the function `account.profile.getPublicProfilesByHandle()`.


 {{< tabs >}}
   {{% tab "NodeJS" %}}

```javascript
const {HandCashConnect} = require('@handcash/handcash-connect');
const handCashConnect = new HandCashConnect('<app-id>'); 

const account = handCashConnect.getAccountFromAuthToken(token);

// Fetch and log user profiles by handle
const userProfiles = await account.profile.getPublicProfilesByHandle(['cryptokang', 'eyeone']);
console.log(currentProfile);
```

{{< output >}}

```javascript
[
  {
      id: "5f15c31c3c177d003028eb97",
      handle: "cryptokang",
      paymail: "cryptokang@handcash.io",
      bitcoinUnit: "DUR",
      displayName: "Brandon",
      avatarUrl: "https://handcash.io/avatar/7d399a0c-22cf-40cf-b162-f5511a4645db",
      localCurrencyCode: "USD",
   },
  {
      id: "5f14c41c3c188d003027eb77",
      handle: "eyeone",
      paymail: "eyeone@handcash.io",
      bitcoinUnit: "SAT",
      displayName: "Ivan",
      avatarUrl: "https://handcash.io/avatar/7d399a0c-22cf-40cf-b162-f5511a4645db",
      localCurrencyCode: "USD",
   },
]

```
   {{% /tab %}}
{{< /tabs >}}




<hr>
<br>



#### Spendable Balance


{{< notice note >}}
This feature requires the **`Pay`** permission. 
{{</ notice >}}

You can fetch the user's **spendable balance** with the `getSpendableBalance` function. 

 {{< tabs >}}
   {{% tab "NodeJS" %}}
```javascript
const {HandCashConnect} = require('@handcash/handcash-connect');
const handCashConnect = new HandCashConnect('<app-id>'); 

const account = handCashConnect.getAccountFromAuthToken(token);
var balance = await account.wallet.getSpendableBalance();

console.log(balance);
```

{{< output >}}

```javascript
{
  spendableSatoshiBalance: 1260000,
  spendableFiatBalance: 2.96,
  currencyCode: 'CAD'
}
```
   {{% /tab %}}
{{< /tabs >}}

{{< notice info >}}
HandCash users have a global limit for how much they're willing to spend on apps, **'spendable balance'** is the amount left before reaching that limit:
</br></br>
![connect-limit](connect-limit.png)
*The default limit is `10USD`.*
{{</ notice >}}

You can also customize the currency conversion options by passing the currency code through the function (if not provided, it will return the user's prefered currency by default).
 {{< tabs >}}
   {{% tab "NodeJS" %}}
```javascript
var balance = await account.wallet.getSpendableBalance("USD");

console.log(balance);
```

{{< output >}}

```javascript
{
  spendableSatoshiBalance: 1260000,
  spendableFiatBalance: 2.03,
  currencyCode: 'USD'
}
```
   {{% /tab %}}
{{< /tabs >}}

<hr>
<br>

#### Friends

{{< notice note >}}
This feature requires the `Friends` permission. Otherwise you will receive an error.
{{</ notice >}}

Return a list of friends, each including their own public profile.


 {{< tabs >}}
   {{% tab "NodeJS" %}}


```javascript
const {HandCashConnect} = require('@handcash/handcash-connect');
const handCashConnect = new HandCashConnect('<app-id>'); 

const account = handCashConnect.getAccountFromAuthToken(token);
const friends = await account.profile.getFriends();

console.log(friends);
```

{{< output >}}

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
{{< /tabs >}}
