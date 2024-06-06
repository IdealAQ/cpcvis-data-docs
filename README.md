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

## Endpoint
The measurement data is requested by calling the following endpoint: [TODO:](#)

The request must contain a valid bearer token. The token is obtained by calling `/token` endpoint. See [authentication](#authentication) section for more details.



| key                 | description                | unit                                                |
|---------------------|----------------------------|-----------------------------------------------------|
| sample_date_time    | date time of the sample    | date time in ISO 8601 format (YYYY-MM-DDThh:mm:ssZ) |
| humidity            | humidity                   | percent \[%\]                                       |
| temperature         | temperature                | celsius \[°C\]                                      |
| co                  | carbon monoxide            |                                                     |
| co2                 | carbon dioxide             | parts per billion \[ppb\]                           |
| h2s                 | hydrogen sulfide           |                                                     |
| nh2                 | amino radical              |                                                     |
| no                  | nitric oxide               |                                                     |
| no2                 | nitrogen dioxide           |                                                     |
| ox                  | oxalate                    |                                                     |
| particles_raw       |                            |                                                     |
| particles_corrected |                            |                                                     |
| rh                  | rhodium                    |                                                     |
| so2                 | sulfur dioxide             |                                                     |
| voc                 | volatile organic compounds |                                                     |
|                     |                            |                                                     |

example:

```json
{
  "sample_date_time": "2024-05-01T00:00:00Z",
  "co": 0.0,
  "co2": 0.0,
  "humidity": 0.0
}
```