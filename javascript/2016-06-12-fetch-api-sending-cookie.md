# Fetch API sending cookie
## Problem
fetch API를 써서 서버와 통신하는데 cookie 전송이 안됨
## Solution
fetch()에서는 default로 cookie를 보내지 않아서 보내려면 옵션을 줘야한다. 방법은 다음과 같다.
```javascript
fetch('/users', {
  credentials: 'same-origin'
})
```
CORS 요청을 보내려면 값을 include로 주면 된다.
## References
<https://github.com/github/fetch>
<https://developers.google.com/web/updates/2015/03/introduction-to-fetch?hl=en>