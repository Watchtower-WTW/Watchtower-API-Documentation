# CRYPTOWATCHTOWER.IO API Description

## Basic flow

You can use our API to detect scam / rug pulls with token name or address.

For authentication, ```x-api-key``` should be added into http request headers, with the value we provide.

If ```x-api-key``` is not present or expired, then it will return 406 Error.

## Hosts

- api.cryptowatchtower.io (live server)

- api-staging.cryptowatchtower.io (staging server)

## Endpoints

- /api/v1/external/request/{token symbol or address} ```[POST]```

It accepts requests and returns results about tokens.

If it is expired or not registered in our database, then it re-scans the token, and return the progress while in the middle of scanning.

So you should invoke this endpoint regularly(in every 3~4 seconds), in order to get the final result.

If the token was scanned recently, then it will return final results directly, with 'completed' state.

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

## Field description

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
