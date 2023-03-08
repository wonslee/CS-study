<details>
<summary>Table of Contents</summary>

- [WEB Security](#웹보안)
- [SQL Injection](#sql-injection)


</details>

# 웹보안

- SQL Injection

- CSRF ( Cross Script Request Forgery )

- XSS ( Cross-Site Scripting )



---


## 웹보안의 취약점



**웹이 보안에 취약한 이유 1>**
방화벽에서 조차 막지 못하는 HTTP 80 포트로 통한 악의적인 공격은 그대로 웹 서버에게 전달 된다.

클라이언트와 웹서버는 잘 알려진 프로토콜인 HTTP 와 공개된 포트인 80을 사용하는데

방화벽에서는 원활한 웹 서비스를 위해 HTTP 프로토콜과 80 포트는 막지
않는 것이 일반적입니다.

즉, 허용 또는 오픈 된 포트로의 공격 시도가 문제 입니다.

**웹이 보안에 취약한 이유 2>**
방화벽 , IDS, IPS 와 같은 보안 장비를 이용한 네트워크 계층에서의
보안으로는 어플리케이션 계층에 대한 공격을 차단할 수 없다.

방화벽에서 Application Gateway을 설정하더라도
**바이러스 코드, 웜과 같은 유해 코드들만** 감지하고 차단하는데,

잘 알려진 SQL 인젝션이나 쿠키변조, XSS 등의 대표 웹 공격패턴에 실려 오는
데이터는 **유해코드도 아니며 잘못된 값도 아닙니다.**



결론적으로 말하자면 웹 환경은 오픈 된 프로토콜과 포트상에서 통신이 이루어 지며

네트워크계층의 보안으로는 웹 응용프로그램 영역의 공격에는 의미가 없다는 것입니다.

---

## OWASP-10 (Open Web Application Security Project)

이러한 이유들 때문에 웹에는 다양한 취약점이 존재하는데

국제 웹보안 표준 기구에서 매년  OWASP-10 (웹취약점 10가지)를 발표한다.

OWASP - 10 중 대표적인 취약점 3가지

- SQL Injection

- CSRF ( Cross Script Request Forgery )

- XSS ( Cross-Site Scripting )

---

# SQL Injection

SQL Injection은 서버에 특정 SQL문을 입력하여 데이터베이스에 접근, 회원정보 등을 유출할 수 있는 공격이다.

특별히 보안관련 지식이 풍부하지 않아도 간단히
수행할 수 있는 공격방법이라서 그 위험성은 아주 크다고 볼 수 있다.

예를 들어

SQL = "Select * From Tbl_Members
Where MemberID = '"&MemberID &"' And Password = '" & Password & "'"
이러한 쿼리를 사용한다면

![](https://velog.velcdn.com/images/hs1430/post/247cad9b-7372-4c8f-a214-6e8f57c33e03/image.png)

그림과 같이 아이디 부분에 admin’-- 을 넣었을때 admin을 아이디로 사용하는 계정이 있다면 비밀번호를 무시하고 로그인이 가능하다.

실제 값을 넣는다면

Select * From Tbl_Members Where MemberID = 'admin'--' And Password = '아무거나'

이고 여기서 -- 는 //와 같이 뒷부분을 주석으로 처리 해주기 때문에 앞부분인 id만 맞으면 로그인이 되는것이다.

---

## DVWA 예시

DVWA에서는 몇가지의 해킹방법을 실제로 체험해볼수 있다.

![](https://velog.velcdn.com/images/hs1430/post/e02f4b10-d81d-4310-afdd-5d0a60e1e159/image.png)

해당 화면에서는 User ID 부분에 자신이 원하는 ID를 입력하면 그에 맞는 이름이
나오는 검색창이 있다.

![](https://velog.velcdn.com/images/hs1430/post/45626669-795f-4a96-8673-5599d9b2b581/image.png)

해당 사진은 검색창에 ' or 1=1 # 이라고 쳤을때 나오는 화면이다.

여기서 or의 앞부분이 무엇이든 1=1 은 true 이므로 참이 되고 #은 --와 같이 뒷부분을 주석 처리 해주어서

데이터 베이스에 있는 모든 ID의 이름이 나온 결과이다.
1=1 대신 2=2 나 그냥 1을 적어주어도 참이기에 상관없다.

여기서 계정정보를 얻어내고 싶다면

먼저 모든 테이블정보로부터 사용자 계정 정보가 존재할 테이블을 찾아야한다.

information_schema.tables : 데이터 베이스와 관련된 정보를 가진 테이블
table_name : 모든 테이블 이름값을 가진 필드



입력 질의는 **' or 1 union SELECT null, table_name FROM information_schema.tables #**

![](https://velog.velcdn.com/images/hs1430/post/e5e39abe-c5c9-4115-9953-22a423c1973f/image.png)

계정정보를 가지고 있을것 같은 테이블을 찾았다.

users 라는 table의 모든 컬럼을 해본다.

질의는 ' or 1 union SELECT null, column_name FROM information_schema.columns WHERE table_name='users' #

![](https://velog.velcdn.com/images/hs1430/post/276d81ae-a828-40a2-8d58-626894d77d52/image.png)

역시 users 테이블은 password 컬럼을 가지고 있었다.

이제 password 값을 알아볼 차례다.

질의는 ' or 1 union SELECT first_name,password FROM dvwa.users #

![](https://velog.velcdn.com/images/hs1430/post/88a3bbcd-25df-4bc1-bb38-2a2a0edd698e/image.png)

밑에 기다란 부분이 암호환된 비밀번호이다.
암호화 된놈은 0~9 , A~F 문자만 사용하므로 MD5 해쉬값으로 예상된다.

John the Ripper 툴을 이용하면 복호화가 가능하다.

---
## SQL Injection - 해결방법

SQL Injection에 대해 대비해두지 않으면 큰일이 난다는것을
잘알았을 것이다.

이를 대비할 방법이 무엇인지 알아보자.

```
<?php

if( isset( $_GET[ 'Submit' ] ) ) {
    // Check Anti-CSRF token
    checkToken( $_REQUEST[ 'user_token' ], $_SESSION[ 'session_token' ], 'index.php' );

    // Get input
    $id = $_GET[ 'id' ];

    // Was a number entered?
    if(is_numeric( $id )) {
        // Check the database
        $data = $db->prepare( 'SELECT first_name, last_name FROM users WHERE user_id = (:id) LIMIT 1;' );
        $data->bindParam( ':id', $id, PDO::PARAM_INT );
        $data->execute();
        $row = $data->fetch();

        // Make sure only 1 result is returned
        if( $data->rowCount() == 1 ) {
            // Get values
            $first = $row[ 'first_name' ];
            $last  = $row[ 'last_name' ];

            // Feedback for end user
            echo "<pre>ID: {$id}<br />First name: {$first}<br />Surname: {$last}</pre>";
        }
    }
}

// Generate Anti-CSRF token
generateSessionToken();

?>
   
   
   ```


먼저 파라미터로 받은 CSRF 토큰을 세션에 저장된 토큰과 검증한 후
전달받은 id 파라미터가 오직 숫자로만 이루어져 있는지 is_numeric() 함수를 이용해
검사하고 있다. 그리고 자체적으로 SQL Injection 필터링이 적용되는
PDO(PHP Data Objects)를 활용하여 미리 생성(prepare)된 쿼리에
안전하게 파라미터 값을 삽입하고 있다.

여기서 PDO는 SQL 인젝션 공격을 걱정하지 않고
외부 입력(사용자ID등) 값을 SQL 쿼리에 넣을 수 있게 해주는 DB접근방식이다.

사용자 입력값에 대해 필터링이 이루어지고 동적 쿼리가 아닌 정적 쿼리를 사용하기 때문에
이 코드는 안전하다고 할 수 있다.

---

## CSRF

CSRF(Cross Site Request Forgery)

CSRF는 악성 스크립트를 공격자가 실행 시키는 것이 아닌 타 사용자로부터 서버에 Request를 하게하여 공격자가 원한는 상황이 발생하게 하는 것이다.

즉, 공격자 수행하지 못하는 행위를 자신보다 높은 권한을 가진 계정(ex.관리자)에게 강제로 수행하게 하는 악성 스크립트를 실행시켜 피해자가 의도치않게 공격자 대신 서버에 요청해주어 정보 유출, 계정 탈취 등의 피해를 입게 된다.
 
---
## CSRF TOKEN

CSRF Token은 임의의 난수를 생성하고 세션에 저장한다.

그리고 사용자의 매 요청마다 해당 난수 값을 포함시켜서 전송시킨다.

이후 백엔드에서는 요청을 받을 때 마다 세션에 저장된 토큰값과 요청 파라미터에 전달된 토큰 값이 같은지 검사한다.

스프링 시큐리티에서는 공식적으로 이 CSRF 공격에 대한 방어 기능을 시큐리티 모듈에서 제공해주고 있다.


---
## CSRF 예시

![](https://velog.velcdn.com/images/hs1430/post/429be633-e04c-4ce1-96fc-9647d427fc8f/image.png)

프록시를 이용하여 Change를 눌렀을때의 패킷을 분석해보면,

패스워드 변경 시 현재 비밀번호를 확인하지 않고 있으며, 프록시 툴로 패스워드 변경 요청 값을 중간에서 잡아본 결과 GET으로 요청하기 때문에 URL 값만으로도 CSRF 공격을 시도할 수 있다.
또한 url 변수는 password_new 와 password_conf 인것을 알 수 있다.

![](https://velog.velcdn.com/images/hs1430/post/7af81fdd-08b6-4920-b375-b41ada51f60f/image.png)

CSRF 공격을 하기 위해 XSS(Stored)로 이동하였다.

잘못된 이미지 경로를 지정하여 에러를 발생하게하고 에러 발생 시 사용자 패스워드 변경 시키는 URL을 강제 Request 시키는 코드를 통해 XSS 취약점이 존재하는 페이지에 접속 시 마다 패스워드가 변경되게 만들어보자.

```
<img src="x" onerror='this.src="http://172.30.1.7/vulnerabilities/csrf/?password_new=TEST123&password_conf=TEST123&Change=Change"'>
```


![](https://velog.velcdn.com/images/hs1430/post/050d825d-149e-4255-a769-d510204b4ab0/image.png)

Message 칸에 코드를 넣으려 했으나 칸수가 모자라 실패하였다.
Elements를 살펴본 결과 maxlength 값을 조정하면 될것 같았다.

![](https://velog.velcdn.com/images/hs1430/post/7c0df669-17e3-4647-bb80-53ff624251b1/image.png)

maxlength 값을 500으로 늘리니까 글자수 제한이 늘어나서
나머지 코드도 입력이 잘된다.


![](https://velog.velcdn.com/images/hs1430/post/d804e3c3-0928-4676-ab74-d09ed45e257c/image.png)

코드를 입력하여 게시글을 작성하게되면, 해당 사이트에 방문한 유저는
우리가 짠 코드에 노출되어버린다.

위의 사진은
```
<img src="x" onerror='this.src="http://172.30.1.7/vulnerabilities/csrf/?password_new=TEST123&password_conf=TEST123&Change=Change"'>
```
에서 TEST123 부분을 원래 비밀번호인 password로 입력한 결과값이다.
방문할때마다 비밀번호가 password로 자동으로 바뀌게 되는 모습을 볼 수 있다.

추가 예제

```
<img src="http://asq.kr/RGQQXKMuyErhy" onerror=alert("CSRF_TEST")>
```

---

## CSRF - 해결방법

1. 패스워드 변경시 공격자가 모르는 정보인 **사용자의 현재 비밀번호**를 같이 입력하게 한다.

2.  2차 인증 수단 등의 본인 인증 확인 절차를 가진다. (CSRF TOKEN)

---

출처

- https://unabated.tistory.com/entry/2-SQL-INJECTION

- https://iamstudying.tistory.com/25

- https://haruhiism.tistory.com/131

- https://414s.tistory.com/6

- https://minkukjo.github.io/cs/2020/08/15/Security-1/

