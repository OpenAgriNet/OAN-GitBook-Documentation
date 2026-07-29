# Market Integrations

## Overview

BharatVistaar integrates with several third-party and government backend systems to source weather, market price, and scheme-related data. This document lists those market integrations, grouped by source system, with endpoint structure and parameters.

Authentication values (tokens, keys, passwords) shown in the original API collection have been replaced with placeholders in this document. Refer to the team's secrets store or the original Postman environment for live values.

## Contents

1. Mandi Price (Agmarknet)
2. Weather (Mausamgram / IMD)
3. Weather (IMD City Weather)
4. PMFBY (Crop Insurance)
5. Soil Health Card (SHC) and GFR
6. PM-KISAN Grievance Notes for Maintainers

***

## 1. Mandi Price (Agmarknet)

Source for agricultural market (mandi) commodity price data, queried by state, district, market, and commodity.

**Base URL:** `http://<agmarknet-service-host>:<port>/v1`

### 1.1 Generate Token

`POST /generate-dynamic-token-agmarknet`\
Issues an access token required by the other Agmarknet endpoints below.

**Body**

```json
{
  "access_name": "<access_name>",
  "password": "<password>"
}
```

### 1.2 Fetch Mandi Prices

`GET /fetch-agmarknet-vistaar`

**Query Parameters**

| Parameter     | Description             |
| ------------- | ----------------------- |
| token         | Access token from 1.1   |
| statecode     | State code              |
| districtcode  | District code           |
| marketcode    | Market code             |
| commoditycode | Commodity code          |
| from\_date    | Start date (DD-MM-YYYY) |
| to\_date      | End date (DD-MM-YYYY)   |

### 1.3 Fetch Market–Commodity Mapping

`GET /fetch-agmarknet-market-commodity-mapping`

**Query Parameters** `token`, `statecode`, `option`, `from_date`, `to_date`

Returns the mapping between markets and commodities available for a given state, used to populate valid `marketcode`/`commoditycode` values for the price endpoint above.

### 1.4 Fetch Master Data

`GET /fetch-agmarknet-master-data`

**Query Parameters** `token`, `option`

Returns reference/master data (the exact dataset depends on the `option` value; not specified in the source collection).

***

## 2. Weather (Mausamgram / IMD)

Source for daily and hourly weather forecast data from the India Meteorological Department's Mausamgram service.

**Base URL:** `https://mausamgram.imd.gov.in`

### 2.1 Daily Forecast — by Grid (Lat/Lon)

`GET /nwpapi/get-daily`

**Query Parameters** `lat`, `lon`

**Headers** `Authorization: Basic <credentials>`

### 2.2 Daily Forecast — by GP Code

`GET /nwpapi/get-daily`

**Query Parameters** `gpcode` (Gram Panchayat code)

**Headers** `Authorization: Basic <credentials>`

### 2.3 Hourly Forecast — by Grid (Lat/Lon)

`POST /nwp/api/GetHourlyData`

**Headers** `user: <user>`\
`key: <api_key>`

**Body**

```json
{
  "lat": 28.5355,
  "lon": 77.391,
  "hourly": 1
}
```

### 2.4 Hourly Forecast — by GP Code

`POST /nwp/api/GetHourlyData`

**Headers** `user: <user>`\
`key: <api_key>`

**Body**

```json
{
  "hourly": 1,
  "gpcode": 262423
}
```

***

## 3. Weather (IMD City Weather)

A separate IMD service providing current city-level weather, distinct from Mausamgram.

**Base URL:** `https://city.imd.gov.in`

### 3.1 City Weather

`GET /api/cityweather_loc.php`

**Query Parameters**

| Parameter | Description      |
| --------- | ---------------- |
| id        | City location ID |

***

## 4. PMFBY (Crop Insurance)

Backend integration (via Amnex) for PMFBY (Pradhan Mantri Fasal Bima Yojana) farmer lookup and authentication.

**Base URL:** `https://pmfbydemo.amnex.co.in`

### 4.1 Check Farmer Registration

`GET /api/v1/services/services/farmerMobileExists`

**Query Parameters**

| Parameter | Description                       |
| --------- | --------------------------------- |
| mobile    | Farmer's registered mobile number |
| authToken | Service auth token                |

### 4.2 Login

`POST /api/v2/external/service/login`

**Body**

```json
{
  "deviceType": "web",
  "mobile": "<mobile>",
  "otp": "<otp>",
  "password": "<password_hash>"
}
```

***

## 5. Soil Health Card (SHC) and GFR

Backend integration with the Soil Health Card / GFR (GraphQL-based) system, used for soil test data, crop registries, and fertilizer recommendations.

**Base URL:** `https://soilhealth4.dac.gov.in`

All requests below are `POST` to the same base URL, using a GraphQL query/variables body. Authenticated calls require a Bearer token obtained via 5.1/5.2.

### 5.1 Generate Access Token

**Body**

```json
{
  "query": "query Query($refreshToken: String!) { generateAccessToken(refreshToken: $refreshToken) }",
  "variables": {
    "refreshToken": "<refresh_token>"
  }
}
```

### 5.2 Validate / Refresh Token

**Headers** `Authorization: Bearer <access_token>`

Confirms the current access token is valid (request body is the same GraphQL query used for soil test retrieval, below — appears to double as a validity check).

### 5.3 Get Soil Test Results

**Headers** `Authorization: Bearer <access_token>`

**Body**

```json
{
  "query": "query GetTestForAuthUser($computedId: String, $phone: PhoneNumber, $state: String, $district: String, $name: String, $farmer: String, $from: Datetime, $to: Datetime, $cycle: String, $locale: String, $scheme: String, $limit: Int, $skip: Int) { getTestForAuthUser(computedID: $computedId, phone: $phone, state: $state, district: $district, name: $name, farmer: $farmer, from: $from, to: $to, cycle: $cycle, scheme: $scheme, limit: $limit, skip: $skip) { id computedID cycle scheme plot { address area surveyNo } farmer { address name phone } crop location testparameters rdfValues status testCompletedAt sampleDate reportData district block village results fertilizer html(locale: $locale) uniqueID } }",
  "variables": {
    "cycle": "2025-26",
    "phone": "<phone_number>",
    "limit": 10,
    "skip": 0,
    "locale": "en"
  }
}
```

Returns soil test data for a farmer matched by phone number, including crop, plot, fertilizer recommendation, and report fields.

### 5.4 Get State List (GFR)

**Body:** GraphQL query (query text not captured in source collection — request body was empty in the reference data).

### 5.5 Get District/Subdistrict by State (GFR)

**Body**

```json
{
  "query": "query Query($state: ID, $subdistrict: Boolean) { getdistrictAndSubdistrictBystate(state: $state, subdistrict: $subdistrict) }",
  "variables": {
    "state": "<state_id>",
    "subdistrict": false
  }
}
```

### 5.6 Get Crop Registries (GFR)

**Body**

```json
{
  "query": "query GetCropRegistries($state: String) { getCropRegistries(state: $state) { name variety irrigationType season splitdose state GFRavailable id combinedName __typename } }",
  "variables": {
    "state": "<state_id>"
  }
}
```

Returns crop variety, irrigation type, season, and split-dose fertilizer schedule data for a given state.

### 5.7 Get Fertilizer Recommendations (GFR)

**Body:** GraphQL query (query text not captured in source collection — request body was empty in the reference data).

***

## 6. PM-KISAN Grievance

Backend integration (via Amnex) for filing and checking PM-KISAN grievances.

**Base URL:** `https://pmkisanstaging.amnex.co.in/exlinkstaging/services/GrievanceServiceTest.asmx`

All three endpoints below use the same request shape: a single encrypted payload field.

**Body (all endpoints)**

```json
{
  "EncryptedRequest": "<encrypted_payload>"
}
```

| Endpoint                 | Purpose                                                    |
| ------------------------ | ---------------------------------------------------------- |
| `/GrievanceAadhaarToken` | Obtain a token using the farmer's Aadhaar number           |
| `/GrievanceStatusCheck`  | Check the status of an existing grievance                  |
| `/LodgeGrievance`        | File a new grievance using registration and Aadhaar number |

***

## Notes for Maintainers

* This document reflects what is present in the current Postman reference collection; several requests (Mausamgram's "test api", the two GFR GraphQL endpoints with empty bodies) are incomplete in the source and noted above where applicable.
* Real credentials, tokens, and keys are intentionally omitted. Refer to the team's secrets store or the original Postman environment for live values.
* The dev/prod distinction is explicit only for the Mandi Price endpoint; other services may also have separate environments not reflected in this collection.
