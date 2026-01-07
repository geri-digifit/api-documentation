# Income Categories
This API allows to list all income categories belonging to a club.  

|Method|URL|Info|URL-params|
|---|---|---|---|
|GET|/api/v1/club/`<club_id>`/income-categories/|Returns all income categories associated with the specified club_id|api_key, club_secret|

## INCOME CATEGORY STRUCTURE
|Field|Type|Optional|Information|
|---|---|---|---|
|income_category_id|string|no|the guid of the income category|
|income_category_name|string|no|the name of the income category|
|default_tax|string|yes|default VAT/Tax|
|default_tax_id|int|yes|default VAT/Tax id|

## GET Example
```json
{
  "status": {
    "statuscode": 200,
    "statusmessage": "Everything OK",
    "result_count": 5,
    "timestamp": 1557392696994,
  },
  "results": [
    {
      "income_category_id": 1,
      "income_category_name": "Memberships",
      "default_tax": "BTW 10% (10.00%)",
      "default_tax_id": 1001
    },
    {
      "income_category_id": 2,
      "income_category_name": "Personal Training",
      "default_tax": "BTW 21% (21.00%)",
      "default_tax_id": 1002
    },
    {
      "income_category_id": 3,
      "income_category_name": "Food & Beverage",
      "default_tax": "BTW 12% (12.00%)",
      "default_tax_id": 1003
    },
    {
      "income_category_id": 4,
      "income_category_name": "Access Cards",
      "default_tax": "Push Test Income Cat 50% (50.00%)",
      "default_tax_id": 1004
    },
    {
      "income_category_id": 5,
      "income_category_name": "Online Training",
      "default_tax": "Online Training 0% (00.00%)",
      "default_tax_id": 1005
    }
  ]
}
```

## Possible error codes and messages
|Statuscode|Message|Description|
|---|---|---|
|200|Everything OK|Everything proceeded according to the specifications|
