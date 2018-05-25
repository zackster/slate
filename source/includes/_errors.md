# Errors

The WorkReduce API uses the following error codes:


Error Code | Meaning
---------- | -------
400 | Bad Request -- Your request is invalid.
401 | Unauthorized -- Your API key is wrong.
403 | Forbidden -- You don't have permission to do that with the API.
404 | Not Found -- You asked us to do something.  We don't think that something exists.
410 | Gone -- The something you requested has been removed from our servers.
418 | I'm a teapot.
429 | Too Many Requests -- You're asking too much from us and hitting the rate limit! Slow down!
500 | Internal Server Error -- We had a problem with our server. Try again later.
503 | Service Unavailable -- We're temporarily offline for maintenance. Please try again later.
