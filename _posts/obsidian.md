---
layout: post
title: 옵시디언 사용
aliases:
  - 옵시디언
published: false
---
![[2023-10-26-Excalidraw]]
- 
- 노트를 뭘 어떻게 할것인지
## plug-in
- publish
- unique note creator
- dataview : use metadata [[obsidian]]
- https://kexplain.com/16

- cmenu : 모바일에서처럼 메뉴 보여주는 플러그인
- 체크박스 만드는게 기본 기능에 안들어있구나
- https://www.youtube.com/watch?v=mUYLE8M5o0I
### checklist
- 전체 문서에서 체크박스 달린 줄만 모아서 볼 수 있다?

## 단축키 설정
- 알트 + 위, 아래 화살표 : 윗줄과 바꾸기
- 슬래시로 컨트롤 피를 대신할 수 있다? 어차피 아직 팔레트를 많이 사용하지 않기도 하고
- 작업 목록으로 변경 : ^0
- 제목 수준 : ^123
- 불릿 목록 : ^\`
- 데일리 노트  : ^Q
## 사용
- 일단 문서 하나 안에서 최대한 적고
- 분할해 나가면서 링크를 남기는 것으로
## 사용하는 기능
- alias
- tag : 아직 뭘 태그로 달아야할지 잘 모르겠다
- title : posting 할 때 필요
- 웹페이지에서 가져오기
	- https://www.youtube.com/watch?v=PucX0AvxwkM
	- cliping
## 필요한 기능
- [ ] 새 파일 열었을 때 템플릿 자동으로 적용될 수 있도록
- [ ] 데일리 노트는 특정 폴더에 만들어지도록
- [ ] 데일리노트에서 문단을 다 작성하고 나서 밖으로 꺼낸다쳐도, 그게 같은 폴더에 생기면 소용없는건데



## 마크다운 문법
- 체크박스 만드는게 기본문법에 있네 뭐
## 백업
- [ ] 폴더를 변경했을 때, 작업환경 유지할 수 있는지?
- 플러그인 새로 전부 설치하고
- 단축키도 다시 설정?

https://kexplain.com/17
https://kexplain.com/21
메타데이터를 뭘 어쩌라는 건지 잘 모르겠음

- https://blacksmithgu.github.io/obsidian-dataview/reference/sources/

```dataview
table
	file.name as "filename"
from #python

```

`=this.file.path`


## 날짜별 파일에, 그날 만든 문서 링크?
- 만들수는 있겠는데 그날 기준으로 고정되는게 아니라서 의미가 있을지 모르겠네
- 물론 만든 날짜는 이후에 변경되지 않으니까 문제가 될건 아니지만
this.file.cday
`=this.file.cday`
```dataview
table
	file.cday as "created date"
where file.cday = this.file.cday
```
