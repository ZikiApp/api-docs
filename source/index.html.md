---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - HTTP
  - JSON

# toc_footers:
#   - <a href='#'>Sign Up for a Developer Key</a>
#   - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: false

code_clipboard: true

meta:
  - name: description
    content: Documentation for the Ziki API
---

# Introduction

## Welcome to the Ziki API

```
           __          __  _
           \ \        / / | |
            \ \  /\  / /__| | ___ ___  _ __ ___   ___
             \ \/  \/ / _ \ |/ __/ _ \| '_ ` _ \ / _ \
              \  /\  /  __/ | (_| (_) | | | | | |  __/
               \/  \/ \___|_|\___\___/|_| |_| |_|\___|

```

The Ziki API is a document based JSON REST API. You can use our API to access Ziki API endpoints, which can get information collected from Ziki app.

We have language bindings in cURL, and HTTP! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

## Base URL

The base URL of Ziki API is: `https://example.com`.
The API provides a set of endpoints, each with its own unique path.

## Response Structure

```json
{
  "statusCode": 200,
  "message": "OK",
  "data": {}
}
```

All response data is returned as a JSON object with the following overall structure:

<table>
  <tr>
    <th>Name</th>
    <th>Type</th>
    <th>Description</th>
  </tr>
  <tr>
  <tr>
    <td>statusCode</td>
    <td>Number</td>
    <td>The HTTP status code of the response.</td>
  </tr>
  <tr>
    <td>message</td>
    <td>String</td>
    <td>A message describing the response.</td>
  </tr>
  <tr>
    <td>data</td>
    <td>Object</td>
    <td>The data returned by the response.</td>
  </tr>
</table>

## Pagination

All top-level API resources have support for bulk fetches via “list” API methods. These requests will be paginated to 20 items by default.

You can specify further pages using the page parameter and specify page size. Other parameters include the sort, which will expect a string based attribute name, followed by asc or desc.

```http
GET https://example.com/api/v1/songs?page=1&limit=10 HTTP/1.1
Content-Type: application/json
Accept: application/json
Authorization: Bearer <token>
```

### Query parameters

| Name  | Description                                                                                    |
| ----- | ---------------------------------------------------------------------------------------------- |
| page  | Specifies the page number to retrieve. Value must be between 0 and 100.                        |
| limit | Indicates how many records each page should contain. Value must be greater than or equal to 1. |

# Authentication

## Sign Up

> Example Request

```http
POST /api/v1/auth/signup HTTP/1.1
Content-Type: application/json
Accept: application/json
{
    "fistName": "Jean",
    "lastName": "Dupont",
    "username": "jean_d01",
    "gender": "Male",
    "email": "jeandupon@gmail.com",
    "phone": 650504571,
    "password": "@Password#123",
    "profilPicUrl": "https://www.google.com/img/logo.png",
    "intro": "I am a web developer"
}
```

> Example Response

```json
{
  "statusCode": 201,
  "message": "Signup Success.",
  "data": {
    "id": 1,
    "fistName": "Jean",
    "lastName": "Dupont",
    "username": "jean_d01",
    "email": "jeandupon@gmail.com",
    "intro": "I am a web developer",
    "profilePicUrl": "https://www.google.com/img/logo.png",
    "registerAt": "2022-01-18T18:00:21.582Z"
  }
}
```

Sign up for a new user account.

### HTTP Request

`POST /api/v1/auth/signup`

### Request Body

| Name         | Type   | Required | Description                                                              |
| ------------ | ------ | -------- | ------------------------------------------------------------------------ |
| fistName     | string | true     | The first name of the user                                               |
| lastName     | string | true     | The last name of the user                                                |
| username     | string | true     | The username of the user                                                 |
| gender       | string | true     | Gender as provided by the user. Can be "Male", "Female" or "Transgender" |
| email        | string | true     | The user email                                                           |
| phone        | number | true     | Phone provided by the user                                               |
| password     | string | true     | The user password                                                        |
| profilPicUrl | string | false    | Profile photo transformed to 128x128 px                                  |
| intro        | string | true     | A short introduction of the user                                         |

## Sign In

> Example Request

```http
POST /api/v1/auth/signin HTTP/1.1
Content-Type: application/json
Accept: application/json
{
    "email": "jeandupon@gmail.com",
    "password": "@Password#123"
}
```

> Example Response

```json
{
  "statusCode": 200,
  "message": "User logged in successfully.",
  "data": {
    "user": {
      "id": 1,
      "fistName": "Jean",
      "lastName": "Dupont",
      "username": "jean_d01",
      "email": "jeandupon@gmail.com",
      "intro": "I am a web developer",
      "profilePicUrl": "https://www.google.com/img/logo.png",
      "registerAt": "2022-01-18T18:49:14.793Z"
    },
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOjEsImVtYWlsIjoiamVhbmR1cG9uQGdtYWlsLmNvbSIsInVzZXJuYW1lIjoiamVhbl9kMDEiLCJpYXQiOjE2NDI1MzE4NjcsImV4cCI6MTY0MjYxODI2N30.Ngj04it4Sbn0Ap3q0qfNpjSnnwAscQwFc6AjN6WkIv4"
  }
}
```

The sign in endpoint is used to authenticate a user. It expects a email and password in the request body. The response will contain a token that can be used to authenticate future requests.

### HTTP Request

`POST /api/v1/auth/signin`

### Request Body

| Name     | Type   | Required | Description       |
| -------- | ------ | -------- | ----------------- |
| email    | string | true     | The user email    |
| password | string | true     | The user password |

# Profile

## Get User Profile

> Example Request

```http
GET /api/v1/profile HTTP/1.1
Content-Type: application/json
Accept: application/json
Authorization: Bearer <token>
```

> Example Response

```json
{
  "statusCode": 200,
  "message": "Get Profile Successful.",
  "data": {
    "fistName": "Jean",
    "lastName": "Freddy",
    "username": "Rogelio19",
    "phone": 650504571,
    "email": "Tomas25@hotmail.com",
    "profilePicUrl": "http://placeimg.com/640/480"
  }
}
```

The profile endpoint is used to retrieve the user profile.

### HTTP Request

`GET /api/v1/profile`

### Request Header

`Authorization: Bearer <token>`

## Update User Profile

> Example Request

```http
PUT /api/v1/profile HTTP/1.1
Content-Type: application/json
Accept: application/json
Authorization: Bearer <token>
{
    "fistName": "Jean",
    "lastName": "Freddy",
    "phone": 650504571,
    "intro": "I am a mobile developer"
}
```

> Example Response

```json
{
  "statusCode": 200,
  "message": "Update Profile Successful.",
  "data": {
    "id": 11,
    "fistName": "Jean",
    "lastName": "Freddy",
    "username": "Rogelio19",
    "email": "Tomas25@hotmail.com",
    "intro": "I am a mobile developer",
    "profilePicUrl": "http://placeimg.com/640/480",
    "registerAt": "2022-01-26T10:51:34.900Z"
  }
}
```

The profile endpoint is used to update the user profile.

### HTTP Request

`PUT /api/v1/profile`

### Request Header

| Name          | Type   | Required | Description    |
| ------------- | ------ | -------- | -------------- |
| Authorization | string | true     | The user token |

### Request Body

| Name     | Type   | Required | Description                      |
| -------- | ------ | -------- | -------------------------------- |
| fistName | string | true     | The user first name              |
| lastName | string | true     | The user last name               |
| phone    | number | true     | The user phone                   |
| intro    | string | false    | A short introduction of the user |

## Get profile by Id

> Example Request

```http
GET /api/v1/profile/public/<ID> HTTP/1.1
Content-Type: application/json
Accept: application/json
Authorization: Bearer <token>
```

> Example Response

```json
{
  "statusCode": 200,
  "message": "Get Public Profile Successful.",
  "data": {
    "username": "Quincy.Strosin",
    "intro": "Reprehenderit soluta sit nostrum dolores minus aspernatur.",
    "profilePicUrl": "http://placeimg.com/640/480",
    "registerAt": "2022-01-26T10:29:26.420Z"
  }
}
```

The profile endpoint is used to retrieve the user profile by id.

### HTTP Request

`GET /api/v1/profile/public/<ID>`

### Request Header

| Name          | Type   | Required | Description    |
| ------------- | ------ | -------- | -------------- |
| Authorization | string | true     | The user token |

### Query Parameters

| Name | Type | Required | Description |
| ---- | ---- | -------- | ----------- |
| ID   | int  | true     | The user id |

# Songs

<aside>
p.s. The songs endpoint is not implemented yet.
</aside>

## Get All Songs

## Get Song by Id

## Get Songs by User Id

## Get Songs by Genre

## Create Song

## Update Song

## Delete Song
