# Membership Instances

For every call to the API, two parameters are mandatory:

- club_secret

- api_key

=> They can be retrieved inside business-settings > advanced

| Method | URL | Info | URL Params |
|--------|-----|------|------------|
| GET |/api/v1/club/`[club_id]`/membership/instance|Returns an array of membership instances of the given club|`api_key` (string) , `club_secret` (string) , `sync_from` (int - miliseconds), `member_id` (int), `from_id` (int)|
| POST | /api/v1/club/`[club_id]`/membership/instance | Returns the created contract | `api_key` (string), `club_secret` (string) |

**Parameters:**

- `sync_from` => (optional) filters membership definitions based on timestamp_edit, returning results with a timestamp greater than the specified value. Default is 0.
- `member_id` => (optional) Indicate the `member_id` that you want to get the membership instances from 
- `from_id` => (optional) In some cases it is better to use `from_id` (coming from the `instance_id` field on the response) as multiple rows might have the same `sync_from` thereby not allowing for pagination. For the first call when using `from_id` it is allowed to pass 0 and it would allow for pagination.

## GET Example

**STRUCTURE**

|Field|Type|Optional|Values|Information|
|---|---|---|---|---|
|instance_id|int|no| |The id of the membership instance|
|member_id|int|no| |The id of the [[Club Members]]|
|membership_id|int|no| |The id of the [[Membership Definition]]|
|active|boolean|no| |If the membership instance is active or inactive|
|cancelled|boolean|no| |If the membership was manually cancelled by an employee with termination in the future|
|contract_autorenewed|boolean|no| |If the membership was already renewed at this point or it's still in its initial period|
|completed|boolean|no| |If the membership has reached the contract end date automatically and was not renewed|
|paused|boolean|no| |If the membership instance was paused by an employee|
|stopped|boolean|no| |If the membership was manually cancelled by an employee with immediate termination|
|start_date|string|no| |The actual start date of the membership (yyyy-mm-dd)|
|contract_start_date|string|no| |The contractual start date of the membership (yyyy-mm-dd)|
|contract_end_date|string|no| |The contractual  end date of the membership (yyyy-mm-dd)|
|membership_name|string|no| |The name of the membership|


Send a GET request to: `https://api.virtuagym.com/api/v1/club/<club_id>/membership/instance?api_key=<api_key>&club_secret=<club_secret>&sync_from=<sync_from>` OR `https://api.virtuagym.com/api/v1/club/<club_id>/membership/instance?api_key=<api_key>&club_secret=<club_secret>&from_id=<from_id>`



```json
{
    "statuscode": 200,
    "statusmessage": "Everything OK",
    "result_count": 1,
    "timestamp": 1429533294,
    "result": [
        {
            "instance_id": 2537,
            "membership_id": 584,
            "member_id": 77038,
            "active": false,
            "cancelled": true,
            "completed": true,
            "paused": false,
            "stopped": false,
            "start_date": "2015-04-01",
            "contract_start_date": "2015-05-01",
            "contract_end_date": "2015-07-31",
            "membership_name": "Test"
        }
    ]
}
```


## POST Example

The POST request allows to create a membership instance (contract) from an existing membership definition (membership) to an existing member.

**STRUCTURE**

| Field               | Type    | Required                              | Values                               | Information                                                                                     |
|---------------------|---------|---------------------------------------|---------------------------------------|-------------------------------------------------------------------------------------------------|
| membership_id       | integer | Yes                                   | -                                     | ID of the membership to assign                                                                  |
| member_id           | integer | Yes                                   | -                                     | ID of the member to assign the membership to                                                    |
| start_date          | string  | Yes                                   | YYYY-MM-DD                            | Start date of the contract                                                                      |
| payment_method      | string  | Yes                                   | cash, card, direct_debit, etc.        | Payment method for the contract                                                                 |
| salesperson_id      | integer | Yes                                   | -                                     | ID of the salesperson who sold the contract                                                     |
| discount_id         | integer | No                                    | -                                     | ID of a preset discount to apply                                                                |
| discount_start_date | string  | No (yes if there’s a discount_id)     | YYYY-MM-DD                            | Start date of the discount (use here if there’s a preset discount - not optional if there’s one)|
| custom_discount     | object  | No                                    | -                                     | Custom discount configuration (see below)                                                       |
| contract_notes      | string  | No                                    | -                                     | Additional notes for the contract                                                               |
| bill_to             | string  | No                                    | -                                     | Business GUID if billing to a business                                                          |

**Custom Discount Object**

| Field               | Type   | Optional | Values                  | Information                        |
|---------------------|--------|----------|-------------------------|-------------------------------------|
| discount_amount     | float  | No       | > 0                     | Amount of the discount              |
| discount_amount_type| string | No       | percentage, monetary    | Type of discount                    |
| discount_start_date | string | No       | YYYY-MM-DD              | Start date of the discount          |
| discount_duration   | object | Yes      | -                       | Duration configuration (see below)  |


**Discount Duration Object**

| Field | Type    | Optional | Values        | Information                   |
|-------|---------|----------|---------------|--------------------------------|
| time  | integer | No       | > 0           | Number of weeks/months         |
| term  | string  | No       | weeks, months | Time unit for the duration     |


**Response Structure**

| Field                  | Type    | Values                     | Information                      |
|------------------------|---------|----------------------------|-----------------------------------|
| id                     | integer | -                          | Contract ID                       |
| contract_number        | string  | -                          | Contract number                   |
| membership_id          | integer | -                          | ID of the assigned membership     |
| member_id              | integer | -                          | ID of the member                  |
| start_date             | string  | YYYY-MM-DD                 | Contract start date                |
| contract_start_date    | string  | YYYY-MM-DD                 | Contract start date                |
| contract_end_date      | string  | YYYY-MM-DD                 | Contract end date                  |
| contract_active        | boolean | true, false                | Contract status                    |
| contract_payment_method| string  | cash, card, direct_debit, etc. | Payment method                 |
| discount_id            | integer | -                          | Discount ID                        |
| discount_name          | string  | -                          | Discount name                      |
| discount_amount        | float   | -                          | Discount amount                    |
| discount_amount_type   | string  | percentage, monetary       | Discount type                      |
| discount_start_date    | string  | YYYY-MM-DD                 | Discount start date                 |
| discount_duration      | object  | -                          | Discount duration information      |
| discount_duration_time | integer | -                          | Duration time                      |
| discount_duration_term | string  | weeks, months              | Duration term                      |



Send a POST request to `https://api.virtuagym.com/api/v1/club/<club_id>/membership/instance?api_key=[api_key]&club_secret=[club_secret]` with the following JSON:

**Without Discount**

```json
{
    "membership_id": 10215539,
    "member_id": 1750534197,
    "start_date": "2025-06-22",
    "payment_method": "cash",
    "salesperson_id": -5
}
```

*Response*

```json
{
    "status": {
        "statuscode": 200,
        "statusmessage": "Everything OK",
        "result_count": 10,
        "timestamp": 1755014123353
    },
    "result": {
        "id": 988796319,
        "contract_number": 2037,
        "membership_id": 10215539,
        "member_id": 1750534197,
        "start_date": "2025-06-22",
        "contract_start_date": "2025-07-01",
        "contract_end_date": "2026-06-30",
        "contract_active": true,
        "contract_payment_method": "cash",
        "discount_duration": {
            "discount_duration_time": null,
            "discount_duration_term": null
        }
    }
}
```


**With Preset Discount**

```json
{
    "membership_id": 10215539,
    "member_id": 1750504236,
    "start_date": "2025-05-25",
    "payment_method": "cash",
    "salesperson_id": -5,
    "discount_id": 123499526,
    "discount_start_date": "2025-05-25"
}
```

*Response*

```json
{
    "status": {
        "statuscode": 200,
        "statusmessage": "Everything OK",
        "result_count": 15,
        "timestamp": 1755014167175
    },
    "result": {
        "id": 988796323,
        "contract_number": 2039,
        "membership_id": 10215539,
        "member_id": 1750504236,
        "start_date": "2025-05-25",
        "contract_start_date": "2025-06-01",
        "contract_end_date": "2026-05-31",
        "contract_active": true,
        "contract_payment_method": "cash",
        "discount_id": 123499526,
        "discount_name": "Student",
        "discount_amount": 5,
        "discount_amount_type": "monetary",
        "discount_start_date": "2025-05-25",
        "discount_duration": {
            "discount_duration_time": null,
            "discount_duration_term": null
        }
    }
}
```

**With Custom Discount**

```json
{
    "membership_id": 10215539,
    "member_id": 1750502732,
    "start_date": "2025-02-01",
    "payment_method": "cash",
    "salesperson_id": -5,
    "custom_discount": {
        "discount_amount": 15,
        "discount_amount_type": "percent",
        "discount_start_date": "2025-02-01",
        "discount_duration": {
            "time": 24,
            "term": "weeks"
        }
    }
}
```

*Response*

```json
{
    "status": {
        "statuscode": 200,
        "statusmessage": "Everything OK",
        "result_count": 13,
        "timestamp": 1755014269859
    },
    "result": {
        "id": 988796413,
        "contract_number": 2041,
        "membership_id": 10215539,
        "member_id": 1750502732,
        "start_date": "2025-02-01",
        "contract_start_date": "2025-02-01",
        "contract_end_date": "2026-01-31",
        "contract_active": true,
        "contract_payment_method": "cash",
        "discount_amount": 15,
        "discount_amount_type": "percent",
        "discount_start_date": "2025-02-01",
        "discount_duration": {
            "discount_duration_time": 24,
            "discount_duration_term": "weeks"
        }
    }
}
```


### Possible error codes and messages

| Status Code | Message                                                                                                             | Description                                                                                      |
|-------------|---------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------|
| 200         | Everything OK                                                                                                       | Everything proceeded according to the specifications                                            |
| 401         | Missing or invalid API Key.                                                                                         | Check if the API Key is correct.                                                                 |
| 401         | Incorrect club key                                                                                                  | Incorrect club_secret or club_id being used                                                      |
| 404         | Invalid API request                                                                                                 | Incorrect URL being used (check the format being used)                                           |
| 400         | Field 'membership_id' is required                                                                                   | A membership_id must be provided.                                                                |
| 400         | Field 'membership_id' must be an integer                                                                            | membership_id must be of integer type.                                                           |
| 400         | Field 'membership_id' must be greater than 0                                                                        | membership_id must be a positive integer.                                                        |
| 404         | Membership not found or inactive                                                                                    | The specified membership ID does not exist or is no longer active.                               |
| 400         | Field 'member_id' is required                                                                                       | A member_id must be provided.                                                                    |
| 400         | Field 'member_id' must be an integer                                                                                | member_id must be of integer type.                                                               |
| 400         | Field 'member_id' must be greater than 0                                                                            | member_id must be a positive integer.                                                            |
| 404         | Member is inactive or doesn't exist                                                                                 | The member ID provided does not correspond to any active member.                                 |
| 400         | Field 'start_date' is required                                                                                      | A start_date must be provided.                                                                   |
| 400         | Field 'start_date' must be a valid date in YYYY-MM-DD format                                                        | The date format is invalid.                                                                      |
| 400         | Field 'start_date' must be greater than [-50 years]                                                                 | The start date must not be more than 50 years in the past.                                        |
| 400         | Field 'start_date' must be less than [+50 years]                                                                    | The start date must not be more than 50 years in the future.                                      |
| 400         | Field 'payment_method' is required                                                                                  | A payment_method must be provided.                                                               |
| 400         | Field 'payment_method' must be one of: [LIST OF AVAILABLE PAYMENT METHODS]                                          | The payment_method is not recognized.                                                            |
| 400         | Field 'salesperson_id' is required                                                                                  | A salesperson_id must be provided.                                                               |
| 400         | Field 'salesperson_id' is must be an integer                                                                        | salesperson_id must be of integer type.                                                          |
| 400         | Field 'salesperson_id' must be greater than 0                                                                       | salesperson_id must be a positive integer.                                                       |
| 404         | salesperson_id does not exist in club                                                                               | The salesperson ID is not associated with the specified club.                                    |
| 400         | Field 'discount_id' must be an integer                                                                              | discount_id must be of integer type.                                                             |
| 400         | Field 'discount_id' must be greater than 0                                                                          | discount_id must be a positive integer.                                                          |
| 404         | discount_id not found                                                                                               | The provided discount ID does not exist.                                                          |
| 400         | Field 'discount_start_date' is required when 'discount_id' is provided                                              | The discount start date must be included if a discount is applied.                               |
| 400         | Field 'discount_start_date' must be a valid date in YYYY-MM-DD format                                               | The discount start date format is incorrect.                                                     |
| 400         | Field 'discount_amount' is required, Field 'discount_amount_type' is required, Field 'discount_start_date' is required | All discount fields must be specified.                                                           |
| 400         | Fields 'discount_id' and 'custom_discount' are mutually exclusive, Fields 'custom_discount' and 'discount_id' are mutually exclusive | Only one of custom_discount or discount_id can be provided. Duplicate declaration not allowed.   |
| 400         | Field 'discount_amount' must be greater than 0                                                                      | The custom discount value must be a positive number.                                             |
| 400         | Field 'discount_amount_type' must be one of: monetary, percent, fixed                                               | The custom discount type is not valid.                                                           |
| 400         | Field 'discount_start_date' must be a valid date in YYYY-MM-DD format                                               | The custom discount start date format is incorrect.                                              |
| 400         | It is not possible to choose a discount start date earlier than the membership instance start date                  | The provided start date must not precede the start of the membership.                            |
| 400         | Field 'time' is required, Field 'term' is required                                                                  | A valid term must be provided. A valid time must be provided.                                    |
| 400         | Field 'time' must be greater than 0                                                                                 | The time must be a positive value.                                                               |
| 400         | Field 'term' must be one of: weeks, months                                                                          | A valid term must be provided.                                                                   |
| 400         | Field 'contract_notes' must be at most 1000 characters long                                                         | The character limit for contract_notes is 1000.                                                   |
| 400         | Invalid bill_to                                                                                                     | The value of bill_to is not valid.                                                                |
| 400         | Invalid bill_to: member not found                                                                                   | The member referenced in bill_to could not be found.                                             |
| 400         | Invalid bill_to: member does not pays for membership                                                                | The member in bill_to is not eligible to pay for the membership.                                 |
| 400         | Duration should be 4 weeks or a period that's a multiple of 4 weeks. (so 8, 12, 16, 20 etc)                         | The duration value must align with standard billing cycles.                                      |
