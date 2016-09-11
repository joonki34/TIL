
# Web API Design Summary

<https://pages.apigee.com/rs/apigee/images/api-design-ebook-2012-03.pdf>

## Nouns are good; verbs are bad
리소스마다 두개의 base url만 있어야 한다. 하나는 컬렉션을 위한 것이고, 나머지 하나는 컬렉션의 특정 element를 위한 것이다.
e.g.
/dogs
/dogs/1234

리소스 이름엔 명사를 써라. 동사는 HTTP method (POST, GET, PUT, DELETE) 뿐이다.

## Plural nouns and concrete names
단수명사보다는 복수명사를 써라. (e.g. dogs, checkins)
모호한 이름은 쓰지마라.

## Simplify association - sweep complexity under the ‘?'
리소스 간의 관계는 이런 식으로 표현하라.  
/owners/5678/dogs -> 5678번 소유주의 강아지들  
이 관계는 복잡해질 수 있다. (e.g. /owners/5678/dogs/123/foods/324)  
하지만 한 리소스의 id를 가지고 있다면 그 상위 레벨은 필요하지않다. 그러므로 URL은 /resource/identifier/resource  보다 깊어지면 안된다.

추가적인 파라미터, 속성값 등은 ? 뒤로 몰아버려라. (e.g. /dogs?color=red&state=running&location=park)

## Handling errors
API 사용자는 70개가 넘는 HTTP status code를 다 외우고 다니지 않는다.  
##### 정말 간단하게는 3가지 상태가 필요하다.  
* 200: OK
* 400: Bad Request
* 500: Internal Server Error

##### 너무 적다고 생각하면 5개를 추가로 더 사용하라.  
* 201: Created
* 304: Not Modified
* 404: Not Found
* 401: Unauthorized
* 403: Forbidden

가능하면 response에 어떤 것이 에러를 발생했는지 자세하게 써주는 것이 좋다. 에러를 설명해주는 페이지의 링크를 같이 넣어서 주는 것을 추천한다.
e.g.
```
{"developerMessage" : "Verbose, plain language description of the problem for the app developer with hints about how to fix it.", "userMessage":"Pass this message on to the app user if needed.”, "errorCode" : 12345, "more info”: "http://dev.teachdogrest.com/errors/12345"}
```

## Tips for versioning
API 버저닝은 필수다. 버전 없이 API를 릴리즈하지마라. URL의 가장 왼쪽에 놓고 v 접두사를 붙여라. (e.g. /v1/dogs)  
버전에 .을 붙이지마라. v1, v2, …, v3 이런 식으로 하라.

## Pagination and partial response
페이징은 limit과 offset을 사용하라. (e.g. /dogs?limit=25&offset=50)  
default 값은 limit=10, offset=0으로 하는 것을 추천한다. limit 값은 데이터양에 따라 다를 수 있다. 양이 많다면 10보다 적게 주는 것이 좋다.

부분 정보는 ? 뒤에 csv 형식으로 넣도록 지원하라. (e.g. /dogs?fields=name,color,location)  
위의 예와 같은 경우 이름, 색깔, 지역 정보만 전달해주면 된다.

## What about responses that don’t involve resources?
resource와 관련 없는 API라면 동사를 쓰는 것이 좋다.  
e.g.  
/convert?from=EUR&to=CNY&amount=100 => 100 유로를 위안으로 변환  
또한, 문서에 resource와 관련이 없다는 사실을 분명히 기입하라.

## Supporting multiple formats
리소스를 여러가지 포맷으로 전달하도록 지원하는 것을 추천한다. (e.g. json, xml, etc.)  
다음과 같은 방식으로 지원하는 것을 추천한다.  
dogs.json  
/dongs/1234.json

기본 포맷은 JSON으로 하는 것을 추천한다. xml보다 간결하고 널리 쓰이며 보통 API는 자바스크립트에서 호출하는 경우가 많다.

## What about attribute names?
리소스 속성 이름은 CamelCase로 하는 것이 좋다. 그 이유는 다른 방식은 응답을 파싱해서 쓸 경우 자바스크립트 컨벤션에 맞지 않기 때문이다.  
e.g.  
var myObject = JSON.parse(response);  
timing = myObject.createdAt; (O)  
timing = myObject.created_at; (X)  
timing = myObject.DateTime; (X)  
그러므로 CamelCase으로 해야 자바스크립트 개발자가 쓰기에 더 좋다.

## Tips for search
다음과 같은 방식으로 하라.  
전역 검색  
/search?q=fluffy+fur  
리소스 내 검색  
/owners/5678/dogs?q=fluffy+fur  

Consolidate API requests in one sub domain  
페이스북의 경우 api를 두가지 제공한다.  
graph.facebook.com  
api.facebook.com  

이렇게 하지말고 서브도메인은 딱 하나만 제공하라. (e.g. api.example.com)

## Tips for handling exceptional behavior
어도비 플래시의 경우 HTTP 응답이 200 OK가 아니라면 Flash container가 응답을 모두 가로채고 사용자에게 플래시 에러 코드를 보여준다. 이렇게 되면 앱 개발자가 에러코드를 처리할 방법이 없어진다.
따라서 `suppress_response_codes`라는 optional 파라미터를 지원하는 것이 좋다. 이 값이 true면 응답 코드는 언제나 200이다. 대신 에러 응답 메세지는 상세히 보여주도록 한다. 또한 응답 메세지에 응답 코드 정보도 같이 넣어주어야 한다.  
e.g.  
URL 주소  
/dogs?suppress_response_codes = true

응답 코드  
200 - OK

응답 메세지  
```
{response_code" : 401, "message" : "Verbose, plain language description of the problem with hints about how to fix it.”, "more_info" : "http://dev.tecachdogrest.com/errors/12345”, "code" : 12345}
```

## Authentication
OAuth 2.0을 써라. OAuth는 반드시 스펙대로 구현하라. 그래야 개발자가 기존의 OAuth 라이브러리를 그대로 사용할 수 있다.

