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
# Gluwa API Documentation
**Table of Contents**

* [Gluwa Overview](#gluwa-overview)
* [Getting Started with Gluwa API](#getting-started-with-gluwa-api)
	* [Auth API and Gluwa API](#auth-api-and-gluwa-api)
	* [OpenID Connect](#openid-connect)
* [Endpoints and Resources](#endpoints-and-resources)
* [Bank](#bank)
	* [List Banks](#list-banks)
	* [Search Banks](#search-banks)
* [Funding Source](#funding-source)
	* [Show a funding source](#show-a-funding-source)
	* [Deactivate a funding source.](#deactivate-a-funding-source)
	* [List funding sources](#list-funding-sources)
	* [Create a micro deposit](#create-a-micro-deposit)
	* [Verify a micro deposit](#verify-a-micro-deposit)
* [Crypto Funding Source](#crypto-funding-source)
	* [Verify Crypto Funding Source.](#verify-crypto-funding-source)
	* [List crypto funding sources](#list-crypto-funding-sources)
	* [Create a cryptocurrency funding source.](#create-a-cryptocurrency-funding-source)
* [Transaction](#transaction)
	* [List of transactions associated to a wallet.](#list-of-transactions-associated-to-a-wallet)
	* [Move funds to/from the current user's wallet](#move-funds-tofrom-the-current-users-wallet)
	* [Retrieve a transaction by ID and a specified wallet.](#retrieve-a-transaction-by-id-and-a-specified-wallet)
	* [Cancel requested deposit](#cancel-requested-deposit)
	* [Calculate transaction fee](#calculate-transaction-fee)
	* [Get a quote for depositing and withdrawing](#get-a-quote-for-depositing-and-withdrawing)
	* [Create a new transaction using quote provided](#create-a-new-transaction-using-quote-provided)
* [User](#user)
	* [Search by username](#search-by-username)
* [Virtual Account](#virtual-account)
	* [Get virtual accounts linked to a wallet](#get-virtual-accounts-linked-to-a-wallet)
* [Wallet](#wallet)
	* [List wallets](#list-wallets)
	* [Create a new wallet for the current user.](#create-a-new-wallet-for-the-current-user)
	* [Search wallet by ID](#search-wallet-by-id)
	* [Activate a locked wallet](#activate-a-locked-wallet)
	* [Get the fee models associated to a wallet.](#get-the-fee-models-associated-to-a-wallet)
	* [Get webhook information](#get-webhook-information)
	* [Create or update webhook URL and secret](#create-or-update-webhook-url-and-secret)
	* [Generate QR code for transaction request](#generate-qr-code-for-transaction-request)

# Gluwa Overview

Gluwa is a fiat-pegged cryptocurrency wallet that provides *Bitcoin* deposit and withdrawal. The Gluwa wallet enables users to process national and international transactions, allowing for secure management of money, credit and identity.

Through Gluwa's public-facing REST API, users can:

* Get information about supported Banks
* Manage funding sources
* Manage transactions
* Manage wallet details


# Getting Started with Gluwa API

## Get an Access Token from Auth API

> Example request

```shell
curl https://auth.gluwa.com/connect/token \
-X POST \
-H 'Content-Type: application/x-www-form-urlencoded' \
-d '{
    "client_id": "8a1c2424-fe73-473a-828f-fb848a3c2cc9",
    "client_secret": "Testing123!",
    "grant_type: "password",
    "username": "gTester1",
    "password": "V0Jkw0nZnz",
    "scope": "wallet.read transaction.read"
}'
```

> Response Token

```json
{
"access_token":"eyJhbGciOiJSUzI1NiIsImtpZCI6IkQyOEJENjcwOTI5QTBDN0U5MTMyRkE4NDgwRUMwNDlDMDBFRTIxQkYiLCJ0eXAiOiJKV1QiLCJ4NXQiOiIwb3ZXY0pLYURINlJNdnFFZ093RW5BRHVJYjgifQ.eyJuYmYiOjE1NTcwMjgyNTgsImV4cCI6MTU1NzAzMTg1OCwiaXNzIjoiaHR0cHM6Ly9hdXRoLmdsdXdhLmNvbSIsImF1ZCI6WyJodHRwczovL2F1dGguZ2x1d2EuY29tL3Jlc291cmNlcyIsIkdsdXdhQXBpIl0sImNsaWVudF9pZCI6IjhhMWMyNDI0LWZlNzMtNDczYS04MjhmLWZiODQ4YTNjMmNjOSIsInN1YiI6ImE2MDkxN2Q5LTRlNWQtNGE0Yi04MGZjLTEwY2FmYmM3MWYzOSIsImF1dGhfdGltZSI6MTU1NzAyODI1OCwiaWRwIjoibG9jYWwiLCJJc1Rlc3RVc2VyIjoiVHJ1ZSIsImVtYWlsX3ZlcmlmaWVkIjp0cnVlLCJwaG9uZV9udW1iZXJfdmVyaWZpZWQiOnRydWUsInNjb3BlIjpbInRyYW5zYWN0aW9uLnJlYWQiLCJ3YWxsZXQucmVhZCJdLCJhbXIiOlsicHdkIl19.gEK1SnM-FKyFR_rvF4WqI5zbwMfRI9lbJIh93wYvMruOzhtkv1m6m2VbwNGQEaxoNHuPNCbcuxXUa6I4TZXnKwMvSWAxXBXC9WoVU9cbtT4Q2nm8Y9-EfkUCOncaYX_ERbDetGjN4joiOyPAjRluKDBU491_uh66DHmAWGsP8o_JFI7tOk8yGKI2p8SUlvP_Y2YcfviAyjkhUOcFEuzTT1laB10CcrXMjUQyygiSU_HGHcskGcndig9HHSdI55LDNOgoCe_111U2AsPmZDzKPMRJ2RfFP3Y2AHB3lmDZvEILvuvJBcHtBEoEhRgZt5vk1arEC0o0cNmC42LBc1uFEQ",
"expires_in":3600,
"token_type":"Bearer"
}
```

In order to obtain an access token using the below method, you need a Client ID, Client Secret, Username and Password.

In this example, we’ll use the following information:

* **Client ID:** `8a1c2424-fe73-473a-828f-fb848a3c2cc9`
* **Client Secret:** `Testing123!`
* **Username:** `gTester1`
* **Password:** `V0Jkw0nZnz`

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
| `client_id`          | String        | yes            | Identifier of the client                 |
| `client_secret`  | String        | yes | Secret key of the client |
| `grant_type`        | String        | yes | Type of the grant |
| `username`            | String        | yes | Username of the client |
| `password`            | String        | yes | Password of the client |
| `scope`                  | String        | yes | One or more registered scopes.           |


# OpenID Connect
Gluwa Auth server uses IdentityServer4 to authenticate/authorize its users. It strictly follows the specification laid out in [OpenID Connect Core 1.0](http://openid.net/specs/openid-connect-core-1_0.html).

## Authorization Code flow

This flow is suitable for Clients that can maintain a Client Secret between themselves and the Authorization Server. For example, clients that have a backend server to keep the client secret safe.

For details of the spec, please see [here](http://openid.net/specs/openid-connect-core-1_0.html#CodeFlowAuth). Note that not all OpenID connect features may be supported.

### Step 1. Prepare a request to Authorize Endpoint.

For request specification, please look [here](http://docs.identityserver.io/en/aspnetcore1/endpoints/authorize.html).

> Example request

```shell
curl https://auth.gluwa.com/connect/authorize?client_id=example_client&scope=openid%20GluwaApi&response_type=code&redirect_uri=https://redirect-after-login.com&state=abcd \
-X GET
```

### Method 

| Method | URL                                      |
|--------|------------------------------------------|
| GET    | <code class="highlighter-rouge">https://auth.gluwa.com/connect/authorize</code> |

### Parameters

| Name          | Type/Value                   | Required | Path/Query | Description                              |
|---------------|------------------------------|----------|------------|------------------------------------------|
| `client_id`     | String  | yes      | Query      | Identifier of the client                 |
| `scope`         | String  | yes      | Quey       | One or more registered scopes.           |
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
curl https://auth.gluwa.com/connect/token \
-X POST \
-H 'Content-Type: application/x-www-form-urlencoded' \
-d 'grant_type=authorization_code&code=<your_authorization_code>&client_id=<your_client_id>&client_secret=<your_client_secret>&redirect_uri=<your_redirect_uri_used_in_1>'
```

> Response

```json
{
    "id_token": "eyJhbGciOiJSUzI1NiIsImtpZCI6IjJhOTEyMWM1ZWI2OGEwNjM5ODYzMjI1YmI1ZTRkYjUyIiwidHlwIjoiSldUIn0.eyJuYmYiOjE1MTAwNzkxNTgsImV4cCI6MTUxMDA3OTQ1OCwiaXNzIjoiaHR0cHM6Ly9sb2NhbGhvc3Q6NDQzOTgiLCJhdWQiOiJtdmMiLCJpYXQiOjE1MTAwNzkxNTgsImF0X2hhc2giOiJyTHhBUmdqeDhnSjg0TG8tZHh5NHd3Iiwic2lkIjoiYTA4Nzg3ZWFhNjU2OTFmNDQ4YTI0YmVhZjRjNWFkYmYiLCJzdWIiOiJjMGQxOGMyNy1iMGNkLTQ0MjgtOWEwYi05N2MyNjBjYmFjM2MiLCJhdXRoX3RpbWUiOjE1MTAwNzkxNTcsImlkcCI6ImxvY2FsIiwiYW1yIjpbInB3ZCJdfQ.HzDzwnx0sI2WJ5mo8OtiXkUP2pLIfrIZhiKVElVTc5M9WE0SaO8Xnr4WzwltEIQNcDtYkn4-rkLp1AKRk6Xf1RvAiIzKbTtz9YrReZPkXvyIerJIkRmF0agD-z-JSHF7HZ2NqKAxrQHHMwRrlvMrBIUd2pDhjzd2A0kVsqRTQvmXWUqsv5Ig_h6-OMYSyUYkNNEkEG8kPB2qmd3VcRy1jLGL8hrpnAu8mRoYZuxycBepmCvHnWbV0_3cWlNZQmSg8U-tYgdHQVkL5ees9j2SVhw4-YbQY7BXebuWTTPET8IzXZsAagHUnOk7cq6KhMV_sw6QTpiyTChYtW7NQkzGnQ",
    "access_token": "eyJhbGciOiJSUzI1NiIsImtpZCI6IjJhOTEyMWM1ZWI2OGEwNjM5ODYzMjI1YmI1ZTRkYjUyIiwidHlwIjoiSldUIn0.eyJuYmYiOjE1MTAwNzkxNTgsImV4cCI6MTUxMDA4Mjc1OCwiaXNzIjoiaHR0cHM6Ly9sb2NhbGhvc3Q6NDQzOTgiLCJhdWQiOlsiaHR0cHM6Ly9sb2NhbGhvc3Q6NDQzOTgvcmVzb3VyY2VzIiwiR2x1d2FBcGkiXSwiY2xpZW50X2lkIjoibXZjIiwic3ViIjoiYzBkMThjMjctYjBjZC00NDI4LTlhMGItOTdjMjYwY2JhYzNjIiwiYXV0aF90aW1lIjoxNTEwMDc5MTU3LCJpZHAiOiJsb2NhbCIsImNvdW50cnkiOiJLUiIsInVzZXJuYW1lIjoiY2hyaXMueW9vbi50ZXN0IiwiZW1haWwiOiJjaHJpcy55b29uQGdsdXdhLmNvbSIsInBob25lX251bWJlciI6IisxNzc4Mzg3MTYxMCIsImVtYWlsX3ZlcmlmaWVkIjoidHJ1ZSIsInBob25lX251bWJlcl92ZXJpZmllZCI6InRydWUiLCJzY29wZSI6WyJvcGVuaWQiLCJHbHV3YUFwaSJdLCJhbXIiOlsicHdkIl19.RNrG6nHHlQtFuT3MaA7c7NceBBxUFvhfdYxqKJXSsB-1SkwWvFP-GJyV5n3Wdf7PtZmGmDO8QHTSuYMCsq-ZbBYZLhi5emBU1L42kjRT-3JXTjVmCHS3ED2z5sM6L2QOmDhZjkGRxXkRMQbVp6QJrUueVnez_txAGEX8jkIw7Zi56TBlLV0FFUXXoBvxIebzIv4ChbtN99ll9u3gnzGacsIbWzUt23yKBgTKnQTJ1AkngaNgk1Odz7cRAegH6d9m4IiZo8yUrGbMax0N7lEkWrWkbC3L_29IntUu6K54NTBoW5ukSaY06zLB5jGTeVvMronNqQyplXbdHruw87bViQ",
    "expires_in": 3600,
    "token_type": "Bearer"
}
```

> Note that the `access_token` and `id_token` is in JWT format. 
> To make a request to Gluwa's public API, use the `access_token` and attach it into Authorization header as Bearer scheme.

### Request


| Method | URL                    | HOST           | Content-Type                      |
|--------|------------------------|----------------|-----------------------------------|
| POST   | connect/token | auth.gluwa.com | application/x-www-form-urlencoded |

**Make sure that the requests to the auth server are always using HTTPS.**


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
curl https://auth.gluwa.com/connect/authorize?client_id=example_client&scope=openid%20GluwaApi&response_type=code%20id_token&redirect_uri=https://redirect-after-login.com&state=abcd&nonce=1234 \
-X GET
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
curl https://redirect-after-login.com/#code=12365sdfsdf445&id_token=sdlfiqlk1231568654&scope=openid%20GluwaApi&state=abcd&session_state=sdfio23123 \
-X GET
```

In hybrid flow all response parameters are added to the fragment component (component after `#` in the URL of the `redirect_uri` Fragment parameters will depend on what you've put as `response_type` in step 1. Following the example given in step 1, the query parameters will be:

- state
- code
- id_token

(since we only requested for code and `id_token` )

`id_token` must be validated before usage as shown in the next step (step 4). After the validation is successful, you can use the code to obtain `access_token` through the token endpoint as shown in step 4 of Authorization Code flow.

Remembe to validate the state field as well.

### Step 4. Validate ID token and Access Token

Before using the ID Token and Access Token, client must validate the tokens as laid out in [OpenID Connect Core 1.0, section 3.1.3.7 and 3.1.3.8](http://openid.net/specs/openid-connect-core-1_0.html#IDTokenValidation). It is up to the client how to validate the tokens, which is outside the scope of this document.

### 5. Sign in to your app

The last step is to sign in to your own app so that you can navigate around secured parts of your app. 

## Client Credentials As User

> Example request

```shell
curl https://auth.gluwa.com/connect/token \
-X POST \
-H 'Content-Type: application/x-www-form-urlencoded' \
-d 'client_id={client_id}&client_secret={client_secret}&scope={scopes}&grant_type=client_credentials_as_user'
```

This flow is used for users who would like to use our API with their client credentials (log in using client id and secret). We assign the user's Gluwa account to a specific client, and the user will be able to login to that account using client id and secret.

You can send the request to the auth server as follows:

| Method | URL                    | Host           | Content-Type | Cache Control |
|--------|------------------------|----------------|--------------|---------------|
| POST   | connect/token | auth.gluwa.com | application/x-www-form-urlencoded | no-cache      |


## Logging out

Logout from your own client, not the Gluwa Auth server. 

Should you really want to logout from both your own client and on Gluwa Auth server, you can use the following endpoints to do so.

**Note that if you do decide to use these endpoints, you are logging the user out completely from Gluwa Auth system, meaning that they will also be logged out of other clients that may not be in your control.**

## Revocation endpoint

> Example request

```shell
curl https://auth.gluwa.com/connect/revocation \
-X POST \
-H 'Content-Type: application/x-www-form-urlencoded' \
-d 'token=<your_refresh_token>&token_type_hint=refresh_token&client_id=<your_client_id>&client_secret=<your_client_secret>'
```

You can revoke token by:

| Method | URL                         | Host           | Content-Type                      |
|--------|-----------------------------|----------------|-----------------------------------|
| POST   | connect/revocation | auth.gluwa.com | application/x-www-form-urlencoded |

You cannot revoke access token. Access token lifetime is set to 1 hour.

## End Session Endpoint

| Method | URL      |
|--------|----------|
| GET    | /connect/endsession?id_token_hint=<your_id_token>&post_logout_redirect_uri=<your_post_log_redirect_uri>&state=abcd |


For the details of each query parameter, please click [here](http://docs.identityserver.io/en/aspnetcore1/endpoints/endsession.html).

Again, if state is provided, the auth server will give this back in your `post_redirect_uri` so you can validate the state after redirection.

## Session management

The documentation for session status detection can be found [here](http://openid.net/specs/openid-connect-session-1_0.html#ChangeNotification). 

**Note that this is up to the client to implement.**



# Endpoints and Resources

Welcome to the Gluwa API Reference Documentation for Endpoints and Resources
These documents describe all resources available in the Gluwa API and covers the endpoints, parameters, requests and responses.

# Bank

Bank API provides the list of financial institutions that Gluwa supports. 
Get the list of banks by country, or search for a specific institution.


## List Banks

Get the list of banks supported by Gluwa.


### Method

| Method | URL                                      |
|--------|------------------------------------------|
| GET    | <code class="highlighter-rouge">/V4.1/Banks/{country}</code> |

### Parameters
| Name    | Type/Value                   | Required | Path/Query | Description                              |
|---------|------------------------------|----------|------------|------------------------------------------|
| country | String enum: [World, US, KR] | yes      | Path       | Filter banks by country.                 |
| offset  | Integer (int64)              | no       | Query      | Number of banks to skip from the beginning of the list; used for pagination. Defaults to 0. |
| limit   | Integer (int64)              | no       | Query      | Number of banks to include in the result. Defaults to 25. |

### Responses

> Successful Response (200)

```json
[
    {
        "InstitutionCode": "string",
        "DisplayName": "string",
        "LogoImageUrl": "string"
        }
]
```

> Invalid country parameter error response (400)

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

| Code | Type/Value                        | Description                            |
|------|-----------------------------------|----------------------------------------|
| 200  | Array: Financial Institution      | List of Banks                          |
| 400  | RequestValidationWithoutBodyError | Invalid country parameter is provided. |

## Search Banks

Search through the list of banks by keyword.

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

> Successful Response (200)


```json
[
    {
        "InstitutionCode": "string",
        "DisplayName": "string",
        "LogoImageUrl": "string"
    }
]
```

> Invalid country parameter error response (400)

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

| Code | Type/Value                        | Description                            |
|------|-----------------------------------|----------------------------------------|
| 200  | Array: Financial Institution      | List of Banks                          |
| 400  | RequestValidationWithoutBodyError | Invalid country parameter is provided. |


# Funding Source

A funding source represents the source of fiat currency, such as bank accounts and credit cards. 
Users can make deposits to Gluwa from their funding sources, or withdraw from Gluwa to their funding sources. 

Funding source API can be used to create funding sources under a user or to retrieve a list of funding sources that belong to a user.

Micro deposit endpoints are for initiating and completing the verification process that is required before a funding source can be used. This resource also includes an endpoint that changes the status of a funding source to `deactivated`.


## Show a funding source

Retrieve a funding source of any type with specified ID.

### Method

| Method | URL                                      |
|--------|------------------------------------------|
| GET    | <code class="highlighter-rouge">/V4.1/FundingSources/{ID}</code> |

### Parameters

| Name | Type/Value   | Required | Path/Query | Description        |
|------|--------------|----------|------------|--------------------|
| ID   | String: uuid | yes      | Path       | Funding Source ID. |

### Responses

> Successful Response (200)

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
```

> Invalid request (400)

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
> Access to this funding source is denied (403)

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
> Funding source is not found (400)

```json 
{

"Code": "string",
"Message": "string",
"ExtraData": "string"

}
```
> Internal server error (500)

```json
{

  "ID": "string (uuid)",
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}

```
Response codes:

| Code | Type/Value                        | Description                              |
|------|-----------------------------------|------------------------------------------|
| 200  |FundingSourceResponse              | Funding source with the specified ID.    |
| 400  | RequestValidationWithoutBodyError | Invalid request.                         |
| 403  | ForbiddenRequestError             | Access to this funding source is denied. |
| 404  | NotFoundError                     | Funding source is not found.             |
| 500  | InternalServerError               | Internal server error.                   |


## Deactivate a funding source.
A deactivated funding source cannot be viewed or used for withdrawals.

### Method

| Method | URL                                      |
|--------|------------------------------------------|
| DELETE    | <code class="highlighter-rouge">/V4.1/FundingSources/{ID}}</code> |

### Parameters

| Name | Type/Value   | Required | Path/Query | Description        |
|------|--------------|----------|------------|--------------------|
| ID   | String: uuid | yes      | Path       | Funding Source ID. |

### Responses

> Successful Response (200)

```json
{}
```

> Invalid request (400)

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
> Access to this funding source is denied (403)

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
> Funding source is not found (400)

```json 
{

"Code": "string",
"Message": "string",
"ExtraData": "string"

}
```
> Internal server error (500)

```json
{

"ID": "string (uuid)",
"Code": "string",
"Message": "string",
"ExtraData": "string"
}
``` 

Response Codes:

| Code | Type/Value                        | Description                              |
|------|-----------------------------------|------------------------------------------|
| 200  | Void                              | Founding source deactivated              |
| 400  | RequestValidationWithoutBodyError | Invalid request.   |
| 403  | ForbiddenRequestError             | Access to this funding source is denied. |
| 404  | NotFoundError                     | Funding source is not found.             |
| 500  | InternalServerError               | Internal server error.                   |


## List funding sources
Retrieve the list of funding sources of all types that belong to the user.


### Method

| Method | URL                                      |
|--------|------------------------------------------|
| GET    | <code class="highlighter-rouge">/V4.1/FundingSources</code> |

### Responses

> Successful Response (200)

```json
[
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
]
```

> Internal Server Error (500)

```json
{

"ID": "string (uuid)",
"Code": "string",
"Message": "string",
"ExtraData": "string"
}
```

Response Codes

| Code | Type/Value                        | Description                              |
|------|-----------------------------------|------------------------------------------|
| 200  | Array: FundingSourceResponse      | List of Funding Resources              |
| 500  | InternalServerError               | Internal server error.                   |


## Create a micro deposit
Initiate a micro deposit for bank account verification.

### Method

| Method | URL                                      |
|--------|------------------------------------------|
| POST    | <code class="highlighter-rouge">/V4.1/FundingSources/{ID}/MicroDeposits</code> |

### Parameters

| Name | Type/Value   | Required | Path/Query | Description        |
|------|--------------|----------|------------|--------------------|
| ID   | String: uuid | yes      | Path       | Funding Source ID. |

### Responses

> Guid Response (201)

```json
{
"ID": "string (uuid)"
}
```

> Invalid Request (400)

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

> Access to this microfunding is denied (403)

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
> Cannot initiate micro deposit due to invalid state of the funding source (403)

```json
{
"Code": "string",
"Message": "string",
"ExtraData": "string"
}

```

> Founding Source not found (404)

```json
{

"Code": "string",
"Message": "string",
"ExtraData": "string"

}
```


> Internal Server Error (500)

```json
{

"ID": "string (uuid)",
"Code": "string",
"Message": "string",
"ExtraData": "string"

}
```

> Service is currently unavailable due to bank downtime (503)

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
| 201  | GuidResponse                      | Microdeposit is initiated for this funding source ID. |
| 202  | GuidResponse                      | Microdeposit is initiated for this funding source ID. |
| 400  | RequestValidationWithoutBodyError | Invalid request.                         |
| 403  | ForbiddenRequestError             | Access to this funding source is denied. |
| 403  | InvalidResourceStateError         | Cannot initiate micro deposit due to invalid state of the funding source. |
| 404  | NotFoundError                     | Funding source is not found.             |
| 500  | InternalServerError.              | Internal server error.                   |
| 503  | ServiceUnavailableError           | Service is currently unavailable due to bank downtime. |

## Verify a micro deposit
Check if a micro deposit has been completed successfully.

### Method

| Method | URL                                      |
|--------|------------------------------------------|
| Patch    | <code class="highlighter-rouge">/V4.1/FundingSources/{ID}/MicroDeposits</code> |


### Parameters

| Name | Type/Value   | Required | Path/Query | Description        |
|------|--------------|----------|------------|--------------------|
| ID   | String: uuid | yes      | Path       | Funding Source ID. |

### Responses

> Micro deposit is verified.

```json
{}
```



> Verification code is not valid. (400)

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

> Cannot verify micro deposit due to invalid funding source state. (403)

```json
{

"Code": "string",
"Message": "string",
"ExtraData": "string"

}
```

> Access to this funding source is denied. (403)

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

> Funding source is not found (404)

```json
{

"Code": "string",
"Message": "string",
"ExtraData": "string"

}
```

> Micro deposit was never initiated. (404)

```json
{

"Code": "string",
"Message": "string",
"ExtraData": "string"

}
```

> Internal server error. (500)

```json
{

"ID": "string (uuid)",
"Code": "string",
"Message": "string",
"ExtraData": "string"

}
```

> Service is currently unavailable due to bank downtime. (503)

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
| 200  | Void                              | Microdeposit verified.                   |
| 400  | RequestValidationWithoutBodyError | Invalid request.                         |
| 400  | Validation Error                  | Verification code is not valid           |
| 403  | ForbiddenRequestError             | Access to this funding source is denied. |
| 403  | InvalidResourceStateError         | Cannot verify micro deposit due to invalid funding source state. |
| 500  | InternalServerError.              | Internal server error.                   |
| 503  | ServiceUnavailableError           | Service is currently unavailable due to bank downtime. |




# Crypto Funding Source
A funding source represents the source of fiat currency such as bank accounts and credit cards. Users can make deposits to Gluwa from their funding source, or withdraw from Gluwa to their funding source. 

These endpoints are used to create cryptocurrency funding sources under a user or to retrieve cryptocurrency funding sources that belong to a user.

## Verify Crypto Funding Source

We only support this for P2PKH BTC addresses.

### Method

| Method | URL                                      |
|--------|------------------------------------------|
| PATCH    | <code class="highlighter-rouge">/V4.1/FundingSources/{ID}/SignedMessage</code> |

### Request Body


| Property                          | Type                   | Required? | Description |
|-----------------------------------|------------------------|-----------|-------------|
| Message: string (up to 256 chars) | string (256 chars max) | yes       |             |
| SignedMessage: string (88 char    | string (88 chars max)  | yes       |             |


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
| ID   | String: uuid | yes      | Path       | Funding Source ID. |

### Responses

> Funding Source is verified. (200)

```json
{}
```

> Invalid request. (400)

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

> Signed message is invalid. (400)

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

> Funding source is not found (404)

```json

"Code": "string",
"Message": "string",
"ExtraData": "string"

}
```


> Internal server error. (500)

```json
{

"ID": "string (uuid)",
"Code": "string",
"Message": "string",
"ExtraData": "string"

}
```

> Service is currently unavailable due to bank downtime. (503)

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
| 200  | Void                              | Microdeposit verified.                   |
| 400  | RequestValidationWithoutBodyError | Invalid request.                         |
| 400  | Validation Error                  | Verification code is not valid           |
| 403  | ForbiddenRequestError             | Access to this funding source is denied. |
| 403  | InvalidResourceStateError         | Cannot verify micro deposit due to invalid funding source state. |
| 404  | NotFoundError                     | Microdeposit was never initiated         |
| 500  | InternalServerError.              | Internal server error.                   |
| 503  | ServiceUnavailableError           | Service is currently unavailable due to bank downtime. |

## List crypto funding sources
Retrieve a list of crypto funding sources that belong to the current user.

### Method

| Method | URL                                      |
|--------|------------------------------------------|
| GET    | <code class="highlighter-rouge"> /V4.1/FundingSources/Crypto</code> |



### Responses

> List of funding sources. (200)

```json
[

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

]
```

Response Codes


| Code | Type/Value                        | Description                              |
|------|-----------------------------------|------------------------------------------|
| 200 | Array: FundingSourceResponse           | List of funding sources.|

## Create a cryptocurrency funding source.
Create a new cryptocurrency funding source for the current user.

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

> Funding Source is created. (201)

```json
{

"ID": "string (uuid)"

}
```

> Invalid request body. (400)

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

> Funding source already exists (409)

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


> Internal server error. (500)

```json
{
  "ID": "string (uuid)",
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```

Response Codes



| Code | Type/Value                        | Description                   |
|------|-----------------------------------|-------------------------------|
| 201  | GuidResponse                      | Funding source is created.    |
| 400  | RequestValidationWithoutBodyError | Invalid request body.         |
| 400  | Validation Error                  | Invalid request Body          |
| 409  | Conflict Error                    | Funding Source Already Exists |
| 500  | InternalServerError.              | Internal server error.        |


# Transaction 
Transaction represents a transfer of Gluwa coin from a source to a destination. 

Transaction has three types: 
* Deposit -adds funds to a user's wallet from a funding source
* Transfer - moves funds from a user's wallet to another user's wallet
* Withdraw - moves funds from a user's wallet into a funding source owned by the user. 

Transaction API resource allows retrieving a transaction or a list of transactions associated with a user's wallet. It also allows creating a Transfer or Withdraw transaction sent from the current user's wallet.

## List of transactions associated to a wallet.
Retrieve a list of transactions sent to or from a specified wallet.

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
| startDateTime | string (date-time)                       | no       | Query      | Only include transactions created after this datetime in ISO 8601 format. |
| endDateTime   | string (date-time)                       | no       | Query      | Only include transactions created before this datetime in ISO 8601 format.. |
| offset        | integer (int64)                          | no       | Query      | Number of transactions to skip the beginning of list. Used for pagination. Defaults to 0. |
| limit         | integer (int64)                          | no       | Query      | Number of transactions to include in the result. Defaults to 25, maximum of 50. |

### Responses 

> List of transactions associated with the wallet. (200)

```json
[
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
    }
    "To": {
      "Type": "string",
      "DisplayName": "string"
    },
    "Note": "string"
  }
]
```

> Invalid request. (400)

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
> Access to this wallet is denied. (403)

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
> Wallet is locked (403)

```json
{
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```
> Wallet is not found. (404)

```json
{
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```

> Server Error (500)

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
| 200  | Transation Response               | List of transactions associated with the wallet. |
| 400  | RequestValidationWithoutBodyError | Invalid request.                         |
| 403  | ForbiddenRequestError             | Access to this wallet is denied.         |
| 403  | InternalServerError               | Wallet is locked.                        |
| 404  | ConflictError                     | Wallet is not found.                     |
| 500  | InternalServerError               | Internal Server Error                    |

## Move funds to/from the current user's wallet
Create a new transaction that moves funds to or from the current user's wallet.

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

### Request Parameters

| Name | Type/Value   | Required | Path/Query | Description        |
|------|--------------|----------|------------|--------------------|
| walletID   | String: uuid | yes      | Path       | Wallet ID. |
|

### Responses 

> Newly created transaction. (201)

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
```

> Invalid request. (400)

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
```
> Validation error. See inner errors for more details. (400)

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
> Bad request. (400)

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
> Access to this wallet is denied. (403)

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

> Wallet is suspended. (403)

```json
{
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```

> Wallet is locked. (403)

```json

{
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```

> Wallet is not found. (404)

```json 
{
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```

> There already exists a requested bank deposit transaction. (409)

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

> Server error. (500)

```json
{
  "ID": "string (uuid)",
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```

> Service unavailable. (503)

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


)
Response Codes

| Code | Type/Value                               | Description                              |
|------|------------------------------------------|------------------------------------------|
| 201  | TransactionResponse                      | List of transactions associated with the wallet. |
| 400  | RequestValidationWithBodyError           | Invalid request.                         |
| 400  | ValidationError[ECreateTransactionValidationError] | Access to this wallet is denied.         |
| 400  | BadRequestError                          | Wallet is locked.                        |
| 403  | ForbiddenRequestError                    | Wallet is not found.                     |
| 403  | Wallet is suspended.                     | Internal Server Error                    |
| 403  | WalletLockedError                        | Wallet is locked.                        |
| 404  | NotFoundError                            | Wallet is not found.                     |
| 409  | ConflictError                            | There already exists a requested bank deposit transaction. |
| 500  | InternalServerError                      | Service error.                           |
| 503  | ServiceUnavailableError                  | Service unavaialable                     |

## Retrieve a transaction by ID and a specified wallet.

### Method


| Method | URL                                      |
|--------|------------------------------------------|
| GET | <code class="highlighter-rouge"> /V4.1/Wallets/{walletID}/Transactions/{transactionID}
</code> |

### Request Parameters


| Name          | Type/Value    | Required | Path/Query | Description |
|---------------|---------------|----------|------------|-------------|
| walletID      | String: uuid  | yes      | Path       | Wallet ID.  |
| transactionID |  String: uuid | yes      | Path       | Trabsaction |
                                                                                                                               
### Responses 

> Transaction with the specified ID and associated with the wallet. (200)

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
 
```

> Invalid request. (400)

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

> Wallet is locked. (403)

```json
{
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```
> Access to this wallet is denied. (403)

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

> Wallet or transaction is not found. (404)

```json
{
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```

> Server error. (500)

```json

{
  "ID": "string (uuid)",
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```


)
Response Codes

| Code | Type/Value                               | Description                              |
|------|------------------------------------------|------------------------------------------|
| 200  | TransactionResponse                      | Transaction with the specified ID and associated with the wallet. |
| 400  | RequestValidationWithBodyError           | Invalid request.                         |
| 403  | ValidationError[ECreateTransactionValidationError] | Access to this wallet is denied.         |
| 403  | BadRequestError                          | Wallet is locked.                        |
| 404  | ForbiddenRequestError                    | Wallet or transaction is not found.      |
| 500  | Wallet is suspended.                     | Internal Server Error                    |

## Cancel requested deposit
Cancel a deposit transaction that has previously been requested.

### Method


| Method | URL                                      |
|--------|------------------------------------------|
| PATCH| <code class="highlighter-rouge">  /V4.1/Wallets/{walletID}/Transactions/{transactionID}
</code> |

### Request Body


| Property | Type                                     | Required? | Description                              |
|----------|------------------------------------------|-----------|------------------------------------------|
| Amount   | string                                   | yes       | Transaction amount to calculate the fee for |
| Type     | string enum: (ExactAmountTransfer, Deposit, Withdraw, Receive) | yes       | Type of transaction to calculate the fee for. |



> Request Body example

```json
{
  "Message": "string",
  "SignedMessage": "string"
}
```
### Request Parameters


| Name          | Type/Value    | Required | Path/Query | Description |
|---------------|---------------|----------|------------|-------------|
| walletID      | String: uuid  | yes      | Path       | Wallet ID.  |
| transactionID |  String: uuid | yes      | Path       | Trabsaction |
                                                                                                                               
### Responses 

> Transaction has been successfully canceled. (200)

```json
{}
 
```

> Invalid request. (400)

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

> Access to this wallet is denied. (403)

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
> Transaction does not have a valid status to be cancelled. (403)

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

> Wallet or transaction is not found. (404)

```json
{
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```

> Server error. (500)

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
| 200  | Void                           | Transaction has been successfully cancelled. |
| 400  | RequestValidationWithBodyError | Invalid request.                         |
| 403  | ForbiddenRequestError          | Access to this wallet is denied.         |
| 403  | InvalidResourceStateError      | Transaction does not have a valid status to be cancelled. |
| 404  | NotFoundError                  | Wallet or transaction is not found.      |
| 500  | InternalServerError            | Internal Server Error                    |

## Calculate transaction fee
Get the fee amount calculated from the amount and the type of transaction.

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
| walletID      | String: uuid  | yes      | Path       | Wallet ID.  |
                                                                                                                               
### Responses 

> Calculated fee amount. (200)

```json
{
"string"
}
```

> Invalid request. (400)

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

> Bad Request (400)

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
> Access to this wallet or transaction is denied. (403)

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
> Wallet is locked. (403)

```json
{
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```
> Wallet or transaction is not found. (404)

```json
{
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```

> Server error. (500)

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
| 200  |                                | Calculated fee amount.                   |
| 400  | RequestValidationWithBodyError | Invalid request.                         |
| 400  | BadRequestError                | Bad Request                              |
| 403  | ForbiddenRequestError          | Access to this wallet or transaction is denied. |
| 403  | WalletLockedError              | Wallet is locked.                        |
| 404  | NotFoundError                  | Wallet is not found                      |
| 500  | InternalServerError            | Server Error.                            |

## Get a quote for depositing and withdrawing
Get a quote for crypto deposit and withdraw transactions.

### Method


| Method | URL                                      |
|--------|------------------------------------------|
| POST| <code class="highlighter-rouge"> /V4.1/Wallets/{walletID}/Quote
</code> |

### Request Body


| Property        | Type                             | Required? | Description                              |
|-----------------|----------------------------------|-----------|------------------------------------------|
| Amount          | string                           | yes       |                                          |
| TransactionType | string enum: (Deposit, Withdraw) | yes       | Type of transaction that the quote is to be used for. |
| CryptoCurrency  | string enum: [BTC]               | yes       | Cryptocurrency for the quote amount to be provided. |
| Password        | string                           | no        | User password for verification.          |



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

> Create quote and checksum. (200)

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
```

> Invalid request. (400)

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

> Validation error. (400)

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
> Access to this wallet or transaction is denied. (403)

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

> Wallet is locked. (403)

```json
{
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```

> Wallet or transaction is not found. (404)

```json
{
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```

> Server error. (500)

```json
{
  "ID": "string (uuid)",
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```

> Service unavailable. (503)

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
| 200  | QuoteResponse                            | Created quote and checksum.              |
| 400  | RequestValidationWithBodyError           | Invalid request.                         |
| 400  | ValidationError[ECreateTransactionValidationError] | Validation error. See inner errors for more details. |
| 400  | BadRequestError                          | Bad Request                              |
| 403  | ForbiddenRequestError                    | Access to this wallet or transaction is denied. |
| 403  | WalletLockedError                        | Wallet is locked.                        |
| 404  | NotFoundError                            | Wallet is not found                      |
| 500  | InternalServerError                      | Server Error.                            |
| 503  | ServiceUnavailableError                  | Service Unavailable                      |

## Create a new transaction using quote provided
Create a new transaction with the quote that was previously provided by Gluwa.

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

> Newly created transaction. (201)

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
```

> Invalid request. (400)

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

> Validation error. (400)

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

> Bad Request. (400)

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
> Access to this wallet is denied. (403)

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

> Wallet is suspended. (403)

```json
{
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```

> Wallet is locked. (403)

```json
{
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```
> Wallet not found. (404)

```json
{
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```


> There already exists a requested crypto deposit transaction. Cancel the pending transaction or wait until it expires. (409)

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
> Client Error. (429)

```json
{
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```
> Server Error (500)

```json
{
  "ID": "string (uuid)",
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```
> Service unavailable. (503)

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
| 201  | TransactionResponse                      | Newly created transaction.               |
| 400  | RequestValidationWithBodyError           | Invalid request.                         |
| 400  | ValidationError[ECreateTransactionValidationError] | Validation error. See inner errors for more details. |
| 400  | BadRequestError                          | Bad Request                              |
| 403  | ForbiddenRequestError                    | Access to this wallet or transaction is denied. |
| 403  | WalletSuspendedError                     | Wallet is suspended.                     |
| 403  | WalletLockedError                        | Wallet is locked                         |
| 404  | NotFoundError                            | Wallet not found                         |
| 409  | ConflictError                            | There already exists a requested crypto deposit transaction. Cancel the pending transaction or wait until it expires. |
| 429  | TooManyRequestsError                     | Client Error                             |
| 500  | InternalServerError                      | Server Error.                            |
| 503  | ServiceUnavailableError                  | Service Unavailable                      |

# User

## Search by username

### Method


| Method | URL                                      |
|--------|------------------------------------------|
| GET| <code class="highlighter-rouge"> /V4.1/Users
</code> |

### Request Parameters


| Name       | Type/Value      | Required | Path/Query | Description                              |
|------------|-----------------|----------|------------|------------------------------------------|
| searchTerm | string          | no       | Query      | Username as the search term.             |
| offset     | integer (int32) | no       | Query      | Number of users to skip from the beginning of list. Used for pagination. Defaults to 0. |
| limit      | integer (int32) | no       | Query      | Number of users in the result. Defaults to 20, maximum of 50. |
                                                                                                                               
### Responses 

> List of users that contain the search term in their username. (200)

```json
[
  {
    "ID": "string (uuid)",
    "Username": "string"
  }
]
```

> Validation error. (400)

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

> Bad Request. (400)

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

> Server Error (500)

```json
{
  "ID": "string (uuid)",
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```

Response Codes

| Code | Type/Value                               | Description                              |
|------|------------------------------------------|------------------------------------------|
| 200  | Array <User>                  | List of users that contain the search term in their username. |
| 400  | RequestValidationWithBodyError          | Invalid request.                         |
| 400  | BadRequestError        | Bad Request                         |
| 500 | InternalServerError                 | Server Error.      |

# Virtual Account

*** Korean Users ONLY. ***

Virtual Account is a virtual bank account that belongs to Gluwa. Each user will get a dedicated virtual bank account number with his/her wallet. 

When fiat currency is deposited into this virtual bank account, Gluwa will accredit the user's wallet with the same amount in Gluwa coin. The VirtualAccount resource can be used to view virtual accounts linked to a wallet.

## Get virtual accounts linked to a wallet

### Method


| Method | URL                                      |
|--------|------------------------------------------|
| GET| <code class="highlighter-rouge"> /V4.1/Wallets/{walletID}/VirtualAccounts </code> |

### Request Parameters


| Name       | Type/Value      | Required | Path/Query | Description                              |
|------------|-----------------|----------|------------|------------------------------------------|
| walletID | string (uuid)      | yes       | Path     | Wallet ID             |
|
                                                                                                                               
### Responses 

> List of virtual accounts. (200)

```json
[
  {
    "InstitutionName": "string",
    "VirtualAccountNumber": "string",
    "DepositorName": "string"
  }
]
```

> Invalid Request (400)

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

> Identity verification is required to use virtual accounts. (403)

```json
{
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```

> Your wallet is locked. (403)

```json
{
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```
> Access to this wallet is denied. (403)

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
> Virtual accounts are not supported because this wallet cannot be used in Korea. (403)

```json
{
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```
> Wallet is not found. (404)

```json
{
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```
> Server Error. (500)

```json
{
  "ID": "string (uuid)",
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```
Response Codes


| Code | Type/Value                        | Description                              |
|------|-----------------------------------|------------------------------------------|
| 200  | Array <VirtualAccountResponse>    | List of virtual accounts.                |
| 400  | RequestValidationWithBodyError    | Invalid request.                         |
| 403  | IdentityVerificationRequiredError | Identity verification is required to use virtual accounts. |
| 403  | WalletLockedError                 | Your wallet is locked.                   |
| 403  | ForbiddenRequestError             | Access to this wallet is denied.         |
| 403  | VirtualAccountsNotSupportedError  | Virtual accounts are not supported because this wallet cannot be used in Korea. |
| 404  | NotFoundError                     | Wallet is not found.                     |
| 500  | InternalServerError               | Server error.                            |

# Wallet

A Wallet is where your Gluwa coin is stored. You can also have multiple wallets across multiple countries and currencies. 

The Wallet API resource allows creating, retrieving, and activating locked wallets.

## List wallets

Retrieve a list of wallets for the current user.

### Method

| Method | URL                                      |
|--------|------------------------------------------|
| GET    | <code class="highlighter-rouge">/V4.1/Wallets</code> |


### Responses

> List of Wallets. (200)

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
> Internal Server Error. (500)

```json
{
  "ID": "string (uuid)",
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```

Response Codes


| Code | Type/Value             | Description           |
|------|------------------------|-----------------------|
| 200  | Array <WalletResponse> | List of wallets       |
| 500  | InternalServerError    | Internal Server Error |

## Create a new wallet for the current user.

### Method

| Method | URL                                      |
|--------|------------------------------------------|
| POST   | <code class="highlighter-rouge"> /V4.1/Wallets</code> |

### Request Body

| Property | Type                         | Required? | Description                             |
|----------|------------------------------|-----------|-----------------------------------------|
| Country  | string enum: [World, US, KR] | yes       | Country that the wallet can be used in. |
| Currency | string enum: [KRW, USD]      | yes       | Currency that the wallet can hold.      |


> Request Body example

```json
{
  "Country": "string",
  "Currency": "string"
}
```

### Responses

> New Wallet created. (201)

```json
{
  "ID": "string (uuid)"
}
```

> Invalid Request (400)

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

> Locked wallet found. User must either unlock the locked wallet or remove the locked wallet first. (403)

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

> Server Error(409)

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

> Wallet with specified country and currency already exists. (500)

```json
{
  "ID": "string (uuid)",
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```

> Server currently unavailable for specified currency our country. (503)

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


| Code | Type/Value                     | Description                              |
|------|--------------------------------|------------------------------------------|
| 201  | Invalid Request                 | New Wallet Created.                      |
| 400  | RequestValidationWithBodyError | Invalid Request.                         |
| 403  | ForbiddenRequestError          | Locked wallet found. User must either unlock the locked wallet or remove the locked wallet first. |
| 409  | ConflictError                  | Wallet with specified country and currency already exists. |
| 500  | InternalServerError            | Server Error.                            |
| 503  | ServiceUnavailableError        | Service is currently not available for the specified country and currency. |



## Search wallet by ID

### Method

| Method | URL                                      |
|--------|------------------------------------------|
| GET  | <code class="highlighter-rouge">/V4.1/Wallets/{ID}</code> |

### Parameters

| Name | Type/Value   | Required | Path/Query | Description        |
|------|--------------|----------|------------|--------------------|
| ID   | String: uuid | yes      | Path       | Wallet ID. |

### Responses

> Wallet with the specified ID. (200)

```json
{
  "ID": "string (uuid)",
  "Balance": "string",
  "Country": "string",
  "Currency": "string",
  "Status": "string"
}
```
> Invalid Request. (400)

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
> Access to this wallet is denied. (403)

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
> Wallet is not found.(404)

```json
{
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```
> Server Error. (500)

```json
{
  "ID": "string (uuid)",
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```
Response Codes


| Code | Type/Value                        | Description                      |
|------|-----------------------------------|----------------------------------|
| 200  | WalletResponse                    | Wallet with the specified ID.    |
| 400  | RequestValidationWithoutBodyError | Invalid request.                 |
| 403  | ForbiddenRequestError             | Access to this wallet is denied. |
| 404  | NotFoundError                     | Wallet is not found              |
| 500  | InternalServerError               | Server error.                    |

## Activate a locked wallet 
Either by unlocking it or creating a new wallet. 

Wallet can be unlocked by verifying the most recent payment made, or it can be closed and have its funds transferred to a new wallet.


### Method

| Method | URL                                      |
|--------|------------------------------------------|
| PATCH    | <code class="highlighter-rouge">//V4.1/Wallets/{ID}/Activation</code> |

### Request Body


| Property                          | Type                   | Required? | Description |
|-----------------------------------|------------------------|-----------|-------------|
| Type | string enum: [TransferFundsAndDelete, Unlock] | yes       |Type of Wallet activation request.             |
| Amount Paid | string | no       | Payment amount for the most recent payment made. Only required when unlocking a wallet for verification purposes.            |

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

> Wallet is unlocked and activated. (200)

```json
{}
```
> Wallet ID of the newly created wallet. The balance is moved to the new wallet. (201)

```json
{
  "ID": "string (uuid)"
}
```
> Invalid Request(400)

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
> Validation error. Please see inner errors for more details. (400)

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
> Requested activation type is not supported. (400)

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
> Access to this wallet is denied. (403)

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
> Wallet cannot be activated at current status. (403)

```json
{
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```
> Wallet is not found. (404)

```json
{
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```
> Server error. (500)

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
| 200  | Void                           | Wallet is unlocked and activated.        |
| 201  | GuidResponse                   | Wallet ID of the newly created wallet. The balance is moved to the new wallet. |
| 400  | RequestValidationWithBodyError | Invelid Request                          |
| 400  | ValidationError                | Validation error. Please see inner errors for more details. |
| 400  | BadRequestError                | Requested activation type is not supported. |
| 403  | ForbiddenRequestError          | Access to this wallet is denied.         |
| 403  | InvalidResourceStateError      | Wallet cannot be activated at current status. |
| 404  | NotFoundError                  | List of virtual accounts.List of virtual accounts. |
| 500  | InternalServerError            | Server Error                             |

## Get the fee models associated to a wallet.


### Method

| Method | URL                                      |
|--------|------------------------------------------|
| GET    | <code class="highlighter-rouge">/V4.1/Wallets/{ID}/FeeModel</code> |


### Parameters

| Name | Type/Value   | Required | Path/Query | Description        |
|------|--------------|----------|------------|--------------------|
| ID   | String: uuid | yes      | Path       | Funding Source ID. |

### Responses

> List of Fee models (200)

```json
[
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
]
```
> Request parameter is not valid. (400)

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
> Access to this wallet is denied. 403)

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
> Wallet is not found (404)

```json
{
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```
> Server Error (500)

```json
{
  "ID": "string (uuid)",
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```
Response Codes


| Code | Type/Value                        | Description                      |
|------|-----------------------------------|----------------------------------|
| 200  | Array <FeeModel>                  | List of fee models.              |
| 400  | RequestValidationWithoutBodyError | Request Parameter is not valid.  |
| 403  | ForbiddenRequestError             | Access to this wallet is denied. |
| 404  | NotFoundError                     | Wallet is not found.             |
| 500  | InternalServerError               | Server Error.                    |

## Get webhook information
You need to be enrolled in our webhook service in order to use this feature.

### Method

| Method | URL                                      |
|--------|------------------------------------------|
| GET   | <code class="highlighter-rouge">/V4.1/Wallets/{ID}/Webhook</code> |

### Parameters

| Name | Type/Value   | Required | Path/Query | Description        |
|------|--------------|----------|------------|--------------------|
| ID   | String: uuid | yes      | Path       | Funding Source ID. |

### Responses

> Returns the wallet's Webhook. (200)

```json
{
  "URL": "string",
  "CreatedDateTime": "string (date-time)",
  "ModifiedDateTime": "string (date-time)",
  "FeeAmount": "string"
}
```
> Invalid request. Check the format of the wallet ID. (400)

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
> Access to this wallet is denied. (403)

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
> Webhook does not exist.(404)

```json
{
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```
> Server error. (500)

```json
{
  "ID": "string (uuid)",
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```

Response Codes


| Code | Type/Value                        | Description                      |
|------|-----------------------------------|----------------------------------|
| 200  | WebhookResponse                   | Return the Wallet's Webhook.     |
| 400  | RequestValidationWithoutBodyError | Invalid Request.                 |
| 403  | ForbiddenRequestError             | Access to this wallet is denied. |
| 404  | NotFoundError                     | Webhook does not exist.          |
| 500  | InternalServerError               | Server Error.                    |

## Create or update webhook URL and secret
You need to be enrolled in our webhook service in order to use this feature.

### Method

| Method | URL                                      |
|--------|------------------------------------------|
| PUT   | <code class="highlighter-rouge"> /V4.1/Wallets/{ID}/Webhook</code> |

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

### Parameters

| Name | Type/Value   | Required | Path/Query | Description        |
|------|--------------|----------|------------|--------------------|
| ID   | String: uuid | yes      | Path       | Funding Source ID. |

### Responses

> Webhook is updated.(200)

```json
{
  "URL": "string",
  "CreatedDateTime": "string (date-time)",
  "ModifiedDateTime": "string (date-time)",
  "FeeAmount": "string"
}
```
> Webhook is created. (201)

```json
{
  "URL": "string",
  "CreatedDateTime": "string (date-time)",
  "ModifiedDateTime": "string (date-time)",
  "FeeAmount": "string"
}
```
> Invalid request. Check the format of the wallet ID.(400)

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
> Access to this wallet is denied. (403)

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
> Wallet does not exist. (404)

```json
{
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```
> Server error. (500)

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
| 200  | WebhookResponse                | Webhook is updated.                      |
| 201  | WebhookResponse                | Webhook is created.                      |
| 400  | RequestValidationWithBodyError | Invalid request. Check the format of the wallet ID. |
| 403  | ForbiddenRequestError          | Access to this wallet is denied.         |
| 404  | NotFoundError                  | Wallet does not exist.                   |
| 500  | InternalServerError            | Server error.                            |



## Generate QR code for transaction request
Returns QR code as an image if URL query parameter `format` was passed.

Supported formats are .jpeg and .png . 

Returns QR code as Base64 string if `format` parameter was not passed.

### Method

| Method | URL                                      |
|--------|------------------------------------------|
| POST   | <code class="highlighter-rouge">/V4.1/Wallets/{ID}/QRCode</code> |

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

### Parameters


| Name   | Type/Value   | Required | Path/Query | Description        |
|--------|--------------|----------|------------|--------------------|
| ID     | string: uuid | yes      | Path       | Funding Source ID. |
| format | string       | yes      | Query      | Request body.      |

### Responses

> QR code image with specified format. (Void) (200)

```json
{}
```
> QR code image with specified format (String) (200)

```json
"string"
```
> Bad Request. (400)

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
> Requested MIME type is not supported. (400)

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
> Access to this wallet is denied. (403)

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
> Wallet not found. (404)

```json
{
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```
> Server error. (500)

```json
{
  "ID": "string (uuid)",
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```

Response Codes


| Code | Type/Value                     | Description                           |
|------|--------------------------------|---------------------------------------|
| 200  | Void.                          | QR code image with specified format.  |
| 200  | String.                        | List of virtual accounts.             |
| 400  | RequestValidationWithBodyError | Bad Request.                          |
| 400  | BadRequestError                | Requested MIME type is not supported. |
| 403  | ForbiddenRequestError          | Access to this wallet is denied.      |
| 404  | NotFoundError                  | Wallet not found.                     |
| 500  | InternalServerError            | Server Error.                         |


