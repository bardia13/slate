# Errors

The Rondino API uses the following error codes:


Error Code | Meaning
---------- | -------
400 | Bad Request -- Your request is invalid.
401 | Unauthorized -- Your Rondino_Id is wrong.
403 | Forbidden -- The Transaction requested is Forbidden for you.
404 | Not Found -- The Transaction could not be found.
405 | Method Not Allowed 
406 | Not Acceptable -- You requested a format that isn't json.
410 | Removed -- The transaction requested has been removed from our servers.
429 | Too Many Requests
500 | Internal Server Error -- We had a problem with our server. Try again later.
503 | Service Unavailable -- We're temporarily offline for maintenance. Please try again later.
