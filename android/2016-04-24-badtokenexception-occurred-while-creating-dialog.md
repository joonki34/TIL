# BadTokenException occurred while creating dialog in toolbar onClickListener

## Problem
Dialog를 toolbar onClickListener에서 만드려는데 BadTokenException이 발생했다.

## Solution
TabActivity로 메뉴가 구현되어있는데 같은 레벨 선상 Activity가 존재하는 구조로 인해 Tab하나에 여러개의 Activity를 연결해야하는 상황이어서 ActivityGroup을 상속받는 부모 Class에서 자식 클래스를 관리하고 있는 상황.

때문에 해당 Dialog를 띄우는 Activity의 주체는 본인이 아니라 실질적 뷰를 관리하고 있는 부모 Activity라는 것이었던 것이다.

new AlertDialog.Builder()를 할때 getParent()함수를 호출해서 부모를 불러줘야 에러가 발생하지 않는다.

아울러 TabActivity는 deprecated되었으므로 fragment를 쓰는 것이 좋다.

## References
<http://nabiko.blogspot.kr/2012/06/activitygroup-activity-dialog.html>