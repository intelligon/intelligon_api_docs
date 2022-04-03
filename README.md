# Intelligon API Documentation

Last reviewed by: Claude Chen <claude@brickx.com>

# Endpoint

https endpoint: `https://api.intelligon.com`

# Authentication

## Login

Used to collect a Token for a registered User.

URL: `/login/`

Method: `POST`

Auth required: `NO`

Payload:

```
{
	"username": <str>,
	"password": <str>
}
```

Response:

Code: `200 OK`

```
{
	"response": "login_successful"
	"access_token": <str>,
	"user_profile": {
		"email": <str>,
		"firstname": <str>,
		"lastname": <str>,
		"role": <str>,
		"user_id: <str>
	}
}
```

Error Response
Code: `401 UNAUTHORIZED`

## Authentication method

Add Bearer token in `Authorization` header

Example:

`Authorization: "Bearer ...LCJhbGciOiJIUzUxMiJ9.eyJpc3M..."`

# User

## User's Profile

A method to acquire user's profile post authentication

URL: `/user/`

Method: `GET`

Auth required: `YES`

Response:

Code: `200 OK`

```
{
	"payload": {
		"company": <str>,
		"country": <str>,
		"firstname": <str>,
		"lastname": <str>,
		"role": "<str>",
		"state": <str>,
		"street": <str>,
		"title": <str>,
		"user_id": <str>
	}
}
```

# Aquaculture Pond Management

## List Ponds

URL: `/aqua/ponds/`

Method: `GET`

Auth required: `YES`

Response:

Code: `200 OK`

```
{
	"list": {
		"id": <str>
		"area": <number>,              # Area
		"boundary_geojson": <geojson>, # Boundary Geojson
		"category": <str>,
		"sub_category": <str>,
		"lat": "<str>",                # Shape centroid or Point Latitude
		"lon": <str>,                  # Shape centroid or Point Longitude
		"label": <str>,
		"is_active": <str>,
		"group": <str>,
		"owner_id": <str>
	}
}
```

## Data Overview

List available fields with overview information

URL: `/aqua/data/_overview`

Example: `/aqua/data/_overview?pond_id=b179b842-a95b-4166-b24e-214bbfff1c4e`

Query string parameters:

- `pond_id` (required): Pond ID

Auth required: `YES`

Response:

Code: `200 OK`

```
{
	"payload": {
		"data": [
			{
				"description": <str>,
				"field_group": <str>,
				"field_id": <str>,
				"field_name": <str>,
				"field_unit": <str>,
				"last_date_collected": <str>,
				"rows": <number>
			}, ...
		]
	}
}
```

## Timeseries Data

Get timeseries data

URL: `/aqua/data/[pond_id|<str>]?fields=<str>`

Example: `/aqua/data/b179b842-a95b-4166-b24e-214bbfff1c4e?fields=weight,ph`

Auth required: `YES`

Response:

Code: `200 OK`

```
{
	"payload": {
		"meta": {
			"fields": [
				"[field1 | <str>]",
				"[field2 | <str>]",
				"[field3 | <str>]",
				...
			],
			"owner_id": <str>,
			"pond_id": <str>,
			"row_count": <number>
		},
		"rows": [
			{
				"datetime": <str>
				"[field1 | <str>]": number,
				"[field2 | <str>]": number,
				"[field3 | <str>]": number,
				...
			}
		]
	}
}
```
