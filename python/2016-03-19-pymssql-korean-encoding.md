# pymssql에서 한글 깨지는 문제

## Problem
conn = pymssql.connect(host='192.168.11.101', user='user', password='password', database='db',charset='utf8')
위와 같이 데이터베이스 연결하여 데이터 가져오는데 다음과 같은 에러가 발생함.
pymssql.OperationalError 20017 'DB-Lib error message 20017, Unexpected EOF from the server

## Solution
이 현상은 한글(euc-kr / cp949)이 ISO-8859-1로 인코딩 되어서 그런것이다.
이것을 euc-kr로 변환 했다가 다시 utf-8로 변환해서 보여주면 된다.

```python
t = row["fieldname"]
t = t.encode('ISO-8859-1')
t=t.decode('euc-kr')
print(t)
```

## References
<http://wooorazil.blogspot.kr/2015/07/pymssql-pymssqloperationalerror-20017.html>