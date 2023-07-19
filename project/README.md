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


# CI/CD (깃허브 액션 + 도커)

- CI/CD의 목표 : **한 개인이 수동으로 빌드 ~ 배포를 진행할 때 생기는 많은 문제(통합 지옥)를 예방.**
    
    [데이터 기반으로 지속적인 CI/CD 개선 환경 만들기](https://engineering.linecorp.com/ko/blog/build-a-continuous-cicd-environment-based-on-data)
    
    현재 수행하고 있는 업무의 목표는 개발된 소스 코드를 사용자의 모바일 장비에 설치할 수 있도록 Google Play Store에 배포하는 것입니다. 
    
    Git 커밋 이후, 소스 코드를 다른 사람이 개발한 소스 코드에 합치기 전에 잘 작동하는지, 그리고 규칙에 맞게 개발되었는지 PR(**Pull Request**)을 보내 자동으로 확인합니다. 그 다음으로 기술 블로그에 자주 소개되었던 동료 개발자들의 **코드 리뷰**가 진행되고, 코드 리뷰가 완료되면 동일한 검증 과정을 거친 동료들의 결과물이 모인 소스 코드로 통합하는 작업(**PR 머지**)이 수행됩니다. 
    
    이제 개발자의 소스 코드는 **실제 배포될 수 있는 코드에 포함**되었습니다. 그러면 **테스터**가 모바일 장치에 설치해 실제로 잘 동작하는지 여러 테스트를 수행합니다. 만약 버그나 오류가 발견되면 수정하는 과정을 거쳐 드디어 Google Play Store에 **배포**합니다.
    
    이런 작업은 수많은 사람들이 밀접하게 연계되어 있으며, 매끄럽고 원활하게 동작시키려면 많은 노력이 필요합니다. 만약 이 과정을 **한 개인이 수동으로 진행한다면 많은 문제가 발생**할 수 있습니다. 그래서 작업 시간을 줄이고 불편함을 최소화하기 위해 관련된 업무 프로세스를 도출하여 자동화하는 업무를 수행합니다. 
    

## CI란?
![cicd](https://github.com/wonseok2877/CS-study/assets/72124326/60e9c5f8-0fae-481d-9c98-a02c61785a0b)


CI(Continuous Integration) :  지속적인 통합 (= **빌드, 테스트 자동화**)

CI가 필요한 환경 : 다수의 개발자가 형상관리 툴(git)을 사용해 공유 레포지토리에서 작업하는 환경

비정상적인 코드가 통합되는 것을 예방할 수 있다.

새로운 코드 변경 사항이 정기적으로 빌드 및 테스트되어 공유 리포지토리에 통합되므로 여러 명의 개발자가 동시에 관련된 코드 작업을 할 경우 발생하는 **충돌 문제를 해결**할 수 있다.

> 지속적 통합의 실행은 소스/버전 관리 시스템에 대한 변경 사항을 정기적으로 커밋하여 모든 사람에게 동일 작업 기반을 제공하는 것으로 시작합니다. 커밋할 때마다 빌드와 일련의 자동 테스트가 이루어져 동작을 확인하고 변경으로 인해 문제가 생기는 부분이 없도록 보장합니다.
> 
> 
> 지속적 통합은 그 자체로 유익하지만 CI/CD 파이프라인을 구현하기 위한 첫 번째 단계이기도 합니다.
> 

- PR 생성시 빌드 테스트 자동화
    
    빌드 실패시엔 merge가 불가능하도록 설정했다.
    ![Screenshot from 2023-07-18 16-25-12](https://github.com/wonseok2877/CS-study/assets/72124326/a1f3206f-8c2e-4a32-a552-4b12b1c67036)
![Screenshot from 2023-07-19 18-04-43](https://github.com/wonseok2877/CS-study/assets/72124326/e27e19ff-31e8-466c-93fa-65a0988b47cc)

## CD란?

CD(Continuous Deployment) ****: 지속적인 서비스 제공(= **배포 자동화**)

코드 변경이 CI 단계를 모두 성공적으로 통과 → 해당 변경 사항을 프로덕션에 자동으로 배포

간단한 코드 변경이 정기적으로 마스터에 커밋되고, 자동화된 빌드 및 테스트 프로세스를 거치며 다양한 사전 프로덕션 환경으로 승격되며, 문제가 발견되지 않으면 최종적으로 배포됩니다. 강력하고 신뢰할 수 있는 자동화 배포 파이프라인을 구축하면 하루에도 여러 번 이루어지는 릴리스가 특별하지 않은 일상이 됩니다.
![cicd2](https://github.com/wonseok2877/CS-study/assets/72124326/2d2afa51-5247-404c-880d-616ac371141f)

<details>
<summary>수동 배포 과정… (5~10분 소요)</summary>

    1. 인스턴스 SSH 접속
    2. 브랜치 최신화
    3. 로컬 파일 (.gitignore) 한땀 한땀 vim으로 최신화
    4. 소스코드 빌드
    5. 기존 서버 셧 다운
    6. 최신 서버 실행
    7. 서버 잘 돌아가는지 확인
        
</details>


        

위 과정에서 **휴먼 에러**가 발생할 여지가 잔뜩 있고, 노이로제에 걸릴 수도 있다.

이 과정을 자동화함으로써 이젠 개발에만 집중할 수 있게 됐다.

- PR merge시 배포 자동화
    ![Screenshot from 2023-07-17 22-27-49](https://github.com/wonseok2877/CS-study/assets/72124326/ae49bd2a-4b40-4e40-8e51-d574e0ba7d8b)
    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0f8e7a59-1899-4ecb-9b9d-5c0ed7d7abe8/Untitled.png](https://miro.medium.com/v2/resize:fit:1179/1*hcGajybBjM1Nwo6fhm9j1A.png)    

## CI/CD 도입 전과 후

도입 전

1. 개발자들이 각자의 feature 브랜치에 코드를 push 
(but, 한 부분에서 에러가 났고 개발자들은 눈치채지 못합니다)
2. PR을 생성하고 merge(통합)
3. **에러가 발생**했지만 어느 부분에서 에러가 났는지 모르므로 다시 어디부분에 에러가 있는지 디버깅하고 코드를 수정
4. (1) ~ (4)의 과정을 반복
5. 배포를 시작합니다. 하지만 배포과정 또한, 개발자가 **직접 배포** 과정을 거치므로 많은 시간을 소요

도입 후

1. 개발자들이 개발하여 feature 브랜치에 코드를 push
2. PR 생성→github actions에서 알아서 Build, Test를 실행하고 Merge 가능 여부를 보여줌
3. 개발자들은 에러가 난 부분이 있다면 에러부분을 수정하고 코드를 base 브랜치에 merge
4. Build, Test가 정상적으로 수행이 되었다면 github actions가 Deploy 과정을 수행

## 느낀 점

- 릴리즈(앱 런칭 이후) 단계에서 CI/CD를 경험해보고 싶다.
- TDD를 적용한다면 CI 과정이 강력해지겠다.
- 지금의 CD에 대해선 부족하다고 생각. 빌드까지만 체크하고, 서버가 비즈니스 로직을 정상적으로 처리할 수 있는지는 테스트하지 않기 때문. 이는 추후 테스트 코드를 넣어서 해봐야 함.
- 도커는 헛쓰고 있다. 부가적인 유틸을 쓰고 있는 중일뿐이다

---
참고:

https://engineering.linecorp.com/ko/blog/build-a-continuous-cicd-environment-based-on-data
https://seosh817.tistory.com/104
