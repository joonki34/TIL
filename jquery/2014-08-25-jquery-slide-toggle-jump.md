# jQuery's slideToggle() jumps around

## Problem
특정 element에 slideToggle() 걸어두면 이벤트 끝나기 직전에 element가 위로 튀는 현상이 발생함(jump).

## Solution
바로 위 element(upper child element)에 margin-bottom을 설정해둬서 생기는 현상이었음. margin-bottom을 세팅하지 않으면 정상동작함.