# Measurement data API
## Authentication
An bearer token is needed for sending API calls.

The token is obtained by calling `POST` request to the `/token` endpoint.
The request requires the following form data:
- grant_type: password
- username: *_\<username\>_*
- username: *_\<password\>_*

example:
```
curl -X 'POST' \
  'http://127.0.0.1:8080/token' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  -d 'grant_type=password&username=aaa&password=bbb'
```
response:
```
{
    "access_token": "123abc",
    "token_type": "Bearer"
}
```

The access token is sent with API request header. 

Example:
```
curl -X 'GET' \
  'http://domain.com/users/me' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer abc123'
```

## Measurement data
The measurement data is requested by calling `GET` request to the `/measurements` endpoint. [TODO:](#)

> **Note:** The request must contain a valid bearer token. The token is obtained by calling `/token` endpoint. See [authentication](#authentication) section for more details.

The request requires the following URL parameters:

| URL parameter  | required | description                                   | example                 |
|----------------|----------|-----------------------------------------------|-------------------------|
| date_time_from | yes      | date time to query data from                  | 2024-05-01T00:00:00Z    |
| date_time_to   | yes      | date time to query data to                    | 2024-05-01T00:00:03Z    |
| device_id      | yes      | ID of device to query data from               | cpc1                    |
| parameters     | no       | parameters to query in comma-separated format | humidity,temperature,co |
| metadata ???   | no       | ?                                             | ?                       |


Measurement parameters:

| parameter key       | description                | unit                                                    |
|---------------------|----------------------------|---------------------------------------------------------|
| sample_date_time    | date time of the sample    | UTC date time in ISO 8601 format (YYYY-MM-DDThh:mm:ssZ) |
| humidity            | humidity                   | percent \[%\]                                           |
| temperature         | temperature                | celsius \[°C\]                                          |
| co                  | carbon monoxide            |                                                         |
| co2                 | carbon dioxide             | parts per billion \[ppb\]                               |
| h2s                 | hydrogen sulfide           |                                                         |
| nh2                 | amino radical              |                                                         |
| no                  | nitric oxide               |                                                         |
| no2                 | nitrogen dioxide           |                                                         |
| ox                  | oxalate                    |                                                         |
| particles_raw       |                            |                                                         |
| particles_corrected |                            |                                                         |
| rh                  | rhodium                    |                                                         |
| so2                 | sulfur dioxide             |                                                         |
| voc                 | volatile organic compounds |                                                         |
|                     |                            |                                                         |


Metadata:
> TODO:

| parameter key | description        |
|---------------|--------------------|
| location_lon  | location longitude |                                                         |
| location_lat  | location latitude  |                                                         |
| location_alt  | location altitude  |                                                         |


example:

```
curl -X 'GET' \
  'http://domain.com/measurements?date_time_from=2024-05-01T00%3A00%3A00Z&date_time_to=2024-05-01T00%3A00%3A03Z&device_id=0&parameters=humidity%2Ctemperature%2Cco' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer abc123'
```

response:
```json
[
  {
    "sample_date_time": "2024-05-01T00:00:00Z",
    "humidity": 50.0,
    "temperature": 27.5,
    "co": 0
  },
  {
    "sample_date_time": "2024-05-01T00:00:01Z",
    "humidity": 50.0,
    "temperature": 27.5,
    "co": 0
  },
  {
    "sample_date_time": "2024-05-01T00:00:02Z",
    "humidity": 50.0,
    "temperature": 27.5,
    "co": 0
  },
  {
    "sample_date_time": "2024-05-01T00:00:03Z",
    "humidity": 50.0,
    "temperature": 27.5,
    "co": 0
  }
]
```