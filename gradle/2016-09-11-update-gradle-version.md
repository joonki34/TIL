# Update Gradle Version

두 가지 방법 중 하나로 하면 된다.  
1. 아래의 명령어 실행   
```
./gradlew wrapper --gradle-version 2.12
```  
2. build.gradle 파일에서 custom wrapper task 만들고 해당 task 실행
```
task wrapper(type: Wrapper) {
    gradleVersion = '2.12'
}
```
태스크 만들고 `./gradlew wrapper` 실행

## References
<http://stackoverflow.com/questions/25205113/how-to-change-the-version-of-the-default-gradle-wrapper-in-intellij-idea>