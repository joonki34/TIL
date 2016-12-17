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

## References
<http://davidbcalhoun.com/2014/what-is-amd-commonjs-and-umd/>
<http://stackoverflow.com/questions/16521471/relation-between-commonjs-amd-and-requirejs>