# How to display errors in PHP

PHP에서 에러 메세지 표시하고 싶으면 파일에 다음과 같이 넣어주면 된다.
```php
ini_set('display_errors', 1);  
ini_set('display_startup_errors', 1);  
error_reporting(E_ALL);  
```

parse error를 표시하려면 php.ini에 다음과 같이 넣어주면 된다.
```
display_errors = on
```

## Reference
<http://stackoverflow.com/questions/1053424/how-do-i-get-php-errors-to-display>