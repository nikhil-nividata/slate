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

```http request
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

```http request
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

```http request
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

```http request
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

```http request
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

```http request
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

```http request
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

```http request
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

## Artist's Fan Rankings

## User's rank for an artist

## Simulate Rank Tweets

## User's Global Rank

# Artist

## Artist's Fans Data

## Artist Listing

# Action

## Actions

## Direct Action

## Webhook

## Ranking Invite

# Get Tweet

## Get tweet by Id
