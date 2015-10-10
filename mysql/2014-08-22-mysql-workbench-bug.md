# MySQL Workbench Bug
테이블 만들 때 DB engine 고르는 란에 default로 innoDB가 선택되어 있길래 그냥 안 건드리고 테이블 만듬.

근데 만들고보니 MyISAM으로 되어있음. (??)
다시 테이블 만들 때 engine 고르는 란에서 innoDB를 선택하고 만드니까 innoDB로 만들어짐.

DB 서버 기본 엔진이 MyISAM이라 workbench에서 초기값이 innoDB로 되있어도 그냥 MyISAM으로 만드는 것 같음.

innoDB로 만드려면 꼭 기본값이 innoDB로 되어 있어도 선택을 해줘야할 듯.
workbench 버그 같음.