---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
- shell

toc_footers:
- <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>
- <a href='https://www.gluwa.com/'>© 2019 Gluwa, Inc.™ All Rights Reserved</a>

includes:
- errors

search: true
---

Test requests
short down open ID
merge responses
user flow doc

# Gluwa API Documentation


* [Gluwa API Documentation](#gluwa-api-documentation)
* [Gluwa Overview](#gluwa-overview)
* [Getting Started with Gluwa API](#getting-started-with-gluwa-api)
	* [Downloading the app and signing up for an account](#downloading-the-app-and-signing-up-for-an-account)
	* [Submitting a request for a client ID and secret](#submitting-a-request-for-a-client-id-and-secret)
	* [Getting an access token](#getting-an-access-token)
	* [Transferring funds to another Gluwa account](#transferring-funds-to-another-gluwa-account)
* [Get an Access Token from Auth API](#get-an-access-token-from-auth-api)
* [OpenID Connect](#openid-connect)
	* [Authorization Code flow](#authorization-code-flow)
	* [Implicit flow](#implicit-flow)
	* [Hybrid flow](#hybrid-flow)
* [Endpoints and Resources](#endpoints-and-resources)
* [Bank](#bank)
	* [List Banks](#list-banks)
	* [Search Banks](#search-banks)
* [Funding Source](#funding-source)
	* [Show a funding source](#show-a-funding-source)
	* [List funding sources](#list-funding-sources)
	* [Deactivate a funding source](#deactivate-a-funding-source)
	* [Create a micro deposit](#create-a-micro-deposit)
	* [Verify a micro deposit](#verify-a-micro-deposit)
* [Crypto Funding Source](#crypto-funding-source)
	* [List crypto funding sources](#list-crypto-funding-sources)
	* [Verify Crypto Funding Source](#verify-crypto-funding-source)
	* [Create a cryptocurrency funding source](#create-a-cryptocurrency-funding-source)
* [Wallet](#wallet)
	* [List wallets](#list-wallets)
	* [Search wallet by ID](#search-wallet-by-id)
	* [Get the fee models associated to a wallet](#get-the-fee-models-associated-to-a-wallet)
	* [Create a new wallet for the current user](#create-a-new-wallet-for-the-current-user)
	* [Activate a locked wallet](#activate-a-locked-wallet)
	* [Get webhook information](#get-webhook-information)
	* [Create or update webhook URL and secret](#create-or-update-webhook-url-and-secret)
	* [Generate QR code for transaction request](#generate-qr-code-for-transaction-request)
* [User](#user)
	* [Search by username](#search-by-username)
* [Transaction](#transaction)
	* [List of transactions associated to a wallet](#list-of-transactions-associated-to-a-wallet)
	* [Retrieve a transaction by ID and a specified wallet](#retrieve-a-transaction-by-id-and-a-specified-wallet)
	* [Calculate transaction fee](#calculate-transaction-fee-1)
	* [Move funds to/from the current user's wallet](#move-funds-tofrom-the-current-users-wallet)
	* [Cancel requested deposit](#cancel-requested-deposit)
	* [Get a quote for crypto depositing and withdrawing](#get-a-quote-for-crypto-depositing-and-withdrawing)
	* [Create a new transaction using quote provided](#create-a-new-transaction-using-quote-provided)
* [Virtual Account](#virtual-account)
	* [Get virtual accounts linked to a wallet](#get-virtual-accounts-linked-to-a-wallet)
* [Glossary](#glossary)


# Gluwa Overview

Gluwa is a fiat-pegged cryptocurrency wallet that provides *Bitcoin* deposit and withdrawal. The Gluwa wallet enables users to process national and international transactions, allowing for secure management of money.
Through Gluwa's public-facing REST API, users can:

* Get information about supported Banks
* Manage funding sources
* Manage transactions
* Manage wallet details


# Getting Started with Gluwa API 

This section will ensure that you have all the necessary details to start using the Gluwa API. It will also show you how to run a few basic requests.

Gluwa uses a smartphone application as the main user interface for managing funds and transactions. In order to have access to the Gluwa API, you will need to set up an account.                                                                                                              
## Downloading the app and signing up for an account

1. Download the free Gluwa application from the <a href="https://play.google.com/store/apps/details?id=com.gluwa.android" target="_blank">Google Playstore </a>
2. Open the application and create an account.
3. Confirm the account by clicking he link provided in the confirmation email.

You will now be able to access the front-end of Gluwa through the smartphone app.

## Submitting a request for a client ID and secret

In order to get access tokens, you will need to find out the client ID and secret associated to your account. This can only be done by contacting Gluwa's customer service.

To receive your Client ID and Client Secret, submit a ticket on <a href="https://gluwa.zendesk.com/hc/ko" target="_blank">Gluwa's helpcentre</a> containing the following template:

```md
Hello

Please provide the client ID and secret associated to the following account

email@provider.com

I am requesting those details in order to have access to the Gluwa API.

```

## Getting an access token

Gluwa has multiple ways of obtaining an access token. This tutorial only covers the OAuth method. Please see ____ for the other ways.

Run the following request to receive an access token:
 
 
[![Run in Postman](https://run.pstmn.io/button.svg)](https://app.getpostman.com/run-collection/d95de6a577f8fb5715ed)

```shell
curl -X POST
"https://auth.gluwa.com/connect/token" 
-H "Content-Type: application/x-www-form-urlencoded"
-d "{
    "client_id": "{clientID}",
    "client_secret": "{clientsecret}",
    "grant_type: "password",
    "username": "{username}",
    "password": "{userpassword}",
    "scope": "wallet.read transaction.read"
}"
```

After submitting the above request, you will receive a token under the following format:

```json
{
"access_token":"{Token}",
"expires_in":3600,
"token_type":"Bearer"
}
```

You will need to use this token type and access token in the header of every request. The access token is only available for one hour.

## Transferring funds to another Gluwa account

The following steps will enable you to make a transaction only if you have balance available in your account.
To transfer funds from your account, we will go through the following steps:

* [Check your balance](#check-your-balance-and-get-wallet-id)
* [Calculate transaction fee](#calculate-transaction-fee)
* [Transfer funds to an account](#transfer-funds-to-an-account)
* [Check transaction details](#check-transaction-details)

[![Run in Postman](https://run.pstmn.io/button.svg)](https://app.getpostman.com/run-collection/d95de6a577f8fb5715ed)

### Check your balance and get wallet ID
[Click here for all resource details](#list-wallets)

This resource will provide you with all the wallet details for the current user.

> Request 

```shell
curl -X GET
  "https://api.gluwa.com/V4.1/Wallets"
  -H "Authorization: Bearer {token}"
```
The response will display the current balance for the account. 
**Make note of the wallet ID provided.**

> Response 

```json
[
  {
    "ID": "string (uuid)",
    "Balance": "string",
    "Country": "string",
    "Currency": "string",
    "Status": "string"
  }
]
```

### Calculate transaction fee
[Click here for all resource details](#get-the-fee-models-associated-to-a-wallet)

All Gluwa transactions have a small fee associated with them. This resource will calculate the transfer fee based on the amount provided.

> Request

```shell
curl -X GET
  "https://api.gluwa.com/V4.1/Wallets/{walletID}/Transactions/Fee"
  -H "Authorization: Bearer {token}"
  -H "Content-Type: application/json"
  -d "{
  "Amount": "{amount of money}",
  "Type": "Transfer"
}"
```
The fee for making a transaction of the amount entered.

> Response

```json
{
"string"
}
```
### Transfer funds to an account
[Click here for all resource details](#move-funds-to/from-the-current-user's-wallet)

This request will transfer the amount of money you wish to a Gluwa account of your choice.
> Request 

```shell
curl -X POST
  "https://api.gluwa.com/V4.1/Wallets/{walletID}/Transactions"
  -H "Authorization: Bearer {token}"
  -H "Content-Type: application/json"
  -d "{
  "Type": "Transfer",
  "Recipient": "{username / email address / phone number}",
  "Password": "{your password}",
  "Amount": "{amount of money}"
}"
```

A successful transfer will give you the following details:

> Response

```json
  {
        "ID": "string",
        "CreatedDateTime": "string",
        "ModifiedDateTime": "string",
        "Amount": "string",
        "FeeAmount": "string",
        "Type": "string",
        "Status": "string",
        "Currency": "string",
        "From": {
            "Type": "string",
            "DisplayName": "string"
        },
        "To": {
            "Type": "string",
            "DisplayName": "string"
        },
        "Note": "string"
    }
```

### Check transaction details
[Click here for all resource details](#list-of-transactions-associated-to-a-wallet)

After a transaction is finished, you can check its details again by running the below request.

> Request

```shell
curl -X GET
  "https://api.gluwa.com/V4.1/Wallets/{walletID}/Transactions"
  -H "Authorization: Bearer {token}"
```
> Response

```json  
  {
        "ID": "string",
        "CreatedDateTime": "string",
        "ModifiedDateTime": "string",
        "Amount": "string",
        "FeeAmount": "string",
        "Type": "string",
        "Status": "string",
        "Currency": "string",
        "From": {
            "Type": "string",
            "DisplayName": "string"
        },
        "To": {
            "Type": "string",
            "DisplayName": "string"
        },
        "Note": "string"
    }
```



# Get an Access Token from Auth API

> Example request - These details are for display purposes only 

```shell
curl -X POST
"https://auth.gluwa.com/connect/token" 
-H "Content-Type: application/x-www-form-urlencoded"
-d "{
    "client_id": "{clientID}",
    "client_secret": "{clientsecret}",
    "grant_type: "password",
    "username": "{username}",
    "password": "{userpassword}",
    "scope": "wallet.read transaction.read"
}"
```

> Response Token - token only for display purposes

```json
{
"access_token":"eyJhbGciOiJSUzI1NiIsImtpZCI6IkQyOEJENjcwOTI5QTBDN0U5MTMyRkE4NDgwRUMwNDlDMDBFRTIxQkYiLCJ0eXAiOiJKV1QiLCJ4NXQiOiIwb3ZXY0pLYURINlJNdnFFZ093RW5BRHVJYjgifQ.eyJuYmYiOjE1NTcwMjgyNTgsImV4cCI6MTU1NzAzMTg1OCwiaXNzIjoiaHR0cHM6Ly9hdXRoLmdsdXdhLmNvbSIsImF1ZCI6WyJodHRwczovL2F1dGguZ2x1d2EuY29tL3Jlc291cmNlcyIsIkdsdXdhQXBpIl0sImNsaWVudF9pZCI6IjhhMWMyNDI0LWZlNzMtNDczYS04MjhmLWZiODQ4YTNjMmNjOSIsInN1YiI6ImE2MDkxN2Q5LTRlNWQtNGE0Yi04MGZjLTEwY2FmYmM3MWYzOSIsImF1dGhfdGltZSI6MTU1NzAyODI1OCwiaWRwIjoibG9jYWwiLCJJc1Rlc3RVc2VyIjoiVHJ1ZSIsImVtYWlsX3ZlcmlmaWVkIjp0cnVlLCJwaG9uZV9udW1iZXJfdmVyaWZpZWQiOnRydWUsInNjb3BlIjpbInRyYW5zYWN0aW9uLnJlYWQiLCJ3YWxsZXQucmVhZCJdLCJhbXIiOlsicHdkIl19.gEK1SnM-FKyFR_rvF4WqI5zbwMfRI9lbJIh93wYvMruOzhtkv1m6m2VbwNGQEaxoNHuPNCbcuxXUa6I4TZXnKwMvSWAxXBXC9WoVU9cbtT4Q2nm8Y9-EfkUCOncaYX_ERbDetGjN4joiOyPAjRluKDBU491_uh66DHmAWGsP8o_JFI7tOk8yGKI2p8SUlvP_Y2YcfviAyjkhUOcFEuzTT1laB10CcrXMjUQyygiSU_HGHcskGcndig9HHSdI55LDNOgoCe_111U2AsPmZDzKPMRJ2RfFP3Y2AHB3lmDZvEILvuvJBcHtBEoEhRgZt5vk1arEC0o0cNmC42LBc1uFEQ",
"expires_in":3600,
"token_type":"Bearer"
}
```

In order to obtain an access token using the below method, you need a Client ID, Client Secret, Username and Password.

An access token will be returned when posting to the below URL with a body containing the following:

### Method

| Method | URL                                      |
|--------|------------------------------------------|
| POST    | <code class="highlighter-rouge">https://auth.gluwa.com/connect/token</code> |

### Header

| Header | URL                                      |
|--------|------------------------------------------|
| Content-Type   | <code class="highlighter-rouge">application/x-www-form-urlencoded</code> |


### Request Body

| Name                    | Type/Value | Required | Description                              |
|----------------------|--------------|----------|------------|------------------------------------------|
| client_id          | String        | yes            | Identifier of the client                 |
| client_secret | String        | yes | Secret key of the client |
| grant_type       | String        | yes | Type of the grant |
| username            | String        | yes | Username of the client |
| password            | String        | yes | Password of the client |
| scope                  | String        | yes | One or more registered scopes.           |


# OpenID Connect
Gluwa Auth server uses IdentityServer4 to authenticate/authorize its users. It strictly follows the specification laid out in [OpenID Connect Core 1.0](http://openid.net/specs/openid-connect-core-1_0.html).

## Authorization Code flow

This flow is suitable for Clients that can maintain a Client Secret between themselves and the Authorization Server. For example, clients that have a backend server to keep the client secret safe.

For details of the spec, please see [here](http://openid.net/specs/openid-connect-core-1_0.html#CodeFlowAuth). Note that not all OpenID connect features may be supported.

### Step 1. Prepare a request to Authorize Endpoint.

For request specification, please look [here](http://docs.identityserver.io/en/aspnetcore1/endpoints/authorize.html).

> Example request

```shell
curl -X GET 
"https://auth.gluwa.com/connect/authorize?client_id={example_client}&scope={openid%20GluwaApi}&response_type={code}&redirect_uri={https://redirect-after-login.com&state=abcd}"

```

### Method 

| Method | URL                                      |
|--------|------------------------------------------|
| GET    | <code class="highlighter-rouge">https://auth.gluwa.com/connect/authorize</code> |

### Parameters

| Name          | Type/Value                   | Required | Path/Query | Description                              |
|---------------|------------------------------|----------|------------|------------------------------------------|
| `client_id`     | String  | yes      | Query      | Identifier of the client                 |
| `scope`         | String  | yes      | Query       | One or more registered scopes.           |
| `redirect_uri`  | String  | yes      | Query      | must exactly match one of the allowed redirect URIs for that client |
| `response_type` | Code    | yes      | Query      |Requests an authorization code       |
| `state` | String    | no      | Query      |identityserver will echo back the state value on the token response, this is for round tripping state between client and provider, correlating request and response and CSRF/replay protection.   |

While the `state` parameter is not necessary, it is recommended. The value needs to be random and hard to guess. For example, `GUID` can work.

*Note: The allowed scope parameter may vary from client to client. Please ask the administrator your client is allowed to request.*

### Step 2. Redirect the user to the URL in step 1

When users click the `login` button on your app, redirect them to the URL formed in Step 1.

Before the redirect, you will need to persist `state` externally, typically using a browser cookie. This is due to the Auth server giving you back this value when it calls your `redirect_uri` as a query string, just after users login and give your app consent.

### Step 3. Auth server redirects back to `redirect_uri` formed in step 1.

Two query parameters will be sent to your `redirect_uri`:

| Name          | Type/Value | Required | Path/Query | Description                    |
|---------------|------------|----------|------------|--------------------------------|
| `scope`         | String     | yes      | Quey       | One or more registered scopes. |
| `response_type` | Code       | yes      | Query      | Requests an authorization code |

You need to validate the `state` that was given back by the Auth server with the one you have persisted in step 2. 

After the validation, you can remove the persisted state.

For any error responses, check the OpenID Connect [Authentication Error Response](http://openid.net/specs/openid-connect-core-1_0.html#AuthError).

### Step 4. Use code to obtain ID Token and the Access Token

Using the code obtained at step 3, you can now request ID Token and Access Token.

> Example request

```shell
curl -X POST
"https://auth.gluwa.com/connect/token"
-H "Content-Type: application/x-www-form-urlencoded"
-d "grant_type=authorization_code&code={your_authorization_code}&client_id={your_client_id}client_secret={your_client_secret}&redirect_uri={your_redirect_uri_used_in_1}"
```

> Note that the `access_token` and `id_token` is in JWT format. 
> To make a request to Gluwa's public API, use the `access_token` and attach it into Authorization header as Bearer scheme.

### Request

| Method | URL                    | HOST           | Content-Type                      |
|--------|------------------------|----------------|-----------------------------------|
| POST   | connect/token | auth.gluwa.com | application/x-www-form-urlencoded |


> Response

```json
{
    "id_token": {token},
    "expires_in": 3600,
    "token_type": "Bearer"
}
```

### Step 5. Validate ID token and Access Token

Before using the ID Token and Access Token, client must validate the tokens as laid out in [OpenID Connect Core 1.0, section 3.1.3.7 and 3.1.3.8](http://openid.net/specs/openid-connect-core-1_0.html#IDTokenValidation).

It is up to the client how to validate the tokens, which is outside the scope of this document.

### Step 6. Sign in to your app

The last step is to sign in to your own app so that you can navigate around secured parts of your app. 

This is client-side implementation.

## Implicit flow

This flow is mainly used by Clients implemented in a browser using a scripting language such as JavaScript or SPA. Since the client using this flow is public (meaning that the source code is available through a browser), you cannot have a `client_secret` with this flow.

For this flow, you can use the guide shown [here](http://docs.identityserver.io/en/aspnetcore1/quickstarts/7_javascript_client.html#reference-oidc-client), as there is a handy javascript library called [oidc-client-js](https://github.com/IdentityModel/oidc-client-js) made by the IdentityServer team. The guide assumes you are using Visual Studio on Windows environment, but since this flow should be used for front-end apps, it should work on any platform.

## Hybrid flow

The Hybrid flow takes the best of Authorization code flow and Implicit flow together. For implicit flow, all tokens are transmitted through a browser. This may be okay for `ID Token`, but `Access Token` is too sensitive to be transmitted via front end. 

Hybrid flow adds a layer of security. The `identity token` is transmitted via the browser channel, so the client can validate it before doing any additional work. If validation is successful, the client opens a back-channel to the token service to retrieve the access token.

The steps for this flow is similar to Authorization Code flow, with a few differences. For more details on the spec, see [here](http://openid.net/specs/openid-connect-core-1_0.html#HybridFlowAuth).

### Step 1. Prepare a request to Authorize Endpoint

> Example request

```shell

curl -X GET
"https://auth.gluwa.com/connect/authorize?client_id={example_client}&scope={openid%20GluwaApi}&response_type={code%20id_token}&redirect_uri={https://redirect-after-login.com&state=abcd&nonce=1234}"

```

### Method 

| Method | URL                                      |
|--------|------------------------------------------|
| GET    | <code class="highlighter-rouge">https://auth.gluwa.com/connect/authorize</code> |

### Parameters

| Name          | Type/Value                               | Required | Path/Query | Description                              |
|---------------|------------------------------------------|----------|------------|------------------------------------------|
| `client_id`     | String                                   | yes      | Query      | Identifier of the client                 |
| `scope`         | String                                   | yes      | Quey       | One or more registered scopes.           |
| `redirect_uri`  | String                                   | yes      | Query      | must exactly match one of the allowed redirect URIs for that client |
| `response_type` | code token, code id_token | yes      | Query      | Requests an authorization code           |
| `state`         | String                                   | no       | Query      | identityserver will echo back the state value on the token response, this is for round tripping state between client and provider, correlating request and response and CSRF/replay protection. |
| `nonce`         |  String                            |no          |Query          |  Nonce is a string value that is associated with the client session to mitigate replay attacks. Where as state only provides a way to bind the client's request and the response from auth server together, nonce provides a way for the client to determine if the token is actually generated for the client by the auth server.                                        |

While the `state` parameter is not necessary, it is recommended. The value needs to be random and hard to guess. For exmaple, `Guid` can work.

**Note: The allowed scope parameter may vary from client to client. Please ask the administrator your client is allowed to request.**

When `nonce` is present in the authorize request, the auth server will include nonce as one of the properties of the id token. The client MUST verify that the nonce in the id token is the same as the nonce it passed in to the authorize request.

### Step 2. Redirect the user to the endpoint in step 1

When users click the `login` button on your app, redirect them to the URL formed in Step 1.

Before the redirect, you will need to persist `state` externally, typically using a browser cookie. This is due to the Auth server giving you back this value when it calls your `redirect_uri` as a query string, just after users login and give your app consent.

### Step 3. Auth server redirects back to `redirect_uri` formed in step 1.

> Example request

```shell
curl -X GET
"https://redirect-after-login.com/#code={12365sdfsdf445}&id_token={sdlfiqlk1231568654}&scope={openid%20GluwaApi}&state={abcd}&session_state={sdfio23123}"

```

In hybrid flow all response parameters are added to the fragment component (component after `#` in the URL of the `redirect_uri` Fragment parameters will depend on what you've put as `response_type` in step 1. Following the example given in step 1, the query parameters will be:

- state
- code
- id_token

(since we only requested for code and `id_token` )

`id_token` must be validated before usage as shown in the next step (step 4). After the validation is successful, you can use the code to obtain `access_token` through the token endpoint as shown in step 4 of Authorization Code flow. State field must also be validated.

### Step 4. Validate ID token and Access Token

Before using the ID Token and Access Token, client must validate the tokens as laid out in [OpenID Connect Core 1.0, section 3.1.3.7 and 3.1.3.8](http://openid.net/specs/openid-connect-core-1_0.html#IDTokenValidation). It is up to the client how to validate the tokens, which is outside the scope of this document.

### Step 5. Sign in to your app

The last step is to sign in to your own app so that you can navigate around secured parts of your app. 

### Client Credentials As User

> Example request

```shell
curl -X POST
"https://auth.gluwa.com/connect/token"
-H "Content-Type: application/x-www-form-urlencoded"
-d "client_id={client_id}&client_secret={client_secret}&scope={scopes}&grant_type=client_credentials_as_user"
```

This flow is used for users who would like to use our API with their client credentials (log in using client id and secret). We assign the user's Gluwa account to a specific client, and the user will be able to login to that account using client id and secret.

You can send the request to the auth server as follows:

| Method | URL                    | Host           | Content-Type | Cache Control |
|--------|------------------------|----------------|--------------|---------------|
| POST   | connect/token | auth.gluwa.com | application/x-www-form-urlencoded | no-cache      |


### Logging out

Logout from your own client, not the Gluwa Auth server. 

Should you really want to logout from both your own client and on Gluwa Auth server, you can use the following endpoints to do so.

**Note that if you do decide to use these endpoints, you are logging the user out completely from Gluwa Auth system, meaning that they will also be logged out of other clients that may not be in your control.**

### Revocation endpoint

> Example request

```shell
curl -X POST
"https://auth.gluwa.com/connect/revocation"
-H "Content-Type: application/x-www-form-urlencoded"
-d "token=<your_refresh_token>&token_type_hint=refresh_token&client_id=<your_client_id>&client_secret=<your_client_secret>"
```

You can revoke token by:

| Method | URL                         | Host           | Content-Type                      |
|--------|-----------------------------|----------------|-----------------------------------|
| POST   | connect/revocation | auth.gluwa.com | application/x-www-form-urlencoded |

You cannot revoke access token. Access token lifetime is set to 1 hour.

### End Session Endpoint

| Method | URL      |
|--------|----------|
| GET    | /connect/endsession?id_token_hint=<your_id_token>&post_logout_redirect_uri=<your_post_log_redirect_uri>&state=abcd |


For the details of each query parameter, please click [here](http://docs.identityserver.io/en/aspnetcore1/endpoints/endsession.html).

Again, if state is provided, the auth server will give this back in your `post_redirect_uri` so you can validate the state after redirection.

### Session management

The documentation for session status detection can be found [here](http://openid.net/specs/openid-connect-session-1_0.html#ChangeNotification). 

**Note that this is up to the client to implement.**



# Endpoints and Resources

Welcome to the Gluwa API Reference Documentation for Endpoints and Resources.
These documents describe all resources available in the Gluwa API and covers the endpoints, parameters, requests and responses.

# Bank

Bank API provides the list of financial institutions that Gluwa supports. 
Get the list of banks by country, or search for a specific institution.


## List Banks

Get the list of banks supported by Gluwa.

> Request Example

```curl
curl -X GET 
  "https://api.gluwa.com/V4.1/Banks/{Country}" 
```

### Method

| Method | URL                                      |
|--------|------------------------------------------|
| GET    | <code class="highlighter-rouge">/V4.1/Banks/{country}</code> |

### Parameters
| Name    | Type/Value                   | Required | Path/Query | Description                              |
|---------|------------------------------|----------|------------|------------------------------------------|
| country | String enum: [World, US, KR] | yes      | Path       | Filter banks by country                  |
| offset  | Integer (int64)              | no       | Query      | Number of banks to skip from the beginning of the list. Used for pagination. Defaults to 0 |
| limit   | Integer (int64)              | no       | Query      | Number of banks to include in the result. Defaults to 25 |

### Responses

> 200 OK: List of banks

```json
[
    {
        "InstitutionCode": "string",
        "DisplayName": "string",
        "LogoImageUrl": "string"
        }
]
```

> 400 Bad Request: Invalid country parameter

```json
{
    "InnerErrors": [
        {
            "Code": "Other",
            "Path": "country",
            "Message": "The value 'XX' is not valid."
        }
    ],
    "Code": "InvalidUrlParameters",
    "Message": "one of more Url parameters are invalid."
}
```

Response Codes

| Code | Type/Value                        | Description                            |
|------|-----------------------------------|----------------------------------------|
| 200 | Array: Financial Institutions      | List of Banks                          |
| 400  | Bad Request | Invalid country parameter |

## Search Banks

Search through the list of banks by keyword.

> Request Example

```shell
curl -X GET 
  "https://api.gluwa.com/V4.1/Banks/{Country}/searches/{keyword}" 
```

### Method

| Method | URL                                      |
|--------|------------------------------------------|
| GET    | <code class="highlighter-rouge">/V4.1/Banks/{country}/Searches/{keyword}</code> |

### Parameters


| Name    | Type/Value                   | Required | Path/Query | Description                              |
|---------|------------------------------|----------|------------|------------------------------------------|
| country | String enum: [World, US, KR] | yes      | Path       | Filter banks by country.                 |
| keyword | String                       | yes      | Path       | Bank name to search for.                 |
| offset  | Integer (int64)              | no       | Query      | Number of banks to skip from the beginning of the list; used for pagination. Defaults to 0. |
| limit   | Integer (int64)              | no       | Query      | Number of banks to include in the result. Defaults to 25. |


### Responses

> 200 OK: List of Banks

```json
[
    {
        "InstitutionCode": "string",
        "DisplayName": "string",
        "LogoImageUrl": "string"
    }
]
```

> 400 Bad Request: Invalid country parameter 

```json
{
    "InnerErrors": [
        {
            "Code": "Other",
            "Path": "country",
            "Message": "The value 'XX' is not valid."
        }
    ],
    "Code": "InvalidUrlParameters",
    "Message": "one of more Url parameters are invalid."
}
```

Response Codes

| Code | Type/Value                        | Description                            |
|------|-----------------------------------|----------------------------------------|
| 200  | Array: Financial Institution      | List of Banks                          |
| 400  | Bad Request | Invalid country parameter |


# Funding Source

A funding source represents the source of fiat currency, such as bank accounts and credit cards. 
Users can make deposits to Gluwa from their funding sources, or withdraw from Gluwa to their funding sources. 

Funding source API can be used to create funding sources under a user or to retrieve a list of funding sources that belong to a user.

Micro deposit endpoints are for initiating and completing the verification process that is required before a funding source can be used. This resource also includes an endpoint that changes the status of a funding source to `deactivated`.


## Show a funding source

Retrieve a funding source of any type with specified ID.

> Request Example

```shell
curl -X GET 
  "https://api.gluwa.com/V4.1/FundingSources/{ID}"
  -H "Authorization: Bearer {token}"
```
  
### Method

| Method | URL                                      |
|--------|------------------------------------------|
| GET    | <code class="highlighter-rouge">/V4.1/FundingSources/{ID}</code> |

### Parameters

| Name | Type/Value   | Required | Path/Query | Description        |
|------|--------------|----------|------------|--------------------|
| ID   | String: uuid | yes      | Path       | Funding Source ID |


### Responses

> 200 OK: Successful Response

```json
{

"ID": "string (uuid)",
"Country": "string",
"Currency": "string",
"Type": "string",
"Status": "string",
"InstitutionDisplayName": "string",
"Last4DigitsOfAccountNumber": "string",
"CryptoAddress": "string",
"Nickname": "string"

}

*Type: [ Checking, Savings, Crypto ] - This is the type of funding source
*Status: [ NotVerified, Verified, MicroDepositSent, Deactivated, MicroDepositSentFailed ] - Funding source status in Gluwa
```


> 400 Bad Request: Request syntax malformed

> 403 Forbidden: Access to this funding source is denied

```json
{

"InnerErrors": [
{
"Code": "string",
"Path": "string",
"Message": "string"
}
],
"Code": "string",
"Message": "string",
"ExtraData": "string"

}
```


> 404 Not Found: Funding source is not found 

```json 
{

"Code": "string",
"Message": "string",
"ExtraData": "string"

}
```

Response codes:

| Code | Type/Value                        | Description                              |
|------|-----------------------------------|------------------------------------------|
| 200 |FundingSourceResponse              | Funding source with the specified ID    |
| 400  | Bad Request | Request syntax malformed                        |
| 403 | ForbiddenRequestError             | Access to this funding source is denied |
| 404 | NotFoundError                     | Funding source is not found             |

## List funding sources
Retrieve the list of funding sources of all types that belong to the user.

> Request Example

```shell
curl -X GET 
  "https://api.gluwa.com/V4.1/FundingSources"
  -H "Authorization: Bearer {token}"
```

### Method

| Method | URL                                      |
|--------|------------------------------------------|
| GET    | <code class="highlighter-rouge">/V4.1/FundingSources</code> |

### Responses

> 200 OK: List of Funding Sources

```json

{
"ID": "string (uuid)",
"Country": "string",
"Currency": "string",
"Type": "string",
"Status": "string",
"InstitutionDisplayName": "string",
"Last4DigitsOfAccountNumber": "string",
"CryptoAddress": "string",
"Nickname": "string"
}

*Type: [ Checking, Savings, Crypto ] - This is the type of funding source
*Status: [ NotVerified, Verified, MicroDepositSent, Deactivated, MicroDepositSentFailed ] - Funding source status in Gluwa
```


Response Codes

| Code | Type/Value                        | Description                              |
|------|-----------------------------------|------------------------------------------|
| 200  | Array: FundingSourceResponse      | List of Funding Resources              |


## Deactivate a funding source
A deactivated funding source cannot be viewed or used for withdrawals.

> Request Example

```shell
curl -X DELETE 
  "https://api.gluwa.com/V4.1/FundingSources/{ID}"
  -H "Authorization: Bearer {token}"
```

### Method

| Method | URL                                      |
|--------|------------------------------------------|
| DELETE    | <code class="highlighter-rouge">/V4.1/FundingSources/{ID}</code> |

### Parameters

| Name | Type/Value   | Required | Path/Query | Description        |
|------|--------------|----------|------------|--------------------|
| ID   | String: uuid | yes      | Path       | Funding Source ID |



### Responses

> OK 200: Funding source deactivated

```json
{}
```

> 400 Bad Request: Request syntax malformed

> 403 Forbidden: Access to this funding source is denied

```json
{

"InnerErrors": [
{
"Code": "string",
"Path": "string",
"Message": "string"
}
],
"Code": "string",
"Message": "string",
"ExtraData": "string"

}
```

> 404 Not Found: Funding Source not found

```json 
{

"Code": "string",
"Message": "string",
"ExtraData": "string"

}
```


Response Codes:

| Code | Type/Value                        | Description                              |
|------|-----------------------------------|------------------------------------------|
| 200  | Void                              | Founding source deactivated              |
| 400  | Bad Request | Request syntax Malformed.   |
| 403  | ForbiddenRequestError             | Access to this funding source is denied |
| 404  | NotFoundError                     | Funding source is not found             |


## Create a micro deposit
Initiate a micro deposit for bank account verification.

> Request Example

```shell
curl -X POST 
  "https://api.gluwa.com/V4.1/FundingSources/{ID}/MicroDeposits"
  -H "Authorization: Bearer {token}"
```

### Method

| Method | URL                                      |
|--------|------------------------------------------|
| POST    | <code class="highlighter-rouge">/V4.1/FundingSources/{ID}/MicroDeposits</code> |

### Parameters

| Name | Type/Value   | Required | Path/Query | Description        |
|------|--------------|----------|------------|--------------------|
| ID   | String: uuid | yes      | Path       | Funding Source ID |

### Responses

> 201 Created: Guid Response

```json
{
"ID": "string (uuid)"
}
```

> 400 Bad Request: Request syntax malformed

> 403 Forbidden: Access to this microfunding is denied

```json
{

"InnerErrors": [
{
"Code": "string",
"Path": "string",
"Message": "string"
}
],
"Code": "string",
"Message": "string",
"ExtraData": "string"

}
```


> 403 Forbidden: Cannot initiate micro deposit due to invalid state of the funding source 

> 404 Not Found: Founding Source not found

```json
{
"Code": "string",
"Message": "string",
"ExtraData": "string"
}

```

> 503 Service Unavailable: Service is currently unavailable due to bank downtime

```json
{
"InnerErrors": [
{
"Code": "string",
"Path": "string",
"Message": "string"
}
],
"Code": "string",
"Message": "string",
"ExtraData": "string"
}
```
Response Codes


| Code | Type/Value                        | Description                              |
|------|-----------------------------------|------------------------------------------|
| 201  | GuidResponse                      | Microdeposit is initiated for this funding source ID |
| 202  | GuidResponse                      | Microdeposit is initiated for this funding source ID |
| 400  | BadRequest | Request Syntax Malformed                         |
| 403  | ForbiddenRequestError             | Access to this funding source is denied |
| 403  | InvalidResourceStateError         | Cannot initiate micro deposit due to invalid state of the funding source |
| 404  | NotFoundError                     | Funding source is not found             |
| 503  | ServiceUnavailableError           | Service is currently unavailable due to bank downtime |

## Verify a micro deposit
Check if a micro deposit has been completed successfully.

> Request Example

```shell
curl -X PATCH 
  "https://api.gluwa.com/V4.1/FundingSources/{ID}/MicroDeposits"
  -H "Authorization: Bearer {token}"
```

### Method

| Method | URL                                      |
|--------|------------------------------------------|
| PATCH   | <code class="highlighter-rouge">/V4.1/FundingSources/{ID}/MicroDeposits</code> |


### Parameters

| Name | Type/Value   | Required | Path/Query | Description        |
|------|--------------|----------|------------|--------------------|
| ID   | String: uuid | yes      | Path       | Funding Source ID |



### Responses

> 200 OK: Micro deposit is verified.

```json
{}
```

> 400 Bad Request: Verification code is not valid / Request syntax malformed

> 403 Forbidden: Access to this funding source is denied 

```json
{

"InnerErrors": [
{
"Code": "string",
"Path": "string",
"Message": "string"
}
],
"Code": "string",
"Message": "string",
"ExtraData": "string"

}
```

> 403 Forbidden: Cannot verify micro deposit due to invalid funding source state

> 404 Not Found: Funding source is not found / Micro deposit was never initiated

```json
{

"Code": "string",
"Message": "string",
"ExtraData": "string"

}
```


Response Codes

| Code | Type/Value                        | Description                              |
|------|-----------------------------------|------------------------------------------|
| 200  | Void                              | Microdeposit verified.                   |
| 400  | Validation Error                  | Verification code is not valid           |
| 403  | ForbiddenRequestError             | Access to this funding source is denied |
| 403  | InvalidResourceStateError         | Cannot verify micro deposit due to invalid funding source state. |


# Crypto Funding Source
These endpoints are used to create cryptocurrency funding sources under a user or to retrieve cryptocurrency funding sources that belong to a user.

## List crypto funding sources
Retrieve a list of crypto funding sources that belong to the current user.

> Request Example

```shell
curl -X GET 
  "https://api.gluwa.com/V4.1/FundingSources/Crypto"
  -H "Authorization: Bearer {token}"
```

### Method

| Method | URL                                      |
|--------|------------------------------------------|
| GET    | <code class="highlighter-rouge"> /V4.1/FundingSources/Crypto</code> |

### Responses

> 200 OK: List of funding sources

```json

{
"ID": "string (uuid)",
"Country": "string",
"Currency": "string",
"Type": "string",
"Status": "string",
"InstitutionDisplayName": "string",
"Last4DigitsOfAccountNumber": "string",
"CryptoAddress": "string",
"Nickname": "string"
}

*Type: [ Checking, Savings, Crypto ] - This is the type of funding source
*Status: [ NotVerified, Verified, MicroDepositSent, Deactivated, MicroDepositSentFailed ] - Funding source status in Gluwa
```

Response Codes


| Code | Type/Value                        | Description                              |
|------|-----------------------------------|------------------------------------------|
| 200 | Array: FundingSourceResponse           | List of funding sources|

## Verify Crypto Funding Source

We only support this for P2PKH BTC addresses.

> Request Example

```shell
curl -X PATCH
  "https://api.gluwa.com/V4.1/FundingSources/{ID}/SignedMessage"
  -H "Authorization: Bearer {token}"
  -d "{
"Message": "string",
"SignedMessage": "string"
}"
```

### Method

| Method | URL                                      |
|--------|------------------------------------------|
| PATCH    | <code class="highlighter-rouge">/V4.1/FundingSources/{ID}/SignedMessage</code> |

### Request Body


| Property                          | Type                   | Required? | Description |
|-----------------------------------|------------------------|-----------|-------------|
| Message: string (up to 256 chars) | string (256 chars max) | yes       | The message used to create the signed message            |
| SignedMessage: string (88 chars)    | string (88 chars max)  | yes       |Signed message created by using Message and the private key. This is used to verify that the address provided is a legitimate P2PKH address.             |


> Request Body example

```json
{

"Message": "string",
"SignedMessage": "string"

}
```

### Parameters

| Name | Type/Value   | Required | Path/Query | Description        |
|------|--------------|----------|------------|--------------------|
| ID   | String: uuid | yes      | Path       | Funding Source ID |

### Responses

> 200 OK: Funding Source is verified

```json
{}
```

> 400 Bad Request: Signed message is invalid / Request syntax malformed

> 503 Service Error: Service is currently unavailable due to bank downtime


```json
{

"InnerErrors": [
{
"Code": "string",
"Path": "string",
"Message": "string"
}
],
"Code": "string",
"Message": "string",
"ExtraData": "string"

}
```


> 404 Not Found: Funding source is not found

```json

"Code": "string",
"Message": "string",
"ExtraData": "string"

}
```

Response Codes




| Code | Type/Value                | Description                              |
|------|---------------------------|------------------------------------------|
| 200  | Void                      | Microdeposit verified                    |
| 400  | Bad Request               | Request Syntax Malformed                 |
| 400  | Validation Error          | Verification code is not valid           |
| 403  | ForbiddenRequestError     | Access to this funding source is denied  |
| 403  | InvalidResourceStateError | Cannot verify micro deposit due to invalid funding source state |
| 404  | NotFoundError             | Microdeposit was never initiated         |
| 503  | ServiceUnavailableError   | Service is currently unavailable due to bank downtime |


## Create a cryptocurrency funding source
Create a new cryptocurrency funding source for the current user.

> Request Example

```shell
curl -X POST
  "https://api.gluwa.com/V4.1/FundingSources/Crypto"
  -H "Authorization: Bearer {token}"
  -d "{
"Currency": "string",
"Address": "string",
"Message": "string",
"SignedMessage": "string",
"Nickname": "string"
}"
```

### Method

| Method | URL                                      |
|--------|------------------------------------------|
| POST    | <code class="highlighter-rouge">/V4.1/FundingSources/Crypto</code> |

### Request Body


| Property      | Type                          | Required? | Description                              |
|---------------|-------------------------------|-----------|------------------------------------------|
| Currency      | string enum [BTC]             | yes       | Cryptocurrency                           |
| Address       | string range (26 to 35 chars) | yes       | The address                              |
| Message       | string range (256 chars max)  | no        | The message used to create SignedMessage. It has to be less than 256 characters. Only available for P2PKH BTC addresses. |
| SignedMessage | string range (88 chars)       | no        | Signed message created by using Message and the private key. This is used to verify that the address provided is a legitimate P2PKH address. |
| Nickname      | string                        | yes       | User-specified nickname for funding source |

> Request Body example

```json
{

"Currency": "string",
"Address": "string",
"Message": "string",
"SignedMessage": "string",
"Nickname": "string"

}
```

### Responses

> 201 Created: Funding Source is created

```json
{

"ID": "string (uuid)"

}
```

> 400 Bad Request: Request syntax malformed

> 409 Conflict: Funding source already exists

```json
{

  "InnerErrors": [
    {
      "Code": "string",
      "Path": "string",
      "Message": "string"
    }
  ],
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```

Response Codes

| Code | Type/Value                        | Description                   |
|------|-----------------------------------|-------------------------------|
| 201  | GuidResponse                      | Funding source is created    |
| 400  | Bad Request | Request syntax malformed        |
| 409  | Conflict Error                    | Funding Source Already Exists |

# Wallet

A Wallet is where your Gluwacoin is stored. You can also have multiple wallets across multiple countries and currencies. 

The Wallet API resource allows creating, retrieving, and activating locked wallets.

## List wallets

Retrieve a list of wallets for the current user.

> Request Example

```shell
curl -X GET
  "https://api.gluwa.com/V4.1/Wallets"
  -H "Authorization: Bearer {token}"
```

### Method

| Method | URL                                      |
|--------|------------------------------------------|
| GET    | <code class="highlighter-rouge">/V4.1/Wallets</code> |

### Responses

> 200 OK: List of Wallets associated to current user

```json
[
  {
    "ID": "string (uuid)",
    "Balance": "string",
    "Country": "string",
    "Currency": "string",
    "Status": "string"
  }
]
```

> 400 Bad Request

```json
{
    "InnerErrors": [
        {
            "Code": "Other",
            "Path": "string",
            "Message": "string"
        }
    ],
    "Code": "string",
    "Message": "string"
}
```

Response Codes

| Code | Type/Value                        | Description                            |
|------|-----------------------------------|----------------------------------------|
| 200 | Array: Financial Institutions      | List of wallets                         |
| 400  | Bad Request | request syntax malformed |
## Search wallet by ID

> Request Example

```shell
curl -X GET
  "https://api.gluwa.com/V4.1/Wallets/{ID}"
  -H "Authorization: Bearer {token}"
 ```
 
### Method

| Method | URL                                      |
|--------|------------------------------------------|
| GET  | <code class="highlighter-rouge">/V4.1/Wallets/{ID}</code> |

### Parameters

| Name | Type/Value   | Required | Path/Query | Description        |
|------|--------------|----------|------------|--------------------|
| ID   | String: uuid | yes      | Path       | Wallet ID. |


### Responses

> 200 OK: Wallet with the specified ID

```json
{
    "ID": "string",
    "Balance": "string",
    "Country": "string",
    "Currency": "string",
    "Status": "string"
}
```
> 400 Bad Request: Request syntax malformed

> 403 Forbidden: Access to this wallet is denied

```json
{
  "InnerErrors": [
    {
      "Code": "string",
      "Path": "string",
      "Message": "string"
    }
  ],
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```
> 404 Bad Request: Wallet is not found

```json
{
    "Code": "NotFound",
    "Message": "Wallet not found."
}
```

Response Codes


| Code | Type/Value                        | Description                      |
|------|-----------------------------------|----------------------------------|
| 200  | WalletResponse                    | Wallet with the specified ID    |
| 400  | Bad Rquest | Request syntax malformed                 |
| 403  | ForbiddenRequestError             | Access to this wallet is denied |
| 404  | NotFoundError                     | Wallet is not found              |

## Get the fee models associated to a wallet

> Request Example

```shell
curl -X GET
  "https://api.gluwa.com/V4.1/Wallets/{ID}/FeeModel"
  -H "Authorization: Bearer {token}"
```

### Method

| Method | URL                                      |
|--------|------------------------------------------|
| GET    | <code class="highlighter-rouge">/V4.1/Wallets/{ID}/FeeModel</code> |


### Parameters

| Name | Type/Value   | Required | Path/Query | Description        |
|------|--------------|----------|------------|--------------------|
| ID   | String: uuid | yes      | Path       | Funding Source ID. |


### Responses

> 200 OK: List of Fee models
 
```json
{
        "Currency": "string",
        "Country": "string",
        "TransactionType": "string",
        "MinimumFee": "string",
        "Tiers": [
            {
                "FeeRate": "string",
                "MonthlyThresholdAmount": "string"
            }
        ]
    }

*TransactionType: [ Deposit, Send, Receive, Withdraw ] - The fee shown is associated to the displayed type of transaction.

```
> 400 Bad Request: Request syntax malformed

> 403 Forbidden: Access to this wallet is denied

```json
{
  "InnerErrors": [
    {
      "Code": "string",
      "Path": "string",
      "Message": "string"
    }
  ],
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```

> 404 Not Found: Wallet is not found

```json
{
    "Code": "NotFound",
    "Message": "Wallet not found."
}
```

Response Codes


| Code | Type/Value                        | Description                      |
|------|-----------------------------------|----------------------------------|
| 200  | Array <FeeModel>                  | List of fee models              |
| 400  | Bad Request | Request syntax malformed  |
| 403  | ForbiddenRequestError             | Access to this wallet is denied |
| 404  | NotFoundError                     | Wallet is not found  |

## Create a new wallet for the current user

> Request Example

```shell
curl -X POST
  "https://api.gluwa.com/V4.1/Wallets/"
  -H "Authorization: Bearer {Token}"
  -H "Content-Type: application/json"
  -d "{
  "Country": "World",
  "Currency": "USD"
}"
```

### Method

| Method | URL                                      |
|--------|------------------------------------------|
| POST   | <code class="highlighter-rouge"> /V4.1/Wallets</code> |

### Request Body

| Property | Type                         | Required? | Description                             |
|----------|------------------------------|-----------|-----------------------------------------|
| Country  | string enum: [World, US, KR] | yes       | Country that the wallet can be used in |
| Currency | string enum: [KRW, USD]      | yes       | Currency that the wallet can hold      |

> Request Body example

```json
{
  "Country": "string",
  "Currency": "string"
}
```

### Responses

> 201 Created: New Wallet created

```json
{
  "ID": "string (uuid)"
}
```

> 400 Forbidden: Request syntax malformed

> 403 Forbidden: Wallet is locked

```json
{
  "InnerErrors": [
    {
      "Code": "string",
      "Path": "string",
      "Message": "string"
    }
  ],
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```

> 409 Conflict: Wallet with specified country and currency already exists

```json
{
  "ID": "string (uuid)",
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```

Response Codes


| Code | Type/Value                     | Description                              |
|------|--------------------------------|------------------------------------------|
| 201  | Invalid Request                 | New Wallet Created                     |
| 400  | Bad Request | Request syntax malformed                        |
| 403  | ForbiddenRequestError          | Locked wallet found. User must either unlock the locked wallet or remove the locked wallet first |
| 409  | ConflictError                  | Wallet with specified country and currency already exists |
| 503  | ServiceUnavailableError        | Service is currently not available for the specified country and currency |




## Activate a locked wallet 
Either by unlocking it or creating a new wallet. 

Wallet can be unlocked by verifying the most recent payment made, or it can be closed and have its funds transferred to a new wallet.

> Request Example

```shell
curl -X PATCH
  "https://api.gluwa.com/V4.1/Wallets/{WalletID}}/Activation"
  -H "Authorization: Bearer {Token}"
  -H "Content-Type: application/json"
  -d "{
  "Type": "TransferFundsAndDelete",
  "AmountPaid": "string"
}"
```

### Method

| Method | URL                                      |
|--------|------------------------------------------|
| PATCH    | <code class="highlighter-rouge">/V4.1/Wallets/{ID}/Activation</code> |


### Request Body

| Property    | Type                                     | Required? | Description                              |
|-------------|------------------------------------------|-----------|------------------------------------------|
| Type        | string enum: [TransferFundsAndDelete, Unlock] | yes       | Type of Wallet activation request.       |
| Amount Paid | string  | no| Payment amount for the most recent payment made. Only required when unlocking a wallet for verification purposes. |

> Request Body example

```json
{
  "Type": "string",
  "AmountPaid": "string"
}
```

### Parameters

| Name | Type/Value   | Required | Path/Query | Description        |
|------|--------------|----------|------------|--------------------|
| ID   | String: uuid | yes      | Path       | Funding Source ID. |

### Responses

> 200 OK: Wallet is unlocked and activated

```json
{}
```
> 201 Created: Wallet ID of the newly created wallet. The balance is moved to the new wallet

```json
{
  "ID": "string (uuid)"
}
```
> 400 Bad Request: Requested activation type is not supported

> 403 Forbidden: Access to this wallet is denied

```json
{
  "InnerErrors": [
    {
      "Code": "string",
      "Path": "string",
      "Message": "string"
    }
  ],
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```

> 403 Forbidden: Wallet cannot be activated at current status

> 404 Not Found: Wallet is not found

```json
{
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```


Response Codes


| Code | Type/Value                     | Description                              |
|------|--------------------------------|------------------------------------------|
| 200  | Void                           | Wallet is unlocked and activated        |
| 201  | GuidResponse                   | Wallet ID of the newly created wallet. The balance is moved to the new wallet |
| 400  | ValidationError                | Validation error. Please see inner errors for more details |
| 400  | BadRequestError                | Requested activation type is not supported |
| 403  | ForbiddenRequestError          | Access to this wallet is denied         |
| 403  | InvalidResourceStateError      | Wallet cannot be activated at current status |
| 404  | NotFoundError                  | Virtual account not found |

## Get webhook information
You need to be enrolled in our webhook service in order to use this feature.

> Request Example

```shell
curl -X GET
  "https://api.gluwa.com/V4.1/Wallets/{ID}/Webhook"
  -H "Authorization: Bearer {token}"
```

### Method

| Method | URL                                      |
|--------|------------------------------------------|
| GET   | <code class="highlighter-rouge">/V4.1/Wallets/{ID}/Webhook</code> |



### Parameters

| Name | Type/Value   | Required | Path/Query | Description        |
|------|--------------|----------|------------|--------------------|
| ID   | String: uuid | yes      | Path       | Wallet ID. |

### Responses

> 200 OK: Returns the wallet's Webhook

```json
{
  "URL": "string",
  "CreatedDateTime": "string (date-time)",
  "ModifiedDateTime": "string (date-time)",
  "FeeAmount": "string"
}
```
> 400 Bad Request: Request syntax malformed

> 403 Forbidden: Access to this wallet is denied

```json
{
  "InnerErrors": [
    {
      "Code": "string",
      "Path": "string",
      "Message": "string"
    }
  ],
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```
> 404 Not Found: Webhook does not exist

```json
{
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```


Response Codes


| Code | Type/Value                        | Description                      |
|------|-----------------------------------|----------------------------------|
| 200  | WebhookResponse                   | Return the Wallet's Webhook     |
| 400  | Bad Request | Request syntax malformed                 |
| 403  | ForbiddenRequestError             | Access to this wallet is denied |
| 404  | NotFoundError                     | Webhook does not exist          |

## Create or update webhook URL and secret
You need to be enrolled in our webhook service in order to use this feature.

> Request Example

```shell
curl -X PUT
  "https://api.gluwa.com/V4.1/Wallets/{ID}/Webhook"
  -H "Authorization: Bearer {token}"
  -d "{
  "WebhookUrl": "string",
  "Secret": "string"
}"
```
### Method

| Method | URL                                      |
|--------|------------------------------------------|
| PUT   | <code class="highlighter-rouge"> /V4.1/Wallets/{ID}/Webhook</code> |

### Parameters

| Name | Type/Value   | Required | Path/Query | Description        |
|------|--------------|----------|------------|--------------------|
| ID   | String: uuid | yes      | Path       | Wallet ID. |

### Request Body

| Property   | Type                                | Required? | Description                              |
|------------|-------------------------------------|-----------|------------------------------------------|
| WebhookURL | string                              | yes       | URL that Gluwa should send webhook requests to |
| Secret     | string range: (16 to 256 chars max) | yes       | Any request Gluwa sends to WebhookUrl will be signed by using SHA256 HMAC hash of the request body json. Secret is used as the key for SHA256 HMAC hash |

> Request Body example

```json
{
  "WebhookUrl": "string",
  "Secret": "string"
}
```

### Responses

> 200 OK: Webhook is updated

```json
{
  "URL": "string",
  "CreatedDateTime": "string (date-time)",
  "ModifiedDateTime": "string (date-time)",
  "FeeAmount": "string"
}
```
> 201 Accepted: Webhook is created

```json
{
  "URL": "string",
  "CreatedDateTime": "string (date-time)",
  "ModifiedDateTime": "string (date-time)",
  "FeeAmount": "string"
}
```
> 400 Bad Request: Request syntax malformed

> 403 Forbidden: Access to this wallet is denied

```json
{
  "InnerErrors": [
    {
      "Code": "string",
      "Path": "string",
      "Message": "string"
    }
  ],
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```
> 404 Not Found: Wallet does not exist

```json
{
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```


Response Codes


| Code | Type/Value                     | Description                              |
|------|--------------------------------|------------------------------------------|
| 200  | WebhookResponse                | Webhook is updated                      |
| 201  | WebhookResponse                | Webhook is created                      |
| 400  | Bad Requst | Request syntax malformed. Check the format of the wallet ID |
| 403  | ForbiddenRequestError          | Access to this wallet is denied         |
| 404  | NotFoundError                  | Wallet does not exist                   |



## Generate QR code for transaction request
Returns QR code as an image if URL query parameter `format` was passed.
Supported formats are .jpeg and .png . 
Returns QR code as Base64 string if `format` parameter was not passed. You can use the https://www.base64decode.org website to convert the Base64 stirng into a QR code.

> Request Example

```shell
curl -X POST
  "https://api.gluwa.com/V4.1/Wallets/{ID}/QRCode"
  -H "Authorization: Bearer {token}"
  -H "Content-Type: application/json"
  -d "{
  "Amount": "string",
  "MerchantOrderID": "string",
  "Note": "string"
}"
```

### Method

| Method | URL                                      |
|--------|------------------------------------------|
| POST   | <code class="highlighter-rouge">/V4.1/Wallets/{ID}/QRCode</code> |

### Parameters

| Name | Type/Value   | Required | Path/Query | Description        |
|------|--------------|----------|------------|--------------------|
| ID   | String: uuid | yes      | Path       | Wallet ID. |
| format   | String [ .jpeg, .png ]| no      | Query       | Request Body |

### Request Body

| Property        | Type                        | Required? | Description                              |
|-----------------|-----------------------------|-----------|------------------------------------------|
| Amount          | string                      | yes       | Amount to be paid to the merchant        |
| MerchantOrderID | string range (60 chars max) | no        | Optional field for the merchant's order ID |
| Note            | string range (250 char max) | no        | Optional field that will appear on the transaction's note |

> Request Body example

```json
{
  "Amount": "string",
  "MerchantOrderID": "string",
  "Note": "string"
}
```


### Responses

> 200 OK: QR code image with specified format (Void)

```json
{}
```
> 200 OK: QR code image with specified format (String)

```json
"string"
```
> 400 Bad Request: Request syntax malformed / Requested MIME type is not supported

> 403 Forbidden: Access to this wallet is denied

```json
{
  "InnerErrors": [
    {
      "Code": "string",
      "Path": "string",
      "Message": "string"
    }
  ],
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```
> 404 Not Found: Wallet not found

```json
{
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```

Response Codes


| Code | Type/Value                     | Description                           |
|------|--------------------------------|---------------------------------------|
| 200  | Void.                          | QR code image with specified format  |
| 200  | String.                        | List of virtual accounts             |
| 400  | BadRequestError                | Requested MIME type is not supported |
| 403  | ForbiddenRequestError          | Access to this wallet is denied      |
| 404  | NotFoundError                  | Wallet not found                     |

# User

## Search by username

> Request Example

```shell
curl -X GET
  "https://api.gluwa.com/V4.1/Users?searchTerm=john&limit=3"
  -H "Authorization: Bearer {token}"
```

### Method


| Method | URL                                      |
|--------|------------------------------------------|
| GET| <code class="highlighter-rouge"> /V4.1/Users
</code> |

### Request Parameters


| Name       | Type/Value      | Required | Path/Query | Description                              |
|------------|-----------------|----------|------------|------------------------------------------|
| searchTerm | string          | no       | Query      | Username as the search term             |
| offset     | integer (int32) | no       | Query      | Number of users to skip from the beginning of list. Used for pagination. Defaults to 0. |
| limit      | integer (int32) | no       | Query      | Number of users in the result. Defaults to 20, maximum of 50. |


### Responses 

> 200 OK: List of users that contain the search term in their username

```json
[
  {
    "ID": "string (uuid)",
    "Username": "string"
  }
]
```

> 400 Bad Request: Request syntax malformed

```json
{
  "InnerErrors": [
    {
      "Code": "string",
      "Path": "string",
      "Message": "string"
    }
  ],
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```


Response Codes

| Code | Type/Value                               | Description                              |
|------|------------------------------------------|------------------------------------------|
| 200  | Array <User>                  | List of users that contain the search term in their username |
| 400  | Bad Request        | Request syntax malformed                         |


# Transaction 
Transaction represents a transfer of Gluwacoin from a source to a destination. 

Transaction has three types: 
* Deposit -adds funds to a user's wallet from a funding source
* Transfer - moves funds from a user's wallet to another user's wallet
* Withdraw - moves funds from a user's wallet into a funding source owned by the user. 

Transaction API resource allows retrieving a transaction or a list of transactions associated with a user's wallet. It also allows creating a Transfer or Withdraw transaction sent from the current user's wallet.

## List of transactions associated to a wallet
Retrieve a list of transactions sent to or from a specified wallet.

> Request Example

```shell
curl -X GET
  "https://api.gluwa.com/V4.1/Wallets/{walletID}/Transactions?type=Deposit&fromType=Wallet"
  -H "Authorization: Bearer {token}"
```

### Method

| Method | URL                                      |
|--------|------------------------------------------|
| GET  | <code class="highlighter-rouge">/V4.1/Wallets/{walletID}/Transactions </code> |


### Request Parameters



| Name          | Type/Value                               | Required | Path/Query | Description                              |
|---------------|------------------------------------------|----------|------------|------------------------------------------|
| walletID      | String: uuid                             | yes      | Path       | Wallet ID.                               |
| type          | string enum [Deposit, Send, Withdraw, Receive] | no       | Query      | Filter transactions by transaction type. |
| fromType      | string enum [Wallet, VirtualAccount, BankDeposit, CryptoDeposit, FundingSource] | no       | Query      | Filter by type of the "sending" party in a transaction. |
| toType        | string enum [Wallet, VirtualAccount, BankDeposit, CryptoDeposit, FundingSource] | no       | Query      | Filter by type of the "receiving" party in a transaction. |
| startDateTime | string (date-time)                       | no       | Query      | Only include transactions created after this datetime in ISO 8601 format |
| endDateTime   | string (date-time)                       | no       | Query      | Only include transactions created before this datetime in ISO 8601 format |
| offset        | integer (int64)                          | no       | Query      | Number of transactions to skip the beginning of list. Used for pagination. Defaults to 0. |
| limit         | integer (int64)                          | no       | Query      | Number of transactions to include in the result. Defaults to 25, maximum of 50. |

### Responses 

> 200 OK: List of transactions associated with the wallet

```json

  {
        "ID": "string",
        "CreatedDateTime": "string",
        "ModifiedDateTime": "string",
        "Amount": "string",
        "FeeAmount": "string",
        "Type": "string",
        "Status": "string",
        "Currency": "string",
        "From": {
            "Type": "string",
            "DisplayName": "string"
        },
        "To": {
            "Type": "string",
            "DisplayName": "string"
        },
        "Note": "string"
    }

*Type: [ Send, Deposit, Withdraw, Receive ] - The type of transaction
*Status: [ RequestPending, RequestCancelled, Failed, Complete ] = Current status of the transaction
*To/From: 
    **Type: [ GluwaWallet, External ] - Type of entity sending or receiving funds in the transaction.
    ** DisplayName - For GluwaWallet type, this is formatted "USERNAME CURRENCY wallet". For External type, this is the string "Bank Account", localized.
```

> 400 Bad Request: Request syntax malformed

> 403 Forbidden: Access to this wallet is denied

```json
{
  "InnerErrors": [
    {
      "Code": "string",
      "Path": "string",
      "Message": "string"
    }
  ],
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```
> 403 Forbidden: Wallet is locked 

> 404 Not Found: Wallet is not found

```json
{
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```


Response Codes

| Code | Type/Value                        | Description                              |
|------|-----------------------------------|------------------------------------------|
| 200 | Transaction Response               | List of transactions associated with the wallet |
| 400 | InvalidUrlParameters | URL Parameters incorrect                         |
| 403 | ForbiddenRequestError             | Access to this wallet is denied / Wallet is locked        |
| 404 | NotFound                    | Wallet is not found                    |

## Retrieve a transaction by ID and a specified wallet

> Request Example

```shell
curl -X GET
  "https://api.gluwa.com/V4.1/Wallets/{walletID}/Transactions/{transactionID}"
  -H "Authorization: Bearer {token}"
```

### Method

| Method | URL                                      |
|--------|------------------------------------------|
| GET | <code class="highlighter-rouge"> /V4.1/Wallets/{walletID}/Transactions/{transactionID}
</code> |

### Request Parameters

| Name          | Type/Value    | Required | Path/Query | Description |
|---------------|---------------|----------|------------|-------------|
| walletID      | String: uuid  | yes      | Path       | Wallet ID.  |
| transactionID |  String: uuid | yes      | Path       | Transaction |
                                                                                                                               
### Responses 

> 200 OK: Transaction with the specified ID and associated with the wallet

```json
  {
        "ID": "string",
        "CreatedDateTime": "string",
        "ModifiedDateTime": "string",
        "Amount": "string",
        "FeeAmount": "string",
        "Type": "string",
        "Status": "string",
        "Currency": "string",
        "From": {
            "Type": "string",
            "DisplayName": "string"
        },
        "To": {
            "Type": "string",
            "DisplayName": "string"
        },
        "Note": "string"
    }

*Type: [ Send, Deposit, Withdraw, Receive ] - The type of transaction
*Status: [ RequestPending, RequestCancelled, Failed, Complete ] = Current status of the transaction
*To/From: 
    **Type: [ GluwaWallet, External ] - Type of entity sending or receiving funds in the transaction.
    ** DisplayName - For GluwaWallet type, this is formatted "USERNAME CURRENCY wallet". For External type, this is the string "Bank Account", localized.
 
```

> 400 Bad Request: Request syntax malformed

> 403 Forbidden: Access to this wallet is denied

```json
{
    "InnerErrors": [
        {
            "Code": "string",
            "Path": "string",
            "Message": "string"
        }
    ],
    "Code": "string",
    "Message": "string"
}
```

> 403 Forbidden: Wallet is locked

> 404 Not Found: Wallet or transaction is not found

```json
{
    "Code": "string",
    "Message": "string"
}
```

Response Codes


| Code | Type/Value                               | Description                              |
|------|------------------------------------------|------------------------------------------|
| 200  | TransactionResponse                      | Transaction with the specified ID and associated with the wallet. |
| 400  | BadRequest                               | Request syntax malformed                 |
| 403  | ValidationError[ECreateTransactionValidationError] | Access to this wallet is denied / Wallet is Locked |
| 404  | NotFound                                 | Wallet or transaction is not found|

## Calculate transaction fee
Get the fee amount calculated from the amount and the type of transaction.

```shell
curl -X GET
  "https://api.gluwa.com/V4.1/Wallets/{walletID}/Transactions/Fee"
  -H "Authorization: Bearer {token}"
```
### Method


| Method | URL                                      |
|--------|------------------------------------------|
| POST| <code class="highlighter-rouge"> /V4.1/Wallets/{walletID}/Transactions/Fee
</code> |

### Request Body

| Property        | Type                                     | Required? | Description                              |
|-----------------|------------------------------------------|-----------|------------------------------------------|
| Amount          | string                                   | yes       | Transaction amount to calculate the fee for. |
| TransactionType | string enum: (ExactAmountTransfer, Deposit, Withdraw, Receive ) | yes       | Type of transaction to calculate the fee for. |

> Request Body example

```json
{
  "Amount": "string",
  "Type": "string"
}
```
### Request Parameters


| Name          | Type/Value    | Required | Path/Query | Description |
|---------------|---------------|----------|------------|-------------|
| walletID      | String: uuid  | yes      | Path       | Wallet ID  |
                                                                                                                               
### Responses 

> 200 OK: Calculated fee amount

```json
{
"string"
}
```

> 400 Bad Request: Request syntax malformed

> 403 Forbidden: Access to this wallet or transaction is denied

```json
{
  "InnerErrors": [
    {
      "Code": "string",
      "Path": "string",
      "Message": "string"
    }
  ],
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```
> 403 Forbidden: Wallet is locked

> 404 Not Found: Wallet or transaction is not found

```json
{
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```



Response Codes

| Code | Type/Value                     | Description                              |
|------|--------------------------------|------------------------------------------|
| 200  | OK                             | Calculated fee amount                   |
| 400  | Bad Requset | Request syntax malformed                         |
| 403  | ForbiddenRequestError          | Access to this wallet or transaction is denied |
| 403  | WalletLockedError              | Wallet is locked                        |
| 404  | NotFoundError                  | Wallet is not found                      |

## Move funds to/from the current user's wallet
Create a new transaction that moves funds to or from the current user's wallet.

```shell
curl -X POST
  "https://api.gluwa.com/V4.1/Wallets/{walletID}/Transactions"
  -H "Authorization: Bearer {token}"
  -d "{
  "Type": "string",
  "DepositType": "string",
  "WithdrawTo": "string (uuid)",
  "Recipient": "string",
  "Password": "string (password)",
  "Amount": "string",
  "IsExactAmountTransfer": "boolean",
  "ExactAmountTransferFee": "string",
  "Note": "string",
  "MerchantOrderID": "string"
}"
```
### Method


| Method | URL                                      |
|--------|------------------------------------------|
| POST | <code class="highlighter-rouge">/V4.1/Wallets/{walletID}/Transactions
</code> |

### Request Body


| Property               | Type                                     | Required? | Description                              |
|------------------------|------------------------------------------|-----------|------------------------------------------|
| Type                   | string enum: [Transfer, Withdraw, Deposit] | yes       | Type of transaction requested - Transfer, Deposit, or Withdraw. |
| DepositType            | string enum: [Bank Deposit]              | no        | Type of deposit transaction requested. Required for Deposit. |
| WithdrawTo             | string (uuid)                            | no        | FundingSource ID to withdraw money to. Required for Withdraw |
| Recipient              | string                                   | no        | Recipient's username, email address or phone number in E164 format. Required for Transfer. |
| Password               | string (password)                        | no        | User password                            |
| Amount                 | string range: x≥0                        | yes       | Transaction amount. For Transfers, by default this is the amount that will be charged to the sender, and the receiver will receive this amount minus the fee. For exact-amount transfers, enable IsExactAmountTransfer and include the ExactAmountTransferFee. For Withdrawals, this is the amount that will be withdrawn from the user's wallet; the user's funding source will receive this amount MINUS the fee. Supports up to 2 decimal places for applicable currencies. |
| IsExactAmountTransfer  | boolean                                  | no        | For Transfers, this enables sending an exact amount instead of having the fee deducted from the amount specified in the Amount field. By default 'false'. If 'true', fee is charged to the sender in addition to the Amount value, and the receiver receives the Amount value. |
| ExactAmountTransferFee | string range: x≥0                        | no        | Fee amount for exact-amount Transfers. If provided, this value must match the value calculated by Gluwa. |
| Note                   | string range (up to 250 chars)           | no        | Extra information to attach to transaction. |
| MerchantOrderID        | string range (up to 60 chars)            | no        | Extra information to attach to transaction. |

> Request Body Example

```json
{
  "Type": "string",
  "DepositType": "string",
  "WithdrawTo": "string (uuid)",
  "Recipient": "string",
  "Password": "string (password)",
  "Amount": "string",
  "IsExactAmountTransfer": "boolean",
  "ExactAmountTransferFee": "string",
  "Note": "string",
  "MerchantOrderID": "string"
}
```

### Request Parameters

| Name | Type/Value   | Required | Path/Query | Description        |
|------|--------------|----------|------------|--------------------|
| walletID   | String: uuid | yes      | Path       | Wallet ID. |

### Responses 

> 201 Created: Newly created transaction

```json

  {
        "ID": "string",
        "CreatedDateTime": "string",
        "ModifiedDateTime": "string",
        "Amount": "string",
        "FeeAmount": "string",
        "Type": "string",
        "Status": "string",
        "Currency": "string",
        "From": {
            "Type": "string",
            "DisplayName": "string"
        },
        "To": {
            "Type": "string",
            "DisplayName": "string"
        },
        "Note": "string"
    }

*Type: [ Send, Deposit, Withdraw, Receive ] - The type of transaction
*Status: [ RequestPending, RequestCancelled, Failed, Complete ] = Current status of the transaction
*To/From: 
    **Type: [ GluwaWallet, External ] - Type of entity sending or receiving funds in the transaction.
    ** DisplayName - For GluwaWallet type, this is formatted "USERNAME CURRENCY wallet". For External type, this is the string "Bank Account", localized.
```

> 400 Bad Request: Request syntax malformed / Recipient cannot accept transfer. This can happen if recipient cannot accept transfers from your wallet's country and currency at this time.

> 403 Forbidden: Access to this wallet is denied

> 409 Conflict: There already exists a requested bank deposit transaction

```json
{
  "InnerErrors": [
    {
      "Code": "string",
      "Path": "string",
      "Message": "string"
    }
  ],
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```

> 403 Forbidden: Wallet is suspended / Wallet is locked

> 404 Not Found: Wallet is not found

```json
{
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```


Response Codes

| Code | Type/Value                               | Description                              |
|------|------------------------------------------|------------------------------------------|
| 201  | TransactionResponse                      | Newly created transacation. |
| 400  | BadRequest           | Request syntax malformed                       |
| 400  | Recipient CannotAcceptTransfer           | Recipient cannot accept transfer. This can happen if recipient cannot accept transfers from your wallet's country and currency at this time.         |
| 403  | ForbiddenRequestError                    | Access to this wallet is denied                     |               |
| 403  | WalletLockedError                        | Wallet is locked.                        |
| 404  | NotFoundError                            | Wallet is not found.                     |
| 409  | ConflictError                            | There already exists a requested bank deposit transaction. |

## Cancel requested deposit
Cancel a deposit transaction that has previously been requested.

> Request Example

```shell
curl -X PATCH
  "https://api.gluwa.com/V4.1/Wallets/{WalletID}/Transactions/{TransactionID}"
  -H "Authorization: Barer {Token}"
  -H "Content-Type: application/json"
  -d "{
  "Action": "Cancel"
}"
```
### Method


| Method | URL                                      |
|--------|------------------------------------------|
| PATCH| <code class="highlighter-rouge">  /V4.1/Wallets/{walletID}/Transactions/{transactionID}
</code> |

### Parameters

| Name          | Type/Value    | Required | Path/Query | Description |
|---------------|---------------|----------|------------|-------------|
| walletID      | String: uuid  | yes      | Path       | Wallet ID.  |
| transactionID |  String: uuid | yes      | Path       | Transaction |
                                                                         
### Request Body


| Property | Type                                     | Required? | Description                              |
|----------|------------------------------------------|-----------|------------------------------------------|
| Action   | String: [Cancel]                          | yes       | Transaction to be canceled |

> Request Body example

```json
{
  "Action": "string"
}
```
 
### Responses 

> 200 OK: Transaction has been successfully canceled

```json
{}
 
```

> 400 Bad Request: Request syntax malformed

> 403 Forbidden: Access to this wallet is denied / 404 Not Found: Wallet or transaction is not found

```json
{
  "InnerErrors": [
    {
      "Code": "string",
      "Path": "string",
      "Message": "string"
    }
  ],
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```

> 404 Not Found: Wallet or transaction is not found

```json
{
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```


Response Codes


| Code | Type/Value                     | Description                              |
|------|--------------------------------|------------------------------------------|
| 200  | Void                           | Transaction has been successfully cancelled. |
| 400  | Bad Request | Request syntax malformed                         |
| 403  | ForbiddenRequestError          | Access to this wallet is denied.         |
| 403  | InvalidResourceStateError      | Transaction does not have a valid status to be cancelled. |
| 404  | NotFoundError                  | Wallet or transaction is not found.      |

## Get a quote for crypto depositing and withdrawing
Get a quote for crypto deposit and withdraw transactions.

> Request Example

```shell
curl -X POST
  "https://api.gluwa.com/V4.1/Wallets/{WalletID}/Quote"
  -H "Authorization: Barer {Token}"
  -H "Content-Type: application/json"
  -d "{
  "Amount": "string",
  "TransactionType": "Deposit",
  "CryptoCurrency": "BTC",
  "Password": "{password}"
}"
```

### Method

| Method | URL                                      |
|--------|------------------------------------------|
| POST| <code class="highlighter-rouge"> /V4.1/Wallets/{walletID}/Quote
</code> |

### Request Body


| Property        | Type                             | Required? | Description                              |
|-----------------|----------------------------------|-----------|------------------------------------------|
| Amount          | string                           | yes       | The value to be quoted                   |
| TransactionType | string enum: (Deposit, Withdraw) | yes       | Type of transaction that the quote is to be used for |
| CryptoCurrency  | string enum: [BTC]               | yes       | Cryptocurrency for the quote amount to be provided |
| Password        | string                           | no        | User password for verification          |

> Request Body example

```json
{
  "Amount": "string",
  "TransactionType": "string",
  "CryptoCurrency": "string",
  "Password": "string (password)"
}
```
### Request Parameters


| Name          | Type/Value    | Required | Path/Query | Description |
|---------------|---------------|----------|------------|-------------|
| walletID      | String: uuid  | yes      | Path       | Wallet ID.  |
                                                                                                                               
### Responses 

> 200 OK: Get he quote and checksum

```json
{
  "SourceAmount": "string",
  "TargetAmount": "string",
  "TargetPrice": "string",
  "SourceCurrency": "string",
  "TargetCurrency": "string",
  "TransactionType": "string",
  "CreatedDateTime": "string (date-time)",
  "TimeToLive": "integer (int64)",
  "Checksum": "string"
}

*SourceAmount - Currency amount that the quote is requested with
*TargetAmount - Cryptocurrency amount that corresponds to the requested currency amount
*TargetPrice - Cryptocurrency price in source currency
*SourceCurrency - Currency for the SourceAmount parameter
*TargetCurrency - Cryptocurrency for the TargetAmount parameter
*TransactionType - Type of transaction that the quote is for
*CreatedDateTime - Date and time when the quote is created
*TimeToLive - Amount of time in seconds in which the quote is valid, from the time when the quote is created
*Checksum - Checksum for the Quote object. Must be obtained from Gluwa via the Quote endpoint.
```

> 400 Bad Request: Request syntax malformed

> 403 Forbidden:  Access to this wallet or transaction is denied

```json
{
  "InnerErrors": [
    {
      "Code": "string",
      "Path": "string",
      "Message": "string"
    }
  ],
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```

> 403 Forbidden: Wallet is locked

> 404 Not Found: Wallet or transaction is not found

```json
{
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```


Response Codes


| Code | Type/Value                               | Description                              |
|------|------------------------------------------|------------------------------------------|
| 200  | QuoteResponse                            | Created quote and checksum.              |
| 400  | Bad Request           | Request syntax Malformed                        |
| 403  | ForbiddenRequestError                    | Access to this wallet or transaction is denied. |
| 403  | WalletLockedError                        | Wallet is locked.                        |
| 404  | NotFoundError                            | Wallet is not found                      |


## Create a new transaction using quote provided
Create a new transaction with the quote that was previously provided by Gluwa.

> Request Example

```shell
curl -X POST
  "https://api.gluwa.com/V4.1/Wallets/{walletID}/Quote"
  -H "Authorization: Bearer {token}"
  -H "Content-Type: application/json"
  -d "{
  "SourceAmount": "string",
  "TargetAmount": "string",
  "TargetPrice": "string",
  "SourceCurrency": "string",
  "TargetCurrency": "string",
  "TransactionType": "string",
  "WithdrawTo": "string (uuid)",
  "CreatedDateTime": "string (date-time)",
  "TimeToLive": "integer (int64)",
  "Checksum": "string"
}"
```

### Method

| Method | URL                                      |
|--------|------------------------------------------|
| POST| <code class="highlighter-rouge">/V4.1/Wallets/{walletID}/Transactions/Quote
</code> |

### Request Body

| Property        | Type                             | Required? | Description                              |
|-----------------|----------------------------------|-----------|------------------------------------------|
| SoruceAmount    | string                           | yes       | Currency amount that the quote is requested with. |
| TargetAmount    | string                           | yes       | Cryptocurrency amount that corresponds to the requested currency amount. |
| TargetPrice     | string                           | yes       | Cryptocurrency price in source currency. |
| TargetCurrency  | string enum: [BTC]               | no        | Currency for the SourceAmount parameter. |
| SourceCurrency  | string enum: [KRW, USD]          | yes       | Cryptocurrency for the TargetAmount parameter. |
| TransactionType | string enum: [Deposit, Withdraw] | yes       | Type of transaction that the quote is to be used for. |
| WithdrawnTo     | string (uuid)                    | yes       | FundingSource ID to withdraw money to. Required for Withdraw. |
| CreatedDateTime | string (Date-Time)               | no        | Date and time when the quote is created. |
| TimeToLive      | integer (int64)                  | yes       | Amount of time in seconds in which the quote is valid, from the time when the quote is created. |
| Checksum        | string                           | yes       | Checksum for the Quote object. Must be obtained from Gluwa via the Quote endpoint. |

> Request Body example

```json
{
  "SourceAmount": "string",
  "TargetAmount": "string",
  "TargetPrice": "string",
  "SourceCurrency": "string",
  "TargetCurrency": "string",
  "TransactionType": "string",
  "WithdrawTo": "string (uuid)",
  "CreatedDateTime": "string (date-time)",
  "TimeToLive": "integer (int64)",
  "Checksum": "string"
}
```
### Request Parameters


| Name          | Type/Value    | Required | Path/Query | Description |
|---------------|---------------|----------|------------|-------------|
| walletID      | String: uuid  | yes      | Path       | Wallet ID.  |
                                                                                                                               
### Responses 

> 200 OK: Transaction created 

```json
{
  "ID": "string (uuid)",
  "CreatedDateTime": "string (date-time)",
  "ModifiedDateTime": "string (date-time)",
  "Amount": "string",
  "FeeAmount": "string",
  "Type": "string",
  "Status": "string",
  "Currency": "string",
  "From": {
    "Type": "string",
    "DisplayName": "string"
  },
  "To": {
    "Type": "string",
    "DisplayName": "string"
  },
  "Note": "string"
}

*Type: [ Send, Deposit, Withdraw, Receive ] - The type of transaction
*Status: [ RequestPending, RequestCancelled, Failed, Complete ] = Current status of the transaction
*To/From: 
    **Type: [ GluwaWallet, External ] - Type of entity sending or receiving funds in the transaction.
    ** DisplayName - For GluwaWallet type, this is formatted "USERNAME CURRENCY wallet". For External type, this is the string "Bank Account", localized.
```

> 400 Bad Request: Request syntax malformed

> 403 Forbidden: Access to this wallet is denied

> 409 Conflict: There already exists a requested crypto deposit transaction. Cancel the pending transaction or wait until it expires

```json
{
  "InnerErrors": [
    {
      "Code": "string",
      "Path": "string",
      "Message": "string"
    }
  ],
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```

> 403 Forbidden: Wallet is suspended

> 404 Not Found: Wallet not found

```json
{
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```


Response Codes

| Code | Type/Value                               | Description                              |
|------|------------------------------------------|------------------------------------------|
| 201  | TransactionResponse                      | Newly created transaction               |
| 400  | Bad request           | Request syntax malformed                        |
| 400  | ValidationError[ECreateTransactionValidationError] | Validation error. See inner errors for more details |
| 403  | ForbiddenRequestError                    | Access to this wallet or transaction is denied |
| 403  | WalletSuspendedError                     | Wallet is suspended                    |
| 403  | WalletLockedError                        | Wallet is locked                         |
| 404  | NotFoundError                            | Wallet not found                         |
| 409  | ConflictError                            | There already exists a requested crypto deposit transaction. Cancel the pending transaction or wait until it expires. |

# Virtual Account

** Korean Users ONLY. **     

Virtual Account is a virtual bank account that belongs to Gluwa. Each user will get a dedicated virtual bank account number with his/her wallet. 

When fiat currency is deposited into this virtual bank account, Gluwa will accredit the user's wallet with the same amount in Gluwacoin. The VirtualAccount resource can be used to view virtual accounts linked to a wallet.

## Get virtual accounts linked to a wallet

> Request Example

```shell
curl -X GET
  "https://api.gluwa.com/V4.1/Wallets/{walletID}/VirtualAccounts"
  -H "Authorization: Bearer {token}"
```

### Method


| Method | URL                                      |
|--------|------------------------------------------|
| GET| <code class="highlighter-rouge"> /V4.1/Wallets/{walletID}/VirtualAccounts </code> |

### Request Parameters


| Name       | Type/Value      | Required | Path/Query | Description                              |
|------------|-----------------|----------|------------|------------------------------------------|
| walletID | string (uuid)      | yes       | Path     | Wallet ID             |
   
### Responses 

> 200 OK: List of virtual accounts

```json
[
  {
    "InstitutionName": "string",
    "VirtualAccountNumber": "string",
    "DepositorName": "string"
  }
]
```

> 400 Bad Request: Request syntax malformed

> 403 Forbidden: Access to this wallet is denied

```json
{
  "InnerErrors": [
    {
      "Code": "string",
      "Path": "string",
      "Message": "string"
    }
  ],
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```

> 403 Forbidden: Identity verification required / Wallet is locked / Account cannot be used in Korea

> 404 Not Found: Wallet is not found

```json
{
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```




Response Codes

| Code | Type/Value                        | Description                              |
|------|-----------------------------------|------------------------------------------|
| 200  | Array <VirtualAccountResponse>    | List of virtual accounts                |
| 400  | Bad Request   | Request syntax error                       |
| 403  | IdentityVerificationRequiredError | Identity verification is required to use virtual accounts |
| 403  | WalletLockedError                 | Your wallet is locked                   |
| 403  | ForbiddenRequestError             | Access to this wallet is denied         |
| 403  | VirtualAccountsNotSupportedError  | Virtual accounts are not supported because this wallet cannot be used in Korea |
| 404  | NotFoundError                     | Wallet is not found                     |

# Glossary 

| Term                  | Definition |
|-----------------------|------------------------------------------|
| Cryptocurrency        | A digital asset which uses cryptography that is designed to work as a medium for secure financial transcations. For example, BitCoin |
| Gluwacoin             | The digital currency used by Gluwa to store funds and make transactions to other Gluwa accounts |
| Fiat                  | The type of currency officially used within countries, both digital and physical. For example, the United States Dollar or the South Korean Won. |
| Bank                  | Finanical institution which allows for depositing and transfering of fiat money. For example, Bank of America, Bank of Korea |
| Funding source        | The source of fiat currency. For example bank accounts and credit cards |
| Crypto funding source | The source of cryptocurrency             |
| Micro deposit         | A small mount of money (0.01$ US) sent by Gluwa to your bank account containing information to verify your account. |
| Wallet                | Secure storage where a user's Gluwa coin is kept. |
| Transaction           | Process of transfering  funds from a source to a destination. Transcations can be either deposits, transfers, or withdrawals |
| Deposit               | Process of adding funds to a user's wallet from a funding source |
| Transfer              | Process of moving Gluwacoin from a user's wallet to another user's wallet |
| Withdrawal            | Process of moving Gluwacoin from a user's wallet into a funding source owned by the user. |
| Virtual account       | A virtual bank account that belongs to Gluwa and is associated with a user's wallet. |

