# Link module project in Android Studio

## Problem
안드로이드 스튜디오에서 로컬에 있는 라이브러리 프로젝트를 사용하는 경우가 있다. 이 때 보통 하는 것이 import module인데 이럴 경우, 해당 모듈이 현재 프로젝트 내부로 복사된 채로 들어오게 되어 기존 라이브러리 프로젝트와의 연결이 끊긴다. 그래서 프로젝트 내부의 모듈을 수정해도 기존 라이브러리 프로젝트는 수정이 안된다. 이런 방식이 아니라 이클립스처럼 기존 프로젝트를 참조하는 형태로 모듈을 가져오고 싶다.

## Solution
1. settings.gradle 파일을 연다.
2. 연결하고자 하는 프로젝트와 프로젝트의 경로를 지정한다.
```
include ':Selphone_API'  
project(':Selphone_API').projectDir=new File('/Users/TedPark/Dropbox/AndroidStudio/selphone/Selphone_API')
```

## References
<http://gun0912.tistory.com/15>