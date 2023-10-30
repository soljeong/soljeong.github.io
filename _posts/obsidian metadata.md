---
layout: post
title: 
aliases: 
published: false
---
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
