# Tapsi Pack API: OAuth Guide

Welcome to the OAuth guide for the Tapsi Pack API. This guide will help you understand how to authenticate with the
Tapsi Pack API using OAuth.

## Introduction

In this document, external teams integrating with our API are referred to as **clients**. Each client is assigned a
unique **client_id** and a corresponding **client_secret**.

To access our external APIs, a token must be included in the header of the client's request. This token ensures secure
access to the APIs and prevents unauthorized access to sensitive data.

The token should be set in the header with the key name **"authorization"**.

There are two types of tokens:

- Client Secret Token
- User Access Token

To obtain your tokens, please contact us at [Tapsi Pack](https://pack.tapsi.ir/landing).

![APIs flow](../images/pack-external-apis-flow.png)

## Token Management for Clients

### 1. Generate Access Token and Refresh Token using Client ID and PAT

Clients can generate both an **Access Token** and a **Refresh Token** using their **Client ID** in combination with the
**user's PAT**. The Access Token that is generated inherits the permissions associated with the corresponding PAT.

The body of the message should be sent as **x-www-form-urlencoded**, and the provided **client secret** should be set as
the **Bearer** authorization header.

URL: `https://api.tapsi.cab/api/v1/delivery/external/oauth2/token`

Method: `POST`

Request (as **Form URL Encoded**):

| Field      | Type   | Description        |
|------------|--------|--------------------|
| grant_type | String | authorization_code |
| code       | String | User PAT           |

Response:

```json5
{
  "access_token": "String",
  "refresh_token": "String",
  "scope": "String",
  "token_type": "Bearer",
  "expires_in": "Int"
  // seconds to expiration
}
```

Example Curl:

```
curl --location 'https://api.tapsi.cab/api/v1/delivery/external/oauth2/token' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--header 'Authorization: Bearer Client Secret' \
--data-urlencode 'grant_type=authorization_code' \
--data-urlencode 'code=User PAT'
```
### Usage in Other Languages

<details>
  <summary>PHP</summary>

```
<?php

$curl = curl_init();

curl_setopt_array($curl, array(
  CURLOPT_URL => 'https://api.tapsi.cab/api/v1/delivery/external/oauth2/token',
  CURLOPT_RETURNTRANSFER => true,
  CURLOPT_ENCODING => '',
  CURLOPT_MAXREDIRS => 10,
  CURLOPT_TIMEOUT => 0,
  CURLOPT_FOLLOWLOCATION => true,
  CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
  CURLOPT_CUSTOMREQUEST => 'POST',
  CURLOPT_POSTFIELDS => 'grant_type=authorization_code&code=User PAT',
  CURLOPT_HTTPHEADER => array(
    'Content-Type: application/x-www-form-urlencoded',
    'Accept: application/json',
    'Authorization: Bearer Client Secret'
  ),
));

$response = curl_exec($curl);

curl_close($curl);
echo $response;

```
</details>

<details>
  <summary>Go</summary>

```
package main

import (
  "fmt"
  "strings"
  "net/http"
  "io/ioutil"
)

func main() {

  url := "https://api.tapsi.cab/api/v1/delivery/external/oauth2/token"
  method := "POST"

  payload := strings.NewReader("grant_type=authorization_code&code=User PAT")

  client := &http.Client {
  }
  req, err := http.NewRequest(method, url, payload)

  if err != nil {
    fmt.Println(err)
    return
  }
  req.Header.Add("Content-Type", "application/x-www-form-urlencoded")
  req.Header.Add("Accept", "application/json")
  req.Header.Add("Authorization", "Bearer Client Secret")

  res, err := client.Do(req)
  if err != nil {
    fmt.Println(err)
    return
  }
  defer res.Body.Close()

  body, err := ioutil.ReadAll(res.Body)
  if err != nil {
    fmt.Println(err)
    return
  }
  fmt.Println(string(body))
}
```
</details>

<details>
  <summary>Python</summary>

```
import requests

url = "https://api.tapsi.cab/api/v1/delivery/external/oauth2/token"

payload='grant_type=authorization_code&code=User PAT'
headers = {
  'Content-Type': 'application/x-www-form-urlencoded',
  'Accept': 'application/json',
  'Authorization': 'Bearer Client Secret'
}

response = requests.request("POST", url, headers=headers, data=payload)

print(response.text)

```
</details>

<details>
  <summary>NodeJS</summary>

```
var axios = require('axios');
var qs = require('qs');
var data = qs.stringify({
  'grant_type': 'authorization_code',
  'code': 'User PAT' 
});
var config = {
  method: 'post',
  url: 'https://api.tapsi.cab/api/v1/delivery/external/oauth2/token',
  headers: { 
    'Content-Type': 'application/x-www-form-urlencoded', 
    'Accept': 'application/json', 
    'Authorization': 'Bearer Client Secret'
  },
  data : data
};

axios(config)
.then(function (response) {
  console.log(JSON.stringify(response.data));
})
.catch(function (error) {
  console.log(error);
});

```
</details>

### 2. Access Token Expiry

Clients can use the access token until it expires. If a client attempts to use an expired access token, the following
response will be generated.

```text
HTTP/1.1 401 Unauthorized
Content-Type: text

Jwt is expired
```

### 3. Generate a new Access Token and Refresh token using the Refresh token

Clients can use the Refresh Token to generate a new Access Token and Refresh Token without needing to involve the user's
PAT or Client ID again. This is useful for maintaining continuous access to the API without frequent user interactions.

To generate a new set of tokens using a Refresh Token, clients should make a POST request to the token endpoint, and the
body of the message should be sent as **x-www-form-urlencoded**, and the client secret as the **Bearer** authorization
header.

URL: `https://api.tapsi.cab/api/v1/delivery/external/oauth2/token`

Method: `POST`

Request (as **Form URL Encoded**):

| Field         | Type   | Description        |
|---------------|--------|--------------------|
| grant_type    | String | refresh_token      |
| refresh_token | String | User Refresh Token |

Response:

```json5
{
  "access_token": "String",
  "refresh_token": "String",
  "scope": "String",
  "token_type": "Bearer",
  "expires_in": "Int"
  // seconds to expiration
}
```

Example Curl:
```
curl --location 'https://api.tapsi.cab/api/v1/delivery/external/oauth2/token' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--header 'Authorization: Bearer Client Secret' \
--data-urlencode 'grant_type=refresh_token' \
--data-urlencode 'refresh_token=User Refresh Token'
```
### Usage in Other Languages

<details>
  <summary>PHP</summary>

```
<?php
$curl = curl_init();

curl_setopt_array($curl, array(
  CURLOPT_URL => 'https://api.tapsi.cab/api/v1/delivery/external/oauth2/token',
  CURLOPT_RETURNTRANSFER => true,
  CURLOPT_ENCODING => '',
  CURLOPT_MAXREDIRS => 10,
  CURLOPT_TIMEOUT => 0,
  CURLOPT_FOLLOWLOCATION => true,
  CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
  CURLOPT_CUSTOMREQUEST => 'POST',
  CURLOPT_POSTFIELDS => 'grant_type=refresh_token&refresh_token=User Refresh Token',
  CURLOPT_HTTPHEADER => array(
    'Content-Type: application/x-www-form-urlencoded',
    'Accept: application/json',
    'Authorization: Bearer Client Secret'
  ),
));

$response = curl_exec($curl);

curl_close($curl);
echo $response;

```
</details>

<details>
  <summary>Go</summary>

```
package main

import (
  "fmt"
  "strings"
  "net/http"
  "io/ioutil"
)

func main() {

  url := "https://api.tapsi.cab/api/v1/delivery/external/oauth2/token"
  method := "POST"

  payload := strings.NewReader("grant_type=refresh_token&refresh_token=User Refresh Token")

  client := &http.Client {
  }
  req, err := http.NewRequest(method, url, payload)

  if err != nil {
    fmt.Println(err)
    return
  }
  req.Header.Add("Content-Type", "application/x-www-form-urlencoded")
  req.Header.Add("Accept", "application/json")
  req.Header.Add("Authorization", "Bearer Client Secret")

  res, err := client.Do(req)
  if err != nil {
    fmt.Println(err)
    return
  }
  defer res.Body.Close()

  body, err := ioutil.ReadAll(res.Body)
  if err != nil {
    fmt.Println(err)
    return
  }
  fmt.Println(string(body))
}

```
</details>

<details>
  <summary>Python</summary>

```
import requests

url = "https://api.tapsi.cab/api/v1/delivery/external/oauth2/token"

payload='grant_type=refresh_token&refresh_token=User Refresh Token'
headers = {
  'Content-Type': 'application/x-www-form-urlencoded',
  'Accept': 'application/json',
  'Authorization': 'Bearer Client Secret'
}

response = requests.request("POST", url, headers=headers, data=payload)

print(response.text)

```
</details>

<details>
  <summary>NodeJS</summary>

```
var axios = require('axios');
var qs = require('qs');
var data = qs.stringify({
  'grant_type': 'refresh_token',
  'refresh_token': 'User Refresh Token' 
});
var config = {
  method: 'post',
  url: 'https://api.tapsi.cab/api/v1/delivery/external/oauth2/token',
  headers: { 
    'Content-Type': 'application/x-www-form-urlencoded', 
    'Accept': 'application/json', 
    'Authorization': 'Bearer Client Secret'
  },
  data : data
};

axios(config)
.then(function (response) {
  console.log(JSON.stringify(response.data));
})
.catch(function (error) {
  console.log(error);
});
```
</details>

Currently, the access token is valid for a duration of 2 days. When it expires, users need to log in using their
Personal Access Token (PAT) again, following the same process as in Step 1.