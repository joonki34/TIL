# CommonJS vs. AMD vs. RequireJS

(주의) Specification을 명세라고 번역했다.

## CommonJS

CommonJS는 `exports` 객체를 이용해 module을 정의하고 사용할 수 있게 해주는 모듈 명세이다. Node.js에서 CommonJS 명세를 이용해 모듈을 구현하고 있다.

## AMD

CommonJS 명세는 모듈을 동기적으로 로드한다. 때문에 클라이언트(대표적으로 브라우저) 사이드에서는 CommonJS를 사용하기 힘들었고 결과적으로 AMD(Asynchronous Module Definition) 명세가 탄생하게 된다. AMD는 모듈을 비동기적으로 로드한다.

## RequireJS

RequireJS는 AMD 명세의 구현체이고, 구현체 중 가장 유명하다.

## UMD

CommonJS와 AMD 모두 유명해서 아직은 공통 규악이 없다. 때문에 두 규약 모두에 compatible한 패턴이 필요했고 그것이 바로 UMD(Universal Module Definition)이다.

## 모듈 처리 방식

이 섹션은 학습을 위해 네이버의 [기술 블로그](http://d2.naver.com/helloworld/2809766)에서 그대로 가져왔습니다. 문제가 되면 즉시 삭제하겠습니다.

### CommonJS의 모듈 처리

CommonJS는 불러온(import) 모듈에 있는 JavaScript 코드 문자열의 평가(evaluate)를 위해 코드를 VM(virtual machine)으로 전달하기 전에 모듈을 함수로 감싼다.

다음은 CommonJS가 불러온 모듈 코드의 예다.

```javascript
const m = 1;  
module.exports.m = m;
```

Node.js는 실제로는 위의 모듈을 처리할 때 다음과 같이 함수로 래핑한다.

```javascript
function (exports, require, module, __filename, __dirname) {  
  const m = 1;
  module.exports.m = m;
}
```

이후 Node.js는 JavaScript를 실행하는 시점에 위의 래핑된 함수를 평가한다. 함수에 전달되는 파라미터는 Node.js에서 사용하는 전역 요소로 보이지만 실제로는 해당 함수가 실행될 때 Node.js에서 전달되는 함수의 파라미터 값이다. 래핑된 함수는 근본적으로는 팩토리 메서드라 할 수 있다.

여기서 중요한 점은 어떤 심벌이 내보내질지(export)는 래핑된 함수가 실행되기 전까지는 미리 알 수 없다는 사실이다.


### ESM의 모듈 처리

ESM은 다음과 같은 코드에 대해 파싱 시점(코드를 평가하기 전)에 모듈 레코드(module record)라 부르는 내부 구조를 생성한다.

```javascript
export const m = 1;  
```

모듈 레코드에는 다른 여러 가지 정보와 함께 모듈에서 내보내지는 심벌에 대한 정적 목록을 포함된다. 이 정적 목록은 파서가 export 키워드의 사용을 인지할 수 있도록 한다. 모듈 레코드에서 심벌은 아직 존재하지 않는 요소를 가리키는 포인터와 같다고 보면 된다.

ESM에서는 어떤 심벌이 내보내지는지가 평가되기 전에 결정되기 때문에 ESM의 export 키워드는 렉시컬하게 정의된다고 할 수 있다.

문제는 다음과 같이 CommonJS 모듈을 불러올 때다. 이미 살펴본 것과 같이 CommonJS에서는 'm' 요소가 내보내진 모듈인지는 평가된 이후에 알 수 있기 때문에 현재 정의된 ESM 표준에서는 다음과 같이 모듈을 불러오는 것은 불가능하다.

```javascript
import {m} from "foo";  
```

## References
<http://davidbcalhoun.com/2014/what-is-amd-commonjs-and-umd/>
<http://stackoverflow.com/questions/16521471/relation-between-commonjs-amd-and-requirejs>
<http://d2.naver.com/helloworld/2809766>