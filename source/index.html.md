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

## Authentication

> You can use the `Authorization` header to authenticate your requests.

```HTTP
GET https://zikiapp.github.io/api-docs/v1/users/me
Authorization: Bearer <token>
```

Based on REST principles, Ziki API Endpoints support authentication with secure API tokens using the standard HTTP Basic Authorization Header. This header must be attached to every request made to the Ziki system.

## Response Structure

```js
{
  "statusCode": Number,
  "message": String,
  "data": {}
}
```

All response data is returned as a JSON object with the following overall structure:

<table>
  <tr>
    <th>Name</th>
    <th>Description</th>
  </tr>
  <tr>
  <tr>
    <td>statusCode</td>
    <td>The HTTP status code of the response.</td>
  </tr>
  <tr>
    <td>message</td>
    <td>A message describing the response.</td>
  </tr>
  <tr>
    <td>data</td>
    <td>The data returned by the response.</td>
  </tr>
</table>

## Pagination

All top-level API resources have support for bulk fetches via “list” API methods. These requests will be paginated to 20 items by default.

You can specify further pages using the page parameter and specify page size. Other parameters include the sort, which will expect a string based attribute name, followed by asc or desc.

### Query parameters

| Name | Description                                                                                    |
| ---- | ---------------------------------------------------------------------------------------------- |
| page | Specifies the page number to retrieve. Value must be between 0 and 100.                        |
| size | Indicates how many records each page should contain. Value must be greater than or equal to 1. |

# User Authentication

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

```js
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

```js
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

# Kittens

## Get All Kittens

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get()
```

```shell
curl "http://example.com/api/kittens" \
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require("kittn");

let api = kittn.authorize("meowmeowmeow");
let kittens = api.kittens.get();
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/api/kittens`

### Query Parameters

| Parameter    | Default | Description                                                                      |
| ------------ | ------- | -------------------------------------------------------------------------------- |
| include_cats | false   | If set to true, the result will also include cats.                               |
| available    | true    | If set to false, the result will include kittens that have already been adopted. |

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```shell
curl "http://example.com/api/kittens/2" \
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require("kittn");

let api = kittn.authorize("meowmeowmeow");
let max = api.kittens.get(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Max",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific kitten.

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

| Parameter | Description                      |
| --------- | -------------------------------- |
| ID        | The ID of the kitten to retrieve |

## Delete a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.delete(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.delete(2)
```

```shell
curl "http://example.com/api/kittens/2" \
  -X DELETE \
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require("kittn");

let api = kittn.authorize("meowmeowmeow");
let max = api.kittens.delete(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "deleted": ":("
}
```

This endpoint deletes a specific kitten.

### HTTP Request

`DELETE http://example.com/kittens/<ID>`

### URL Parameters

| Parameter | Description                    |
| --------- | ------------------------------ |
| ID        | The ID of the kitten to delete |
