# 4. Status Codes

Success: 200 OK, 201 Created (Location header), 204 No Content.

Client errors: 400 Bad Request, 401 Unauthorized, 403 Forbidden, 404 Not Found, 409 Conflict, 422 Validation Error.

Server: 500 Internal Server Error (log internally, minimal response outward).

Stay consistent; avoid inventing non-standard codes.
