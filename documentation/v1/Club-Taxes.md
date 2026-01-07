# Income Categories
This API allows to list all income categories belonging to a club.  

|Method|URL|Info|URL-params|
|---|-------------------------------------|------------------------------------------------------------|---|
|GET| /api/v1/club/`<club_id>`/club-taxes/|Returns all club taxes associated with the specified club_id|api_key, club_secret|

## INCOME CATEGORY STRUCTURE
|Field|Type|Optional|Information|
|---|---|------|--------------------------------------------|
|tax_id|int|no|the guid of the club tax|
|tax_name|string|no|the name of the club tax|
|tax_perc|string|no|the percentage value of the club tax|
|date_from|string|no|the start date when the club tax becomes active|

## GET Example
```json
{
  "status": {
    "statuscode": 200,
    "statusmessage": "Everything OK",
    "result_count": 4,
    "timestamp": 1557392696994,
  },
  "results": [
    {
      "tax_id": "a8e0c9fd8cf7267f8fe63c3f6d484daac7ab",
      "tax_name": "BTW 6%",
      "tax_perc": "6.00",
      "date_from": "2010-01-01"
    },
    {
      "tax_id": "b08b30b67046299bdb4c74697f2feae187e4",
      "tax_name": "BTW 21%",
      "tax_perc": "21.00",
      "date_from": "2010-01-01"
    },
    {
      "tax_id": "02a4af753eacd84a5773bdcd5d7b0b01c352",
      "tax_name": "BTW 12%",
      "tax_perc": "12.00",
      "date_from": "2010-01-01"
    },
    {
      "tax_id": "0d3c060d8c621fc77865cfc185a10408f657",
      "tax_name": "BTW 15%",
      "tax_perc": "15.00",
      "date_from": "2010-01-01"
    }
  ]
}
```

## Possible error codes and messages
|Statuscode|Message|Description|
|---|---|---|
|200|Everything OK|Everything proceeded according to the specifications|
