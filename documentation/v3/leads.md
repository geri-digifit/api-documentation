# Virtuagym Leads API

In order to implement successful communication between your backend and Virtuagym lead management endpoints, follow the steps below.

---

## 1. Register as a Client

Request to register as a client in the Auth Server by sending an email to: api@virtuagym.com

After successful registration, you will get a `client_id` and `client_secret`.  
These credentials are required to communicate with the Auth Server and retrieve an access token.

---

## 2. Get an Access Token

```bash
curl --request POST \
  --url https://iam.services.virtuagym.com/auth/realms/virtuagym/protocol/openid-connect/token \
  --header 'content-type: application/x-www-form-urlencoded' \
  --header 'x-represent-club-id: <club_id>' \
  --data client_id=<client_id> \
  --data grant_type=client_credentials \
  --data client_secret=<client_secret>

```

The response is a JSON object containing the access token.

Each club requires its own access token.

Tokens expire, so you must handle token renewal.

---

## 3. Get a Lead

```bash

curl --request GET \
  --url https://gateway.services.virtuagym.com/v3/clubs/<club_id>/leads \
  --header 'authorization: Bearer <access_token>' \
  --header 'content-type: application/json'

```

---


## 4. Create a Lead

```bash   

curl -X POST \
  --url https://gateway.services.virtuagym.com/v3/clubs/<club_id>/leads \
  --header "Content-Type: application/json" \
  --header "Authorization: Bearer <access_token>" \
  -d '{
    "firstname": "test lead",
    "lastname": "virtuagym",
    "email": "test@virtuagym.com",
    "status_id": "5",
    "source": "2",
    "note": "this is a test"
  }'

```

Parameters

|Parameter|Mandatory|Default|Description|Type|URL Max Length
|---|---|---|---|---|---|
|firstname|yes|-|First name of lead|String|50|
|lastname|yes|-|Last name of lead|String|50|
|email|no*|-|Email address|String|60|
|phone|no*|-|Phone number|String|42|
|mobile|no*|-|Mobile number|String|42|
|gender|no|-|'f' = Female, 'm' = Male|String|10|
|birthday|no|-|Format: YYYY-MM-DD|String|10|
|address|no|-|Street address|String|255|
|address_2|no|-|Additional address|String|255|
|zip_code|no|-|Postal code|String|255|
|city|no|-|City|String|255|
|state|no|-|State|String|255|
|country|no|-|Country|String|255|
|formatted_address|no|-|Full address string|String|255|
|language|no|-|Language code|String|2|
|status_id|yes|1 (New)|Lead status|Integer|-|
|source|no|-|Lead source ID|Integer|-|
|note|no|-|Notes|String|3500+|
|owner_id|no|-|Staff member ID who owns the lead|Integer|-|
|external_id|no|-|External system ID|String|200|
|lead_since|no|Current date|Format: YYYY-MM-DD|String|10|
|deleted|no|0|0 = not deleted, 1 = deleted|Integer|-|

*One of these fields is mandatory: email, phone, or mobile.

Status IDs

|Status ID|Status|
|---|---|
|1|New|
|2|Contacted|
|3|In Contact|
|4|Appointment made|
|5|Appointment held|
|6|Free Trial|
|7|Sign up scheduled|
|8|No show|
|9|Closed refused|
|10|Closed lost contact|
|11|Closed disqualified|
|12|Closed won|
|13|Closed - third party aggregators|

---

## 5. Response Codes

|HTTP Code|Message|Cause|
|---|---|---|
|200|Success|The lead was created successfully|
|400|[field] is required|Missing required field or parameter|
|401|Unauthorized|Invalid token|
|403|Forbidden|No access to this club|

Example Success (200)

```bash  

{
  "status": "success",
  "message": "",
  "data": {
    "status": "success",
    "message": "Lead created",
    "id": "1234"
  }
}

Example Error (400)

{
  "status": "fail",
  "error": {
    "status": "fail",
    "message": "At least one of fields `email`, `phone` and `mobile` are required."
  }
}

```

---

## 6. Update a Lead

```bash  

curl -X PUT \
  --url https://gateway.services.virtuagym.com/v3/clubs/<club_id>/leads/<lead_id> \
  --header "Content-Type: application/json" \
  --header "Authorization: Bearer <access_token>" \
  -d '{PARAMETER_NAME: PARAMETER_VALUE,...}'

```

Parameters are the same as Create a Lead.

Example Response (200)

```bash  

{
  "status": "success",
  "message": "",
  "data": {
    "status": "success",
    "message": "Lead updated",
    "id": 2274
  }
}

```
