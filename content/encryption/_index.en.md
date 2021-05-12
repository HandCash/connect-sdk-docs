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

<hr>
<br>

#### User-to-App

User-to-App is an schema that allows both the user and the app to decrypt messages.

<br/>

The below snippet shows how to encrypt and decrypt a message with the user encryption key:

 {{< tabs >}}
   {{% tab "NodeJS" %}}
```javascript
const ECIES = require('electrum-ecies');
const { HandCashConnect } = require('@handcash/handcash-connect');
const accessKey = '';
const handcashConnect = new HandCashConnect('<app-id>');

(async () => {
   const cloudAccount = handcashConnect.getAccountFromAuthToken(accessKey);
   const { publicKey } = await cloudAccount.profile.getEncryptionKeypair();
   const { privateKey } = await cloudAccount.profile.getEncryptionKeypair();
   const encryption = ECIES.encrypt('Hello', publicKey);
   const decryptedResult = ECIES.decrypt(encryption, privateKey);
   console.log(decryptedResult.toString());
})();
```

{{< output >}}

```javascript
Hello
```
   {{% /tab %}}
{{< /tabs >}}
