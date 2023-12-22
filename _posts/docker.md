## volume
## bind
## dev container
- clone repository in container volume
## 시도
- 그냥 명령어만 입력해서 원하는 상태까지 만들기
- 파이썬 설치
- 볼륨
- 포트 포워딩
## 컨테이너 직접 실행
 도커 컨테이너를 실행하기 위해서는 다음과 같은 명령을 사용할 수 있습니다:

```
docker run -d \
  --name 컨테이너이름 \
  -v 볼륨소스:볼륨타겟 \
  -p 호스트포트:컨테이너포트 \
  이미지이름
```

예를 들어 mysqldb라는 이름의 컨테이너를 실행하고, 호스트의 /my/own/datadir 디렉토리를 컨테이너의 /var/lib/mysql 디렉토리에 마운트하며, 호스트의 3306 포트를 컨테이너의 3306 포트에 연결하려면 다음과 같이 할 수 있습니다:

```
docker run -d \
  --name mysqldb \
  -v /my/own/datadir:/var/lib/mysql \ 
  -p 3306:3306 \
  mysql
```

주요 옵션 설명:

- -d: 백그라운드 모드로 컨테이너 실행
- --name: 컨테이너 이름 설정  
- -v: 호스트와 컨테이너의 디렉토리를 연결(마운트)
- -p: 호스트와 컨테이너의 포트 포워딩 설정
```
docker run -t \
  --name pyserver \
  -v d:\\volume:/workspace \ 
  -p 8000:8000 \
  python
```
- pip install --upgrade pip
- pip install -r requirements.txt