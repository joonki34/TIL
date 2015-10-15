# Get last inserted id in mybatis
## How to
mybatis를 이용한 insert 메소드를 통해 last inserted id로 받는 것에는 두가지 방법이 있다.

### 1. selectKey 이용
```
<insert id="insertWidget" parameterType="com.myapp.Widget">
    INSERT INTO
        myapp.widgets
        (
            widget_name
        )
        VALUES
        (
            #{widget.name}
        );

    <selectKey keyProperty="id" resultType="int" order="AFTER">
        SELECT CURRVAL('widget_id_seq') AS widget_id
    </selectKey>
</insert>
```
### 2. useGeneratedKeys 이용
```
<insert id="createEmpty" parameterType="Project" useGeneratedKeys="true" keyProperty="project.projectId" keyColumn="PROJECT_ID">
    INSERT INTO PROJECT (TITLE,DESCRIPTION)
    VALUES
    (#{title},#{description})
</insert>
```
여기서 keyColumn은 primary key가 테이블의 첫 column이 아닌 특정 db에서만 필요하다. (e.g. PostgreSQL)

## selectKey vs. useGeneratedKeys
차이는 정확히는 모르겠는데 selectKey가 더 다용도로 쓰이는 기능인 듯하다. selectKey를 쓰면 굳이 last inserted id가 아니더라도 다른 값을 받아올 수 있다 (e.g. last inserted id + 1).

## 주의사항
이와 같은 방식을 사용하면 last inserted id가 parameter로 사용한 POJO 혹은 map의 property/key 값에 반영된다. **insert 메소드의 리턴값에 반영되는 것이 아니다.**

## References
<https://mybatis.github.io/mybatis-3/sqlmap-xml.html>
<http://stackoverflow.com/a/12106243>
<http://stackoverflow.com/a/18523424>