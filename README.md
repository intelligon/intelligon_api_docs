# Intelligon API Documentation

Last reviewed by: Claude Chen <claude@intelligon.com> Aug 2022

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

Error Response Code: `401 UNAUTHORIZED`

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

## List Farms

URL: `/aqua/farms`

Method: `GET`

Auth required: `YES`

Response:

Code: `200 OK`

```
{
	"payload": {
		"rows": [{
			"farm_id": <str>,               # NOTE: this is required for requesting ponds
			"label": <str>,                 # farm label
			"lat": "<num>",                	# Shape centroid or Point Latitude
			"lon": <num>,                  	# Shape centroid or Point Longitude
			"owner_id": <str>,              # NOTE: this should be your user_id
			"country": <str>,
			"country_code": <num>,
			"place": <str>,
			"region": <str>,
			"total_area": <num>,
			"total_ponds": <num>,
			"created_at": <geojson>,
			"updated_at": <str>,
		},...]
	}

}
```

## List Ponds

URL: `/aqua/ponds?farm_id={farm_id}`

Method: `GET`

Auth required: `YES`

Response:

Code: `200 OK`

```
{
  "list": [{
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
  	"owner_id": <str>,
    "farm_id": <str>,
    "farm_label": <str>,
    "country": <str>,
    "country_code": <num>,
    "place": <str>,
    "region": <str>,
  }, ...]
}
```

## Ponds GeoJson

URL: `/aqua/ponds/_geojson?farm_id={farm_id}`

Parameters

-   farm_id: Farm ID

Method: `GET`

Auth required: `YES`

Response:

Code: `200 OK`

```
{
	"geojson": <geojson content>
}
```

## List data fields

Get list of available field ids in order to access timeseries data

URL: `/aqua/fields`

Parameters: N/A

Method: `GET`

Auth required: `YES`

Response:

Code: `200 OK`

```
{
	"payload: [
		{
			"description": <str>,
			"field_group": <str>,
			"field_id": <str>,
			"field_name": <str>,
			"field_sub_group": <str>,
			"field_type": <str>,
			"field_unit": <str>,
			"is_derived": <bool>,
			"position": <number>
		}, ...
	]
}
```

NOTE:

field_id: this is what we use in timeseries data API

## Timeseries Data

Get timeseries data

NOTE: Please refer to "Fields" section to get full list of available field ids

URL: `/aqua/data/[pond_id|<str>]?fields=<str>&from=<str>&to=<str>`

Parameters:

-   fields: list of field ids separated by comma signs
-   [OPTIONAL] from: starting date of timeseries
-   [OPTIONAL] to: ending date of timeseries

Method: `GET`

Example: `/aqua/data/b179b842-a95b-4166-b24e-214bbfff1c4e?fields=weight,ph&from=2017-01-01&to=2018-01-01`

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
				"[field1 | <str>]": <number>,
				"[field2 | <str>]": <number>,
				"[field3 | <str>]": <number>,
				...
			},
			...
		]
	}
}
```

## Cycle

Get cycles for a specific pond

URL: `/aqua/cycles?pondId=<str>`

Method: `GET`

Example: `/aqua/cycles?pondId=b179b842-a95b-4166-b24e-214bbfff1c4e`

Auth required: `YES`

Response:

Code: `200 OK`

```
{
	"payload": {
		"pond_id": <str>,
		"rows": [
			{
				"id": <str>,
				"pond_id": <str>,
				"label": <str>,
				"created": <str>,
				"updated_at": <str>,
				"start_date": <str>,
				"pond_id": <str>,
				"end_date": <str>,
			}
		]
	}
}
```
