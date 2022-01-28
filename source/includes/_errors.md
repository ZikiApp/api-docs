# Errors

The Ziki API uses the following error codes:

Error Code | Meaning
---------- | -------
400 |  Bad Request -- Your request is invalid.
401 | Unauthorized -- Your token is wrong.
403 | Forbidden -- The resource requested is not available with your permissions
404 | Not Found -- The specified resource could not be found.
414 | Request URI too long -- You have applied too many filters on a GET request
422 | Unprocessable Entity -- Your request is invalid
429 | Too Many Requests -- You're requesting too many resource! Slow down!
500 | Internal Server Error -- We had a problem with our server. Try again later
503 | Service Unavailable (Time out) -- The server was unable to finish the request in time, either the server was overloaded or if fetching large pages of data, please try using a smaller page size
