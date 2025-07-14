## Description
Same idea like [[root.hacking.smuggling.cl-te]], but the other way around. Frontend uses `Transfer-Encoding`, while backend uses `Content-Length`. Difference is exploited to leave data unprocessed during handling the attack request:
```
POST / HTTP/1.1
Host: vulnerable-website.com
Content-Length: 3
Transfer-Encoding: chunked 

8 
SMUGGLED
0
```
The frontend uses `Transfer-Encoding`, so it will forward the entire thing and stop at 0, but backend will use the `Content-Length: 3` so anything after 8 will be ignored by the request handler.