# CRYPTOWATCHTOWER.IO API Guides

## Introduction

Cryptowatchtower API provides Scam / Rug Pull scanning features for 3rd party platforms.

It includes the following functional end points:

- Individual token scanning

- Get all risky tokens list (blacklist of tokens)

For authentication, ```x-api-key``` should be added into every http request headers, with the value we provide.

If ```x-api-key``` is not present or expired, then it will return 406 Error.

## Hosts

- api.cryptowatchtower.io (live server)

- api-staging.cryptowatchtower.io (staging server)

## Fields description

- riskRating

type: number, range: 0 ~ 10

It indicates risk rating about the token.

The higher score, the more safe.

7 ~ 10 : Low Risk

4 ~ 6 : Medium Risk

0 ~ 3 : High Risk

- unLaunched, boolean

It indicates whether the token is launched or in the pre-sale period.

- scannedAt, number

It indicates timestamp when it was scanned at last.



# Endpoints

## /api/v1/external/request/{individual token symbol or address} ```[POST]```

It accepts scanning request of a token and returns result.

If the token was scanned recently and keeps latest, then it will return final results directly, with 'completed' state.

If the token data is not registered in our database or expired, then it re-scans the token, and returns the progress while in the middle of scanning.

So you should invoke this endpoint regularly(in every 3~4 seconds), in order to get the final result.

If token symbol or address is invalid, then it will reject with 404 Error.

Please refer to the following "Response" examples.

Example:

/api/v1/external/request/0x1967ABfdc4ae7961C5a8A5395469222260C27c02

Response:

```
{
  "state": "pending",
  "result": {}
}
```

```
{
  "state": "active",
  "progress": 30,
  "result": {}
}
```

```
{
  "state": "completed",
  "result": {
    "address": "0x1967abfdc4ae7961c5a8a5395469222260c27c02",
    "network": "bsc",
    "name": "Watchtower",
    "symbol": "WTW",
    "decimals": 9,
    "riskRating": 8,
    "scannedAt": 1634712192228,
    "unLaunched": false
  }
}
```

## /api/v1/external/blacklists  ```[GET]```

It returns all risky tokens list with pagination.

query parameters: 

- network: bsc / ethereum, default bsc
- page: starts from 1, default 1
- perPage: <2000, default 1000

Example:

/api/v1/external/blacklists?page=7&perPage=2000&network=ethereum


Response: 

```
{
  "network": "bsc",
  "counts": 17852,
  "perPage": 1000,
  "pages": 18,
  "page": 1,
  "results": [
    {
      "address": "0xfc0c4bb5abeef7f5fa7abdbdcbf997e682ff0394",
      "riskRating": 0
    },
    {
      "address": "0xfed8f06d6cdf7a0f1f917838fb5c263eda61e82a",
      "riskRating": 1
    },

    ...
    
    {
      "address": "0x57264840205f613fa47cce12d61651657accf925",
      "riskRating": 1
    }
  ]
}
```
