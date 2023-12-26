## 수업
- request
- 파이썬으로 get 요청을 보냈을 때 회피하는 서버가 있어서 다른 방법이 필요하다.
- 서버에서 gateway를 통해 감시하고 있다.
- header에 있는 user-agent 정보를 확인해서 : 신분증 검사와 마찬가지
- 위장이 필요하다
- 딕셔너리 형태로 새 user-agent 정보를 request에 넘겨준다
- 특정 태그에 있는 데이터를 파싱해서 가져온다 : beautifulsoup
- table 태그 처리 노가다를 줄이기 위해서 pandas 사용
	- pd.read_html(r.text) : 테이블 태그만 가지고와준다
> 파이썬스러운 코드? 리스트컴프리헨션?
> 파이썬은 생산성 때문에 사용한다

> python은 stack에 제한이 있어서 재귀를 무한정 돌 수 없다
> kafka 같은 message server는 queue 구조로 되어있다
> 톰캣 앞에 nginx나 apache를 붙인다
> 로드 밸런싱을 해야한다

> 딕셔너리, 리스트, 튜플 중에 딕셔너리가 가장 빠르다
> hash 구조로 되어있기 때문?

> 파이썬 패키지, 스탠다드 라이브러리와 서드파티 라이브러리로 구분된다

## 웹 접속
- urllib 이게 스텐다드 라이브러리
- 이거 대신 requests 사용

## payload
- post 보낼 때 이걸 같이 보내야한다

## beautiful Soup
- html을 파싱해준다.
`from bs4 import BeautifulSoup`
- 혹시 에러나면
- `pip install lxml` : 여기에 기반해서 bs4를 만들었다
> https://t1.daumcdn.net/osa/notice/26/1r8z2PaUry/notice.html
> 카카오톡도 오픈소스를 기반으로 만들어져있다.

## request header

```python
head = {"User-Agent":

"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36 Edg/120.0.0.0"}

melon_r = requests.get("https://www.melon.com/chart/index.htm",headers=head)
```
## 정규식도 사용해서

```python
import re 
p = re.compile("[0-9]+")
for x in bs.findAll("div", class_="wrap_song_info")[::2]:
    print(x.find("a").text, end= ", ")
    print(p.findall(x.find("a")['href'])[-1])
```