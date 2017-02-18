# How to check ssh connection logs

## Problem

A 서버에서 B 서버에 SSH 접속을 했을때 그 접속이 잘 이루어졌는지 확인해보고 싶다.

## Solution

linux 로그는 `/var/log`에 저장되고 SSH 접속 로그는 `/var/log/secure`에 있다. 따라서 B 서버에서 해당 로그를 확인해보면 된다.

## References

<http://www.thegeekstuff.com/2011/08/linux-var-log-files>