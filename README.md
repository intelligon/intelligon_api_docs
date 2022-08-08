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
			"country_code": <number>,
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

## Ponds GeoJson

URL: `/aqua/ponds/_geojson?farm_id={farm_id}`

Method: `GET`

Auth required: `YES`

Response:

Code: `200 OK`

```
{
	"geojson": <geojson content>
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
			}, 
			...
		]
	}
}
```
