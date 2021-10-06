# CRYPTOWATCHTOWER.IO API Description

## Basic flow

You can use our API to detect scam / rug pulls with token name or address.

Process of searching new token consists of 2 steps: sending request, and polling result.

For authentication, ```x-api-key``` should be added into http request headers, with the value we generated.

If ```x-api-key``` is not present or expired, then it will return 406 Error.

## Endpoints

- /api/v1/external/request/{token symbol or address} ```[POST]```

It accepts scanning requests, and returns response id.

If token symbol or address is invalid, then it will reject with 404 Error.

Example:

/api/v1/external/request/0x1967ABfdc4ae7961C5a8A5395469222260C27c02

/api/v1/external/request/shark


Response:

```
{
  "id": "15"
}
```

- /api/v1/external/result/{responseId} ```[GET]```

With response id, you can get the result of searching status.

If it is still pending, then it will return ```active``` state with job progress.

If it is failed, then it will return ```failed``` status.

```
{
  "id": "8",
  "state": "active",
  "progress": "30",
  "data": {}
}
```

```
{
  "id":"6",
  "state":"failed",
  "progress":30,
  "reason":"error-while-fetching-lp",
  "data":{}
}
```

If it is completed, then it returns full result under the field of ```data```.

```
{
  "id": "30",
  "state": "completed",
  "progress": 100,
  "data": {
    "token": {
      "id": "watchtower",
      "name": "Watchtower",
      "symbol": "WTW",
      "decimals": 9,
      "createdAt": "2021-07-05 21:26:56",
      "address": "0x1967ABfdc4ae7961C5a8A5395469222260C27c02",
      "network": "bsc",
      "mainToken": "WBNB",
      "dexTitles": [
        "Pancake v2",
        "Pancake"
      ]
    },
    "riskRating": 8,
    "unLaunched": false,
    "scannedAt": 1633051197921
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

It indicates timestamp when it was scanned finally.
