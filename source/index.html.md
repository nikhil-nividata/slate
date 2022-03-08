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

- name: description content: Documentation for the Kittn API

---

# Introduction

Welcome to Made In Latino API. These APIs are used to provide all the services to Made In Latino users.

We have language binding in HTTP Requests! You can view code examples in the dark area to the right, and you can switch
the programming language of the examples with the tabs in the top right.

This API documentation page was created with [Slate](https://github.com/slatedocs/slate). Feel free to edit it and use
it as a base for your own API's documentation.

# Users

This set of API endpoints is used to interact with User object.

## User Login

We use twitter's OAuth to log users in. Twitter uses a 3 legged OAuth scheme to provide User Authentication. You can
refer to Twitter's
OAuth [here](https://developer.twitter.com/en/docs/authentication/oauth-1-0a/obtaining-user-access-tokens).

To summarise, we send a request to backend for user login, the server sends a request to Twitter using our Consumer Key
and Secret. Twitter sends back the url to log the user in and shows the login and permissions page. Once user gives
permission to our app twitter redirects back to our application with user tokens passed as query params. We send the
user params back to the server where the params are stored and the token for user is generated and sent back to the
frontend.

### Steps for user login.

#### Step 1

> Example Request

```request 
GET https://madeinlatino.net/users/auth-url/
```

> Server responds with JSON structured like this

```json
{
  "redirect_url": "<Login URL for Twitter>"
}
```

Get the redirect URL from backend. From the frontend make a get request to this url

`GET https://madeinlatino.net/users/auth-url/`

#### Step 2

Redirect user to the received URL. Make sure to configure a callback URL in twitter developer settings. On the callback
page, get the tokens from URL Parameters and send them back to the server

HTTP Request

`POST https://madeinlatino.net/users/twitter/login/`

Request Body

|Parameter|Type|Required|Description|
|---------|----|--------|-----------|
|access_token|string|Required|User Twitter OAuth Token|
|token_secret|string|Required|User Twitter Token Secret|

> Example Request

```request 
POST https://madeinlatino.net/users/twitter/login/
Content-Type: application/json

{
  "access_token": "...",
  "token_secret": "..."
}
```

> Server responds with JSON structured like this

```json
{
  "key": "Token for the user"
}
```

## User Logout

This API endpoint is used to logout the user. It deletes the token associated with the user's login.

HTTP Request
`POST https://madeinlatino.net/users/logout/`

> Example Request

```request 
POST https://madeinlatino.net/users/logout/
Authorization: Token <User Token>
```

> Server responds with

```
User logged out successfully
```

## User Info

This API endpoint is used to get info for the logged in user.

HTTP Request

`GET https://madeinlatino.net/users/info/`

> Example Request

```request
GET https://madeinlatino.net/users/info/
Authorization: Token <User Token>
```

> Sever responds with json structured like this

```json
{
  "twitter_user_name": "...",
  "gender": "...",
  "twitter_user_profile_image": "...",
  "profile_image": "...",
  "global_points": 1000.00,
  "first_name": "...",
  "last_name": "...",
  "email": "user@example.com",
  "mobile_number_prefix": 25,
  "mobile_number": "...",
  "date_of_birth": "1989-07-07",
  "is_flag_action": true
}
```

## User Profile

This endpoint is used to interact with user profile.

### Get User Profile

Make a GET request to the endpoint.

`GET https://madeinlatino.net/users/profile/`

> Example Request

```request
GET https://madeinlatino.net/users/profile/
Authorization: Token <User Token>
```

> Server responds with JSON structured like this

```json
{
  "first_name": "...",
  "last_name": "...",
  "email": "user@example.com",
  "date_of_birth": "1989-07-07",
  "mobile_number_prefix": 3,
  "mobile_number": "...",
  "is_flag_action": true
}
```

### Update User Profile

Make a PATCH request to the endpoint.

Redirect from frontend to the above URL.

`PATCH https://madeinlatino.net/users/profile/`

Request Body

|Parameter|Type|Required|Description|
|---------|----|--------|-----------|
|first_name|string|Optional|User first name|
|last_name|string|Optional|User last name|
|email|string|Optional|User email|
|date_of_birth|string|Optional|User date of birth|
|mobile_number_prefix|number|Optional|Id of mobile number prefix object|
|mobile_number|string|Optional|User mobile number|
|is_flag_action|boolean|Optional|User's consent to flag action|

> Example Request

```request
PATCH https://madeinlatino.net/users/profile/
Authorization: Token <User Token>
Content-Type: application/json

{
  "last_name": "DOE"
}
```

> Server responds with updated User Profile

```json
{
  "first_name": "John",
  "last_name": "Doe",
  "email": "user@example.com",
  "date_of_birth": "1989-07-07",
  "mobile_number_prefix": 33,
  "mobile_number": "...",
  "is_flag_action": true
}
```

## Mobile Prefixes

This API endpoint is used to get the Mobile Prefix data for all countries.

Request

`GET https://madeinlatino.net/users/mobile-prefix/`

> Example Request

```request
GET https://madeinlatino.net/users/mobile-prefix/
```

> Server responds with JSON structured like this

```json
[
  {
    "pk": 1,
    "dial_code": "+93",
    "name": "Afghanistan"
  }
]
```

## Invitation Data

This API endpoint is used to get the invitation data when inviting a user. To lower the loading time, we do all the
requests at once. The invitee, inviter and artist have their twitter usernames passed in query params.

Request

`GET https://madeinlatino.net/users/invitation-data/`

Request Parameters

|Parameter|Type|Required|Description|
|---------|----|--------|-----------|
|artist|string|Required|Artist's Twitter Username|
|invitee|string|Required|Invitee's Twitter Username|
|inviter|string|Required|Inviter's Twitter Username|

> Example Request

```request
GET https://madeinlatino.net/users/invitation-data/?artist=Harry&invitee=Jane&inviter=John
```

> Server responds with JSON structured like this

```json

{
  "invitee": {
    "twitter_user_name": "Jane",
    "twitter_user_profile_image": "...",
    "profile_image": "...",
    "first_name": "Jane",
    "last_name": "Doe"
  },
  "inviter": {
    "twitter_user_name": "John",
    "twitter_user_profile_image": "...",
    "profile_image": "...",
    "first_name": "John",
    "last_name": "Doe"
  },
  "artist": {
    "id": 3,
    "twitter_user_name": "Harry",
    "name": "Harry Style",
    "artist_profile_image": "..."
  },
  "invitee_rank": 2
}
```

## Delete Account

This API endpoint is used delete account of user. Request

`DELETE https://madeinlatino.net/users/delete-my-account/user_id/`

> Example Request

```request
DELETE https://madeinlatino.net/users/delete-my-account/10/
```

# Ranking

This set of APIs is used to interact with fan rankings.

## Artist's Fan Rankings

This API endpoint is used to get the fan ranking of an artist.

The general URL is

`https://madeinlatino.net/ranking/artist_id/ARTIST-ID/`

The structure of response is

|Parameter|Type|Description|
|---------|----|-----------|
|count|number|Number of users in ranking|
|next|url|URL for next page of ranking|
|previous|url|URL for previous page|
|results|array|Array of [Username, Points] objects in ranking|

> Example Request

```request
GET https://madeinlatino.net/ranking/artist_id/7/
Authorization: Token <User Token>
```

> Server returns a JSON structured like this

```json
{
  "count": 189,
  "next": "https://madeinlatino.net/ranking/artist_id/7/?limit=100&offset=100",
  "previous": null,
  "results": [
    [
      "John",
      20.0
    ]
  ]
}
```

## User's rank for an artist

This API endpoint is used to get the ranking data of the logged in user corresponding to a particular user.

The General url is

`https://madeinlatino.net/ranking/my_rank/artist_id/:artist-id/`

> Example Request

```request
GET https://madeinlatino.net/ranking/my_rank/artist_id/7/
Authorization: Token <User Token>
```

> Server responds with JSON structured like this

```json
{
  "rank": 1,
  "score": 20.0
}
```

## User's Global Rank

This API endpoint is used to get the logged in user's gloabal rank and score.

Request

`GET https://madeinlatino.net/ranking/my_global_rank/`

> Example Request

```request
GET https://madeinlatino.net/ranking/my_global_rank/
Authorization: Token <User Token>
```

> Server responds with JSON structured like this

```json
{
  "rank": 1,
  "score": 680.0
}
```

## Download registered Users

This API endpoint is used to get the Registered users.

Request

`GET https://madeinlatino.net/ranking/download-users/`

> Example Request

```request
GET https://madeinlatino.net/ranking/download-users/
Authorization: Token <User Token>
```

> Server responds with JSON structured like this

```file
csv file
```

# Rewards

This set of APIs is used to create and listing of rewards.

## Reward listing

This API endpoint is used to get the rewards of an artist.

The general URL is

`https://madeinlatino.net/reward/rewards/ARTIST-ID/`

The structure of response is

|Parameter|Type|Description|
|---------|----|-----------|
|count|number|Number of rewards|
|next|url|URL for next page of rewards|
|previous|url|URL for previous page|
|results|array|Array of [title, image, description] objects in rewards|

> Example Request

```request
GET https://madeinlatino.net/reward/rewards/7/
Authorization: Token <User Token>
```

> Server returns a JSON structured like this

```json
{
    "count": 2,
    "next": null,
    "previous": null,
    "results": [
        {
            "title": "Reward 2",
            "image": null,
            "description": "description of Reward 2"
        },
        {
            "title": "Reward1",
            "image": null,
            "description": "Reward1"
        }
    ]
}
```

# Artist

This set of API endpoints is used to interact with Artist objects.

## Artist's Fans Data

This endpoint is used to get the data of Fans. We pass list of fan twitter usernames in the query params.

Request

`GET https://madeinlatino.net/artist/fan-data/`

Request Params

|Parameter|Type|Description|
|---------|----|-----------|
|fan|string|Values of User Twitter Names whose data is needed|

> Example Request

```request
GET https://madeinlatino.net/artist/fan-data/?fan=John,Jane
Authorization: Token <User Token>
```

> Server Responds with JSON structured like this

```json
{
  "John": {
    "twitter_user_profile_image": "...",
    "is_registered": true,
    "can_invite": false
  },
  "Jane": {
    "twitter_user_profile_image": "...",
    "is_registered": false,
    "can_invite": false
  }
}
```

> The `can_invite` flag denotes if the logged in user can invite a particular user

## Artist Listing

This API endpoint is used to get a list of all the registered Artists in the system.

Request

`GET https://madeinlatino.net/artist/listing/`

> Example Request

```request
GET https://madeinlatino.net/artist/listing/
```

> Server responds with JSON structured like this

```json
{
  "count": 1,
  "next": null,
  "previous": null,
  "results": [
    {
      "id": 3,
      "name": "Harry Style",
      "artist_profile_image": "...",
      "twitter_user_name": "harry",
      "remaining_days": 5,
      "fans": 2329,
      "due_day": 3
    }
  ]
}
```

> The `count` variable refers to number of artists. `remaining_days` in an artist refers to number of days to go for the artists
> due day and `fans` in an artist refer to total fans of the artist.

# Action

This set of API endpoints is used to interact with Action objects.

## Actions

A bit of terminology. We refer to action as the type of actions that are there. Like `TWEET_100`, `TWEET_150` and
others. Recommendation is recommended to the Users, recommendation is one of the many actions of a type generated by
admin or generated automatically. Like a recommendation to Tweet inviting a user to platform is of action
type `TWEET_150`

### Get recommendations for a user.

Make a get request to the endpoint.

Request

`GET https://madeinlatino.net/action/`

Request Params

|Parameter|Type|Description|
|---------|----|-----------|
|artist|number|Artist id|

> Make a GET request with the user token. Artist id is passed as a query param

```request
GET https://madeinlatino.net/action/?artist=3
Authorization: Token <User Token>
```

> Server responds with JSON structured like this

```json
{
  "data": [
    {
      "id": 115,
      "action": {
        "id": 4,
        "type": "...",
        "points": 10.0
      },
      "tweet_data": "...",
      "tweet_id": null,
      "button_name": null,
      "description": null,
      "link": null,
      "points": 0.0
    }
  ]
}
```

> `action` contains the action which this recommendation is type of. `action.type` refers to the type of actions from the predefined types <br>
> The content of `tweet_data` is handled according to the type of action being performed. <br>
> The values of `tweet_id`, `button_name`, `description`, `link` vary according to the type of action being recommended.

### Perform a recommendation for a user

You can perform actions for a user by posting to this endpoint.

Request

`POST https://madeinlatino.net/action/`

Request Body

|Parameter|Type|Required|Description|
|---------|----|--------|-----------|
|action_id|number|Required|Id of action to be performed|
|recommend_id|number|Required|Id of recommendation to be performed|
|artist_id|number|Required|Id of artist for which action is to be performed|

> Example Request

```http

POST https://madeinlatino.net/action/
Authorization: Token <User Token>
Content-Type: application/json

{
  "action_id":11,
  "recommend_id":46,
  "artist_id":3
}

```

> `action_id` refers to the type of action being performed. It can be get from `action.id` property of the data from the GET request <br>
> `recommend_id` refers to the recommendation being performed.
> `artist_id` refers to the artist for which the action is being performed.

> Server responds with JSON structured like this

```json
{
  "message": "Action Performed"
}
```

## Direct Action

This endpoint allows us to get the direct actions that can be performed by the user.

Request

`GET https://madeinlatino.net/action/direct-action/`

> Example Request

```request
GET https://madeinlatino.net/action/direct-action/
Authorization: Token <User Token>
```

> Server responds with JSON structured like this

```json
{
  "id": 117,
  "action": {
    "id": 14,
    "type": "DIRECT_ACTION",
    "points": 10.0
  },
  "tweet_data": "...",
  "tweet_id": "...",
  "button_name": "DIRECT",
  "description": "",
  "link": "",
  "points": 0.0,
  "artist": {
    "id": 3,
    "twitter_user_name": "Harry",
    "name": "Harry Styles",
    "artist_profile_image": "..."
  }
}
```

To perform this action, use the normal POST method for performing actions.

## Ranking Invite

API endpoint to invite users from Rankings Page.

Request

`POST https://madeinlatino.net/action/ranking-invite/`

Request Body

|Parameter|Type|Required|Description|
|---------|----|--------|-----------|
|artist|number|Required|Ranking Artist's ID|
|user|string|Required|Username of user to be invited|

> Example Request

```request
POST https://madeinlatino.net/action/ranking-invite/
Authorization: Token <User Token>
Content-Type: application/json

{
  "artist":3,
  "user":"John"
}
```

> `artist` param takes the artist id. `user` param takes the twitter username of the user to invite.

## follow

API endpoint to Follow user.

Request

`POST https://madeinlatino.net/action/follow/`

Request Body

|Parameter|Type|Required|Description|
|---------|----|--------|-----------|
|is_user|boolean|Required|some time it will ne artist or users|
|points|integer|Required|points of the action|
|user|json|Required|user data|

> Example Request

```request
POST https://madeinlatino.net/action/follow/
Authorization: Token <User Token>
Content-Type: application/json

```
> `is_user` param takes the boolean value. `user` param takes the user data. `points` param takes the point of action

```json
{
  "message": "success",
  "old_point": 20,
  "point": 60,
  "rank": 3
}
```


## follow Users

GET API endpoint to list of users for follow.

Request

`GET https://madeinlatino.net/action/follow-users/`


> Example Request

```request
GET https://madeinlatino.net/action/follow-users/
Authorization: Token <User Token>
Content-Type: application/json
```

> Server responds with JSON structured like this

```json
{
  "is_user": true,
  "points": 40,
  "user": {
    "id": 693,
    "twitter_user_name":"abc"
  }
}
```

# Get Tweet

API Endpoint that gets the tweet from twitter.

## Get tweet by Id

This endpoint works only when called as an admin user. This endpoint uses session based authentication Request

`https://madeinlatino.net/get_tweet/:tweet-id/`


> Example Request

```request
GET https://madeinlatino.net/get_tweet/1461393548872458250/
Cookie: sessionid=<Session Cookie>;
```

> Server responds with JSON structured like this

```json
{
  "text": "Today @thewavexr 6pm PT / 9pm ET #BieberWave Join me here: https://t.co/8KNStRidql https://t.co/yj5ULPrV7R"
}
```
