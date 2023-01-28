# HTML

## HTTP Request Methods

* GET
* POST
* PUT
* HEAD
* OPTIONS
* DELETE
* OPTIONS
* TRACE
* CONNECT
* PATCH

### Safe Methods

Safe Methods are considered safe to use
because they only fetch information and do not
cause changes on the server. The Safe Methods are: GET, HEAD, OPTIONS,
and TRACE

### Idempotent Methods

Idempotence - A quality of an action such that
repetitions of the action have no further effect on the
outcome. PUT and DELETE are Idempotent Methods . Safe Methods (GET, HEAD, TRACE, OPTIONS) are
also Idempotent

## HTTP Status Code

* 100s : Are informational in nature
* 200s : Denote success
* 300s : Redirections
* 400s : Client error
* 500s : Server error

### Common Status Codes

* 200 : ok
* 201 : created
* 204 : accepted
* 301 : moved permanently
* 400 : Bad Request.
* 401 : Unauthorized  
* 404 : Resource not found
* 500 : Internal service error
* 503 : Server unavialable
