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

We have language bindings in Shell, Python, and JavaScript! You can view code examples in the dark area to the right,
and you can switch the programming language of the examples with the tabs in the top right.

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

Steps for user login.

Step 1
> From the frontend make a get request to this url

```request 
GET https://madeinlatino.net/users/auth-url/
```

> Server responds with JSON structured like this

```json
{
  "redirect_url": "<Login URL for Twitter>"
}
```

Step 2
> Redirect user to the received URL. Make sure to configure a callback URL in twitter developer settings. On the callback page, get the tokens from URL Parameters and send them back to the server

```request 
POST https://madeinlatino.net/users/twitter/login/
Content-Type: application/json

{
  "access_token": "OAuth token from twitter",
  "token_secret": "OAuth token secret"
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
> Make a POST request with the user token

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
> Make a GET request with the use token

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

> Make a GET request with user token

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

> Make a PATCH request with user token and data

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
> Make a GET request

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

> Make a GET request

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

# Ranking

This set of APIs is used to interact with fan rankings.

## Artist's Fan Rankings

This API endpoint is used to get the fan ranking of an artist.

The general URL is

```
https://madeinlatino.net/ranking/artist_id/ARTIST-ID/
```

The structure of response is

```json
{
  "count": "Number of users in ranking",
  "next": "URL for next page",
  "previous": "URL for previous page",
  "results": [
    [
      "User Twitter Name",
      "Points"
    ]
  ]
}
```

> Make a GET request with the user token

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

```
http://localhost:8000/ranking/my_rank/artist_id/ARTIST-ID/
```

> Make a GET request with the user token

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

> Make a GET request with the user token

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

# Artist

This set of API endpoints is used to interact with Artist objects.

## Artist's Fans Data

This endpoint is used to get the data of Fans. We pass list of fan twitter usernames in the query params.
> Make a GET request with the user token

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
> Make a GET request

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

> Make a POST request with the user token and following data.

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

> Make a GET request with the user token

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

[comment]: <> (## Webhook)

[comment]: <> (Maybe this isnt part of API )

## Ranking Invite

API endpoint to invite users from Rankings Page.

> Make a POST request with the user token and the following data

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

> Server responds with JSON status 200

# Get Tweet

API Endpoint that gets the tweet from twitter.

## Get tweet by Id

This endpoint works only when called as an admin user. This endpoint uses session based authentication
> Generic form
```
https://madeinlatino.net/get_tweet/TWEET-ID/
```
> `TWEET-ID` is the ID of a tweet that can be obtained from Twitter.


> Make a GET request
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
