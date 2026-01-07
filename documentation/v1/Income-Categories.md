# Income Categories

------

*Please note https is required for ALL API requests.*  

------

**Please note, in API endpoints we pass the "Club Key" as `club_secret`**

*(Club Key can be found in System Settings → Business Info → Advanced)*

------

## METHODS

| Method | URL | Info | URL-params |
| --- | --- | --- | --- |
| GET | /api/v1/club/`<club_id>`/income-categories | Returns all income categories defined for the specified club | api_key, club_secret |

## STRUCTURE

| Field | Type | Optional | Information |
| --- | --- | --- | --- |
| income_category_id | string | no | The ID of the income category |
| income_category_name | string | no | Display name of the income category |
| default_tax | string | yes | null if not set. Name of the default tax assigned to this income category |
| default_tax_id | int | yes | The ID of the tax associated with the income category |

## GET Example

Send a GET request to:  
`https://api.virtuagym.com/api/v1/club/<club_id>/income-categories?api_key=<api_key>&club_secret=<club_secret>`

```json
{
  "status": {
    "statuscode": 200,
    "statusmessage": "Everything OK",
    "result_count": 13,
    "timestamp": 1741690381107
  },
  "result": [
    {
      "income_category_id": "807351352f4ba59de9bfe1c5cae3847f206d",
      "income_category_name": "Food & drinks",
      "default_tax": "Default Tax",
      "default_tax_id": 1
    },
    {
      "income_category_id": "1dcc5329ea94d192aa6cb1e677e272b90399",
      "income_category_name": "Memberships",
      "default_tax": null,
      "default_tax_id": null
    }
  ]
}

``` 

## Possible error codes and messages

| Statuscode | Message               | Description                                                   |
|------------|-----------------------|---------------------------------------------------------------|
| 200        | Everything OK         | Successful response with income category data                |
| 403        | Forbidden             | Missing or invalid `api_key` or `club_secret`                |
| 404        | Club not found        | Specified `<club_id>` does not exist or access is restricted |
| 500        | Internal server error | Unexpected error occurred on the server                      |
