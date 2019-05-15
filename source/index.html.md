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

# Introduction

Welcome to the Gluwa API Reference Documentation.

# Bank

Bank API provides the list of financial institutions that Gluwa supports. Get the list of banks by country, or search for a specific institution.

## List banks

Get the list of banks supported by Gluwa

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

## Search banks

Search through the list of banks by keyword

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
Response Codes

| Code | Type/Value                        | Description                            |
|------|-----------------------------------|----------------------------------------|
| 200  | Array: FinancialInstitution      | List of Banks in aphabetical order.    |
| 400  | RequestValidationWithoutBodyError | Invalid country parameter is provided. |

# Funding Source

A funding source represents the source of fiat currency such as bank accounts and credit cards. Users can make deposits to Gluwa from their funding sources, or withdraw from Gluwa to their funding sources. Funding source API can be used to create funding sources under a user or to retrieve a list of funding sources that belong to a user. Micro deposit endpoints are for initiating and completing the verification process that is required before a funding source can be used. This resource also includes an endpoint that change the status of a funding source to deactivated.

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

Response Codes

| Code | Type/Value                        | Description                              |
|------|-----------------------------------|------------------------------------------|
| 200  | Array: FundingSourceResponse                           | List of Funding Resources              |
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

## Verify a crypto funding source
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


### Create a new cryptocurrency funding source

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
