---
layout: post
title: 레포 쪼개기
aliases: 
published: true
---
## 레포의 하위 파일을 새 레포로 분할
프라이빗 레포에서 작업하다가 그 중 일부 파일만 별개의 레포로 만들어서 공개하고 싶다. 새 레포를 만들고 파일을 복사해버리면 되지만, 이러면 새 파일을 만드는 게 되니까 커밋 히스토리가 없어진다. 레포 전체를 복사해서 공개하고 나머지 비공개 파일을 삭제하는 커밋을 한다면, 커밋 히스토리에 공개하고 싶지 않은 파일이 들어가게 된다. 이미 한창 커밋한 레포에서 일부분만 새 레포로 재구성해야한다.

### 과정
- https://docs.github.com/ko/get-started/using-git/splitting-a-subfolder-out-into-a-new-repository
- 레포를 새로 클론. git에서 'fresh'한 클론을 요구한다.
- 원하는 파일만 남기고 삭제. 파일시스템에서 삭제하는게 아니고 관련된 깃 히스토리를 삭제하는 것
- 이렇게 재구성된 레포를 퍼블릭인 새 리모트 레포에 푸시
### git filter-repo
#### 설치
- git-filter-repo는 파이썬으로 작성된 코드
- git 명령어로 이걸 실행시킨다
- pip install, global에 설치한다. git이 실행시킬 수 있도록
```
pip install git-filter-repo
```
#### --path
- 이것만 남긴다
```
git filter-repo --path filename.txt
```
#### --invert-paths --path
- 이것만 뺴고 나머지를 남긴다
#### --path-rename
- 파일 이름 변경
- 파일시스템에서 이름 변경하면 이전 파일 지우고 새 파일 만든걸로 처리된다
```
git filter-repo --path-rename filename1.txt:filename2.txt
```
