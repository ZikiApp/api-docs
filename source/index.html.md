---
title: Ziki API Documentation

language_tabs: # must be one of https://git.io/vQNgJ
  - CURL

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

The base URL of Ziki API is: `https://example.com/api/v1`. You can use this URL to access the Ziki API endpoints.
The API provides a set of endpoints, each with its own unique path.

## i18n (Internationalization)

The Ziki API is internationalized. You can use the `x-custom-lang` header to specify the language you want to use. The list of supported languages is: [`en`, `fr`] (default: `en`) and default language is `en`.

## Response Structure

```json
{
  "statusCode": 200,
  "message": "Success message",
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

```curl
curl -X GET '<baseUrl>/songs?page=1&limit=10' -H 'Authorization: Bearer <token>'
```

### Query parameters

| Name  | Description                                                                                    |
| ----- | ---------------------------------------------------------------------------------------------- |
| page  | Specifies the page number to retrieve. Value must be between 0 and 100.                        |
| limit | Indicates how many records each page should contain. Value must be greater than or equal to 1. |

# Authentication

## Sign Up

> Example Request

```curl
curl -X POST '<baseUrl>/auth/signup'  -d '{
  "firstName": "Jean",
    "lastName": "Dupont",
    "username": "jean_d01",
    "gender": "Male",
    "email": "jeandupon@gmail.com",
    "phone": 650504571,
    "password": "@Password#123",
    "profilPicUrl": "https://www.google.com/img/logo.png",
    "intro": "I am a web developer"
}'
```

> Example Response

```json
{
  "statusCode": 201,
  "message": "User created successfully.",
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

`POST /auth/signup`

### Request Body

| Name         | Type   | Required | Description                                                              |
| ------------ | ------ | -------- | ------------------------------------------------------------------------ |
| firstName    | string | true     | The first name of the user                                               |
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

```curl
curl -X POST '<baseUrl>/auth/signin' -H 'x-custom-lang: fr'  -d '{
  "email": "jeandupon@gmail.com",
  "password": "@Password#123"
}'
```

> Example Response

```json
{
  "statusCode": 200,
  "message": "Utilisateur connecté avec succès",
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

`POST /auth/signin`

### Request Body

| Name     | Type   | Required | Description       |
| -------- | ------ | -------- | ----------------- |
| email    | string | true     | The user email    |
| password | string | true     | The user password |

# Profile

## Get User Profile

> Example Request

```curl
curl -X GET '<baseUrl>/profile' -H 'Authorization: Bearer <token>'
```

> Example Response

```json
{
  "statusCode": 200,
  "message": "Profile successfully fetched",
  "data": {
    "id": 1,
    "fistName": "Jean",
    "lastName": "Freddy",
    "username": "Rogelio19",
    "gender": "Male",
    "phone": 650504571,
    "email": "Tomas25@hotmail.com",
    "profilePicUrl": "http://placeimg.com/640/480",
    "isVerified": false,
    "followersCount": 0,
    "followingCount": 0,
    "songCount": 0,
    "playlistCount": 0,
    "lastLogin": "2022-03-27T22:39:32.081Z",
    "status": {
      "id": 1,
      "name": "Active"
    }
  }
}
```

The profile endpoint is used to retrieve the user profile.

### HTTP Request

`GET /profile`

### Request Header

| Name          | Type   | Required | Description    |
| ------------- | ------ | -------- | -------------- |
| Authorization | Bearer | true     | The user token |

If the user is not authenticated, the response will contain a 401 status code.
The response will also contain a message explaining the error.

## Update User Profile

> Example Request

```curl
curl -X PUT '<baseUrl>/profile' -H 'Authorization: Bearer <token>' -d '{
   "fistName": "Jean",
    "lastName": "Freddy",
    "username": "Rogelio20",
    "phone": 650504570,
}'
```

> Example Response

```json
{
  "statusCode": 200,
  "message": "Profile successfully updated",
  "data": {
    "id": 1
  }
}
```

The profile endpoint is used to update the user profile.
If the username is already taken, the response will contain a 409 status code. Same for the email and the phone.

### HTTP Request

`PUT /profile`

### Request Header

| Name          | Type   | Required | Description    |
| ------------- | ------ | -------- | -------------- |
| Authorization | string | true     | The user token |

### Request Body

| Name         | Type   | Required | Description          |
| ------------ | ------ | -------- | -------------------- |
| firstName    | string | false    | The user first name  |
| lastName     | string | false    | The user last name   |
| phone        | number | false    | The user phone       |
| username     | string | false    | The user username    |
| email        | string | false    | The user email       |
| profilPicUrl | string | false    | Profile photo        |
| intro        | string | false    | A short introduction |

## Upload Profile Picture

> Example Request

```curl
curl -X POST '<baseUrl>/profile/profile-pic' -H 'Authorization: Bearer <token>' -d '{
   "profilePicUrl": "https://images.pexels.com/photos/220453/pexels-photo-220453.jpeg"
}'
```

> Example Response

```json
{
  "statusCode": 200,
  "message": "Profile picture successfully uploaded",
  "data": {
    "userId": 1
  }
}
```

This endpoint is used to upload a profile picture.

### HTTP Request

`POST /profile/profile-pic`

### Request Header

| Name          | Type   | Required | Description    |
| ------------- | ------ | -------- | -------------- |
| Authorization | string | true     | The user token |

### Request Body

| Name         | Type   | Required | Description          |
| ------------ | ------ | -------- | -------------------- |
| profilPicUrl | string | true     | User profile picture |

## Change Password

> Example Request

```curl
curl -X POST '<baseUrl>/profile/change-password' -H 'Authorization: Bearer <token>' -d '{
  "oldPassword": "Password1",
  "newPassword": "Password2"
}
```

> Example Response

```json
{
  "statusCode": 200,
  "message": "Password successfully changed",
  "data": {
    "userId": 1
  }
}
```

This endpoint is used to change user's password.

### HTTP Request

`POST /profile/change-password`

### Request Header

| Name          | Type   | Required | Description    |
| ------------- | ------ | -------- | -------------- |
| Authorization | string | true     | The user token |

### Request Body

| Name        | Type   | Required | Description           |
| ----------- | ------ | -------- | --------------------- |
| oldPassword | string | true     | Current user passowrd |
| newPassword | string | true     | New user password     |

If the old password is the same as the new password, the response will contain a 400 status code.
The response will also contain a message explaining the error.

# Songs

## Create Song

> Example Request

```curl
curl -X POST '<baseUrl>/profile/change-password' -H 'Authorization: Bearer <token>' -d '{
  "title": "Sound Helix",
  "imageUrl": "https://images.pexels.com/photos/220453/pexels-photo-220453.jpeg",
  "songUrl": "https://www.soundhelix.com/examples/mp3/SoundHelix-Song-1.mp3",
  "duration": 360,
  "genre": "Pop"
}
```

> Example Response

```json
{
  "statusCode": 200,
  "message": "Song created successfully",
  "data": {
    "songId": 1
  }
}
```

This endpoint is used to create a song.

### HTTP Request

`POST /songs/create`

### Request Header

| Name          | Type   | Required | Description    |
| ------------- | ------ | -------- | -------------- |
| Authorization | string | true     | The user token |

### Request Body

| Name     | Type   | Required | Description       |
| -------- | ------ | -------- | ----------------- |
| title    | string | true     | The song title    |
| imageUrl | string | true     | The song image    |
| songUrl  | string | true     | The song url      |
| duration | number | true     | The song duration |
| genre    | string | true     | The song genre    |

<!-- <aside>
p.s. The songs endpoint is not implemented yet.
</aside>

## Get All Songs

## Get Song by Id

## Get Songs by User Id

## Get Songs by Genre

## Create Song

## Update Song

## Delete Song -->
