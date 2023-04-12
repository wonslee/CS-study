# **Swagger**

Swagger는 웹 서비스 명세를 문서화 해주는 오픈 소스 소프트웨어 프레임워크이다.

즉 웹 서비스가 어떤 로직을 수행하고, 이 로직을 수행하기 위해서는 어떤 값을 요청하며, 이에 따른 응답값은 무엇인지 정리해서 문서화해주는 프로젝트이다.

보통 웹 어플리케이션을 개발할 때 프론트와 백엔드로 팀을 나누어서 개발을 하는데, 이 때, 백엔드 팀이 만든 서비스를 swagger로 문서화해서 프론트 팀으로 넘겨 로직의 이해도를 높이고, 소통한다. swagger를 사용하면 개발과정 속에서 계속 변경되는 명세 문서를 알아서 주기적으로 업데이트해주기 때문에 번거로움을 없애고, 시간을 절약할 수 있다.

### **springboot 3.0.0 이상부터는 springfox가 아닌 springdoc-openapi-ui 라이브러리 사용 권장.**

spring에는 2가지 스웨거가 존재한다. springfox와 springdoc이 있는데 과거 springfox를 주로 사용했다. 하지만 springfox는 2020년 7월 14일 기준으로 더 이상 업데이트가 안되고 있다. 반면 springdoc은 주기적으로 업데이트 중이다. springboot 버전이 올라갈수록 더더욱 springdoc-openapi를 사용해야 할 듯 하다.

---

### **bulid.gradle에 dependency 추가**

```bash
	implementation 'org.springdoc:springdoc-openapi-starter-webmvc-ui:2.1.0'
```

### **application 설정**

```java
springdoc:
  swagger-ui:
    path: /api-docs.html
  api-docs:
    path: /api-docs
  show-actuator: true
  default-produces-media-type: application/json
```

### **Swagger 확인**

이후 http://localhost:8080/swagger-ui/index.html에 접속하게 되면 아래와 같이 API 문서를 확인할 수 있다.

![https://blog.kakaocdn.net/dn/tcmOb/btr8Xy23kEe/qr6E151rKSAPkPSy7E6561/img.png](https://blog.kakaocdn.net/dn/tcmOb/btr8Xy23kEe/qr6E151rKSAPkPSy7E6561/img.png)

여기에 controller를 추가하게 되면 다음과 같다.

```java
@Tag(name="API", description = "Sample API 입니다.")
@RestController
@RequestMapping("/api")
public class SampleController {

	@Autowired
    private SampleService sampleService;

	@PostMapping("/getUser/{age}")
	public List<User> getUser(@Parameter(description = "나이") @PathVariable("age") int age) {
		return sampleService.getUser(age);
	}

  	 @Hidden
   	 @GetMapping("/ignore")
   	 public String ignore() {
  	      return "무시되는 API";
    }
}
```

![https://blog.kakaocdn.net/dn/brR3Wj/btr8P0Fxm7W/IrOIsvWCowqJtLsrcNNqB0/img.png](https://blog.kakaocdn.net/dn/brR3Wj/btr8P0Fxm7W/IrOIsvWCowqJtLsrcNNqB0/img.png)

@Hidden은 숨김기능을 하기 떄문에 나타나지 않는다.

![https://blog.kakaocdn.net/dn/cFnpoE/btr821Ybpr2/lwjaHkkpJn9ee5djBZDMmk/img.png](https://blog.kakaocdn.net/dn/cFnpoE/btr821Ybpr2/lwjaHkkpJn9ee5djBZDMmk/img.png)

### **Test 결과 확인**

![https://blog.kakaocdn.net/dn/dul1Fr/btr8T3PN2xF/RzaYOWNRZdLnzLbluE6leK/img.png](https://blog.kakaocdn.net/dn/dul1Fr/btr8T3PN2xF/RzaYOWNRZdLnzLbluE6leK/img.png)

### **Springdoc Annotation**

![https://blog.kakaocdn.net/dn/bVxPhj/btr9dXneJe5/uyLJDj2VJ79ptg2Kn4tWo1/img.png](https://blog.kakaocdn.net/dn/bVxPhj/btr9dXneJe5/uyLJDj2VJ79ptg2Kn4tWo1/img.png)

> 단점
>

Swagger 코드가 들어가서 코드가 길어보이고 지저분해 보인다.

---

참고:

[https://velog.io/@coastby/SpringDoc-annotation](https://velog.io/@coastby/SpringDoc-annotation)

[https://jong-bae.tistory.com/11](https://jong-bae.tistory.com/11)

[https://velog.io/@stpn94/Swagger-%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B3%A0](https://velog.io/@stpn94/Swagger-%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B3%A0)