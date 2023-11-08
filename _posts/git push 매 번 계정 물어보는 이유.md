If you work with multiple different identities on a single Git hosting service, you may be wondering if Git Credential Manager (GCM) supports this workflow. The answer is yes, with a bit of complexity due to how it interoperates with Git.

```bash
git config --get credential.helper
```
- saved credential 어디에서 찾을 수 있을지 모르겠네 저장하는 방법이 여러가지
- ssh를 한번 사용한 적 있었는데 그게 언제 어디서였는지 모르겠네
-  Git의 Credential-Helper 시스템의 기본 명령은 `git credential` 이다.
- [Git - Credential 저장소 (git-scm.com)](https://git-scm.com/book/ko/v2/Git-%EB%8F%84%EA%B5%AC-Credential-%EC%A0%80%EC%9E%A5%EC%86%8C)

## 모르겠어
