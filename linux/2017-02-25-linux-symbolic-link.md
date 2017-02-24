# Symbolic link with directory

## Problem

A 디렉토리(target)를 B 디렉토리(source)에 symbolic link 걸고 싶어서 `ln -sfn B A`를 했는데 자꾸 B가 아닌 B/A에 symbolic link가 생성되었다.

## Solution

B/A에 걸린 이유는 내가 이전에 B 디렉토리를 미리 생성해놨기 때문이다. B 디렉토리를 지우고 symbolic link를 걸면 잘 동작한다. 왜 그런지 이유는 찾지 못했지만 실제 디렉토리가 존재하기 때문에 하위에 symbolic link를 거는 듯하다.

참고로 디렉토리에 걸린 symbolic 링크를 변경할 때는 `-f` 옵션과 더불어 `-n` 옵션을 사용하는 것이 좋다.

## References
<http://superuser.com/questions/81164/why-create-a-link-like-this-ln-nsf>