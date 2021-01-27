---
title: "Encrypt & Decrypt"
date: 2018-12-27T11:02:05+06:00
icon: "ti-lock"
description: "Generate an encryption key"
type : "docs"
weight: 5
---


#### Encryption

Each HandCash user-app relationship forms a unique keypair:
> User 1 + App 1 = Keypair 1 <br/> User 1 + App 2 = Keypair 2

<br/>
This can be used by the developer to encrypt data, without requiring any custody of the encryption key.

{{< notice note >}}
This feature requires the `Decrypt & Encrypt` permission. Otherwise you will receive an error.
{{</ notice >}}

#### User-to-App

User-to-App is an schema that allows both the user and the app to decrypt messages.

<br/>

The below snippet shows how to encrypt a message between the user and the app:

 {{% tabs %}}
   {{% tab "NodeJS" %}}
```javascript
const {HandCashaccount} = require('@handcash/handcash-connect');
const {PublicKey} = require('bsv');
const ECIES = require('bsv/ecies');
const account = handCashaccount.fromAuthToken(token);
const {publicKey} = await account.profile.getEncryptionKeypair();
const ecPublicKey = PublicKey.fromString(publicKey);
const plainText = 'hello!';
const encryptedBuffer = ECIES().publicKey(ecPublicKey).encrypt(plainText);
console.log(encryptedBuffer.toString('base64'));
```

**Output:**
```javascript
QklFMQPg/OQVAP3NgDAHicFFeXh5jGVVpBrCO811JgzH89c1NGhjPXQXg8hJnWolfhLZiKee91hqqXmazZC0luy3BaV4gL0r/o+yXfmU8583UfiYQA==
```
   {{% /tab %}}
{{% /tabs %}}

On the other hand, you may decrypt a message with the following:

 {{% tabs %}}
   {{% tab "NodeJS" %}}
```javascript
const {HandCashaccount, Environments} = require('@handcash/handcash-connect');
const account = handCashaccount.fromAuthToken(token);
const {PrivateKey} = require('bsv');
const ECIES = require('bsv/ecies');
const {privateKey} = await account.profile.getEncryptionKeypair();
const ecPrivateKey = PrivateKey.fromWIF(privateKey);
const encryptedBuffer = Buffer.from('QklFMQPg/OQVAP3NgDAHicFFeXh5jGVVpBrCO811JgzH89c1NGhjPXQXg8hJnWolfhLZiKee91hqqXmazZC0luy3BaV4gL0r/o+yXfmU8583UfiYQA==', 'base64');
const decryptedBuffer = ECIES().privateKey(ecPrivateKey).decrypt(encryptedBuffer);
console.log(decryptedBuffer.toString('utf8'));
```
**Output:**
```javascript
hello!
```
   {{% /tab %}}
{{% /tabs %}}
