---
layout: post
title: Obsidian + GitHub Actions로 구성한 개인 블로그 자동 배포 구조
aliases:
published: true
tags:
  - obsidian
  - github
  - blog
---

# Obsidian + GitHub Actions로 구성한 개인 블로그 자동 배포 구조

이 글에서는 **Obsidian에서 글을 작성하고,  
비공개 저장소(private repo)를 유지한 채  
GitHub Pages용 공개 저장소(public repo)로 글만 자동 배포하는 구조를 설명한다.

핵심 목표는 다음과 같다.

- 글 작성은 **Obsidian**에서 편하게
    
- 원본 노트(Vault)는 **비공개로 유지**
    
- 블로그에 필요한 글만 **자동으로 공개 레포에 반영**
    
- 수동 복사 / 업로드 없이 **GitHub Actions로 자동화**

---

## 전체 구조 한눈에 보기

이 블로그는 **두 개의 GitHub 저장소**와 **하나의 GitHub Action**으로 구성되어 있다.

```
[로컬 PC]
Obsidian
  └─ Vault (Private Repo)
       └─ _posts/
            └─ 글.md
                ↓ (git push)

[GitHub Actions]
  └─ _posts 변경 감지
      └─ 공개 레포로 복사 & push

[Public Repo]
soljeong.github.io
  └─ _posts/
       └─ 글.md
            ↓

[GitHub Pages]
블로그에 글 반영
```

---

## 1. 블로그 기본 구성

### GitHub Pages 기반 블로그

- 블로그는 **GitHub Pages**를 이용해 운영한다.
    
- 공개 저장소:  
    👉 [https://github.com/soljeong/soljeong.github.io](https://github.com/soljeong/soljeong.github.io)
    
- 이 저장소는 **블로그 그 자체**이며,  
    `_posts` 디렉토리에 있는 Markdown 파일들이 곧 게시글이다.

즉,

- `_posts/2026-01-01-post.md`
    
- `_posts/2026-01-10-workflow.md`

같은 파일들이 **바로 블로그 글**이 된다.

---

## 2. Obsidian Vault는 왜 비공개인가?

Obsidian Vault에는 보통 다음과 같은 것들이 함께 들어 있다.

- 개인 메모
    
- 정리 중인 글
    
- 초안
    
- 링크만 모아둔 노트
    
- 공개할 생각이 없는 기록들

그래서 Vault 전체를 공개 저장소로 두는 건 부담스럽다.

### 선택한 방식

- **Obsidian Vault 전체 → private repo**
    
- 그 안에 **블로그용 글만 모아두는 디렉토리**를 별도로 둔다

예시:

```
Obsidian Vault
├─ daily/
├─ ideas/
├─ references/
└─ _posts/        ← 블로그용 글만
    ├─ 2026-01-01-obsidian-blog.md
    └─ 2026-01-05-github-action.md
```

이 `_posts` 디렉토리만 **자동으로 공개 레포에 복사**한다.

---

## 3. GitHub Action의 역할

이제 핵심인 **GitHub Actions 워크플로우**를 살펴보자.

이 액션의 역할은 단순하다.

> “비공개 레포의 `_posts`가 바뀌면  
> 공개 레포의 `_posts`를 통째로 덮어쓴다”

---

## 4. 워크플로우 트리거 조건

```yaml
on:
  push:
    branches:
      - main
    paths:
      - '_posts/**'
```

### 의미

- **main 브랜치에 push가 발생했을 때**
    
- 그리고 **변경 내용이 `_posts` 디렉토리 안에 있을 때만**
    
- 이 워크플로우가 실행된다

즉,

- 다른 노트 수정 → ❌ 실행 안 됨
    
- 설정 파일 변경 → ❌ 실행 안 됨
    
- `_posts`에 글 추가/수정 → ✅ 실행

👉 **불필요한 액션 실행을 막고, 글 변경에만 반응**하도록 설계했다.

---

## 5. 실제 동작 단계별 설명

### ① 비공개 저장소 체크아웃

```yaml
- name: Checkout source repository
  uses: actions/checkout@v4
```

- GitHub Actions 실행 환경에
    
- **현재(private) 저장소의 코드**를 내려받는다

즉, Obsidian Vault의 `_posts`가 여기 존재하게 된다.

---

### ② 공개 저장소 클론

```bash
git clone https://${{ secrets.ACCESS_TOKEN }}@github.com/soljeong/soljeong.github.io ../target_repo
```

- 공개 블로그 레포를 **별도의 디렉토리**로 클론한다
    
- `ACCESS_TOKEN`은:
    
    - private → public 레포에 push 하기 위한 인증 수단
        
    - GitHub Actions Secret으로 저장되어 있다

이 시점에서 작업 공간은 이렇게 된다.

```
/workspace
├─ (private repo)
│   └─ _posts/
└─ target_repo/
    └─ _posts/
```

---

### ③ 공개 레포의 `_posts`를 완전히 교체

```bash
rm -rf ../target_repo/_posts/*
cp -r _posts/* ../target_repo/_posts/
```

이 구조의 핵심 철학은 **“동기화가 아니라 덮어쓰기”**다.

- 공개 레포의 기존 글을 모두 삭제
    
- 비공개 레포의 `_posts` 내용을 그대로 복사

이렇게 하면:

- 글 삭제 / 수정 / 파일명 변경이 **완벽하게 반영**
    
- 상태 불일치가 생길 여지가 거의 없음

---

### ④ 커밋 & 푸시

```bash
git add _posts
git commit -m "Sync _posts directory from source to target repository"
git push origin main
```

- 변경된 `_posts`를 커밋
    
- 공개 레포의 `main` 브랜치에 push

이 순간 GitHub Pages가 자동으로 빌드되고,  
블로그에 새 글이 반영된다.

---

## 6. 이 구조의 장점

### 1️⃣ Obsidian에만 집중하면 된다

- 글 작성
    
- 수정
    
- 삭제

👉 그냥 Obsidian에서 하고 push만 하면 끝

---

### 2️⃣ Vault는 끝까지 비공개

- 개인 메모
    
- 아이디어
    
- 초안
    
- 업무 기록

👉 어떤 것도 실수로 공개되지 않는다

---

### 3️⃣ 블로그 레포는 항상 “배포 결과물”만

- 공개 레포에는:
    
    - 글
        
    - 테마
        
    - 설정  
        만 존재

👉 블로그 용도로 깔끔하다

---

### 4️⃣ CI/CD 사고방식과 잘 맞는다

- Source of Truth: **private repo**
    
- Output Artifact: **public repo**
    
- 자동 배포 파이프라인

👉 개발자 관점에서도 매우 자연스러운 구조

---

## 마무리

이 방식은 단순히 “Obsidian 글을 블로그에 올린다”를 넘어서,

- **개인 지식 관리**
    
- **공개 콘텐츠 관리**
    
- **자동 배포**

를 깔끔하게 분리한다.

Obsidian을 메인 작업 공간으로 쓰면서  
GitHub Pages 블로그를 운영하고 싶다면  
충분히 안정적이고 확장 가능한 패턴이다.
