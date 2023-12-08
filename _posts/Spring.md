
## 교재 따라서 하기
	- https://wikidocs.net/160444
## 다른 내용
- 컨트롤러
- getmapping
- postmapping
- dependencies
	- spring web
	- lombok : extension으로 설치? 이것만으로 괜찮나?
	- devtool : vscode에서 작동하는 법 모르겠는데, 잘 안되네
	- com.h2database:h2
		- http://localhost:8080/h2-console
- 내 jdk가 21버전임
- ORM(object relational mapping)
- extension
	- postman
		- post request를 만들어서 테스트하도록
## mvc 패턴?
- model
	- database
- view
- controller
	- request를 처리하고 응답
		- get
		- post
## component
```java
package com.mysite.sbb;

import org.commonmark.node.Node;
import org.commonmark.parser.Parser;
import org.commonmark.renderer.html.HtmlRenderer;
import org.springframework.stereotype.Component;

@Component
public class CommonUtil {
    public String markdown(String markdown) {
        Parser parser = Parser.builder().build();
        Node document = parser.parse(markdown);
        HtmlRenderer renderer = HtmlRenderer.builder().build();
        return renderer.render(document);
    }
}
```
- 이게 뭘까?
- https://wikidocs.net/162799
## CRUD
- CRUD는 **대부분의 컴퓨터 소프트웨어가 가지는 기본적인 데이터 처리 기능인 Create(생성), Read(읽기), Update(갱신), Delete(삭제)를 묶어서 일컫는 말이다**. 사용자 인터페이스가 갖추어야 할 기능(정보의 참조/검색/갱신)을 가리키는 용어로서도 사용된다.
## MVC
![[Pasted image 20231205192226.png]]
https://tecoble.techcourse.co.kr/post/2021-04-25-dto-layer-scope/
https://gnidinger.tistory.com/469
## ModelAttribute
https://galid1.tistory.com/769
## thymeleaf
### th:text, th:utext
- https://www.thymeleaf.org/doc/tutorials/3.1/usingthymeleaf.html#more-on-texts-and-variables
- Unescaped Text
- `th:text`를 사용할 경우 HTML의 태그들이 이스케이프(escape)처리되어 태그들이 그대로 화면에 보인다.