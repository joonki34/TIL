# What is DOCTYPE

`<!DOCTYPE>`은 HTML tag가 아니다. DOCTYPE은 브라우저에게 해당 페이지가 어떤 HTML 버전으로 작성되어있는지 알려주는 역할을 한다. 브라우저는 DOCTYPE에 따라 브라우저를 다르게 렌더링한다.

HTML5 DOCTYPE은 `<!DOCTYPE html>`이고 HTML 4.01 DOCTYPE은 `<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN"
   "http://www.w3.org/TR/html4/strict.dtd">`이다. (strict DTD 기준)
   
DOCTYPE은 필수로 명시되어야 한다. 만약 해당 페이지에 DOCTYPE이 없다면 브라우저는 페이지를 quirks mode로 렌더링한다. Quirks mode는 IE 5 혹은 그 이하 버전의 backward compatibility를 위한 모드이고 오래된 브라우저의 behavior를 emulate하려고 한다. 이 때문에 불필요한 CSS 규칙이 추가될 수 있다.

## References
<http://stackoverflow.com/questions/6076432/why-do-i-need-a-doctype-what-does-it-do>
<https://en.wikipedia.org/wiki/Quirks_mode#Comparison_of_document_types>