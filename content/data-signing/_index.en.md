---
title: "Data Signing"
date: 2018-12-27T11:02:05+06:00
icon: "ti-pencil-alt"
description: "Publicly sign a transaction"
type : "docs"
weight: 7
---


#### Data Signing

HandCash users can sign transactions publicly.

<br/>

This can be used by the developer to issue a public record of the user's intent to publish.

{{< notice note >}}
This feature requires the `Data Signing` permission. Otherwise you will receive an error.
{{</ notice >}}

<hr>
<br>

#### Sign data

There are three different types of data you can sign:

{{< tabs >}}
    {{% tab "UTF-8" %}}

```javascript
"Sign this message!!!"
```
    {{%/ tab %}}
    {{% tab "Base64" %}}

```javascript
'IlNpZ24gdGhpcyBtZXNzYWdlISEhIgo='
```
    {{% /tab %}}
    {{% tab "HEX" %}}

```javascript
'225369676E2074686973206D65737361676521212122'
```
    {{%/ tab %}}
{{< /tabs >}}


Publicly sign a message using the code example below:

 {{< tabs >}}
   {{% tab "NodeJS" %}}

```javascript
const {HandCashConnect} = require('@handcash/handcash-connect');
const handCashConnect = new HandCashConnect('<app-id>'); 

const account =  handCashConnect.getAccountFromAuthToken(token);

const message = Buffer.from("Sign this message!!!").toString('hex')
const { publicKey, signature } = await account.profile.signData({
    value: message,
    format: 'hex'
})
```

   {{% /tab %}}
{{< /tabs >}}


Verify a public signature with the following:

 {{< tabs >}}
   {{% tab "NodeJS" %}}

```javascript
const { PublicKey } = require('bsv')
const Message = require('bsv/message');

const message = '225369676E2074686973206D65737361676521212122';
console.log(Buffer.from(message).toString())

const verified = Message.verify(Buffer.from(message, 'hex'), PublicKey(publicKey).toAddress(), signature);
console.log(verified);
```

{{< output >}}

```console
Sign this message!!!
true
```
   {{% /tab %}}
{{< /tabs >}}

#### Combine with a transaction. 

Attach data with signature to a transaction:


 {{< tabs >}}
   {{% tab "NodeJS" %}}
```javascript
const {HandCashConnect} = require('@handcash/handcash-connect');
const handCashConnect = new HandCashConnect('<app-id>'); 

const text = "Sign this message!!!"
const message = Buffer.from(text).toString('hex')

const { signature } = await account.profile.signData({
    value: message,
    format: 'hex'
})

const account = handCashConnect.getAccountFromAuthToken(token);
const paymentParameters = {
    appAction: "publish",
    payments: [
        { destination: 'nosetwo', currencyCode: 'USD', sendAmount: 0.25 },
    ],
    attachment: { 
        format: 'json', 
        value: {
            "message": message, 
            "signature": signature
        } 
    },
};

const paymentResult = await account.wallet.pay(paymentParameters);
console.log(paymentResult);
```

   {{% /tab %}}
{{< /tabs >}}



