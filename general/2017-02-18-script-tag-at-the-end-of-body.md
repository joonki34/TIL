# Why is it good to put `<script>` tag at the end of body tag

## Question

브라우저는 html을 파싱하다가 `<script>` 태그를 만나면 `<script>` 태그가 DOM을 조작할 수 있기 때문에 파싱을 멈추고 `<script>`를 다운로드 및 실행한 다음, 다시 파싱을 재개한다.

`<script>` 파일은 html에서 `<body>` 태그 맨 끝에 놓는게 좋다고 한다. `<script>`가 실행되기 전에 DOM를 먼저 보여줄 수 있다는 이유에서다. 하지만 브라우저가 `<script>` 태그를 만난 즉시 파싱을 멈춘다면 어떻게 DOM을 먼저 보여줄 수 있을까? 결국 `<script>` 태그의 위치는 아무 의미가 없는 것이 아닌가?

## Answer

브라우저는 `<script>` 태그가 다운로드되는 동안에 이전까지 파싱된 DOM의 일부를 표시한다. 그래서 `<script>` 태그의 위치는 의미가 있다.

현대 브라우저는 `async` 혹은 `defer` 옵션을 제공하기 때문에 태그 위치를 이용한 old trick보다는 현대적인 방법을 쓰는 것이 바람직하다.

## References
<http://stackoverflow.com/questions/17140386/why-is-it-good-to-put-script-tag-in-the-end-of-body-tag>