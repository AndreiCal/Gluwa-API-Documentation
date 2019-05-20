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
# Introduction

Welcome to the Gluwa API Reference Documentation.

# Bank

Bank API provides the list of financial institutions that Gluwa supports. Get the list of banks by country, or search for a specific institution.

## Get the list of banks supported by Gluwa
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

> Sucessful Response (200)

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

## Search through the list of banks by keyword

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

> Succesful Response (200)

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
Response codes:

| Code | Type/Value                        | Description                            |
|------|-----------------------------------|----------------------------------------|
| 200  | Array: FinancialInstitution      | List of Banks in aphabetical order.    |
| 400  | RequestValidationWithoutBodyError | Invalid country parameter is provided. |

# Funding Source

A funding source represents the source of fiat currency such as bank accounts and credit cards. Users can make deposits to Gluwa from their funding sources, or withdraw from Gluwa to their funding sources. Funding source API can be used to create funding sources under a user or to retrieve a list of funding sources that belong to a user. Micro deposit endpoints are for initiating and completing the verification process that is required before a funding source can be used. This resource also includes an endpoint that change the status of a funding source to deactivated.

## Retrieve a funding source of any type with specified ID.
### Method

| Method | URL                                      |
|--------|------------------------------------------|
| GET    | <code class="highlighter-rouge">/V4.1/FundingSources/{ID}</code> |

### Parameters

| Name | Type/Value   | Required | Path/Query | Description        |
|------|--------------|----------|------------|--------------------|
| ID   | String: uuid | yes      | Path       | Funding Source ID. |

### Responses

> Sucessful Response (200)

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

> Sucessful Response (200)

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

## Retrieve the list of funding sources of all types that belong to the user.

### Method

| Method | URL                                      |
|--------|------------------------------------------|
| GET    | <code class="highlighter-rouge">/V4.1/FundingSources</code> |

### Responses

> Sucessful Response (200)

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

> Response Codes


| Code | Type/Value                        | Description                              |
|------|-----------------------------------|------------------------------------------|
| 200  | Array: FundingSourceResponse                           | List of Funding Resources              |
| 500  | InternalServerError               | Internal server error.                   |

## Initiate a micro deposit for bank account verification.

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

> Access to this microfuning is denied (403)

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
| 201 | GuidResponse           | Microdeposit is initiated for this funding source ID.|
| 202  | GuidResponse               | Microdeposit is initiated for this funding source ID.                  |
| 400  | RequestValidationWithoutBodyError               | Invalid request.                   |
| 403  | ForbiddenRequestError               | Access to this funding source is denied.                  |
| 403  | InvalidResourceStateError              | Cannot initiate micro deposit due to invalid state of the funding source.                   |
| 404  | NotFoundError              | Funding source is not found.                   |
| 500  | InternalServerError.               | Internal server error.                   |
| 503  | ServiceUnavailableError              | Service is currently unavailable due to bank downtime.                  |

## Verify a micro deposit

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
| 200 | Void           | Microdeposit verified.|
| 400  | RequestValidationWithoutBodyError               | Invalid request.                   |
| 400  | Validation Error               | Verification code is not valid                   |
| 403  | ForbiddenRequestError               | Access to this funding source is denied.                  |
| 403  | InvalidResourceStateError              | Cannot verify micro deposit due to invalid funding source state.|
| 500  | InternalServerError.               | Internal server error.                   |
| 503  | ServiceUnavailableError              | Service is currently unavailable due to bank downtime.                  |

## Verify Crypto Funding Source. 
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
| 200 | Void           | Microdeposit verified.|
| 400  | RequestValidationWithoutBodyError               | Invalid request.                   |
| 400  | Validation Error               | Verification code is not valid                   |
| 403  | ForbiddenRequestError               | Access to this funding source is denied.                  |
| 403  | InvalidResourceStateError              | Cannot verify micro deposit due to invalid funding source state.                   |
| 404  | NotFoundError              | Microdeposit was never initiated                  |
| 500  | InternalServerError.               | Internal server error.                   |
| 503  | ServiceUnavailableError              | Service is currently unavailable due to bank downtime.                  |

# FundingSourceCrypto
A funding source represents the source of fiat currency such as bank accounts and credit cards. Users can make deposits to Gluwa from their funding source, or withdraw from Gluwa to their funding source. These endpoints are used to create cryptocurrency funding sources under a user or to retrieve cryptocurrency funding sources that belong to a user.

## Retrieve a list of crypto funding sources that belong to the current user.

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


## Create a new cryptocurrency funding source.

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


| Code | Type/Value                        | Description                              |
|------|-----------------------------------|------------------------------------------|
| 201 | GuidResponse           | Funding source is created.|
| 400  | RequestValidationWithoutBodyError               | Invalid request body.                   |
| 400  | Validation Error               | Invalid request Body                  |
| 409 | Conflict Error             | Funding Source Already Exists                  |
| 500  | InternalServerError.               | Internal server error.                   |

# FundingSourceCrypto

A funding source represents the source of fiat currency such as bank accounts and credit cards. Users can make deposits to Gluwa from their funding source, or withdraw from Gluwa to their funding source. These endpoints are used to create cryptocurrency funding sources under a user or to retrieve cryptocurrency funding sources that belong to a user.

## Retrieve a list of crypto funding sources that belong to the current user.

### Method

| Method | URL                                      |
|--------|------------------------------------------|
| GET   | <code class="highlighter-rouge">/V4.1/FundingSources/Crypto</code> |


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
| 200 | Array: FundingSourceResponse           | List of funding sources|

## Create a new cryptocurrency funding source.

### Method

| Method | URL                                      |
|--------|------------------------------------------|
| POST  | <code class="highlighter-rouge">/V4.1/FundingSources/Crypto</code> |

### Request Body


| Property      | Type                          | Required? | Description                              |
|---------------|-------------------------------|-----------|------------------------------------------|
| Currency      | string enum [BTC]             | yes       | Cryptocurrency                           |
| Address       | string range (26 to 35 chars) | yes       | The address                              |
| Message       | string range (256 chars max)  | no        | The message used to create SignedMessage. It has to be less than 256 characters. Only available for P2PKH BTC addresses. |
| SignedMessage | string range (88 chars)       | no        | Signed message created by using Message and the private key. This is used to verify that the address provided is a legitimate P2PKH address. |
| Nickname      | string                        | yes       | User-specified nickname for funding source |

> Request Body Example

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

> Funding source is created. (201)

```json
{
  "ID": "string (uuid)"
}
```

> Invalid Body Request. (400)

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

> Funding Source already exists. (409)

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

> Internal Server error. (500)

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
| 201 | GuidResponse           | Funding source is created |
| 400 | RequestValidationWithBodyError | Invalid Body Request |
| 409 | ConflictError | Funding source already exists | 
| 500 | InternalServerError | Internal Server Error |

# Transaction 
Transaction represents a transfer of Gluwa coin from a source to a destination. Transaction has three types: Deposit, Transfer and Withdraw. A Deposit transaction adds funds to a user's wallet from a funding source; a Transfer transaction moves funds from a user's wallet to another user's wallet; a Withdraw transaction moves funds from a user's wallet into a funding source owned by the user. Transaction API resource allows retrieving a transaction or a list of transactions associated with a user's wallet. It also allows creating a Transfer or Withdraw transaction sent from the current user's wallet.

## Retrieve a list of transactions sent to or from a specified wallet.
### Method

| Method | URL                                      |
|--------|------------------------------------------|
| GET  | <code class="highlighter-rouge">/V4.1/Wallets/{walletID}/Transactions </code> |

### Request Parameters

| Name | Type/Value   | Required | Path/Query | Description        |
|------|--------------|----------|------------|--------------------|
| walletID   | String: uuid | yes      | Path       | Wallet ID. |
| type  | string enum [Deposit, Send, Withdraw, Receive] | no      | Query       | Filter transactions by transaction type. |
| fromType  | string enum [Wallet, VirtualAccount, BankDeposit, CryptoDeposit, FundingSource] | no     | Query     | Filter by type of the "sending" party in a transaction. |
| toType   | string enum [Wallet, VirtualAccount, BankDeposit, CryptoDeposit, FundingSource] | no     | Query        | Filter by type of the "receiving" party in a transaction. |
| startDateTime  | string (date-time) | no     | Query         | Only include transactions created after this datetime in ISO 8601 format. |
| endDateTime   | string (date-time) | no      | Query         | Only include transactions created before this datetime in ISO 8601 format.. |
| offset   | integer (int64) | no      | Query         | Number of transactions to skip the beginning of list. Used for pagination. Defaults to 0. |
|  limit  | integer (int64) | no      | Query         | Number of transactions to include in the result. Defaults to 25, maximum of 50. |

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
| 200 | Transation Response         | List of transactions associated with the wallet. |
| 400 | RequestValidationWithoutBodyError | Invalid request. |
| 403 | ForbiddenRequestError| Access to this wallet is denied. | 
| 403 | InternalServerError | Wallet is locked. |    
| 404 | ConflictError | Wallet is not found. | 
| 500 | InternalServerError | Internal Server Error |  

## Create a new transaction that moves funds from/to the current user's wallet.

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
| 403  | WalletLockedError                         | Wallet is locked.                        |
| 404  | NotFoundError             | Wallet is not found.                                     |
| 409  | ConflictError                            | There already exists a requested bank deposit transaction. |
| 500  | InternalServerError                      | Service error.                           |
| 503  | ServiceUnavailableError                  | Service unavaialable                     |

## Retreive a transaction by ID and a specified wallet.

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

## Cancel a deposit transaction that has previously been requested.
### Method


| Method | URL                                      |
|--------|------------------------------------------|
| PATCH| <code class="highlighter-rouge">  /V4.1/Wallets/{walletID}/Transactions/{transactionID}
</code> |

### Request Body

| Property                          | Type                   | Required? | Description |
|-----------------------------------|------------------------|-----------|-------------|
| Amount | string | yes       | Transaction amount to calculate the fee for
| Type   | string enum: (ExactAmountTransfer, Deposit, Withdraw, Receive)  | yes       |  Type of transaction to calculate the fee for.           |



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

> Transaction has been successfully cancelled.. (200)

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

| Code | Type/Value                               | Description                              |
|------|------------------------------------------|------------------------------------------|
| 200  | Void                    | Transaction has been successfully cancelled. |
| 400  | RequestValidationWithBodyError           | Invalid request.                         |
| 403  | ForbiddenRequestError | Access to this wallet is denied.         |
| 403  | InvalidResourceStateError                        | Transaction does not have a valid status to be cancelled.     |
| 404  | NotFoundError                  | Wallet or transaction is not found.      |
| 500  | InternalServerError                     | Internal Server Error                    |

## Get the fee amount calculated from the amount and the type of transaction.
### Method


| Method | URL                                      |
|--------|------------------------------------------|
| POST| <code class="highlighter-rouge"> /V4.1/Wallets/{walletID}/Transactions/Fee
</code> |

### Request Body


| Property        | Type                             | Required? | Description                              |
|-----------------|----------------------------------|-----------|------------------------------------------|
| Amount          | string                           | yes       | Transaction amount to calculate the fee for.                                         |
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

| Code | Type/Value                               | Description                              |
|------|------------------------------------------|------------------------------------------|
| 200  |                 | Calculated fee amount. |
| 400  | RequestValidationWithBodyError           | Invalid request.                         |
| 400  | BadRequestError | Bad Request         |
| 403  | ForbiddenRequestError                | Access to this wallet or transaction is denied.     |
| 403  | WalletLockedError                     | Wallet is locked.                 |
| 404  | NotFoundError                        | Wallet is not found     |
| 500  | InternalServerError                 | Server Error.      |

## Get a quote for crypto deposit and withdraw transactions.
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
| 200  | QuoteResponse                   | Created quote and checksum. |
| 400  | RequestValidationWithBodyError           | Invalid request.                         |
| 400  | ValidationError[ECreateTransactionValidationError] | Validation error. See inner errors for more details.         |
| 400  | BadRequestError                        | Bad Request    |
| 403  | ForbiddenRequestError                  | Access to this wallet or transaction is denied.     |
| 403  | WalletLockedError                     | Wallet is locked.                 |
| 404  | NotFoundError                        | Wallet is not found     |
| 500  | InternalServerError                 | Server Error.      |
| 503  | ServiceUnavailableError| Service Unavailable                |

## Create a new transaction with the quote that was previously provided by Gluwa.
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
| 201  | TransactionResponse                   | Newly created transaction. |
| 400  | RequestValidationWithBodyError          | Invalid request.                         |
| 400  | ValidationError[ECreateTransactionValidationError] | Validation error. See inner errors for more details.         |
| 400  | BadRequestError                        | Bad Request    |
| 403  | ForbiddenRequestError                  | Access to this wallet or transaction is denied.     |
| 403  | WalletSuspendedError                     | Wallet is suspended.                |
| 403  | WalletLockedError                     | Wallet is locked    |
| 404  | NotFoundError              | Wallet not found      |
| 409  | ConflictError | There already exists a requested crypto deposit transaction. Cancel the pending transaction or wait until it expires.              |
| 429  | TooManyRequestsError                       | Client Error    |
| 500 | InternalServerError                 | Server Error.      |
| 503  | ServiceUnavailableError| Service Unavailable                |

# User
Gluwa users can be searched by username.

## Search for a user by username.

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

# VirtualAccount

*** Korean Users ONLY. ***

Virtual Account is a virtual bank account that belongs to Gluwa. Each user will get a dedicated virtual bank account number with his/her wallet. When fiat currency is deposited into this virtual bank account, Gluwa will accredit the user's wallet with the same amount in Gluwa coin. The VirtualAccount resource can be used to view virtual accounts linked to a wallet.

## Get virtual accounts linked to a wallet.

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

| Code | Type/Value                               | Description                              |
|------|------------------------------------------|------------------------------------------|
| 200  | Array <VirtualAccountResponse>                  | List of virtual accounts. |
| 400  | RequestValidationWithBodyError          | Invalid request.                         |
| 403  | IdentityVerificationRequiredError      | Identity verification is required to use virtual accounts.                       |
| 403 | WalletLockedError            | Your wallet is locked.    |
| 403  | ForbiddenRequestError                  | Access to this wallet is denied. |
| 403  | VirtualAccountsNotSupportedError        | Virtual accounts are not supported because this wallet cannot be used in Korea.                         |
| 404  | NotFoundError       | Wallet is not found.                        |
| 500 | InternalServerError                 | Server error.     |

# Wallet

Wallet is where your Gluwa coin is stored. You can also have multiple wallets across multiple countries and currencies. Wallet API resource allows creating, retrieving, and activating locked wallets.

## Retrieve a list of wallets for the current user.

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

| Code | Type/Value                               | Description                              |
|------|------------------------------------------|------------------------------------------|
| 200  | Array <WalletResponse>                  | List of wallets |
| 500  | InternalServerError                  | Internal Server Error|

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

| Code | Type/Value                               | Description                              |
|------|------------------------------------------|------------------------------------------|
| 201  | Invalid Reuest                 | New Wallet Created. |
| 400  | RequestValidationWithBodyError                 | Invalid Request. |
| 403  | ForbiddenRequestError                  | Locked wallet found. User must either unlock the locked wallet or remove the locked wallet first. |
| 409  | ConflictError                 | Wallet with specified country and currency already exists. |
| 500  | InternalServerError                  | Server Error. |
| 503  | ServiceUnavailableError                 | Service is currently not available for the specified country and currency. |



## Retrieve a wallet.

### Method

| Method | URL                                      |
|--------|------------------------------------------|
| GET  | <code class="highlighter-rouge">/V4.1/Wallets/{ID}</code> |

### Parameters

| Name | Type/Value   | Required | Path/Query | Description        |
|------|--------------|----------|------------|--------------------|
| ID   | String: uuid | yes      | Path       | Wallet ID. |

### Responses

> Wallet wit the specified ID. (200)

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

| Code | Type/Value                               | Description                              |
|------|------------------------------------------|------------------------------------------|
| 200  | WalletResponse            | Wallet with the specified ID. |
| 400  | RequestValidationWithoutBodyError                 | Invalid request. |
| 403  | ForbiddenRequestError               | Access to this wallet is denied. |
| 404  | NotFoundError                  | Wallet is not found |
| 500  | InternalServerError                  | Server error. |

## Activate a locked wallet 
Either by unlocking it or creating a new wallet. Wallet can be unlocked by verifying the most recent payment made, or it can be closed and have its funds transferred to a new wallet.


### Method

| Method | URL                                      |
|--------|------------------------------------------|
| PATCH    | <code class="highlighter-rouge">//V4.1/Wallets/{ID}/Activation</code> |

### Request Body


| Property                          | Type                   | Required? | Description |
|-----------------------------------|------------------------|-----------|-------------|
| Type | string enumL:[TransferFundsAndDelete, Unlock] | yes       |Type of Wallet activation request.             |
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

| Code | Type/Value                               | Description                              |
|------|------------------------------------------|------------------------------------------|
| 200  | Void                 | Wallet is unlocked and activated. |
| 201  | GuidResponse         | Wallet ID of the newly created wallet. The balance is moved to the new wallet.|
| 400  | RequestValidationWithBodyError                 | Invelid Request |
| 400  | ValidationError                  | Validation error. Please see inner errors for more details. |
| 400  | BadRequestError                  | Requested activation type is not supported. |
| 403  | ForbiddenRequestError                 | Access to this wallet is denied. |
| 403  | InvalidResourceStateError                  | Wallet cannot be activated at current status. |
| 404  | NotFoundError                 |   List of virtual accounts.List of virtual accounts. |
| 500  | InternalServerError                 | Server Error |

## Get the fee models associated with a wallet.


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

| Code | Type/Value                               | Description                              |
|------|------------------------------------------|------------------------------------------|
| 200  | Array <FeeModel>                  | List of fee models. |
| 400  | RequestValidationWithoutBodyError                  | Request Parameter is not valid. |
| 403  | ForbiddenRequestError | Access to this wallet is denied. |
| 404  | NotFoundError                 | Wallet is not found. |
| 500  | InternalServerError                 | Server Error. |

## Get webhook information. 
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

| Code | Type/Value                               | Description                              |
|------|------------------------------------------|------------------------------------------|
| 200  | WebhookResponse                 | Return the Wallet's Webhook. |
| 400  | RequestValidationWithoutBodyError                 | Invalid Request. |
| 403  | ForbiddenRequestError                 | Access to this wallet is denied. |
| 404  | NotFoundError                 | Webhook does not exist. |
| 500  | InternalServerError                 | Server Error. |

## Creates or updates webhook url and secret. 
You need to be enrolled in our webhook service in order to use this feature.

### Method

| Method | URL                                      |
|--------|------------------------------------------|
| PUT   | <code class="highlighter-rouge"> /V4.1/Wallets/{ID}/Webhook</code> |

### Request Body


| Property                          | Type                   | Required? | Description |
|-----------------------------------|------------------------|-----------|-------------|
| WebhookURL | string | yes       | URL that Gluwa should send webhook requests to            |
| Secret | string range: (16 to 256 chars max) | yes       | Any request Gluwa sends to WebhookUrl will be signed by using SHA256 HMAC hash of the request body json. Secret is used as the key for SHA256 HMAC hash            |



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

| Code | Type/Value                               | Description                              |
|------|------------------------------------------|------------------------------------------|
| 200  | WebhookResponse                | Webhook is updated. |
| 201  | WebhookResponse                  | Webhook is created. |
| 400  | RequestValidationWithBodyError                 | Invalid request. Check the format of the wallet ID. |
| 403  |ForbiddenRequestError                 | Access to this wallet is denied. |
| 404  | NotFoundError             | Wallet does not exist. |
| 500  | InternalServerError               | Server error. |



## Generates QR code containing transaction request.
Returns QR code as an image if url query parameter 'format' was passed. Supported formats are image/jpeg and image/png. Returns QR code as Base64 string if 'format' parameter was not passed.

### Method

| Method | URL                                      |
|--------|------------------------------------------|
| POST   | <code class="highlighter-rouge">/V4.1/Wallets/{ID}/QRCode</code> |

### Request Body


| Property                          | Type                   | Required? | Description |
|-----------------------------------|------------------------|-----------|-------------|
| Amount | string  | yes       |Amount to be paid to the merchant             |
| MerchantOrderID | string range (60 chars max) | no      | Optional field for the merchant's order ID            |
| Note | string range (250 char max) | no    |Optional field that will appear on the transaction's note             |

> Request Body example

```json
{
  "Amount": "string",
  "MerchantOrderID": "string",
  "Note": "string"
}
```

### Parameters

| Name | Type/Value   | Required | Path/Query | Description        |
|------|--------------|----------|------------|--------------------|
| ID   | string: uuid | yes      | Path       | Funding Source ID. |
| format   | string | yes      | Query     | Request body. |

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

| Code | Type/Value                               | Description                              |
|------|------------------------------------------|------------------------------------------|
| 200  | Void.           | QR code image with specified format. |
| 200  | String.                | List of virtual accounts. |
| 400  | RequestValidationWithBodyError                 | Bad Request. |
| 400  | BadRequestError             | Requested MIME type is not supported. |
| 403  | ForbiddenRequestError                 | Access to this wallet is denied. |
| 404  | NotFoundError                 | Wallet not found. |
| 500  | InternalServerError            | Server Error. |
    
