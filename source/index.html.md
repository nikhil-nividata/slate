---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

toc_footers:
  - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true

code_clipboard: true

meta:
  - name: description
    content: Documentation for the Kittn API
---

# Introduction

Welcome to Made In Latino API. These APIs are used to provide all the services to Made In Latino users. 

We have language bindings in Shell, Python, and JavaScript! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

This API documentation page was created with [Slate](https://github.com/slatedocs/slate). Feel free to edit it and use it as a base for your own API's documentation.

# Users

## User Login

This API is used to login a user into the system using the Twitter OAuth library.

> To authenticate a user this code:

```shell
curl -d '{"access_token":"ACCESS TOKEN"}' \
    -H "Content-Type: application/json" \
    -X POST "http://madeinlatino.net/api/users/twitter/login/" 
```

> The above command returns JSON structured like this:

```json
{
  "auth_key" : "Auth key"
}
```

### HTTP Request

`POST http://madeinlatino.net/api/users/twitter/login/`

## Get User Profile

This API is used to access a User's profile.

```shell
curl "http://madeinlatino.net/api/users/1/"
```

> The above command returns JSON structured like this:

```json
{
  "first_name" : "First",
  "last_name": "Last", 
  "photo": "URL",
  "twitter_user_name": "",
  "twitter_user_picture": "URL",
  "global_points": 0
}
```

## Update User Profile

This API is used to update a User's profile.

```shell
curl -d '{"first_name": "Tim"}' -X POST  \
    -H "Content-Type: application/json" \
    "http://madeinlatino.net/api/users/1/"
```

> The above command returns JSON structured like this:

```json
{
  "first_name" : "Tim",
  "last_name": "Last", 
  "photo": "URL",
  "twitter_user_name": "",
  "twitter_user_picture": "URL",
  "global_points": 0
}
```


# Artists

## Get All Artists

```shell
curl "http://madeinlatino.net/api/artists/"
```


> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Artist",
    "photo": "Photo URL",
    "twitter_user_name": "Twitter User name",
    "due_date": 7
  },
  {
    "id": 2,
    "name": "Artist",
    "photo": "Photo URL",
    "twitter_user_name": "Twitter User name",
    "due_date": 9
  }
]
```

This endpoint retrieves all artists.

### HTTP Request

`GET http://madeinlatino.net/api/artists`

## Get a Specific Artist

```shell
curl "http://madeinlatino.net/api/artists/2" 
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Artist",
  "photo": "Photo URL",
  "twitter_user_name": "Twitter User name",
  "due_date": 9
}
```

This endpoint retrieves a specific artist.

### HTTP Request

`GET http://madeinlatino.net/api/artists/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the artist to retrieve

## Get fan rankings of an artist

```shell
curl "http://madeinlatino.net/api/artists/2/rankings/"
```

> The above command returns JSON structured like this:

```json
{
  "rankings": [
    {
      "rank": 1,
      "twitter_user_name": "Twitter user name",
      "twitter_profile_link":"URL",
      "points": 1000,
      "photo": "Photo URL",
    },
    {
      "rank": 2,
      "twitter_user_name": "Twitter user name",
      "twitter_profile_link":"URL",
      "points": 1000,
      "photo": "Photo URL",
    },
    {
      "rank": 3,
      "twitter_user_name": "Twitter user name",
      "twitter_profile_link":"URL",
      "points": 1000,
      "photo": "Photo URL",
    }
  ],
  "page":1
}
```

This endpoint returns the paginated data of artist's fan rankings

### HTTP Request

`http://madeinlatino.net/api/artists/2/rankings/`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the artist

### Query Parameters

Parameter | Description
--------- | -----------
PAGE | The page number to load

# Actions

## Get actions

```shell
curl "http://madeinlatino.net/api/actions/"
```

> The above command returns JSON structured like this:

```json
[
  {
    "id":1,
    "type": "1",
    "code": "101",
    "points": 10,
    "tweet_data": "Tweet Data"
  },{
    "id":2,
    "type": "1",
    "code": "150",
    "points": 20,
    "tweet_data": "Tweet Data"
  },
]
```

This endpoint retrieves all the actions that can be performed by the current user.

### HTTP Request

`GET http://madeinlatino.net/api/actions/`

## Perform an action

```shell
curl -d '{"action_code": "101"}' \
-H "Content-Type: application/json" \
-X POST "http://madeinlatino.net/api/actions/"
```

> The above command returns JSON structured like this:

```json
{
  "message" : "Message"
}
```

The endpoint is used to register actions done by the user.

### HTTP Request

`POST http://madeinlatino.net/api/actions/`
