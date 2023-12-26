## IP
- public
- private
- ip v4 : 32bit = 8bit + 8bit +8bit + 8bit
	- 클래스 구분
		- a,b,c class 는 크기로 구분
	- 서브넷, 서브넷 마스크, 논리적인 파티션, 소리를 질러도 파티션 밖으로는 들리지 않는다.
	- 다 차있는건 브로드캐스팅이라고 다른 기능이 있다.
	- 다 비어있는것도 다른 기능이 있고
- ip v6 
> 병렬처리를 하려면 네트워크 지식이 있어야 한다.


```d2
sl : Slave(datanode) {
protocol : TCP/IP
}

ma : Master(Namenode)

sl <-> ma : ip
sl1 <-> ma
sl2 <-> ma


sl1 : Slave(datanode)
sl2 : Slave(datanode)

```

### loopback ip
- 127.0.0.1
- LO
## port
- 다른 서비스가 사용하고 있는 포트를 또 사용할 수 없다
- 누가 어떤 포트를 사용하고 있는지 검사해야해
`netstat -intp | grep 8888`
## PID
- process ID
- `vim a.txt &` : 백그라운드로 프로세스 실행하는 명령 
- `ps -ef | grep vim`
## background foreground
- 유튜브 프리미엄이 아닌 경우 background 실행을 허용하지 않는다.
- 프로세스의 상태
- 예를 들어
- `# nohup java -jar project.jar` : 백그라운드 실행, 세션 끊어지지 않도록
[[Dataset과 DataLoader — 파이토치 한국어 튜토리얼 (PyTorch tutorials in Korean)]]

