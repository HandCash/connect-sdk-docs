---
title: "Examples"
date: 2018-12-27T11:02:05+06:00
icon: "ti-layout-list-thumb"
description: "Explore Connect in a real world scenario"
type : "docs"
weight: 10
---


#### Wrench App


Wrench App is the official sample app for HandCash Connect. It demonstrates each function of the SDK. All code can be found in the Github Page, and specific code implementations are provided below. 

</br>

Wrench app is live! So you can connect your handcash account and try it out here:
[Wrench App](https://wrenchapp.herokuapp.com)

</br>


{{< notice note >}}
The goal of these examples are to highlight how the SDK is used inside the controller functions in WrenchApp. There are imports, router functions, view rendering and an express server which are not shown here. </br></br> To get a complete view of the implementation please view the full source code.
{{</ notice >}}

#### Authentication

Supply the redirect url and register the auth token through the redirect endpoint. 

 {{< tabs >}}
   {{% tab "Supply Auth Redirect" %}}
```javascript
// login page
module.exports.getLoginLink = async (req, res, next) => {

  // fetch authentication url using the SDK
  const redirectUrl = await handCashConnect.getRedirectionUrl();
  
  // return page with a login button
  res.render('index', {
    redirectUrl: redirectUrl,
    docTitle: 'Login',
    path: '/'
  })
};
```
    {{%/ tab %}}
 {{< /tabs >}}
 {{< tabs >}}
   {{% tab "Save Authentication Token" %}}
```javascript
// authenticate
module.exports.getAuthenticate = async (req, res, next) => {

  // create a user upon a new login
  const authToken = req.query.authToken;

  // get user profile, and save alias to the user 
  const account = await handCashConnect.getAccountFromAuthToken(authToken);
  const { publicProfile } = await account.profile.getCurrentProfile()
  const handcashId = publicProfile.id

  // check if the user exists, if not create a new one
  let user = await User.findOne({handcashId: handcashId})
  if(!user){
    user = new User();
    user.handcashId = handcashId
  }

  // update authToken
  user.connectAuthToken = authToken
  
  // save user
  await user.save();

  // generating a jwt
  req.session.accessToken = user.generateAuthToken();
  res.redirect('/auth/dashboard'); 
};
```
    {{%/ tab %}}
{{< /tabs >}}

#### Profile

 {{< tabs >}}
   {{% tab "Display Profile Information" %}}
```javascript
// returns user's information
module.exports.getProfile = async (req, res) => {

  // fetch the authenticated user and their profile
  const user = await User.findById(req.user._id);
  const account = await handCashConnect.getAccountFromAuthToken(user.connectAuthToken);
  const { publicProfile, privateProfile } = await account.profile.getCurrentProfile();
  const spendableBalance = await account.wallet.getSpendableBalance()
  const permissions = await account.profile.getPermissions()
  // print it out

  // display public profile
  res.render('profile', {
    publicProfile: publicProfile,
    privateProfile: privateProfile,
    spendableBalance: spendableBalance,
    permissions: permissions,
    path: '/profile'
  }) 
}
```
    {{%/ tab %}}
{{< /tabs >}}

#### Friends

Fetch a list of friends

 {{< tabs >}}
   {{% tab "Display Friends List" %}}
```javascript
// returns user's information
module.exports.getFriends = async (req, res) => {

  // fetch the authenticated user and their profile
  const user = await User.findById(req.user._id);
  const account = await handCashConnect.getAccountFromAuthToken(user.connectAuthToken);
  const friends = await account.profile.getFriends()

  // print it out
  console.log(friends)

  // display public profile
  res.render('friends', {
    friends: friends,
    path: '/friends'
  }) 
}
```
    {{%/ tab %}}
{{< /tabs >}}


#### Payment

Sending a simple payment given a request with authenticated user, and transaction parametars.

 {{< tabs >}}
   {{% tab "Simple Payment" %}}
```javascript
// sends a transaction on behalf of the user
module.exports.sendTransaction = async (req, res, next) => {

    // fetch the authenticated user and their profile
    const user = await User.findById(req.user._id);
    const account = await handCashConnect.getAccountFromAuthToken(user.connectAuthToken);

    // define parameters 
    const handle = parseHandle(req.body.handle)
    const amount = parseInt(req.body.amount)
    const note = req.body.note
    const currencyCode = 'DUR'

    // construct the payment
    const paymentParameters = {
        description: note,
        payments:
        [
            {
            destination: handle,
            currencyCode: currencyCode,
            sendAmount: amount,
            },
        ],
    };

    // make the payment
    const payment = await account.wallet.pay(paymentParameters).catch(err => {console.log(err)})
    console.log(payment)

    // display public profile with the recent transaction
    res.redirect("/auth/get-transaction?txid=" + payment.transactionId)
}
```
    {{%/ tab %}}
{{< /tabs >}}


#### MultiPayment

Sending a multiparty payment given a request with authenticated user, and transaction parametars.

 {{< tabs >}}
   {{% tab "MultiParty Payment" %}}
```javascript
module.exports.sendMultisendTransaction = async (req, res, next) => {
  console.log("here")
  // fetch the authenticated user and their profile
  const user = await User.findById(req.user._id);
  const account = await handCashConnect.getAccountFromAuthToken(user.connectAuthToken);

  // define parameters 
  const handles = parseHandleArray(req.body.handles)
  const amount = parseInt(req.body.amount)
  const note = req.body.note
  const currencyCode = 'DUR'

  const payments = handles.map(handle => {return {
    destination: handle,
    currencyCode: currencyCode,
    sendAmount: amount
  }})

  console.log(payments)
  // configure the payment
  const paymentParameters = {
    description: note,
    appAction: "test-multi-send",
    payments: payments
  };

  // make the payment
  const payment = await account.wallet.pay(paymentParameters)
  console.log(payment)

  // display public profile with the recent transaction
  res.redirect("/auth/get-transaction?txid=" + payment.transactionId)
}
```
    {{%/ tab %}}
{{< /tabs >}}

#### Data

Sending a data transactino given a request with authenticated user, and some data.

 {{< tabs >}}
   {{% tab "Data Attachment" %}}
```javascript
// sends a transaction on behalf of the user
module.exports.sendDataTransaction = async (req, res, next) => {

  // fetch the authenticated user and their profile
  const user = await User.findById(req.user._id);
  const account = await handCashConnect.getAccountFromAuthToken(user.connectAuthToken);
  const { publicProfile } = await account.profile.getCurrentProfile()

  // define parameters 
  const handle = publicProfile.handle
  const amount = 500
  const note = 'Posting data to the chain'
  const data = ConvertStringToHex(req.body.text)
  console.log(data)
  const currencyCode = 'SAT'

  // construct the payment
  const paymentParameters = {
    description: note,
    payments:
      [
        {
          destination: handle,
          currencyCode: currencyCode,
          sendAmount: amount,
        },
      ],

    //attachment: { format: 'base64', value: 'ABEiM0RVZneImQCqu8zd7v8=' },
    attachment: { format: 'hex', value: data },
  };

  // make the payment
  const payment = await account.wallet.pay(paymentParameters).catch(err => console.log(err))

  // display public profile with the recent transaction
  res.redirect("/auth/get-transaction?txid=" + payment.transactionId)
}

```
    {{%/ tab %}}
{{< /tabs >}}

#### Encrypt

Encrypting data with user's encryption key

 {{< tabs >}}
   {{% tab "Encrypt Data" %}}
```javascript
// sends a transaction on behalf of the user
module.exports.postEncrypt = async (req, res, next) => {

  // fetch the authenticated user and their profile
  const user = await User.findById(req.user._id);
  const account = await handCashConnect.getAccountFromAuthToken(user.connectAuthToken);

  const { publicKey, privateKey } = await account.profile.getEncryptionKeypair();
  console.log(publicKey);

  const ecPrivateKey = PrivateKey.fromWIF(privateKey);
  const ecPublicKey = PublicKey.fromString(publicKey);
  const plainText = req.body.encryptText;

  const encryptedBuffer = ECIES().publicKey(ecPublicKey).encrypt(plainText);
  console.log(encryptedBuffer.toString('base64'));

  const decryptedBuffer = ECIES().privateKey(ecPrivateKey).decrypt(encryptedBuffer);
  console.log(decryptedBuffer.toString('utf8'));

  console.assert(decryptedBuffer.toString('utf8') == plainText);

  // display public profile with the recent transaction
  res.render('encryption', {
    encryptionDetails: {
      ecPrivateKey: ecPrivateKey,
      ecPublicKey: ecPublicKey,
      plainText: plainText,
      encryptedBuffer: encryptedBuffer.toString('hex'),
      decryptedBuffer: decryptedBuffer
    },
    path: '/encryption'
  })
}

```
    {{%/ tab %}}
{{< /tabs >}}

#### FetchTX

Fetching a payment that the user has previously made with your app.

 {{< tabs >}}
   {{% tab "Fetch Payment" %}}
```javascript
// sends a transaction on behalf of the user
module.exports.getTransaction = async (req, res, next) => {

  // fetch the authenticated user and their profile
  const user = await User.findById(req.user._id);
  const account = await handCashConnect.getAccountFromAuthToken(user.connectAuthToken);
  const paymentResult = await account.wallet.getPayment(req.query.txid)
 
  paymentResult.attachments = paymentResult.attachments.map(attachment => {
    if(attachment.format == 'hex') 
      attachment.hexValue = ConvertHexToString(attachment.value)
    return attachment
  })
  
  console.log(paymentResult)
  
  // display public profile with the recent transaction
  res.render('transaction', {
    tx: paymentResult,
    path: '/transaction'
  })
}
```
    {{%/ tab %}}
{{< /tabs >}}