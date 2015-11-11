# Rollback transaction after @Test

## Problem
서비스 관련 단위 테스트들이 서로에게 영향을 준다. 예를 들어, Test 1에서 A를 저장했을 경우 Test 2는 A가 저장된 채로 진행되어서 정상적으로 동작이 안되는 경우가 생긴다.

## Solution
Test 클래스에 *@Transactional* 어노테이션을 추가하면 각 테스트 진행 후에 DB가 롤백된다.

## References
<http://stackoverflow.com/questions/12626502/rollback-transaction-after-test>