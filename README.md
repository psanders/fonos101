# Programmable Telecommunications in the Cloud without Twilio

This is an introduction to the basics of Programmable Telecommunications in the Cloud with [Fonos](https://github.com/fonoster/fonos). I created this repo as part of the [Hacking With The Homies Developer Conference](https://github.com/detroitblacktech/hwthdc2020). 

I will not discuss installing a Fonos deployment in this repo because it is impractical for the event. Instead, you can request access to the SaaS edition of Fonos at [Fonoster.com](http://fonoster.com/).

## Requirements

- NodeJS 12+ (Use nvm if possible)
- Fonos command-line tool (install with `npm install -g @fonos/ctl`)
- Request early access at [Fonoster.com](http://fonoster.com/), to receive a set of credentials
- A set of credentials to a VoIP provider (provided)

## Overview

In this demo, I will show how to create a SIP Network and a Programmable Voice Application. We will then use the SDK to generate a call to a phone number on the PSTN. 

> Please keep in mind that this is an early version of the platform.  Issues might occur.

## Login into the platform

Use the credentials you received to gain access to the platform. You can gain access with:

```bash
$ fonos auth:login
? api endpoint (api.fonoster.io:50052)
? access key id ${YOUR KEY ID}
? access key token [input is hidden] ${YOUR TOKEN}
```

## Creating a Programmable Voice Application (PVA)

First, use the command `fonos apps:init` to create an empty project.

```bash
mkdir default
cd default
fonos apps:init
```

> You may use the default values

Then, open the index.js file and change with the following:

```
module.exports = chan => chan.say('Hello world')
```

Last, you need to push your app to platform with:

```
fonos apps:deploy
```

## Creating a SIPNetwork

<details><summary>Define a VoIP service provider</summary>
  
  <br />
  <p>First, let's define a VoIP service provider with:</p>

```bash
$ fonos providers:create

This utility will help you create a new Provider
Press ^C at any time to quit.
? friendly name HWTHDC2021
? username hwth
? secret [hidden]
? host newyork1.voip.ms
? transport tcp
? expire 300
? does everything look good? Yes
Creating provider HWTHDC2021... Done
```
> Use the information provided to you by homies

</details>

<details><summary>Add a new DID</summary>

  <br />
  <p>Add new DID with:</p>


```bash
$ fonos numbers:create

This utility will help you create a new Number
Press ^C at any time to quit.
? number in E.164 format (e.g. +16471234567) +17853178070
? service provider HWTHDC2021
? aor link (leave empty)
? ingress app default
? does everything look good? Yes
Creating number 17853178070... Done
```
</details>

<details><summary>Create a new Domain</summary>
  <br />
  <p>Before creating the domain you must obtain the numbers reference with 'fonos numbers:ls'</p>
  <p>Create new DID with:</p>

```bash
$ fonos domains:create

This utility will help you create a new Domain
Press ^C at any time to quit.
? friendly name acme corp
? domain uri acme.com
? egress number 19106376139
? egress rule .*
? access deny list 0.0.0.0/1
? access allow list 
? does everything look good? Yes
Creating domain acme corp... Done
```
</details>

> You can check your work with `fonos [command]:ls

## Using the SDK to call a Number

From within the folder of your `default` application, run the following command:

```bash
npm i --save @fonos/sdk 
```

Then, create a script with this code:

```javascript
// This will load the SDK and reuse your Fonos credentials
const Fonos = require('@fonos/sdk')
const callManager = new Fonos.CallManager()

// A number you want to call and the application identifier
callManager.call({
 from: '+19198972120',
 to: '+17853178070',
 app: 'hYTHYCYv_U'
})
.then(r => console.log(r))
.catch(e => console.error(e))
```
