## Rest Docs 문서 만들 때 GET 요청에서 IllegalArgumentException이 발생할 때 해결 방법

### 에러가 발생한 코드

```java
mockMvc.perform(get("/authors/{id}",id)
```

### Exception

```bash
java.lang.IllegalArgumentException: urlTemplate not found. If you are using MockMvc did you use RestDocumentationRequestBuilders to build the request?
```

mockMvc에서 사용한 get() 메서드가 MockMvcRequestBuilders.get()인 경우 발생하는 에러이다.

- get(urlTemplate, urlVariables) 호출시에만 발생한다.
- get(uri)를 호출하면 발생하지 않는다.

### 해결

메시지 내용처럼 get 메서드를 RestDocumentationRequestBuilder.get()으로 변경한다.

```java
// 기존
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;

// 변경
import static org.springframework.restdocs.mockmvc.RestDocumentationRequestBuilders.get;
```

- 기타 사용법은 동일하다.

### 참고

![image](https://github.com/sang5c/problem-solutions/assets/28697758/491e43c3-0dad-4502-a35f-01398f856cd7)
- RestDocumentationRequestBuilders.get() 내부에서 requestAttributes()를 호출하고 있는 것을 볼 수 있다. 
