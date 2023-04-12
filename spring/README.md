
<details>
<summary>Table of Contents</summary>

- [DAO vs DTO vs VO](#dao-vs-dto-vs-vo)

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