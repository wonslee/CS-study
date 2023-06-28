<details>
<summary>Table of Contents</summary>

- [Swagger](#swagger)
- [FCM](#fcmfirebase---cloud---messaging)

</details>

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

---

# **FCM(Firebase - Cloud - Messaging)**

FCM은 무료로 메시지를 안정적으로 전송할 수 있는 교차 플랫폼 메시징 솔루션이다. 모든 사용자에게 알림 메세지를 전송할 수 있고, 그룹을 지어 메시지를 전송할 수도 있다. FIrebase의 서비스는 요금 정책에 따라, 이용할 수 있는 범위가 다르지만 FCM은 요금 정책에 구분 없이 무료로 사용하는 것이 가능하다.

### **왜 FCM을 써야할까?**

기존에는 iOS, Android, Web 등의 플랫폼에서 Push 메시지를 보내기 위해서는 각 플랫폼 환경별로 개발해야 하는 불편함이 있었다. 하지만 FCM은 교차 플랫폼 메시지 솔루션이기 때문에 FCM을 이용해서 개발을 진행하면, 플랫폼에 종속되지 않고 Push 메시지를 전송할 수 있다. 따라서 위의 문제를 해결할 수 있게 된다.

또한 서버에서 어떤 데이터를 앱에 전달하려면 네트워크 연결은 필수이다. 하지만 알림을 한두번 보내기 위해서 계속해서 서버와 연결을 지속하는건 비효율적이다. 만약 FCM서버를 이용한다면, 서버의 데이터를 앱에 직접 전달하지 않고 FCM 서버를 거쳐서 앱에 전달하는 방식으로 클라우드 메시징을 이용하면 서버와 앱이 네트워크 연결을 지속해서 유지하지 않아도 되며 앱이 포그라운드 상황이 아니라고 하더라도 데이터를 받을 수 있다.

※ 푸시:  PC 혹은 모바일 디바이스 내에 보이는 팝업창을 의미

FCM을 이용해 각 유저들에게 푸시메시지를 전송하기 위해선 두가지 방법 TOKEN과 TOPIC을 활용해서 푸시 메시지를 보낼 수 있다.

---

### **FCM의 TOKEN이란?**

!https://blog.kakaocdn.net/dn/SoGg7/btslDYuKfpb/s1GiElUP0REjLKo6bhlXP1/img.png

- 앱이 FCM 서버와 통신하기 위해 사용되는 고유한 식별자
- 앱은 서버와 통신할 때 토큰을 사용하여 FCM 서버에서 앱을 식별하고, 이를 통해 메시지 전송을 할 수 있다.
- FCM의 토큰은 앱이 설치된 디바이스마다 고유하다. 앱이 설치된 디바이스를 추가하거나 삭제할 때 토큰이 변경될 수 있다.
- 서버는 이러한 FCM 토큰을 사용하여 특정 디바이스에 메시지를 전송할 수 있다.

위에서 언급한 것처럼 FCM Token은 디바이스마다 다르게 부여되는데, 서버는 이 토큰으로 디바이스를 구분한다. 이 토큰으로 구분하지 않는다면, 서버는 어떤 디바이스에 알림을 줘야하는지 모르기 때문이다.

### **FCM Token을 사용해서 메시지를 보내는 과정**

!https://blog.kakaocdn.net/dn/GHp4O/btslGBlRmKS/70HU01QC7FljxM8WVJGNp1/img.png

흐름을 요약하자면 다음과 같다.

1. FCM에 앱을 등록하고, 앱도 FCM과 연동한다.

2. 앱 프론트는 필요할 때 FCM 토큰을 발급받고 백엔드로 보낸다.

3. 백엔드는 토큰을 유저 정보와 연동하여 DB에 보관하다가, 필요할 때 조회해서 Notification Provider에 푸시 알림을 요청한다.

### **FCM 토큰에 관해**

**토큰의 만료 :**

FCM 토큰은 정해진 수명이나 갱신 주기가 없다.

따라서, 시간과 관계없이 아래 이벤트가 발생하지 않는다면 만료되지 않는다.

- 앱이 인스턴스 ID를 삭제한 경우
- 앱이 새 기기에서 복원되었을 경우
- 사용자가 앱을 제거 / 재설치한 경우
- 사용자가 앱 데이터를 지운 경우

**토큰 관리** :

- 서버에 등록 토큰을 저장한다.
- 토큰의 신선도 보장을 위해 오래된 등록 토큰을 제거한다.
- 토큰을 언제 서버에 보낼까?
    - 로그인 시점에 보낸다 or 앱을 실행할 때마다 보낸다.
- 토큰은 언제 삭제할까?
    - Provider가 Notfication Server로 푸시 요청을 했을 때, 토큰이 만료되어 에러 메시지가 오는경우, 핸들링하여 그 토큰을 삭제한다.

**고려해야 할 부분 :**

- 같은 기기에서 다른 아이디를 쓰는 경우
    - 로그아웃 시점에 서버에서 유저 토큰을 삭제한다.
- 같은 아이디로 여러 기기를 쓰는 경우
    - 하나의 유저가 여러 토큰을 보유할 수 있게 스키마를 구성한다.

---

### **FCM의 TOPIC 이란?**

!https://blog.kakaocdn.net/dn/djYXQ9/btslLAyNwbl/PLWscdG9hxnTB5ejWY4P1k/img.png

- 토픽(Topic)은 일종의 채널로서, 이를 통해 일련의 수신자들에게 메시지를 전송할 수 있다.
- 구독 및 구독취소 요청 시, FCM은 구독한 유저들을 내부적으로 관리한다.
- subscribe, unsubscribe 메서드를 통해 구독과 구독 취소요청을 FCM에 전송할 수 있다.
- 토픽을 통한 푸시 발송시, 토픽을 구독한 사용자들에게만 메시지를 전송할 수 있도록 한다.

TOPIC 발송의 경우 유튜브의 구독시스템과 비슷한 구조라고 생각할 수 있다.

내가 구독을 누른 유튜버가 생방송을 시작하거나 새로운 영상이 올라왔을 때 다음과 같이 구독한 사람들에게만 푸시알림이 가는 것을 볼 수 있다.

!https://blog.kakaocdn.net/dn/mUFNp/btslGAtFsZn/MsqRktzaj3kmCGg7KmYhZk/img.png

따라서 여러 사용자에게 한번에 알림을 보내고 싶다면 TOPIC, 개인(예를들어 내 게시글에 답변이 달렸습니다)에게 알림을 보내고 싶다면 Token을 이용하면 좋다고 느껴졌다.

---

참고:

https://zuminternet.github.io/FCM-PUSH/

https://donghun.dev/Firebase-Cloud-Messaging

https://firebase.google.com/docs/cloud-messaging?hl=ko

https://seungwoolog.tistory.com/88