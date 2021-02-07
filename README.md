# Programmable Telecommunications in the Cloud without Twilio

## Requirements

- NodeJS 12+ (Use nvm if possible)
- Fonos command-line tool (install with `npm install -g @fonos/ctl`)
- Request early access to [Fonoster.com], you will receive a set of credentials
- A set of credentials to a VoIP provider (provided)

## Overview

This is an introduction with the basics of Programmable Telecommunications in Cloud. I will not discuss installing a Fonos deployment in this repo, because it is impractical in this settings. Instead, you can request access to SaaS edition of Fonos at `Fonoster.com`. In this repo, I will show how to create a basic SIP Network and a Programmable Voice Application. We will then use the SDK to generate a call to phone number on the PSTN.

## Login into the platform

Use the following command to gain access to the platform, using the credentials you received:

```bash
$ fonos login
? accessKeyId ${YOUR ACCESS_KEY_ID}
? accessKeyToken ${YOUR ACCESS_KEY_TOKEN}
```

## Creating a Programmable Voice Application (PVA)

## Creating a SIPNetwork

➊ First lets define a VoIP service provider and a DID with:

> Use the information, provided to you by the conference team

```bash
$ fonos providers:create

This utility will help you create a new Provider
Press ^C at any time to quit.
? friendly name HWTHDC2021
? username hwth
? secret [hidden]
? host hwthdc.net
? transport tcp
? expire 3600
? does everything look good? Yes
Creating provider HWTHDC2021... All done
```

> You can verify your setup with `fonos providers:ls`

And a new DID with:

```bash
$ fonos numbers:create

This utility will help you create a new Number
Press ^C at any time to quit.
? number in e164 format 7853178070
? service provider HWTHDC2021
? aor link (leave empty)
? ingress app default
? does everything look good? Yes
Creating number 17853178070... All done
```

> You can verify your setup with `fonos numbers:ls`

➋ Next, lets create a Domain

```bash
$ fonos fonos domains:create

This utility will help you create a new Domain
Press ^C at any time to quit.
? domain name ACME Corp
? domain uri acme.com
? egress rule 
? number reference 
? access deny list 0.0.0.0/1
? access allow list 
? does everything look good? Yes
Creating domain ACME Corp... All done
```

➋ Next, lets create an Agent

```bash
$ fonos fonos agents:create

This utility will help you create a new Agent
Press ^C at any time to quit.
? friendly name Jane Doe
? username 1001
? secret [hidden]
? domains acme.com
? privacy Private
? does everything look good? Yes
```

## Using the SDK to call a Number

From within the folder of your `default` application, run the following command:

```
npm i --save @fonos/sdk 
```

Then, create a script with the this code:

```
// This will load the SDK and reuse your Fonos credentials
const SDK = required(`@fonos/sdk`)
const manager = new SDK.CallManager()

// A number you want to call and the application identifier
manager.call('1785318070', 'a445hds')
```

