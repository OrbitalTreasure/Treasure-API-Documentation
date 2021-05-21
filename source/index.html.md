---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - javascript

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

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
  clientId: "insert client ID",
  scope: ["identity"],
  redirectUri: "insert redirectUri",
  permanant: true,
  state: "h5t1jcb",
};

snoowrap.getAuthUrl(options);
```

> Ensure that the <code>redirectUri</code> is the same as the redirectUri declared when getting the Reddit API

Generates the authorization URL based on your Reddit API key. When redirected to this URL, the user can give authorization for you app to access the Reddit API on their behalf.

### Parameters

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
