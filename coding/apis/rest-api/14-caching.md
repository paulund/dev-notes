# 14. Caching

Use Cache-Control / ETag on cacheable GET responses.

Clients send If-None-Match; respond 304 when unchanged.

Speeds clients & reduces server load.
