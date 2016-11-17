# `<b>` vs. `<strong>`

두 태그는 브라우저에서 똑같이 텍스트를 볼드 처리한다. 하지만 근본적인 차이가 존재한다. 

우선, Bold는 스타일이다. 그래서 시각장애인에게는 아무런 의미가 없고 모바일 기기에서는 화면이 작기 때문에 텍스트가 이미 볼드 처리되어있는 경우가 많다.

`<b>`는 앞에서 말한 스타일과 함께 해당 텍스트는 강조되어야 한다는 의미를 갖고 있다. 반면에 `<strong>`은 강조 의미를 뜻하고 스타일에는 관여하지 않는다. 이를 semantic하다고도 한다. HTML은 스타일을 다루지 않는 것이 옳기 때문에 `<b>`보다는 `<strong>`이 더 HTML 표준(semantic web)에 부합한다고 볼 수 있다. `<i>`와 `<em>`의 차이도 마찬가지다. 

`<section>`이나 `<article>` 등도 semactic web에 맞는 태그들이다.

하지만 `<b>`와 `<i>`가 deprecated된 것은 아니다.

*Note that `<b>` and `<i>` have not been deprecated in HTML5. Their new use is to semantically represent styles (or intended presentation), whereas `<strong>` and `<em>` represent structure.* ([참고](http://stellify.net/html5-b-and-i-tags-are-going-to-be-useful-read-semantic-again/))

위 소스에 따르면 `<b>`는 강조 의미없이 볼드를 표현하는 역할을 하고 `<i>`는 이텔릭을 표현하는 역할을 하도록 의미(semantic)가 바뀌었다고 한다.

## References
<http://stackoverflow.com/questions/271743/whats-the-difference-between-b-and-strong-i-and-em>  
<http://stellify.net/html5-b-and-i-tags-are-going-to-be-useful-read-semantic-again/>