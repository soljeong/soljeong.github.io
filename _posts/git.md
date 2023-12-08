## drop
- 중간 커밋을 드롭하면, 드롭만 제외하고 똑같은 가지가 두개 생긴다.
- 가지마다 끝에는 브랜치 명이 붙어있다.
- 가지 끝에 있는 브랜치를 삭제하면 그 가지의 커밋들은 사라진다.
- 멀쩡한 방법은 아닌것 같은데
## head
- 해당 브랜치의 최종 커밋
## rebase
- 하나의 커밋에 두개의 파일을 수정했다. 이걸 두개의 커밋으로 쪼개고 싶은데
- https://wikidocs.net/153961
- https://wbluke.tistory.com/26
- 뭐가 어떻게 된건지 솔직히 잘 모르겠다
- 해당 브랜치의 가지를 잘라서 다른데 붙인다.
- 타겟은 다른 커밋이 아니고 다른 브랜치


If you work with multiple different identities on a single Git hosting service, you may be wondering if Git Credential Manager (GCM) supports this workflow. The answer is yes, with a bit of complexity due to how it interoperates with Git.

```bash
git config --get credential.helper
```
- saved credential 어디에서 찾을 수 있을지 모르겠네 저장하는 방법이 여러가지
- ssh를 한번 사용한 적 있었는데 그게 언제 어디서였는지 모르겠네
-  Git의 Credential-Helper 시스템의 기본 명령은 `git credential` 이다.
- [Git - Credential 저장소 (git-scm.com)](https://git-scm.com/book/ko/v2/Git-%EB%8F%84%EA%B5%AC-Credential-%EC%A0%80%EC%9E%A5%EC%86%8C)

## 모르겠어


# 깃
- 삭제했다가 다시 설치?
- git credential manager 여기가 문제인데 엉뚱한걸 재설치했나? 아니면 포함되어있나?
- 재설치 괜히했어 젠장
- [windows - Remove credentials from Git - Stack Overflow](https://stackoverflow.com/questions/15381198/remove-credentials-from-git)
- 헐 윈도우에 저장되어 있는거였나?
> 성공


## 하위 폴더를 새 리포지토리로 분할
- https://docs.github.com/ko/get-started/using-git/splitting-a-subfolder-out-into-a-new-repository
- git-filter-repo 설치
- path에 넣으라길래, 깃 설치된 폴더에 넣음
- https://blog.syki66.com/2020/09/12/git-subtree/
- git subtree split -P 서브디렉토리이름 -b 브랜치이름
# https://scoop.sh/
- 윈도우용 패키지 매니저
- winget 대신