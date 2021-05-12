---
title: "Errors"
date: 2018-12-27T11:02:05+06:00
icon: "ti-alert"
description: "Why am I getting an error?"
type : "docs"
weight: 11
---


#### Overview

While using the sdk, you may run into an error. Usually this is caused by an invalid parameter in your implementation. 

<br>

Here is an example of one error you may run into:

<br>

```javascript
{ 
    'httpStatusCode': 401,
    'message': 'Invalid destination provided',
    'info': { unknowns: [ 'moffdd' ] } 
}
```

<br>


To learn more about these and how to solve them, review the error messages below.

<hr>
<br>


#### Payment Errors:

<br>    

{{< error error="Request exceeds users global connect spend limit." errorCode="spend_limit_violation" why="The user has reached their Connect limit. Prompt the user to increase their Connect limit from within the HandCash app." code=409 title="Spend limit violation"  >}}

{{< error error="Insufficient funds." why="The user account does not hold enough funds to process the transaction. Prompt the user to top up thier account." code=409 title="Insufficient funds" >}}

{{< error error="Request contains invalid parameters" why="You've provided an invalid parameter. Check `info` from the response to find our which one." code=400 title="Invalid parameters" >}}

{{< error error="Invalid destination provided" why="The handle, paymail or address you provided is invalid. Ensure the destination you're providing is a valid one." code=409 title="Invalid destination" >}}

{{< error error="Wallet is not TSS type" why="This user has not migrated their wallet to HandCash 2.5." code=401 title="Non-TSS wallet" >}}

<hr>
<br>

#### Other Errors:

<br>

{{< error error="This app didn't create the requested transaction" why="You are requesting a transaction made by a different app." code=409 title="Invalid transaction query" >}}

{{< error error="Permission not established" why="You are trying to use a feature that requires a permission not yet granted to you by this user." code=401 title="Missing permission" >}}

{{< error error="Missing authorization" why="The authorization token is missing or is no longer valid." code=401 title="Missing authorization" >}}

<hr>
<br>

#### HandCash Cloud Errors:

If you recieve an error code with status code `5xx` this is an error on the our end. 

<br>

Here is an example:


```javascript
{ 
    'httpStatusCode': 503,
    'message': 'External service not available',
    'serviceName': ''
    'info': { } 
}
```

<br>

{{< notice note >}}
If you recieve any error with status code **`5xx`**, please get in touch with a HandCash Connect representative to resolve it.
{{</ notice >}}

<br>

{{< error error="External service not available" why="We are having difficulties connecting to an external service, and are unable to proccess your request." code=503 title="External service unavailable" >}}

{{< error error="Internal server error" why="HandCash has experienced an internal server error, and is unable to process your request." code=500 title="Missing authorization" >}}


