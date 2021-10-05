## External API Endpoints
```x-api-key``` should be added in http request headers, with the value we gave.

If ```x-api-key``` is not present or expired, then it will return 406 Error in every requests.

- /api/v1/external/request/{token symbol or address} ```[POST]```

It accepts scanning requests, and return response id.

If not found the token name or not valid address, then rejects with 404 Error.

example:

/api/v1/external/request/0x1967ABfdc4ae7961C5a8A5395469222260C27c02

/api/v1/external/request/shark


response:

```
{
  "id": "15"
}
```

- /api/v1/external/result/{responseId} ```[GET]```

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
    "scannedAt": 1633051197921
  }
}
```
