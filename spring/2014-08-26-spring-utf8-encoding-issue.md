# UTF-8 Encoding

스프링 프로젝트 처음 실행해보면 한글이 깨진다. web.xml에 다음과 같이 추가해줘야 한다:
```
<jsp-config>
<jsp-property-group>
<url-pattern>*.jsp</url-pattern>
<page-encoding>UTF-8</page-encoding>
<trim-directive-whitespaces>true</trim-directive-whitespaces>
</jsp-property-group>
</jsp-config>

<filter>
<filter-name>encodingFilter</filter-name>
<filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
<init-param>
<param-name>encoding</param-name>
<param-value>UTF-8</param-value>
</init-param>
</filter>

<filter-mapping>
<filter-name>encodingFilter</filter-name>
<url-pattern>/*</url-pattern>
</filter-mapping>
```

\<jsp-config\>는 jsp 페이지 인코딩해주고 필터는 servlet response를 인코딩해준다. 보통 필터만 적용하면 잘 되지만 Apache Tiles나 Sitemesh를 쓰면 layout 페이지가 servlet 안 거쳐가는 건지 layout 페이지 한글이 깨지는 현상이 발생한다. 그래서 \<jsp-config\> 넣어서 모든 jsp를 인코딩해주도록 해야 한다.