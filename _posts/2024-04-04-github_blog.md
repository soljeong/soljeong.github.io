---
layout: post
title: 옵시디언에서 깃헙블로그로 푸시
aliases: 
published: true
---

## 문제

- 옵시디언을 깃헙 프라이빗 레포에 백업
- 블로그는 퍼블릭 레포지토리에
- 옵시디언에서 마크다운을 작성하고 옵시디언 레포지토리에 푸시하는 것만으로 블로그에 포스팅되면 좋겠다
- 한쪽 레포의 특정 서브디렉토리의 내용을 다른 레포의 특정 서브디렉토리와 동기화해야한다. 변경은 한 방향으로만 일어난다

## 시도: git submodule

- 하나의 레포가 다른 레포의 하위디렉토리가 되게 할 수 있다
- 하위디렉토리는 거기에서만 커밋할 수 있다. 상위 레포(슈퍼프로젝트)에서 하위 레포를 커밋하는건 불가
- 이 경우에는 양쪽 레포에서 모두 서브디렉토리이기 때문에, 옵시디언에서 상위 디렉토리를 열고 있기 때문에 불가

## 시도: git hooks

- local에 두개의 레포를 두고 로컬에서 스크립트를 실행하는 방법
- 옵시디언 레포를 pc에서만 작성하는 경우라면 이 방법도 가능
- 하지만, 안드로이드에서도 편집하기 때문에, 양쪽 모두에 블로그 레포지토리를 클론하고 양쪽 모두에 훅을 위한 스크립트를 서로 다른 언어로 작성하는건 너무 심난해서 포기
- wsl이 있으니까 꼭 두가지 언어로 작성해야하는 건 아닐수도 있겠지만, 아무튼 블로그 레포는 굳이 안드로이드에까지 있을 필요가 없으니까 왠지 싫어서 기각

## 시도: github action

- 그 전에도 시도했던 방법인데 계속 fail 떠서 방치하고 있었다.
```yaml
on:
  push:
    branches:
      - main
    paths:
      - '_posts/**'

jobs:
  push:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v4
      with:
        fetch-depth: 0
        
    - name: Merge main branch
      run: |
        git remote add destination https://${{ secrets.ACCESS_TOKEN }}@github.com/soljeong/soljeong.github.io
        git switch main
        git merge origin/main
        git push destination main

```

오늘 보니까 말도 안되는 시도였다. 옵시디언 레포와 블로그 레포는 하나의 서브디렉토리만 공통점으로 가지는거고 나머지는 서로 전혀 상관없는데, 이쪽에서 만든 커밋을 저쪽에 통채로 푸시해도 당연히 될 리가 없지. 애당초 트리거만 서브디렉토리로 지정되어있을 뿐이고, 커밋 하나에 타겟 디렉토리 밖의 변경도 포함되어 있을 수 있는데. 왜 여태 몰랐는지 어이가 없네. 

## 해결: github action

- 깃허브액션은 스크립트 실행을 위해 새 인스턴스를 만드는것 같다
- 그 인스턴스에 작업중인 레포의 파일 내용이 들어있다
- 그 인스턴스에 타겟 레포(블로그 레포)도 전부 클론해서, 동기화하고싶은 서브디렉토리를 오버라이트한다
- 타겟 레포에 커밋한다
- 이쪽에서 만든 커밋을 저쪽에 그대로 푸시한다는게 얼마나 어처구니없는 짓이었는지

## 지피티가 새로 작성한 스크립트

기존 설명한 스크립트를 수정하여, `_posts` 디렉토리의 내용을 타겟 레포지토리의 `_posts` 디렉토리에 오버라이트하고, 해당 변경사항을 타겟 레포지토리에서 커밋 및 푸시하는 작업을 자동화하는 방법은 다음과 같습니다. 이 과정에서는 변경사항을 타겟 레포지토리에 복사하고, 커밋하며, 푸시하는 단계로 구성됩니다.

```yaml
name: Sync Posts to Public Repository

on:
  push:
    branches:
      - main
    paths:
      - '_posts/**'

jobs:
  sync:
    runs-on: ubuntu-latest
    steps:
    - name: Checkout source repository
      uses: actions/checkout@v4
      with:
        fetch-depth: 0
        
    - name: Setup Git config
      run: |
        git config --global user.email "jeongsol@outlook.coom"
        git config --global user.name "githubaction"
    
    - name: Clone target repository
      run: |
        git clone https://${{ secrets.ACCESS_TOKEN }}@github.com/soljeong/soljeong.github.io ../target_repo

    - name: Copy _posts to target repository
      run: |
        rm -rf ../target_repo/_posts/*
        cp -r _posts/* ../target_repo/_posts/
        
    - name: Commit and push changes to target repository
      run: |
        cd ../target_repo
        git add _posts
        git commit -m "Sync _posts directory from source to target repository"
        git push origin main
```

### 설명:

- **Checkout source repository**: 원본 레포지토리에서 코드를 체크아웃합니다.

- **Setup Git config**: Git 사용자 이름과 이메일을 설정합니다. 이는 커밋을 만들 때 사용됩니다.

- **Clone target repository**: 타겟 레포지토리를 현재 워크플로우가 실행되는 가상 환경에 복제합니다. 여기서는 타겟 레포지토리를 `target_repo`라는 디렉토리로 복제합니다.

- **Copy _posts to target repository**: 원본 레포지토리의 `_posts` 디렉토리 내용을 타겟 레포지토리의 `_posts` 디렉토리에 복사합니다. 먼저 타겟 디렉토리 내의 기존 파일을 삭제하고, 새로운 파일을 복사합니다.

- **Commit and push changes to target repository**: 변경사항을 타겟 레포지토리에 커밋하고 푸시합니다. 이 단계에서는 타겟 레포지토리 디렉토리로 이동(`cd`)한 후, `_posts` 디렉토리의 변경사항을 스테이징(`git add`), 커밋(`git commit`), 그리고 푸시(`git push`)합니다.

이 스크립트는 `_posts` 디렉토리의 변경사항이 원본 레포지토리의 `main` 브랜치에 푸시될 때마다 자동으로 실행되어, 해당 변경사항을 타겟 레포지토리의 `_posts` 디렉토리에 반영합니다.

