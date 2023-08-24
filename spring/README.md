
<details>
<summary>Table of Contents</summary>

- [DAO vs DTO vs VO](#dao-vs-dto-vs-vo)
- [Mock이란?](#mock)

</details>

# **DAO vs DTO vs VO**

### **DAO(Data Access Object)**

DAO는 실제로 데이터베이스(DB)의 data에 접근하기 위한 객체이다. Database 접근을 위한 로직과 비지니스 로직을 분리하기 위해 사용한다.

- DB에 접근하여 data를 삽입, 삭제, 조회, 수정 등 CRUD 기능을 수행한다.
- Service와 DB를 연결하는 연결고리 역할을 한다.
- Repository package가 DAO이다.

### **예제**

**1. Sping Data JPA**

```bash
public interface UserRepository extends JpaRepository<User, Long> {

}
```

**2. JPA**

```bash
@Repository
@RequiredArgsConstructor
public class MemberRepository {

    private final EntityManager em;

    public void save(Member member) {
        em.persist(member);
    }

    public Member findOne(Long id) {
        return em.find(Member.class, id);
    }

    public List<Member> findAll() {
        return em.createQuery("select m from Member m", Member.class).getResultList();
    }

    public List<Member> findByName(String name) {
        return em.createQuery("select m from Member m where m.name =:name", Member.class)
                .setParameter("name", name)
                .getResultList();
    }
}
```

---

### **DTO(Data Transfer Object)**

DTO(Data Transfer Object)는 계층간 데이터 교환을 위한 객체를 의미한다. DTO는 로직을 가지지 않는 순수한 데이터 객체(Java Beans)이다.

- DTO는 getter/setter 매서드만 가진 클래스를 의미한다.
- DB에서 데이터를 얻어서 Service나 Controller 등으로 보낼 때 사용한다.
- 엔티티를 DTO 형태로 변환해서 사용한다.
- 아래와 같이 계층간 데이터 교환할 때 Entity가 아닌 DTO를 사용한다.

![https://blog.kakaocdn.net/dn/bfQiLI/btr3vOwsitV/cGKeLsy2SMQ0IWWAAwuKU0/img.png](https://blog.kakaocdn.net/dn/bfQiLI/btr3vOwsitV/cGKeLsy2SMQ0IWWAAwuKU0/img.png)

**Entity와 DTO 분류 이유**

- DB Layer = Persistence Tier, View Later = Presentation Tier 의 역할을 철저하게 분리한다.
- Entity는 실제 테이블과 매핑되어 만일 변경하게 되면 여러 다른 Class에 영향을 끼친다.
- DTO는 View와 통신하며 자주 변경된다.

1) Entity

```java
@Entity
@Getter
public class User {

    @Id
    @GeneratedValue
    private Long id;

    private String email;
    private String password;
    private int age;

// Entity에는 setter를 사용하지 않는다.@Builder
    public User(String email, String password, int age) {
        this.name = email;
        this.password = password;
        this.age = studentNumber;
    }
}
```

2) DTO

```java
    @Getter
    static class UserDto {
        private String email;
        private String age;
// password 삭제

        @Builder
        public UserDto(String email, int age) {
        this.email = email;
        this.age = age;
        }

// 이처럼 한번에 Entity로 만드는 함수를 자주 사용한다.// 여기서는 UserDto의 역할이 password를 감추는 용도이니 toEntity가 어울리진 않음.public User toEntity() {
        	return User.builder()
                .email(email)
                .age(age)
        }
    }
```

3) Controller

```java
    @PostMapping("/delete")
    public ResponseEntity<String> join(@RequestBody UserDto userDto) {
        userService.delete(userDto);// service에 유저 삭제 로직이 있다고 가정return ResponseEntity.ok().body("삭제 완료");
    }
```

이처럼 DTO는 View, Controller, Service,DAO를 연결해주는 연결고리 역할을 하고, password같은 민감한 데이터를 감출 수 있다.

DTO는 Response, Request 모두 사용할 수 있다.

---

### **VO(Value Object)**

VO(Value Object)는 DTO와 혼용해서 쓰이긴 하지만 미묘한 차이가 있다.

1. 변하지 않는 값을 가지는 객체(불변성)

- 값이 변하지 않음을 보장하며 코드의 안정성과 생산성을 높임

2. 값이 같다면 동일한 객체

- 각 객체를 비교하는데 사용되는 ID가 없다.
- 같은 객체인지 판단하기 위해 각 속성들의 값을 비교한다.
- equals() 메서드와 hashCode() 메서드를 오버라이드해서 객체 비교를 구현한다.

PostsVO 클래스는 다음과 같이 작성할 수 있다. equals()와 hashCode()가 재정의되어 객체 값 자체를 비교하는 것을 볼 수 있다.

```java
@Getter
public class PostsVO {

    private Long id;
    private String title;
    private String content;
    private String author;

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        PostsVO postsVO = (PostsVO) o;
        return Objects.equals(id, postsVO.id) && Objects.equals(title, postsVO.title) && Objects.equals(content, postsVO.content) && Objects.equals(author, postsVO.author);
    }

    @Override
    public int hashCode() {
        return Objects.hash(id, title, content, author);
    }
```

### 

### **DTO와 VO의 공통점과 차이점**

**공통점**

레이어 간 데이터를 전달할 떄 사용 가능

- VO는 불변을 보장하기 때문에 데이터 전달 용도로 사용 가능

**차이점**

| DTO | VO |
| --- | --- |
| 값이 변할 수 있음(가변 객체) | 값이 변하지 않음(불변 객체) |
| 레이어와 레이어 사이에서 사용 가능 | 모든 레이어에서 사용 가능 |
| 내부의 속성(필드) 값이 같아도 다른 객체로 식별 | 내부의 속성(필드)값들이 같다면 같은 객체로 식별 |
| 데이터 접근 이외의 기능을 가지지 않음 | 특정한 비즈니스 로직을 가질 수 있음 |

---

참고:

[https://parkadd.tistory.com/53](https://parkadd.tistory.com/53)

[https://dkswnkk.tistory.com/500](https://dkswnkk.tistory.com/500)

---
# **Mock**

Mock은 한글로 “모의, 가짜의”라는 뜻으로 테스트할 때 필요한 실제 객체와 동일한 모의 객체를 만들어 테스트의 효용성을 높이기 위해 사용된다.

```java
@Service
public class StudentService {

    public Student getStudent() {
// DB에서 Student 테이블 조회 (부하가 많이 걸리는 작업)
    }
}

```

위처럼 DB에서 Student 테이블을 읽어 Student 객체를 리턴하는 메소드를 테스트하고 싶은데 매번 테스트할 때마다 DB를 읽어오는 것이 부하가 많이 걸리고, 시간도 많이 걸린다면 번거로워 질 것이다. 그래서 Student를 DB에서 읽어오지 않고 가짜 객체 즉, mock을 만들어서 DB에 있는 테이블 접근을 최소화할 수 있다.

![https://blog.kakaocdn.net/dn/b7CxXE/btsfeAlXMku/dNOInffbb7xVIO4BtMu4qk/img.png](https://blog.kakaocdn.net/dn/b7CxXE/btsfeAlXMku/dNOInffbb7xVIO4BtMu4qk/img.png)

mock으로 객체를 만들면 테스트 시간을 줄이고, 불필요한 소비를 막고 객체의 행동까지 개발자 마음대로 조정할 수 있으니 테스트할 때 매우 유익한 테스트 방법이다.

JetBrains이 발표한 2020년 통계에 따르면 유닛 테스트를 하는 자바 개발자 중 43%가 mockito를 사용하고 있다고 한다.

![https://blog.kakaocdn.net/dn/cgDNvC/btsfegHUxhW/Bb3QKym6SRSqyuXFxzXNjK/img.png](https://blog.kakaocdn.net/dn/cgDNvC/btsfegHUxhW/Bb3QKym6SRSqyuXFxzXNjK/img.png)

---

# **Mockito란?**

mock을 쉽게 만들고 mock의 행동을 정하는 stubbing, 정상적으로 작동하는지에 대한 verify등 다양한 기능을 제공해주는 프레임워크이다.

주로 Stub이라는 기술을 사용한다.

Stub이란 메소드의 결과를 미리 지정하는 것이다.

```java
Optional<User> user = Optional.of(User.builder()
                .email("YD@naver.com")
                .password("zzzz")
                .build());
when(userRepository.findById(anyLong())).thenReturn(user);
```

userRepositoy.findById의 어떤 Long값이 들어가도 결과는 미리 선언한 user가 될 것이다.

Mockito를 사용하려면 테스트 클래스 위에 @ExtendWith(MockitoExtension.class)를 붙혀줘야 한다.

```java
@ExtendWith(MockitoExtension.class)
public class UserServiceTest {

}
```

### **Mock 생성**

mock 생성 관련 어노테이션은 @Mock, @Spy, @InjectMock 등이 있다.

### **@Mock**

@Mock으로 만든 mock 객체는 가짜 객체이며 그 안에 메소드 호출해서 사용하려면 반드시 스터빙(stubbing)을 해야 한다.

만약, 스터빙을 하지 않고 그냥 호출한다면 primitive type은 0, 참조형은 null을 반환한다.

### 

### **User 도메인 생성**

```java
import jakarta.persistence.*;
import lombok.*;

@Entity
@Table(name = "users")
@Builder
@AllArgsConstructor
@Getter
@NoArgsConstructor(access = AccessLevel.PROTECTED)
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "user_id")
    private Long id;
    private String email;
    private String password;
}
```

### **UserRepository 생성**

```java
public interface UserRepository extends JpaRepository<User, Long> {
    Optional<User> findByEmail(String email);
    Optional<User> findById(Long id);

}
```

### **UserService 생성**

```kotlin
@Service
@RequiredArgsConstructor
public class UserService {

    private final UserRepository userRepository;

    public Optional<User> findUser(Long id) {
        Optional<User> user = userRepository.findById(id);
        return user;
    }
}

```

### **오직 userService만을 테스트**

저장소에서 어떤 값이 나오는지 예측을 미리 하고 그 값이 나오면, UserService의 결과가 이것일 것이다.라는 형식의 테스트를 작성한다.

기본적으로 UserService를 테스트 하기 위해서는 필수적으로 userRepository를 생성자 주입을 시켜줘야하기 때문에 다음과 같이 셋팅을 해준다.

```java
@ExtendWith(MockitoExtension.class)
public class UserServiceTest {

    UserService userService;

    @Mock
    UserRepository userRepository;

    @BeforeEach//test실행전 이것부터 실행void setUp() {
        this.userService = new UserService(userRepository);
    }
```

우리는 userRepository의 값을 예측할 것이기 때문에, Mock의 대상자는 userRepository가 된다.

다음과 같은 순서로 테스트 코드를 작성할 것이다.

- email은 [YD@naver.com](mailto:YD@naver.com), 비밀번호는 zzzz인 유저를 만들고 - given
- 미리 만들어진 user가 저장소에서 나왔을 때 - when
- 저장소에서 나온 user은 userService의 findUser을 통해 user을 찾았을 때 그 값은 동일한가? -then

```java
@Test
@DisplayName("mock_test")
void mock_test() {
//given
   Optional<User> user = Optional.of(User.builder()
            .email("YD@naver.com")
            .password("zzzz")
            .build());
//when
   when(userRepository.findById(anyLong())).thenReturn(user);
//then
   assertEquals(userService.findUser(1L).get(), user.get());
   System.out.println(user.get().getEmail());

   verify(userRepository,times(1)).findById(1L);
}
```

![https://blog.kakaocdn.net/dn/dN9eDc/btsfg3gV532/cZcrwoU58fjn4uNPaso2w0/img.png](https://blog.kakaocdn.net/dn/dN9eDc/btsfg3gV532/cZcrwoU58fjn4uNPaso2w0/img.png)

성공적으로 테스트 된 것을 볼 수 있다.

**메소드 설명**

| when | stub을 하는 구문 예측할. 메소드를 지정. |
| --- | --- |
| anyLong() | ‘어떠한 Long 값 이어도’ 라는 뜻을 가지고 있다. |
| thenReturn | 메소드의 결과값을 임의로 정한다. |
| verify | Mock 객체를 대상으로 검증한다. |

### **@InjectMocks**

@InjectMocks라는 어노테이션을 사용한다면 해당 클래스가 필요한 의존성과 맞는 Mock 객체들을 감지하여 해당 클래스의 객체가 만들어질 때 사용하여 객체를 만들고 해당 변수에 객체를 주입하게 된다.

이전과 다르게

```java
@ExtendWith(MockitoExtension.class)
public class UserServiceTest {

    UserService userService;

    @Mock
    UserRepository userRepository;

    @BeforeEach//test실행전 이것부터 실행void setUp() {
        this.userService = new UserService(userRepository);
    }
}
```

```
@ExtendWith(MockitoExtension.class)
public class UserServiceTest {
	@InjectMocks
	UserService userService;
	@Mock
	UserRepository userRepository;

}
```

의존성 주입을 하는 코드가 없어진 것을 볼 수 있다.

### **@Spy**

@Spy로 만든 mock 객체는 진짜 객체이며 메소드 실행 시 스터빙을 하지 않으면 기존 객체의 로직을 실행한 값을, 스터빙을 한경우엔 스터빙 값을 리턴한다.

```java
@Service
public class SpyUserService {

    public User getUser() {
        return new User.UserBuilder()
                .email("YD@naver.com")
                .password("zzzz")
                .build();
    }
}

```

다음과 같이 SpyUserService를 만들고 Test를 하게 되면

```
@Spy
SpyUserService spyUserService;

@Test
void testSpy_스터빙X() {
    User user = spyUserService.getUser();

    assertEquals("YD@naver.com", user.getEmail());
}

@Test
void testSpy_스터빙O() {
    User userDummy = User.builder()
                .email("spy@naver.com")
                .password("zzzz")
                .build();
    when(spyUserService.getUser()).thenReturn(userDummy);
    User user = spyUserService.getUser();
    assertEquals(userDummy.getEmail(), user.getEmail());
}

```

스터빙을 하지 않았을 때는 기존의 값이, 스터빙을 했을 땐 스터빙한 값이 나오는 것을 볼 수 있다.

![https://blog.kakaocdn.net/dn/cnwZkd/btsfehNzai5/PyKtpctHJOiH6eS6Q0gwU0/img.png](https://blog.kakaocdn.net/dn/cnwZkd/btsfehNzai5/PyKtpctHJOiH6eS6Q0gwU0/img.png)

결론 : Mockito, 그리고 Controller를 테스트할 수 있게 해주는 MockMVC 등을 활용해서 하나의 서비스 또는 컨트롤러를 테스트하고 싶거나, DB를 실행해서 테스트하기 부담스러울 때, 아직 개발이 되지 않은 부분 (예를들어 repository는 개발이 안된 service)이 있다면 mock(가짜)를 활용해서 테스트를 하면 좋을 것 같다.

---

참고:

[https://effortguy.tistory.com/142](https://effortguy.tistory.com/142)

[https://www.nextree.io/mockito/](https://www.nextree.io/mockito/)

[https://doyoung.tistory.com/12](https://doyoung.tistory.com/12)