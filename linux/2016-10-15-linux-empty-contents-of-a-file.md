# Empty contents of a file

로그 파일 등 특정 파일의 내용을 지우고 싶은 경우 이 command를 실행하면 된다.
```
cat /dev/null > logfile
```

`/dev/null`은 Null device이고 이 device에 쓰여진 data는 다 지워진다. 그리고 읽으려고 하면 빈 파일처럼 보이게 된다. 위의 command는 이를 이용해 파일을 비운다.

## References
<http://unix.stackexchange.com/questions/92384/how-to-clean-log-file>
<https://en.wikipedia.org/wiki/Null_device>