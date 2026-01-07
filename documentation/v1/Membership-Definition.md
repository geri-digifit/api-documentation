# Membership Definition

For every call to the API, two parameters are mandatory:

- club_secret

- api_key

=> They can be retrieved inside business-settings > advanced


| Method | URL                                                       | Description                                                 | URL Parameters                                                                 |
|--------|-----------------------------------------------------------|-------------------------------------------------------------|--------------------------------------------------------------------------------|
| **GET** | /api/v1/club/`[club_id]`/membership/definition           | Returns an array of membership instances of the given club  | `api_key` (string), `club_secret` (string), `sync_from` *(int - miliseconds)*, `page` (int), `status` (string) |


**Parameters:**

- `sync_from` => (optional) filters membership definitions based on timestamp_edit, returning results with a timestamp greater than the specified value. Default is 0.

- `status` => (optional) a filter that specifies the membership status. Acceptable values include all, active, and inactive. Retrieves only membership definitions that match the selected statuses. Default is all

- `page` => (optional) 25 results are displayed per page, to get the remaining ones the call needs to indicate the specific page (see how many results are still remaining in the results_remaining field)


## Membership Object

| Field                           | Type     | Values                             | Description                                           |
|---------------------------------|----------|-------------------------------------|-------------------------------------------------------|
| membership_id                   | integer  | -                                   | Unique identifier for the membership.                 |
| membership_name                 | string   | -                                   | Name of the membership.                               |
| membership_group                | string   | -                                   | Group the membership belongs to.                      |
| membership_notes                | string   | -                                   | Additional notes for the membership.                  |
| membership_availability_start   | date     | -                                   | Start date of membership availability.                |
| membership_availability_end     | date     | -                                   | End date of membership availability.                  |
| membership_available_online     | boolean  | `true`, `false`                     | Whether the membership is available online.           |
| membership_duration             | integer  | -                                   | Duration of the membership.                           |
| membership_duration_type        | string   | `weeks`, `months`                   | Type of duration.                                     |
| membership_auto_renew           | boolean  | `true`, `false`                     | Whether the membership auto-renews.                   |
| membership_pro_rata_start       | boolean  | `true`, `false`                     | Whether pro-rata billing is applied at the start.     |
| membership_renew_duration       | integer  | -                                   | Duration for renewal.                                 |
| membership_renew_term           | string   | `weeks`, `months`                   | Term for renewal duration.                            |
| membership_renew_before         | integer  | -                                   | Days before renewal.                                  |
| membership_renew_before_term    | string   | `days`, `weeks`, `months`           | Term for renewal before.                              |
| membership_renew_price          | decimal  | -                                   | Price for renewal.                                    |
| membership_price                | decimal  | -                                   | Price of the membership.                              |
| membership_price_term           | string   | `total`, `monthly`, `weekly`, `four_weekly` | Term for the price.                                   |
| membership_income_category      | string   | -                                   | Income category for the membership.                   |
| membership_registration_fee     | decimal  | -                                   | Registration fee for the membership.                  |
| membership_club_tax             | object   | -                                   | Tax information for the membership. *(see below)*     |
| membership_billing_cycle        | string   | -                                   | Billing cycle for the membership.                     |
| default_payment_method          | string   | -                                   | Default payment method for the membership.            |
| tmp_default_payment_method      | string   | -                                   | Temporary default payment method for the membership.  |
| membership_creation_date        | date     | -                                   | Date the membership was created.                      |
| membership_last_edit_date       | date     | -                                   | Date the membership was last edited.                  |
| membership_invoice_creation_term| string   | -                                   | Term for invoice creation.                            |
| access_times                    | array    | -                                   | Access times for the membership. *(see below)*        |

---

### `membership_club_tax` Object

| Field         | Type     | Values | Description                  |
|---------------|----------|--------|------------------------------|
| tax_id        | integer  | -      | Unique identifier for the tax. |
| tax_name      | string   | -      | Name of the tax.              |
| tax_percentage| decimal  | -      | Percentage of the tax.        |

---

### `access_times` Array

Each element in the array has the following fields:

| Field      | Type   | Values | Description           |
|------------|--------|--------|-----------------------|
| day        | string | -      | Day of the week.      |
| start_time | time   | -      | Start time for access.|
| end_time   | time   | -      | End time for access.  |



### GET example

Send a GET request to: `https://api.virtuagym.com/api/v1/club/<club_id>/membership/definition?api_key=[api_key]&club_secret=[club_secret]&page=[number]&status=[status]&sync_from=<sync_from>`




```json
{
    "status": {
        "statuscode": 200,
        "statusmessage": "Everything OK",
        "result_count": 4,
        "timestamp": 1750691269288,
        "results_remaining": 0
    },
    "result": [
        {
            "membership_id": 10067756,
            "membership_name": "Webshop avail vis test",
            "membership_group": "default",
            "membership_notes": "Webshop avail vis test",
            "membership_availability_start": "2025-02-28",
            "membership_availability_end": "2025-03-20",
            "membership_available_online": false,
            "membership_duration": 12,
            "membership_duration_type": "months",
            "membership_auto_renew": false,
            "membership_pro_rata_start": true,
            "membership_renew_price": 0,
            "membership_price": 10,
            "membership_price_term": "monthly",
            "membership_income_category": "memberships",
            "membership_club_tax": {
                "tax_id": 1,
                "tax_name": "",
                "tax_percentage": 0
            },
            "membership_billing_cycle": "monthly",
            "default_payment_method": "mollie_sepa",
            "tmp_default_payment_method": "direct_debit",
            "membership_creation_date": "2025-03-20",
            "membership_last_edit_date": "2025-03-20",
            "membership_invoice_creation_term": "1 weeks"
        },
        {
            "membership_id": 10056451,
            "membership_name": "Credits Limited Access",
            "membership_group": "default",
            "membership_availability_start": "2025-06-17",
            "membership_availability_end": "2025-06-17",
            "membership_available_online": false,
            "membership_duration": 1,
            "membership_duration_type": "months",
            "membership_auto_renew": false,
            "membership_pro_rata_start": false,
            "membership_renew_price": 0,
            "membership_price": 10,
            "membership_price_term": "monthly",
            "membership_income_category": "memberships",
            "membership_club_tax": {
                "tax_id": 1,
                "tax_name": "",
                "tax_percentage": 0
            },
            "membership_billing_cycle": "monthly",
            "default_payment_method": "cash",
            "tmp_default_payment_method": "cash",
            "membership_creation_date": "2025-03-14",
            "membership_last_edit_date": "2025-06-17",
            "membership_invoice_creation_term": "1 days",
            "access_times": [
                {
                    "day": "Sunday",
                    "start_time": "00:00:00",
                    "end_time": "00:00:00"
                },
                {
                    "day": "Monday",
                    "start_time": "04:45:00",
                    "end_time": "13:00:00"
                },
                {
                    "day": "Monday",
                    "start_time": "14:00:00",
                    "end_time": "19:00:00"
                },
                {
                    "day": "Tuesday",
                    "start_time": "09:00:00",
                    "end_time": "17:00:00"
                },
                {
                    "day": "Wednesday",
                    "start_time": "00:00:00",
                    "end_time": "00:00:00"
                },
                {
                    "day": "Thursday",
                    "start_time": "07:00:00",
                    "end_time": "13:00:00"
                },
                {
                    "day": "Friday",
                    "start_time": "10:00:00",
                    "end_time": "12:00:00"
                },
                {
                    "day": "Saturday",
                    "start_time": "03:00:00",
                    "end_time": "16:00:00"
                },
                {
                    "day": "Saturday",
                    "start_time": "17:00:00",
                    "end_time": "22:00:00"
                }
            ]
        },
        {
            "membership_id": 10143648,
            "membership_name": "4 weekly",
            "membership_group": "default",
            "membership_notes": "4 weekly",
            "membership_availability_start": "1970-01-01",
            "membership_availability_end": "1970-01-02",
            "membership_available_online": false,
            "membership_duration": 12,
            "membership_duration_type": "months",
            "membership_auto_renew": true,
            "membership_pro_rata_start": true,
            "membership_renew_duration": 1,
            "membership_renew_term": "months",
            "membership_renew_before": 1,
            "membership_renew_before_term": "months",
            "membership_renew_price": 50,
            "membership_price": 50,
            "membership_price_term": "four_weekly",
            "membership_income_category": "memberships",
            "membership_club_tax": {
                "tax_id": 1,
                "tax_name": "",
                "tax_percentage": 0
            },
            "default_payment_method": "cash",
            "tmp_default_payment_method": "cash",
            "membership_creation_date": "2025-05-21",
            "membership_last_edit_date": "2025-06-19",
            "membership_invoice_creation_term": "4 days",
            "access_times": [
                {
                    "day": "Monday",
                    "start_time": "06:00:00",
                    "end_time": "18:00:00"
                },
                {
                    "day": "Tuesday",
                    "start_time": "09:00:00",
                    "end_time": "16:00:00"
                }
            ]
        },
        {
            "membership_id": 5774756,
            "membership_name": "Basistarif 1",
            "membership_group": "default",
            "membership_notes": "test test",
            "membership_availability_start": "1970-01-01",
            "membership_availability_end": "1970-01-02",
            "membership_available_online": false,
            "membership_duration": 6,
            "membership_duration_type": "months",
            "membership_auto_renew": true,
            "membership_pro_rata_start": false,
            "membership_renew_duration": 1,
            "membership_renew_term": "months",
            "membership_renew_before": 1,
            "membership_renew_before_term": "months",
            "membership_renew_price": 79,
            "membership_price": 79,
            "membership_price_term": "monthly",
            "membership_income_category": "memberships",
            "membership_club_tax": {
                "tax_id": "1",
                "tax_name": "BTW 21%",
                "tax_percentage": 21
            },
            "membership_billing_cycle": "two_monthly",
            "default_payment_method": "direct_debit",
            "tmp_default_payment_method": "direct_debit",
            "membership_creation_date": "2024-05-14",
            "membership_last_edit_date": "2025-06-23",
            "membership_invoice_creation_term": "10 days"
        }
    ]
}
```

### Possible error codes and messages

| Status Code | Message                                                           | Description                                                                                      |
|-------------|-------------------------------------------------------------------|--------------------------------------------------------------------------------------------------|
| 200         | Everything OK                                                     | Everything proceeded according to the specifications.                                           |
| 401         | Missing or invalid API Key.                                       | Check if the API Key is correct.                                                                 |
| 401         | Incorrect club key                                                 | Incorrect `club_secret` or `club_id` being used.                                                 |
| 404         | Invalid API request                                                | Incorrect URL being used (check the format being used).                                          |
| 420         | Invalid status parameter. Allowed values are: `all`, `active`, `inactive` | We allow only 3 filter values:<br>**Active** – currently active memberships<br>**Inactive** – currently inactive memberships (not assignable to any member)<br>**All** – both membership types (active + inactive) |
