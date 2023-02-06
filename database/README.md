<details>
<summary>Table of Contents</summary>

- [데이터베이스](#데이터베이스)
- [릴레이션과 키](#릴레이션--relation--과-키--key-)

</details>

# 데이터베이스
## 데이베터이스 시스템(DBMS)을 쓰는 이유

**파일 정보 시스템의 문제점**: 
- 데이터 중복성, 비일관성
- 검색의 비효율성
- 데이터들간에 연결성 부족(여러 파일에 같은 종류의 데이터가 흩어져있을 수 있음)
- 동시 접근 불가능 및 데이터 무결성 부족(여러 사용자가 접근할 때 데이터에 손상이 생길 수 있다)
- 복구 어려움

**데이터베이스 관리 시스템(DBMS)의 장점**:  
- 데이터의 독립성   
물리적 독립성 : 데이터베이스 사이즈를 늘리거나 성능 향상을 위해 데이터 파일을 추가하더라도 관련된 응용 프로그램을 수정할 필요가 없다.   
논리적 독립성 : 데이터베이스는 논리적인 구조로 다양한 응용 프로그램의 논리적 요구를 만족시켜줄 수 있다.
- 데이터 종속성 최소화  
데이터 저장 방식과 접근 방식에 변화가 생기더라도 관련된 응용 프로그램 수정의 부담이 훨씬 적다.  
- 데이터 중복성 최소화  
데이터베이스 안에 전체 데이터를 통합 관리하기 때문에 데이터 중복을 방지할 수 있다. (성능 향상이 필요할 땐 중복 허용)
- 데이터 동시 공유 가능
- 일관성 유지가 쉬움. 연관된 정보들을 논리적인 구조로 관리함으로서 어떤 데이터의 변경에 따라 발생하는 데이터의 불일치를 제거할 수 있다.  
- 보안성 : 인가된 사용자들만 데이터베이스나 데이터베이스 내의 자원에 접근할 수 있도록 계정 관리 또는 접근 권한을 설정

데이터베이스의 단점:
- 운영 비용 증가
- 중앙화에 따른 취약성

|  | 파일시스템 | 데이터베이스 |
| --- | --- | --- |
| 구조 | 특정한 구조가 존재 X | TABLE 형태로 저장. row(tuple) 단위 |
| 중복 | O | 비교적 덜 발생 |
| 불일치 | O | 비교적 덜 발생 |
| 일관성 유지 | 어려움 | 쉬움(정규화) |
| 무결성 유지 | 어려움 | 쉬움 |
| 데이터 검색 효율성 | X | O |
| 트랜잭션 | X | O |
| 다중 사용자의 접속 | X | O |
| 보안 및 유저 권한 | 각 프로그램마다 데이터를 가지고 있기 때문에 보안이 까다로움. | 사용자들마다 다른 권한 부여 가능(ㅊ |

## 관계형 데이터베이스 시스템(RDBMS)
![](https://miro.medium.com/v2/resize:fit:828/format:webp/1*PNtpYf2WHEd_r20L6adqnA.png)
> **테이블** 형태의 저장구조를 가지며 데이터들 사이의 연관 **관계**를 테이블의 **외래키(foreign key)** 를 통해 표현하는 저장방식.

- **테이블** : row(record)와 column(field)로 이뤄진 기본 데이터 저장 단위
- 관계형 데이터베이스 : 상호 관련성을 가진 테이블들의 **집합**
- **정규화**를 통해 중복 데이터를 최소화
- 테이블간의 관계(**JOIN**)를 이용해 관계에 따라 여러 데이터를 검색

## 데이터베이스 언어(SQL)
SQL은 RDBMS의 데이터를 관리하기 위해 설계된 특수목적의 프로그래밍 언어다.  
DBMS의 필수 기능에는 정의, 조작, 제어가 있다.  
- 정의 : 데이터베이스 구조를 생성하거나 변경, 삭제
- 조작 : 데이터베이스 안에 저장된 데이터에 접근해 생성, 수정, 삭제 및 검색
- 제어 : DBMS는 여러 사용자가 동시에 접근하더라도 항상 데이터를 정확하고 안전하게 유지하도록 통제한다. 

SQL은 여기에 TCL을 더해 4가지 종류가 있다.   

![](https://f4n3x6c5.stackpathcdn.com/UploadFile/BlogImages/09252014061246AM/command%20types%20in%20SQL%20DataBase.jpg)

### DDL (데이터 정의 언어, Data Definition Language)


DB 객체 (TABLE, INDEX, VIEW 등)를 생성, 수정, 삭제할 목적으로 사용되는 언어.   
CREATE, ALTER, DROP, RENAME 등이 있다.  

> CREATE 학생(학번, 이름, 학년, 성별, 전공)


![](https://d1whtlypfis84e.cloudfront.net/guides/wp-content/uploads/2019/01/01111318/Relational-databse.png)

데이터베이스 구조는 스키마(schema)라고도 한다.    
테이블 이름과 열 이름으로 표현될 수 있다.

### DML (데이터 조작 언어, Data Manipulation Language)
데이터베이스 안의 데이터를 실제 조작하는 언어.     
SELECT, INSERT, UPDATE, DELETE 등이 있다.  

### DCL (데이터 제어 언어, Data Control Language) 
데이터베이스를 제어하기 위한 언어.  
데이터베이스가 안정적으로 동작하고 성능을 유지하도록 각종 제약이나 옵션을 설정함으로써  
DBMS가 데이터베이스를 올바르게 관리하도록 한다.  


GRANT, REVOKE, CREATE USER 등이 있다.  

### TCL (Transaction Control Language) : 트랜잭션 제어어
DML 명령문으로 수행한 변경을 관리 (트랜잭션 관리)하는 언어.    
COMMIT, ROLLBACK, SAVEPOINT 등이 있다.  

# 릴레이션(Relation)과 제약조건(Constraint)
> 그래서 관계형 데이터베이스는 데이터를 **어떻게** 저장하는데? 

## 관계형 데이터 모델
> 관계형 데이터베이스는 관계형 데이터 모델의 개념을 쓰는데,  
관계형 데이터 모델은 **릴레이션**(relation)이라는 **수학적 집합** 개념에 기초하고 있다.


릴레이션(relation)은 동일한 구조로 이뤄진 **튜플(tuple)의 집합**이고 **2차원 테이블(table)** 형태의 단순한 구조다.  
(테이블 개념은 내부 저장 구조에 대한 추상적인 표현일 뿐이고, 물리적으로는 복잡한 구조 속에 데이터가 저장된다.)  

관계형 데이터베이스는 데이터들을 릴레이션의 형태로 저장한다.

## 릴레이션 용어 정리
`Student`라는 릴레이션을 상상해보자. 

| 학번  | 이름 | 학과   | 나이 | 전화번호      | MBTI |
|-----|----|------|----|-----------|------|
| 170 | 원석 | 컴퓨터SW | 27 | 010123123 | INTP |
| 180 | 영두 | 컴퓨터SW | 24 | 010456456 | ISTJ |
| 181 | 성욱 | 정보보호 | 24 | 010789789 | INFP |


- **속성**(attribute) : 데이터를 정의하는 **가장 기본적인 단위**. 테이블의 열(column)에 해당  
e.g. `이름`은 `Student` 릴레이션의 속성


- **튜플**(tuple, record) : 테이블이라는 틀로 찍어낸 각각의 개체를 의미하며 테이블의 각 행(row)에 해당  
e.g. `180-영두-컴퓨터SW-24-010456456-ISTJ` 는 하나의 튜플 


- **속성 도메인**(attribute domain) : 각 속성이 취할 수 있는 모든 값들의 집합을 정의한 것.    
e.g. `나이` 속성의 집합은 자연수값만을 가져야 한다.  


- 차수(degree) : 릴레이션을 구성하는 전체 속성의 개수. 각 튜플이 가지는 속성값의 개수는 해당 릴레이션의 차수와 같다.  
e.g. `Student` 릴레이션의 차수는 5


<details>
<summary>학생 테이블 생성 (MYSQL)</summary>

```mysql
CREATE TABLE Student(
  학번    INT,
  이름    CHAR,
  학과    CHAR,
  나이    INT,
  전화번호 CHAR,
  MBTI   CHAR
)
```  
</details>


## 키(Key)

> 특정 튜플을 다른 튜플들과 **구별**할 수 있는 기준이 되는 속성

키(Key)는 조건에 만족하는 튜플을 찾거나, 순서대로 정렬할 때 쓰인다.

그리고 각각의 튜플을 구별하기 위해 유일성과 최소성이라는 개념이 등장한다.  

### 유일성
> Key로 하나의 튜플을 **유일하게 식별**할 수 있는 성질


e.g. `Student`의 `나이`, `MBTI`, `이름`, `학과`는 모두 중복 가능.  
반면 `학번`은 모두 다르기 때문에 `Student`에서 유일성을 갖는 키이다.


### 최소성
> Key를 구성하고 있는 속성들이 정말 각 튜플을 구분하는 데 **꼭 필요한** 속성들로만 구성되어 있는지

e.g. `(학번,이름)`을 키로 지정하면, 튜플을 구별하는데엔 문제 없다.  
그러나 이미 `학번`만으로 튜플을 구분할 수 있기 때문에 최소성을 만족하지 않는다. 


### 슈퍼키(Super Key)
> **유일성** O 최소성 X

투플을 유일하게 식별할 수 있는 속성들의 집합.  
꼭 필요한 속성이 아니어도 포함된다.

e.g. `(학번,이름,나이,MBTI)`는 유일성을 당연히 만족하지만.. TMI 임.  

<img src="https://velog.velcdn.com/images/duck-ach/post/8b2ea421-f688-4d37-896c-8eecdf1bb68b/image.png" width="50%" height="50%">


### 후보키(Candidate Key)
> **유일성 O 최소성 O**  
> PK로 쓸 수 있는 속성들.  

e.g. `학번`, `전화번호`, `주민번호`

### 기본키(Primary Key)
> 튜플을 **대표**하도록 선정된 후보키.

조건 만족:
- NULL값을 가질 수 없음(**NOT NULL**)
- 튜플들간에 중복된 값을 가질 수 없음(**UNIQUE**)


### 대체키(Alternate Key)
> 기본키로 **선정되지 못한** 후보키. 

e.g. `전화번호`도 PK가 될 수는 있지만, `학번`을 이미 PK로 선택했기 때문에 후보키가 됐음


### 외래키(Foreign Key)
> 외부 릴레이션의 키를 **참조**하는 키

외래키의 값은 참조될 다른 릴레이션의 **기본키(PK)** 중에서 하나를 골라 정한다.


## 제약 조건(Constraint)
> 데이터 **무결성(integrity)** 은 데이터베이스에 저장된 데이터의 **일관성**과 **정확성**을 지키는 것을 말한다.
### 개체 무결성(PK constraint)
> 기본키(PK)로 지정한 모든 속성은 **NULL값을 가질 수 없고** 릴레이션 안에서 중복되지 않는 **유일한 값**을 가지도록 하는 제약 조건


개체의 **유일성**을 위해, 왠만하면 릴레이션마다 PK를 정의해야 한다.  
보통은 테이블을 생성할 때 `PRIMARY KEY(기본키)`를 선언함으로써 적용된다.  

e.g. `학번` 없는 학생, 중복된 `학번`의 학생은 없다.

<details>
<summary>PK 지정 (MYSQL)</summary>

```mysql
CREATE TABLE Student(
  학번    INT PRIMARY KEY ,
  이름    CHAR,
  학과    CHAR,
  나이    INT,
  전화번호 CHAR,
  MBTI   CHAR
)
```  
</details>

### 참조 무결성(FK constraint)
> 외래키(FK)로 지정한 속성은 참조하는 릴레이션의 **기본키(PK) 값과 일치**하거나 **NULL값**을 가지도록 하는 제약 조건

어떤 개체의 외래키가 NULL 값을 갖는다는건 관련된 개체가 없음을 의미한다.   
반면에 어떤 값을 갖는다면, 그 값은 반드시 관련된 개체의 기본키값과 일치해야 한다.  
만약 관계된 개체를 삭제하려면, 해당 개체를 참조중인 개체를 먼저 삭제하거나 관계를 끊어야 한다.   

전공을 나타낼 `Major` 테이블을 상상해보자.

| CODE  | 전공명  |
|-------|------|
| CS    | 컴퓨터SW |
| IS    | 정보보호 |
| MS    | 미디어SW |
| IC    | 정보통신 |


<details>
<summary>FK 지정 (MYSQL)</summary>

```mysql
CREATE TABLE Major (
  CODE  INT,
  전공명 CHAR
);

CREATE TABLE Student (
  # ...
  전공_코드  INT, # 이제 학과명 대신 전공_코드(FK)로 관계가 맺어짐
   FOREIGN KEY (`전공_코드`) REFERENCES `Major`(`CODE`)
  #... 
);
```  
</details>


### 속성 도메인(domain constraint)
> 튜플의 모든 속성 값이 각 속성 **도메인에 속한 값만을 취하도록** 하는 제약 조건. 

SQL에서 테이블 생성시 각 열의 타입을 명시하거나  
`NULL` 혹은 `NOT NULL`, `DEFAULT(디폴트 값)`, `CHECK(값 범위 체크 조건)` 등의 키워드 설정을 써서 제약을 명시한다.  

<details>
<summary>속성 도메인 (MYSQL)</summary>

```mysql
CREATE TABLE Student(
  학번        INT UNSIGNED PRIMARY KEY ,
  이름        CHAR NOT NULL ,
  전공_코드    CHAR(45) NOT NULL ,
  나이        INT UNSIGNED,
  전화번호     CHAR(20),
  MBTI       CHAR(4)
)
```  
</details>


### 유일성(uniqueness constraint)
> **키** 속성 값이 서로 중복되지 않고 **유일하도록** 하는 제약 조건.

SQL에서 테이블 생성시에 `UNIQUE(유일 조건)` 키워드 설정을 써서 명시한다.  

<details>
<summary>UNIQUE 지정 (MYSQL)</summary>

```mysql
CREATE TABLE Student(
  #...
  전화번호     CHAR(20) UNIQUE,
  MBTI       CHAR(4)
)
```  
</details>

# JOIN

## JOIN이란?

> 둘 이상의 릴레이션에 흩어져 있는 튜플들을 특정 조건으로 **조합**하여 **하나의 릴레이션을 구성**하도록 **검색**(`SELECT`)하는 방법.

조인은 릴레이션들의 **공통 속성**을 기준으로 하므로 테이블들간에 최소한 하나의 속성을 공유하고 있어야 한다.

여러가지 조인 **조건**에 따라 검색 결과를 다르게 할 수 있다.

아래는 업데이트된 `과목`, `수강` 테이블.

| 과목번호 | 과목명 | 강의 교수 |
| --- | --- | --- |
| 001 | 컴퓨터구조 | 장성태 |
| 002 | 정보보호개론 | 양수미 |
| 003 | 디지털논리설계 | 장성태 |
| 004 | 네트워크보안 | 양수미 |

| 학번 | 과목번호 | 학점 |
| --- | --- | --- |
| 180 | 001 | B |
| 181 | 002 | B+ |
| 180 | 003 | A+ |
| 181 | 004 | A |

## 크로스 조인(CROSS JOIN)

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile10.uf.tistory.com%2Fimage%2F993F4E445A8A2D281AC66B" width="30%" height="30%">


테이블 행들 사이의 **모든 조합**을 행으로 갖는 하나의 통합 테이블을 만들어낸다. 조인 조건식 없이 이루어진다. (관계 대수의 카티션 프로덕트 연산, 곱집합의 결과.)

대부분의 행이 의미 없는 결합인 경우가 많다.

```sql
SELECT * FROM 학생, 수강;
```

```sql
SELECT * FROM 학생 
	CROSS JOIN 수강;
```

| 학번 | 이름 | 주전공_ID | 복수전공_ID | 학번 | 과목번호 | 학점 |
| --- | --- | --- | --- | --- | --- | --- |
| 180 | 영두 | 9 | 2 | 180 | 001 | B |
| 181 | 성욱 | 1 | NULL | 180 | 001 | B |
| 180 | 영두 | 9 | 2 | 181 | 002 | B+ |
| 181 | 성욱 | 1 | NULL | 181 | 002 | B+ |
| 180 | 영두 | 9 | 2 | 180 | 003 | A+ |
| 181 | 성욱 | 1 | NULL | 180 | 003 | A+ |
| 180 | 영두 | 9 | 2 | 181 | 004 | A |
| 181 | 성욱 | 1 | NULL | 181 | 004 | A |

## 내부 조인(INNER JOIN)

<img src="https://blog.codinghorror.com/content/images/uploads/2007/10/6a0120a85dcdae970b012877702708970c-pi.png" width="30%" height="30%">

**교집합**. 두 테이블 모두에서 조건을 만족하는 튜플을 가져온다.

크로스조인과는 달리 조건이 있기 때문에 의미 있는 조합만을 검색할 수 있다.

```sql
SELECT * FROM 학생 
	INNER JOIN 수강 ON 학생.학번 = 수강.학번;
```

```sql
SELECT * FROM 학생, 수강 
	WHERE 학생.학번 = 수강.학번;
```

| 학번 | 이름 | 주전공_ID | 복수전공_ID | 학번 | 과목번호 | 학점 |
| --- | --- | --- | --- | --- | --- | --- |
| 180 | 영두 | 9 | 2 | 180 | 001 | B |
| 181 | 성욱 | 1 | NULL | 181 | 002 | B+ |
| 180 | 영두 | 9 | 2 | 180 | 003 | A+ |
| 181 | 성욱 | 1 | NULL | 181 | 004 | A |

## 셀프 조인(SELF JOIN)

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile25.uf.tistory.com%2Fimage%2F99341D335A8A363D0614E8" width="30%" height="30%">

**같은 테이블**에 속하는 행들끼리 하는 조인.

자신이 갖고 있는 속성을 다양하게 변형시켜 활용할 때 쓰인다. (e.g. 쌍 만들기 )

```sql
SELECT c1.과목명, c2.과목명
	FROM 과목 c1 JOIN 과목 c2 ON c1.강의교수 = c2.강의교수
	WHERE c1.과목번호 < c2.과목번호
```

| 과목번호 | 과목명 | 강의 교수 |
| --- | --- | --- |
| 001 | 컴퓨터구조 | 장성태 |
| 002 | 정보보호개론 | 양수미 |
| 003 | 디지털논리설계 | 장성태 |
| 004 | 네트워크보안 | 양수미 |
| 005 | 졸업프로젝트 | 장성태 |

| 과목명 | 과목명 |
| --- | --- |
| 컴퓨터구조 | 디지털논리설계 |
| 디지털논리설계 | 네트워크보안 |

## OUTER JOIN이란?

일반적인 조인인 내부 조인을 하게 되면 조인 조건을 만족하는 행들만 결과에 포함된다.

때로는 조인 **대상 테이블의 모든 행들이 결과에 포함**되기를 원하는 경우도 있다. 그럴 때 외부 조인(OUTER JOIN)을 사용한다.

외부조인의 결과로 몇몇 매칭되지 않는 컬럼에서는 NULL 값을 가질 수 있다.

조인 결과에 포함되기를 원하는 릴레이션의 대상에 따라 OUTER JOIN은 **3가지**(LEFT, RIGHT, FULL)로 분류한다.

## LEFT OUTER JOIN

<img src="https://i.stack.imgur.com/VkAT5.png" width="30%" height="30%">

`JOIN` 연산자의 **왼쪽** 테이블의 **모든 행**들이 빠짐없이 결과에 포함된다.

`Student` 릴레이션에 아무 과목을 듣지 않는 두 학생이 추가됐다고 가정해보자.

| 학번 | 이름 |
| --- | --- |
| 180 | 영두 |
| 181 | 성욱 |
| 182 | 진욱 |
| 183 | 원석 |

```sql
SELECT 학생.학번, 이름, 학점 FROM 학생 
	LEFT JOIN 수강 ON 학생.학번 = 수강.학번;
```

| 학번 | 이름 | 학점 |
| --- | --- | --- |
| 180 | 영두 | B |
| 181 | 성욱 | B+ |
| 180 | 영두 | A+ |
| 181 | 성욱 | A |
| 182 | 진욱 | NULL |
| 183 | 원석 | NULL |

## RIGHT OUTER JOIN

<img src="https://media.geeksforgeeks.org/wp-content/uploads/20220515095048/join.jpg" width="30%" height="30%">


`JOIN` 연산자의 **오른쪽** 테이블의 모든 행들이 빠짐없이 결과에 포함됨.

`Student` 릴레이션에 수업을 들었던 두 학생이 삭제됐다고 가정해보자.

```sql
SELECT 학생.학번, 이름, 학점 FROM 학생 
	RIGHT JOIN 수강 ON 학생.학번 = 수강.학번;
```

| 학번 | 이름 | 학점 |
| --- | --- | --- |
| NULL | NULL | B |
| NULL | NULL | B+ |
| NULL | NULL | A+ |
| NULL | NULL | A |

## FULL OUTER JOIN

<img src="https://i.stack.imgur.com/3Ll1h.png" width="30%" height="30%">

**합집합**. 두 테이블의 모든 데이터가 검색된다. LEFT JOIN과 RIGHT JOIN의 결과를 합친 것과 같다.

| 학번 | 이름 | 학점 |
| --- | --- | --- |
| NULL | NULL | B |
| NULL | NULL | B+ |
| NULL | NULL | A+ |
| NULL | NULL | A |
| 182 | 진욱 | NULL |
| 183 | 원석 | NULL |


# TODO
- TCL 보충.. 트랜잭션 모름
- 데이터 독립성? 응용 프로그램과 데이터의 관계
- SQL 코테 문제랑 연결해보기

# 출처
- 데이터베이스의 정석 - 박성진
- https://www.geeksforgeeks.org/difference-between-file-system-and-dbms/
- https://towardsdatascience.com/designing-a-relational-database-and-creating-an-entity-relationship-diagram-89c1c19320b2
- https://ko.wikipedia.org/wiki/SQL
- https://victorydntmd.tistory.com/126
- http://wiki.hash.kr/index.php/%EC%88%98%ED%8D%BC%ED%82%A4
- https://www.geeksforgeeks.org/relational-model-in-dbms/
- [https://gyoogle.dev/blog/computer-science/data-base/Join.html](https://gyoogle.dev/blog/computer-science/data-base/Join.html)
- [https://www.geeksforgeeks.org/sql-join-set-1-inner-left-right-and-full-joins/](https://www.geeksforgeeks.org/sql-join-set-1-inner-left-right-and-full-joins/)
