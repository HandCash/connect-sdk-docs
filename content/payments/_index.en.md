---
title: "Payments"
date: 2018-12-27T11:02:05+06:00
icon: "ti-money"
description: "Execute instant payments"
type : "docs"
weight: 4
---

#### Simple Payments


HandCash Connect enables you to construct and execute transactions on behalf of your connected users. This can be done by passing **payment parameters** through the `wallet.pay` function:

 {{% tabs %}}
   {{% tab "NodeJS" %}}

```javascript
const {HandCashConnect} = require('@handcash/handcash-connect');
const handCashConnect = new HandCashConnect('<app-id>'); 
const account = await handCashConnect.getAccountFromAuthToken(token);
const paymentParameters = {
    description: "Hold my beer!🍺",
    appAction: "like",
    payments: [
        { destination: 'nosetwo', currencyCode: 'USD', sendAmount: 0.25 },
    ]
};
const paymentResult = await account.wallet.pay(paymentParameters);
console.log(paymentResult);
```

**Output:**

```javascript
{
  transactionId: '4c7b7cdc18702bb1a09c75a47bc2fa9630545761fbbd53b8c38735c73173e043',
  note: 'Hold my beer!🍺',
  type: 'send',
  time: 1604958667,
  satoshiFees: 113,
  satoshiAmount: 15555,
  fiatExchangeRate: 160.74284545024352,
  fiatCurrencyCode: 'USD',
  participants: [
    {
      type: 'user',
      alias: 'nosetwo',
      displayName: 'Nose two',
      profilePictureUrl: 'https://res.cloudinary.com/hk7jbd3jh/image/upload/v1574787300/gntqxv6ed7sacwpfwumj.jpg',
      responseNote: ''
    }
  ],
  attachments: [],
  appAction: 'like'
}
```
   {{% /tab %}}
{{% /tabs %}}

{{< notice info >}}
**appAction** is used for transaction labeling and notification grouping. This way, HandCash can display more insightful information to users.
{{</ notice >}}

#### Recipient Types
HandCash Connect supports three different recipient types:

{{% tabs %}}
    {{% tab "Handles" %}}
>Example: `eyeone` <br/> Recommended for sending money to another HandCash user. 
    {{% /tab %}}
    {{% tab "Paymails" %}}
>Example: `name@moneybutton.io`, `name@relayx.io`, `etc...` <br/>  Recommended for sending money to another services.     
    {{%/ tab %}}
    {{% tab "P2PKH Addresses" %}}
>Example: `1BvBMSEYstWetqTFn5Au4m4GFg7xJaNVN2` <br/> Recommended for sending money to a custom addresses. 
    {{%/ tab %}}
{{% /tabs %}}

#### Pay to multiple people

The Connect SDK allows up to `200` recipients per payment. To send to multiple people, add more recipients to the `payments` array while constructing your app parameters:

 {{% tabs %}}
   {{% tab "NodeJS" %}}

```javascript
const paymentParameters = {
    description: "Hold my beer!🍺",
    appAction: "like",
    payments: [
        { to: 'eyeone', currencyCode: 'USD', amount: 0.25 },
        { to: 'eyeone@moneybutton.com', currencyCode: 'EUR', amount: 0.25 },
        { to: '131xrWSKXHbhucFPTfZqnxF8ZhjpMxJH7K', currencyCode: 'SAT', amount: 50000 },
    ]
};
```
   {{% /tab %}}
{{% /tabs %}}

#### Video Walkthroug: Simple Payments

Below is a video walkthrough illustrating a demo implementation of the simple payments and multi-recipient payments:

 {{% tabs %}}
   {{% tab "NodeJS" %}}
{{< youtube ZRrJZq2HfIw >}}
   {{% /tab %}}
{{% /tabs %}}

#### Attach Data

With the SDK, you can alse attach public data to payments. This data is uploaded to a public and immutable blockchain, meaning it cannot be modified or deleted. This can be used as a data timestamping service, and can lend transparency to your platform.

<br/>

You may attach data in the following formats: 


{{% tabs %}}
    {{% tab "Base64" %}}

```javascript
attachment: { format: 'base64', value: 'ABEiM0RVZneImQCqu8zd7v8=' }
```
    {{% /tab %}}
    {{% tab "HEX" %}}

```javascript
attachment: { format: 'hex', value: '0011223344556677889900AABBCCDDEEFF' }
``` 
    {{%/ tab %}}
    {{% tab "JSON" %}}

```javascript
attachment: { format: 'json', value: {"param1": "value1", "param2": "value2"} }
```
    {{%/ tab %}}
{{% /tabs %}}

To attach data, add an `attachment` field to your **payment parameters**:


 {{% tabs %}}
   {{% tab "NodeJS" %}}
```javascript
const {HandCashConnect} = require('@handcash/handcash-connect');
const handCashConnect = new HandCashConnect('<app-id>'); 
const account = await handCashConnect.getAccountFromAuthToken(token);
const paymentParameters = {
    description: "Hold my beer!🍺",
    appAction: "like",
    payments: [
        { destination: 'nosetwo', currencyCode: 'USD', sendAmount: 0.25 },
    ]
    attachment: { format: 'json', value: {"param1": "value1", "param2": "value2"} },
};
const paymentResult = await account.wallet.pay(paymentParameters);
console.log(paymentResult);
```

**Output:**

```javascript
{
  transactionId: '4c7b7cdc18702bb1a09c75a47bc2fa9630545761fbbd53b8c38735c73173e043',
  note: 'Hold my beer!🍺',
  type: 'send',
  time: 1604958667,
  satoshiFees: 113,
  satoshiAmount: 15555,
  fiatExchangeRate: 160.74284545024352,
  fiatCurrencyCode: 'USD',
  participants: [
    {
      type: 'user',
      alias: 'nosetwo',
      displayName: 'Nose two',
      profilePictureUrl: 'https://res.cloudinary.com/hk7jbd3jh/image/upload/v1574787300/gntqxv6ed7sacwpfwumj.jpg',
      responseNote: ''
    }
  ],
  attachments: [ { value: [Object], format: 'json' } ],
  appAction: 'like'
}
```

    {{%/ tab %}}
{{% /tabs %}}

#### Fetch a Payment

You may use the SDK to fetch information about a transaction, using the **transaction ID** as reference.

 {{% tabs %}}
   {{% tab "NodeJS" %}}
```javascript
const paymentResult = await account.wallet.getPayment("4c7b7cdc18702bb1a09c75a47bc2fa9630545761fbbd53b8c38735c73173e043")
console.log(paymentResult)
```

**Output:**

```javascript
{
  transactionId: '4c7b7cdc18702bb1a09c75a47bc2fa9630545761fbbd53b8c38735c73173e043',
  note: 'Hold my beer!🍺',
  type: 'send',
  time: 1604958667,
  satoshiFees: 113,
  satoshiAmount: 15555,
  fiatExchangeRate: 160.74284545024352,
  fiatCurrencyCode: 'USD',
  participants: [
    {
      type: 'user',
      alias: 'nosetwo',
      displayName: 'Nose two',
      profilePictureUrl: 'https://res.cloudinary.com/hk7jbd3jh/image/upload/v1574787300/gntqxv6ed7sacwpfwumj.jpg',
      responseNote: ''
    }
  ],
  attachments: [ { value: [Object], format: 'json' } ],
  appAction: 'like'
}
```
    {{%/ tab %}}
{{% /tabs %}}

#### Video Walkthrough: Transactions with Data

Below is a video walkthrough illustrating a demo implementation of transactions with data attached:

 {{% tabs %}}
   {{% tab "NodeJS" %}}
{{< youtube CGiZc2kSDmk >}}
   {{% /tab %}}
{{% /tabs %}}

#### Supported Currencies

Connect supports the following currencies conversions:

<br/>

| Currency Code | Currency Name        |
| ------------- | -------------------- |
| ARS           | Argentinian Peso     |
| AUD           | Australian Dollar    |
| BRL           | Brazilian Real       |
| CAD           | Canadian Dollar      |
| CHF           | Swiss Franc          |
| CNY           | Chinese Yuan         |
| COP           | Colombian Peso       |
| CZK           | Czech Koruna         |
| DKK           | Danish Krone         |
| EUR           | Eurozone Euro        |
| GBP           | British Pound        |
| HKD           | Hong Kong Dollar     |
| JPY           | Japanese Yen         |
| KRW           | Korean Won           |
| MXN           | Mexican Peso         |
| NOK           | Norwegian Krone      |
| NZD           | New Zealand Dollar   |
| PHP           | Philippine Peso      |
| RUB           | Russian Ruble        |
| SEK           | Swedish Krona        |
| SGD           | Singapore Dollar     |
| THB           | Thai Baht            |
| USD           | United States Dollar |
| ZAR           | South African Rand   |

<br/>

Additionally, Connect supports the following BitCoin SV currencies:

<br/>

| Currency Code     | Currency Name | Description                    |
| ----------------- | ------------- | ------------------------------ |
| SAT               | satoshis      | Indivisible unit               |
| BSV               | bitcoins      | 1 bitcoin = 100000000 satoshis |
