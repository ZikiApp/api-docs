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

The base URL of Ziki API is: `http://188.166.62.35/api`. You can use this URL to access the Ziki API endpoints.
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
curl -X GET '<baseUrl>/songs?page=1&perPage=10' -H 'Authorization: Bearer <token>'
```

### Query parameters

| Name    | Description                                                                  |
| ------- | ---------------------------------------------------------------------------- |
| page    | Specifies the page number to retrieve. Value must be between 1 and 100.      |
| perPage | Indicates how many records each page should contain. The default value is 20 |

# Authentication

## Register

> Example Request

```curl
curl -X POST '<baseUrl>/auth/register'  -d '{
 "firstName": "John",
  "lastName": "Doe",
  "username": "johndoe",
  "email": "johndoe@gmail.com",
  "password": "Password@123"
}'
```

> Example Response

```json
{
  "statusCode": 201,
  "message": "User registered successfully.",
  "data": {
    "user": {
      "id": 1,
      "email": "johndoe@gmail.com",
      "provider": "email",
      "profile": {
        "firstName": "John",
        "lastName": "Doe",
        "username": "johndoe",
        "gender": "Unspecified",
        "phone": null,
        "intro": null,
        "status": "Pending",
        "profilePicUrl": null,
        "deletedAt": null
      }
    },
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9"
  }
}
```

Register for a new user account.

### HTTP Request

`POST /auth/register`

### Request Body

| Name      | Type   | Required | Description                |
| --------- | ------ | -------- | -------------------------- |
| firstName | string | true     | The first name of the user |
| lastName  | string | true     | The last name of the user  |
| username  | string | true     | The username of the user   |
| email     | string | true     | The user email             |
| password  | string | true     | The user password          |

## Login

> Example Request

```curl
curl -X POST '<baseUrl>/auth/login' -H 'x-custom-lang: fr'  -d '{
  "email": "johndoe@gmail.com",
  "password": "Password#123"
}'
```

> Example Response

```json
{
  "statusCode": 200,
  "message": "User logged in successfully.",
  "data": {
    "user": {
      "id": 1,
      "email": "johndoe@gmail.com",
      "provider": "email",
      "profile": {
        "firstName": "John",
        "lastName": "Doe",
        "username": "johndoe",
        "gender": "Unspecified",
        "phone": null,
        "intro": null,
        "status": "Pending",
        "profilePicUrl": null,
        "deletedAt": null
      }
    },
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9"
  }
}
```

The login endpoint is used to authenticate a user. It expects a email and password in the request body. The response will contain a token that can be used to authenticate future requests.

### HTTP Request

`POST /auth/login`

### Request Body

| Name     | Type   | Required | Description       |
| -------- | ------ | -------- | ----------------- |
| email    | string | true     | The user email    |
| password | string | true     | The user password |

<!-- ## Login with Google -->

<!-- ## Login with Facebook -->

## Forgot Password

> Example Request

```curl
curl -X POST '<baseUrl>/auth/forgot-password' -H 'x-custom-lang: fr'  -d '{
  "email": "johndoe@gmail.com"
}'
```

> Example Response

```json
{
  "statusCode": 200,
  "message": "Password reset email sent successfully.",
  "data": {
    "success": true
  }
}
```

This endpoint is used to send an OTP code in the user's email that allows the user to reset his password. It expects a email in the request body.

### HTTP Request

`POST /auth/forgot-password`

### Request Body

| Name  | Type   | Required | Description    |
| ----- | ------ | -------- | -------------- |
| email | string | true     | The user email |

<!-- ## Check OTP Code -->

## Reset Password

> Example Request

```curl
curl -X POST '<baseUrl>/auth/forgot-password' -H 'x-custom-lang: fr'  -d '{
  "password": "@Password#1234",
  "otpCode": "123456"
}'
```

> Example Response

```json
{
  "statusCode": 200,
  "message": "Password reset successfully.",
  "data": {
    "success": true
  }
}
```

This endpoint is used to reset the user password. It expects a password and otpCode in the request body.

### HTTP Request

`POST /auth/reset-password`

### Request Body

| Name     | Type   | Required | Description                   |
| -------- | ------ | -------- | ----------------------------- |
| password | string | true     | The new user password         |
| otpCode  | string | true     | The otp code sent to the user |

|

<!-- ## Verify Email -->

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
    "firstName": "John",
    "lastName": "Doe",
    "username": "johndoe",
    "gender": "Male",
    "phone": "672505050",
    "email": "johndoe@gmail.com",
    "intro": "Hi! I'm a mobile developer. I love to create apps for the Android and iOS platforms.",
    "status": "Pending",
    "profilePicUrl": "https://images.pexels.com/photos/220453/pexels-photo-220453.jpeg?auto=compress",
    "isVerified": false,
    "followersCount": 0,
    "followingCount": 0,
    "songCount": 0,
    "playlistCount": 0,
    "lastLogin": "2022-07-23T03:04:01.348Z",
    "registeredAt": "2022-07-22T23:17:00.550Z",
    "updatedAt": "2022-07-23T02:04:01.355Z",
    "deletedAt": null
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

# Genres

## Get list of genres

> Example Request

```curl
curl -X GET '<baseUrl>/genres/list'
```

> Example Response

```json
{
  "statusCode": 200,
  "message": "Successfully retrieved list",
  "data": [
    {
      "id": 1,
      "name": "Pop"
    },
    {
      "id": 2,
      "name": "Rock"
    },
    {
      "id": 3,
      "name": "Rap"
    },
    {
      "id": 4,
      "name": "Electronic"
    },
    {
      "id": 5,
      "name": "Jazz"
    }
  ]
}
```

This endpoint is used to retrieve the list of genres.

### HTTP Request

`GET /genres/list`

# Songs

## Get songs by user id

> Example Request

```curl
curl -X POST '<baseUrl>/songs?page=1&perPage=2' -H 'Authorization: Bearer <token>'
```

> Example Response

```json
{
  "statusCode": 200,
  "message": "All songs retrieved successfully",
  "data": {
    "totalItems": 8,
    "totalPage": 4,
    "currentPage": 3,
    "itemsPerPage": 2,
    "items": [
      {
        "id": 7,
        "title": "Song tile",
        "imageUrl": "http://placeimg.com/640/480",
        "songUrl": "https://ethelyn.info",
        "duration": 450,
        "playsCount": 0,
        "likesCount": 0,
        "commentsCount": 0,
        "createdAt": "2022-03-28T06:25:01.565Z",
        "updatedAt": "2022-03-28T06:25:01.565Z",
        "genre": {
          "id": 1,
          "name": "Blues",
          "createdAt": "2022-03-24T02:32:25.572Z",
          "updatedAt": "2022-03-24T02:32:25.572Z"
        }
      },
      {
        "id": 8,
        "title": "Song tile",
        "imageUrl": "http://placeimg.com/640/480",
        "songUrl": "http://stevie.net",
        "duration": 450,
        "playsCount": 0,
        "likesCount": 0,
        "commentsCount": 0,
        "createdAt": "2022-03-28T06:26:22.242Z",
        "updatedAt": "2022-03-28T06:26:22.242Z",
        "genre": {
          "id": 1,
          "name": "Blues",
          "createdAt": "2022-03-24T02:32:25.572Z",
          "updatedAt": "2022-03-24T02:32:25.572Z"
        }
      }
    ]
  }
}
```

This endpoint is used to retrieve the list of songs. The response will contain the total number of songs,
the total number of pages, the current page, the number of items per page and the list of songs.

### HTTP Request

`GET /songs?page=1&perPage=20`

Please note that the page and perPage parameters are optional. If they are not provided, the default values will be used.

### Request Header

| Name          | Type   | Required | Description    |
| ------------- | ------ | -------- | -------------- |
| Authorization | string | true     | The user token |

### Query Parameters

| Name    | Type   | Required | Description                  |
| ------- | ------ | -------- | ---------------------------- |
| page    | number | false    | The page number              |
| perPage | number | false    | The number of items per page |

## Get song by id

> Example Request

```curl
curl -X GET '<baseUrl>/songs/8' -H 'Authorization: Bearer <token>'
```

> Example Response

```json
{
  "statusCode": 200,
  "message": "Song retrieved successfully",
  "data": {
    "id": 8,
    "title": "Song tile",
    "imageUrl": "http://placeimg.com/640/480",
    "songUrl": "http://stevie.net",
    "duration": 450,
    "playsCount": 0,
    "likesCount": 0,
    "commentsCount": 0,
    "genre": {
      "id": 1,
      "name": "Blues",
      "createdAt": "2022-03-24T02:32:25.572Z",
      "updatedAt": "2022-03-24T02:32:25.572Z"
    }
  }
}
```

This endpoint is used to retrieve a song.

### HTTP Request

`POST /songs/<ID>`

### Request Header

| Name          | Type   | Required | Description    |
| ------------- | ------ | -------- | -------------- |
| Authorization | string | true     | The user token |

### Path Parameters

| Name | Type   | Required | Description |
| ---- | ------ | -------- | ----------- |
| id   | number | true     | The song id |

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
