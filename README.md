# Measurement data API
## Authentication
A Bearer token is needed for sending API calls.

The token is obtained by calling `POST` request to the `/token` endpoint.
The request requires the following form data:
- grant_type: password
- username: *_\<username\>_*
- password: *_\<password\>_*

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

| URL parameter | required | description                                   | example                 |
|---------------|----------|-----------------------------------------------|-------------------------|
| time_from     | yes      | date time to query data from                  | 2024-05-01 00:00:00     |
| time_to       | yes      | date time to query data to                    | 2024-05-01 00:00:03     |
| device        | yes      | code of device to query data from             | cpc1                    |
| parameters    | no       | parameters to query in comma-separated format | humidity,temperature,co |


## Measurement parameters

| parameter key | description                | unit                                |
|---------------|----------------------------|-------------------------------------|
| time          | date time of the sample    | UTC date time (YYYY-MM-DD hh:mm:ss) |
| location_lon  | location longitude         | degrees \[°]                        |
| location_lat  | location latitude          | degrees \[°]                        |
| location_alt  | location altitude          | degrees \[°]                        |
| humidity      | relative humidity          | percent \[%\]                       |
| temperature   | temperature                | celsius \[°C\]                      |
| pressure      | barometric pressure        | ?                                   |
| co            | carbon monoxide            | ?                                   |
| co2           | carbon dioxide             | parts per billion \[ppb\]           |
| h2s           | hydrogen sulfide           |                                     |
| nh2           | amino radical              |                                     |
| no            | nitric oxide               |                                     |
| no2           | nitrogen dioxide           |                                     |
| ox            | oxalate                    |                                     |
| pn            | particle number            | particles per cubic cm \[#/cm³]     |
| rh            | rhodium                    |                                     |
| so2           | sulfur dioxide             |                                     |
| voc           | volatile organic compounds |                                     |
|               |                            |                                     |
| bc            | black carbon               |                                     |
| pm1           | pm1                        | micrograms per cubic m \[μg/m³]     |
| pm2_5         | pm2.5                      | micrograms per cubic m \[μg/m³]     |
| pm10          | pm10                       | micrograms per cubic m \[μg/m³]     |
| noise         |                            |                                     |
| light         |                            |                                     |
|               |                            |                                     |


## Example

request:
```
curl -X 'GET' \
  'http://domain.com/measurements?time_from=2024-05-01%2000%3A00%3A00&time_to=2024-05-01%2000%3A00%3A03&device_id=0&parameters=humidity%2Ctemperature%2Cco' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer abc123'
```

response:
```json
[
  {
    "time": "2024-05-01 00:00:00",
    "humidity": 50.0,
    "temperature": 27.5,
    "co": 0
  },
  {
    "time": "2024-05-01 00:00:01",
    "humidity": 50.0,
    "temperature": 27.5,
    "co": 0
  },
  {
    "time": "2024-05-01 00:00:02",
    "humidity": 50.0,
    "temperature": 27.5,
    "co": 0
  },
  {
    "time": "2024-05-01 00:00:03",
    "humidity": 50.0,
    "temperature": 27.5,
    "co": 0
  }
]
```

## Limitations
- The requested data to be returned cannot exceed 5MB in size before filters are applied (queried parameters). If the data size is exceeded, the response contains only the amount of data items that fit in the limitation and subsequent items are not returned.
- One way to overcome the limitation mentioned above is the use of subsequent requests.
- Subsequent requests might trigger an error in the service of our third-party storage service provider. For this reason, between subsequent requests there must be a sufficient waiting time.

## Recommendations
- query no more than 15 minutes of time-series data for one device.
- avoid subsequent requests if possible. 
- in case of subsequent requests, use sufficient waiting time between the requests. The length of the waiting time can be determined experimentally.

> **Note:** The system is still in development mode and has limited resources. For that reason, call API with caution and keep in mind that there is only a limited number of requests the current setup can handle at a time! This might be also the reason for unexpected errors. The system might be jammed either by you or someone else.

In case of persisting issues, contact [dominik.rohal@helsinki.fi](mailto:dominik.rohal@helsinki.fi?subject=[CPC%20vis%20API]%20Issue)
