# HTTP status code 200 (cache) vs. 304

## 200 (cache)
HTTP 헤더에 `Cache-Control` 혹은 `Expires` 포함할 경우 브라우저에서 해당 요청 캐시 가능 (`Cache-Control`이 최신 스펙)

## 304
처음에 브라우저가 서버에 요청을 하면, 서버는 응답 헤더에 ETag를 실어보낸다. 그 후에 브라우저가 서버에 `If-None-Match: ETag 값`을 헤더에 담아 해당 요청을 또 하면, 서버는 현재 리소스의 ETag 값과 요청의 ETag 값을 비교하여 같으면 304 Not Modified 응답을 전달한다. 브라우저는 304를 받으면 브라우저 캐시 주기를 갱신하고 캐시를 사용한다. 

`200 (cache)`는 서버에 요청 없이 브라우저가 저장한 캐시를 쓰는 반면에 `304`의 경우 서버에 요청을 해서 현재 브라우저 캐시가 최신 리소스인지 확인하는 과정이 있는 것이 다른 점이다.

## References
<http://stackoverflow.com/questions/1665082/what-is-the-difference-between-http-status-code-200-cache-vs-status-code-304>
<https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/http-caching>