# Jupyter server
## 서버 비밀번호 설정
`jupyter server password`
/home/gen2/.jupyter/jupyter_server_config.json 파일에 비밀번호 저장됨
### cat
- 파일을 보는 용도?
{
  "IdentityProvider": {
    "hashed_password": "argon2:$argon2id$v=19$m=10240,t=10,p=8$TX352GYr5q6yxl1+Kf4bJA$A2+5u1iW1h+Z3iAlRg1ZICrS6wNvp5LPEcI9m+J3pMs"
  }

## 환경파일을 만든다
`jupyter notebook --generate-config`
## 환경 파일 안에 비밀번호 저장
`vim /home/[여러분들계정]/.jupyter/jupyter_notebook_config.py
- c.ServerApp.password 에 저장
## 서버 실행
jupyter lab --ip=0.0.0.0
	모든 아이피 허용