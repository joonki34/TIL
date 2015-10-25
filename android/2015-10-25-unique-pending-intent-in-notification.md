# Unique pending intent in notification

## Problem
특정 url로 이동하는 pending intent를 가진 notification이 있는데 2개 이상 띄울 경우 모든 notification이 가장 최근에 온 notification의 pending intent로 통일되는 현상이 발생

## Solution
PendingIntent의 requestCode를 unique하게 부여하면 해결 가능.

## Reference
<http://stackoverflow.com/a/6001960>