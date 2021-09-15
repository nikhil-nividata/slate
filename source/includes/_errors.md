# Errors

The Made in Latino API uses the following error codes:


Error Code | Meaning
---------- | -------
400 | Bad Request -- Your request is invalid.
401 | Unauthorized -- The user is not authenticated.
403 | Forbidden -- The request is forbidden for the current user.
404 | Not Found -- The specified resource could not be found.
405 | Method Not Allowed -- Invalid HTTP method.
406 | Not Acceptable -- You requested a format that isn't json.
410 | Gone -- The resource requested has been removed from our servers.
429 | Too Many Requests -- Made too many requests.
500 | Internal Server Error -- We had a problem with our server. Try again later.
503 | Service Unavailable -- We're temporarily offline for maintenance. Please try again later.
