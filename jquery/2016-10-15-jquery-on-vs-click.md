# What is the difference between .click and .on in jQuery?

다음 두 방식은 똑같이 동작한다.

1. .click을 활용
```javascript
$('#dataTable tbody tr').click(function() {
  alert($(this).text());
});
```
2. .on을 활용
```javascript
$('#dataTable tbody tr').on("click", function() {
  alert($(this).text());
});
```

jQuery 공식 문서에 따르면 `.click()`은 `.on("click", handler)`의 shorthand라고 한다. 이 방식은 selector와 매칭되는 모든 element에 각각의 handler를 생성한다 (element가 3개면 handler도 3개).

이 방식은 동적으로 새로 추가되는 element에 handler를 생성해주지는 않아서 직접 rebind해주어야 한다.

하지만 다음과 같은 방식은 다르게 동작한다.
```javascript
$( "#dataTable tbody" ).on( "click", "tr", function() {
  alert( $( this ).text() );
}); 
```

이 방식은 child element의 event가 상위 element로 propagate하도록 하는데, 이를 **event bubbling (= event propagation)**이라고 한다. 상위 element에서 event bubbling을 이용해 event를 처리하는 것을 **event delegation**이라고 한다.

상위 element에서 event를 처리하기 때문에 이전 방식과 달리 하나의 handler로 처리하고 동적으로 생성된 child element들도 알아서 처리해준다.

하지만 이 방식은 event가 발생할 때마다 selector와 해당 selector가 가리키는 DOM 하위의 모든 element를 비교하므로 selector를 target element와 최대한 가깝게 설정해야한다. `document` 혹은 `document.body`를 쓰는 것을 지양해야 한다.


## References
<http://stackoverflow.com/questions/9122078/difference-between-onclick-vs-click>  
<https://www.quora.com/What-is-the-difference-between-click-and-on-click-in-jQuery>  
<https://api.jquery.com/click/>