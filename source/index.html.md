---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - javascript

toc_footers:

includes:
  - errors

search: true

code_clipboard: true
---

# Introduction

Welcome to the API documentation for Treasure Orbital. In this document, we document the internal and external APIs that we use in our project. We use these APIs to access different third party applications that we use in our project.

We access these third party APIs using JavaScript.

# Snoowrap API

Snoowrap is a library that wraps around the Reddit API and allow us to interact with the Reddit API with JavaScript methods.

## Get Authorization URL

> Obtains the authorization URL

```javascript
const snoowrap = require("snoowrap");

const options = {
  clientId: "DzfZOF3d3708Yy",
  scope: ["identity"],
  redirectUri: "http://localhost:3000/auth-callback",
  permanant: true,
  state: "hywioqj",
};

snoowrap.getAuthUrl(options);
```

> Ensure that the <code>redirectUri</code> is the same as the redirectUri declared when getting the Reddit API

Generates the authorization URL based on your Reddit API key. When redirected to this URL, the user can give authorization for you app to access the Reddit API on their behalf.

### Query Parameters

| Parameter | Type   | Description                                                                            |
| --------- | ------ | -------------------------------------------------------------------------------------- |
| options   | object | The options that is required for the authentification server to generate the right URL |

### options Parameters

| Name           | Type            | Argument  | Description                                                                                                                                                                                                |
| -------------- | --------------- | --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| clientId       | string          | mandatory | clientId given when generating the Reddit API key                                                                                                                                                          |
| scope          | Array\<String\> | mandatory | Scope of the permissions needed by the application. All scopes can be found [here](https://www.reddit.com/api/v1/scopes)                                                                                   |
| redirect Uri   | string          | mandatory | Redirect URL that was given when generating the Reddir API key                                                                                                                                             |
| permanent      | boolean         | optional  | If true, the app will have indefinite access to the user's account. If false, access to the user's account will expire after 1 hour.                                                                       |
| state          | string          | optional  | A string that can be used to reserve the state of the application. It is also important in preventing CSRF attacks. More infomation can be found [here](https://auth0.com/docs/protocols/state-parameters) |
| endpointDomain | string          | optional  | Endpoint domain of the url. Not require if authenticating on reddit.com                                                                                                                                    |

### Returns:

A URL where the user can authenticate with the given options.

## Authorization Code Exchange

> Exchanges the authorization code with an access token and refresh token.

```javascript
const snoowrap = require("snoowrap");

const options = {
  code: code,
  userAgent: "TreasureOrbital v1.0",
  clientId: "DzfZOF3d3708Yy",
  clientSecret: "sBfggKGPAq5b7IbZoYXrKmKPqoEdoU",
  redirectUri: "http://localhost:3000/auth-callback",
};

snoowrap.fromAuthCode(options).then((r) => r.getMe().then(console.log));
```

Exchanges the authorization code from the authorization url with an access token and a refresh token.

### Query Parameters

| Parameter | Type   | Description                                                                                                                     |
| --------- | ------ | ------------------------------------------------------------------------------------------------------------------------------- |
| options   | object | The options that is required for the authentification server to generate the access token and refresh token for the application |

### Options Parameters

| Name           | Type   | Argument  | Description                                                             |
| -------------- | ------ | --------- | ----------------------------------------------------------------------- |
| code           | string | mandatory | The authorization code                                                  |
| userAgent      | string | mandatory | A unique string describing what the application does                    |
| clientId       | string | mandatory | clientId given when generating the Reddit API key                       |
| clientSecret   | string | mandatory | The client secret of the application provided by Reddit                 |
| redirect Uri   | string | mandatory | Redirect URL that was given when generating the Reddir API key          |
| endpointDomain | string | optional  | Endpoint domain of the url. Not require if authenticating on reddit.com |

### Returns:

A promise that fulfils with a snoowrap instance.

## Getting the User

> Gets the user details

```javascript
r.getMe().then((data) => console.log(data));
```

Gets the information on the requester's own user profile.

### Returns:

A RedditUser object corresponding to the requester's profile.

# MetaMask API

Metamask is a digital wallet for ethereum and it adds on to your desktop’s browser as an extension. We use the Metamask API to interact with the user's cryptowallet.

## Getting Wallet ID

> Retrieves Wallet ID

```javascript
ethereum
  .request({ method: "eth_requestAccounts" })
  .then(console.log)
  .catch(console.log);
```

Requests that the user provides an Ethereum address to be identified by. Returns a Promise that resolves to an array of a single Ethereum address string. If the user denies the request, the Promise will reject with a 4001 error.

The request causes a MetaMask popup to appear. You should only request the user's accounts in response to user action, such as a button click. You should always disable the button that caused the request to be dispatched, while the request is still pending.

## Getting the Balance Amount

> Getting the Balance Amount

```javascript
ethereum.request({
  method: eth_getBalance,
  params: ["0xBAECA86200e7AC866B397E3A00A4De0Ecf3f4100", "0x0"],
});
```

> The above command returns a promise with the following JSON structured like this:

```json
{
  "jsonrpc": "2.0",
  "result": "0x0",
  "id": 0
}
```

Gets the balance amount of the Metamask wallet associated with the window. Uses the Ethereum RPC API method <code>eth_getBalance</code>. Takes in a <code>params</code> argument as a list.

### params Parameters

| Name        | Type   | Argument  | Description           |
| ----------- | ------ | --------- | --------------------- |
| address     | string | mandatory | Wallet address        |
| blockNumber | string | optional  | block number to check |

### Returns:
A promise with the requested data.