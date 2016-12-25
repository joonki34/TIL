# Webfont with IE and Cache-Control

## Problem

IE에서 SSL(https) 커넥션을 통해 웹폰트(`@font-face`)를 사용하고 HTTP header에 `pragma=no-cache` 설정을 해놓을 경우 브라우저에서 웹폰트 로드에 실패하는 현상이 생김

## Solution

HTTP header에서 `pragma=no-cache`를 포함시키지 않으면 정상동작하게 된다.

## References
<https://github.com/FortAwesome/Font-Awesome/issues/6454>
<https://connect.microsoft.com/IE/feedbackdetail/view/992569/font-face-not-working-with-internet-explorer-and-http-header-pragma-no-cache>