<details>
<summary>Table of Contents</summary>

- [데이터베이스](#데이터베이스)
- [릴레이션과 키](#릴레이션--relation--과-키--key-)
- [RDBMS와 NOSQL 차이점](#rdbms-vs-nosql)

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
)
```  

```mysql
CREATE TABLE Student (
  # ...
  전공_코드  INT, # 이제 학과명 대신 전공_코드(FK)로 관계가 맺어짐
   FOREIGN KEY (`전공_코드`) REFERENCES `Major`(`CODE`)
  #... 
)
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

# 정규화(Normalization)

## 데이터 이상(data anomaly)


: 릴레이션 처리 과정에서 **불필요한 데이터 중복**으로 인해 발생하는 **부작용**

일단 아무 생각 없이 릴레이션을 짰을 때 무슨 문제가 생기는지 알아보자. 여기 `Student` 릴레이션이 있다.

| 학번 | 이름 | 학과 | 학과 위치 | 나이 |
| --- | --- | --- | --- | --- |
| 170 | 원석 | 컴퓨터SW | 304호 | 27 |
| 180 | 영두 | 컴퓨터SW | 304호 | 24 |
| 181 | 성욱 | 정보보호 | 303호 | 24 |

얼핏 보면 별 문제 없어보이고 검색(SELECT)까지만 해도 전혀 문제가 없지만 삽입, 갱신, 삭제시에 치명적인 문제가 발생한다.

### 삽입 이상


: **불필요한 데이터나 거짓 데이터**를 함께 입력하지 않고서는 원하는 데이터의 **입력이 불가능**한 상황.

신생 학과로 알고리즘 학과와 그 사무실이 생긴 상황을 가정해보자.

(’알고리즘’, ‘ICT 대학 306호’)라는 정보를 릴레이션에 추가해야 하는데, 릴레이션의 기본키인 `학번` 의 값이 비어있게 된다. 기본키의 값은 NULL이 될 수 없기 때문에, 억지로 `학번` 에 거짓 정보를 넣거나 거짓 학생을 넣어서라도 강제 삽입해야 한다.

### 갱신 이상


: 중복된 속성 값 중 일부가 수정되지 않을 경우 생기는 **데이터 불일치**

컴퓨터 SW 학과의 사무실 위치가 501호로 바뀐 경우를 가정해보자.

학과가 ‘컴퓨터SW’인 튜플 모두를 빠짐없이 갱신해야 한다(이미 여기서부터 비효율적). 게다가 만약 DB 관리자의 실수로 ‘원석’ 튜플만 갱신했다면, ‘영두’ 튜플의 학과 위치는 여전히 ‘304호’로 남게 되고 이는 ‘원석’ 튜플의 학과 위치 정보와 불일치하게 된다.

### 삭제 이상


: 원하지 않는 유용한 데이터까지 함께 삭제되어 **데이터 손실**이 발생할 수 있는 상황

‘성욱’ 튜플이 사라지는 경우(!)를 가정해보자. 학번 `181` 인 튜플을 찾아 삭제하게 되는데, 정보보호학과의 과 사무실 위치가 ‘303호’라는 유용한 정보도 함께 삭제된다.

### 정규화의 개념

기본적으로 **연관성**을 갖는 속성끼리 **그룹화**해서 하나의 릴레이션으로 구성하는게 바람직하다.

정리해보면, 속성 사이의 많은 연관 관계를 무리하게 하나의 릴레이션으로 우겨넣을 때 이상 현상이 발생한다. 이를 방지하기 위해 속성 사이의 **연관 관계**(**함수** **종속성**, dependency)를 분석한 뒤에 단계적으로 **릴레이션을 분해**하는 과정인 정규화가 필요하다.

- 중복성 최소화
- 데이터 모형 단순화
- 일관성, 정확성 보장
- 유연한 데이터 구축
- 조인 연산 비용 증가 (조회 성능 저하)

## 정규형(Normal Form)


: **정규화 과정**에서 릴레이션이 만족해야 하는 특정한 **함수 종속성의 충족 조건**

1NF ~ 5NF까지 있지만, 현실적으로 보통은 3NF까지만 정규화를 하기 때문에 3NF까지만 다뤘다.

### 제1정규형(1NF)


: 어떤 릴레이션에 속한 모든 속성들의 **도메인**이 **원자값**만을 가지는 정규형 (속성당 하나의 값만)

| 학번 | 이름 | 학과 | 학과 위치 |
| --- | --- | --- | --- |
| 180 | 영두 | 컴퓨터SW, 글로벌 비즈니스 | 304호, 606호 |
| 181 | 성욱 | 정보보호 | 303호 |

속성 하나에 복수의 값이 들어간다면 애초에 **릴레이션의 정의에 위배**된다. 여기선 학과 속성을 ‘주전공’, ‘복수전공’으로 쪼개서 해결할 수 있다.

| 학번 | 이름 | 주전공 | 복수전공 | 주전공 위치 | 복수전공 위치 |
| --- | --- | --- | --- | --- | --- |
| 180 | 영두 | 글로벌 비즈니스 | 컴퓨터SW | 304호 | 606호 |
| 181 | 성욱 | 정보보호 | NULL | 303호 | NULL |

하지만 여전히 **데이터의 중복**은 그대로다.

### 제2정규형(2NF)


: 1NF를 만족하고, 부분 함수 종속을 제거하여 기본키외의 모든 속성이 기본키에 **완전 함수 종속**인 정규형

즉, **기본키에 속한 모든 속성 값**(결정자)을 통해서만 일반 속성들을 결정할 수 있음을 의미한다. 기본키를 이루는 속성이 딱 하나라면 이미 만족된 것.

아래는 각 수업의 수강기록을 나타내는 `CourseRecord` 릴레이션이다. 기본키는 {학번, 과목번호} 조합이다.

| 학번 | 과목번호 | 학점 | 강의 교수 |
| --- | --- | --- | --- |
| 180 | a001 | C | 딜립 쿠말 |
| 181 | a002 | C++ | 양수미 |

‘학점’은  {학번, 과목번호} 조합을 통해서만 결정되므로 완전 함수 종속이다.

반면 ‘강의 교수’ 속성은 기본키의 일부인 ‘과목번호’ 속성만으로 결정되므로 부분 함수 종속이다. 따라서 위 릴레이션은 제 2정규형 조건을 충족하지 못한다.

두 릴레이션으로 쪼갠 다음, ‘강의 교수’를 `Cource` 릴레이션으로 보내면 해결된다.

| 학번 | 과목번호 | 학점 |
| --- | --- | --- |
| 180 | a001 | C |
| 181 | a002 | C++ |

| 과목번호 | 강의 교수 |
| --- | --- |
| a001 | 딜립 쿠말 |
| a002 | 양수미 |


### 제3정규형(3NF)


: 2NF를 만족하고, **이행적 함수 종속을 제거**하는 정규형.

기본키가 아닌 다른 속성이 결정자인지를 검사한다. 만약 일반 속성끼리 **직접적으로 종속적인 관계**를 갖고 있다면 제3정규형에 위배된다.

| 학번 | 이름 | 주전공 | 복수전공 | 주전공 위치 | 복수전공 위치 |
| --- | --- | --- | --- | --- | --- |
| 180 | 영두 | 글로벌 비즈니스 | 컴퓨터SW | 304호 | 606호 |
| 181 | 성욱 | 정보보호 | NULL | 303호 | NULL |

‘주전공 위치’, ‘복수전공 위치’는 각각 ‘주전공’, ‘복수전공’에 종속적이다. `학번 → 주전공 → 주전공 위치` 형태.

두 릴레이션으로 **쪼개면** 해결된다.

| 학번 | 이름 | 주전공_ID | 복수전공_ID |
| --- | --- | --- | --- |
| 180 | 영두 | 9 | 2 |
| 181 | 성욱 | 1 | NULL |

| ID | 이름 | 사무실 위치 |
| --- | --- | --- |
| 1 | 정보보호 | 303호 |
| 2 | 컴퓨터SW | 304호 |
| 9 | 글로벌 비즈니스 | 606호 |


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

# 인덱스(index)

## 인덱스란?


: 테이블 안의 데이터를 쉽고 **빠르게** **찾을 수 있도록** 만든 데이터베이스 객체.
책의 목차나 색인을 통해 더 빠르게 내용을 찾는것과 같은 개념이다.
특정 속성에 인덱스를 생성하면, 해당 속성에 대한 데이터들을 정렬해서 별도의 메모리 공간에 데이터의 **물리적 주소값**(디스크 블록의 주소값)과 함께 저장된다.

<details>
<summary>DB와 디스크</summary>

![https://user-images.githubusercontent.com/39291812/71639689-5cc45300-2cbe-11ea-9102-f7a9d0efe8b4.png](https://user-images.githubusercontent.com/39291812/71639689-5cc45300-2cbe-11ea-9102-f7a9d0efe8b4.png)  

DB 데이터가 **물리적**으로 저장되는 곳은 디스크(보조기억장치)이기 때문에, SQL 실행시 디스크로부터 필요한 데이터를 찾게 된다. DBMS는 필요한 디스크 **블럭**을 반복해서 주기억장치로 읽어오게 되는데, 문제는 **디스크 접근 속도**가 주기억장치보다 비교할 수 없을 정도로 **너무** **느리다**는 점이다.

가령 한 블록이 512byte라고 할 때, 100byte짜리 레코드의 정보를 모두 저장하는것보다는 그 레코드들의 PK값 (int라면 대충 4~8byte)만을 저장하는게 유리할 것이다.

DB의 테이블 데이터들은 모두 블록 단위로 관리된다. 만약 쿼리를 통해 1개의 레코드를 읽고 싶더라도 결국은 하나의 블럭을 읽어야 하는 식이다. 쿼리에서 읽으려는 데이터가 많을수록, 데이터들의 크기를 최대한 작게 해서 한 블럭에 최대한 많은 데이터들을 저장할 수 있도록 하는게 중요해진다.

인덱스를 통해 결과적으로 몇번의 디스크 블록 검색만으로 수많은 행들 중에서 원하는 행을 찾을 수 있다.

</details>




인덱스의 가장 큰 특징은, 사용자가 인덱스로 설정한 **key를 기준으로** 데이터들이 **미리 정렬**(오름차순 혹은 내림차순) 되어있다는 점.

![https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbPb8pb%2FbtrePWRO9HY%2FqrzMfX84KAAuFgkyZkKtKK%2Fimg.png](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbPb8pb%2FbtrePWRO9HY%2FqrzMfX84KAAuFgkyZkKtKK%2Fimg.png)

대부분의 DBMS는 **B-트리**(Balanced tree) **구조**의 인덱스를 지원한다. B-tree는 루트노드와 리프 노드까지의 탐색 길이가 같아서 모든 데이터에 대한 **일정 수준의 검색 시간을 보장**한다는 장점이 있다.

## 그래서 왜 쓰는데?

인덱스 덕분에, 데이터를 조건적으로 **검색**할 때 모든 행을 다 불러들어는게 아니라 **필요한 일부 행만** 주기억장치로 읽어들여올 수 있게 된다.

- **조건검색** (WHERE, JOIN) 절의 **효율성** : 테이블을 만들고 안에 데이터가 쌓이게 되면, 테이블의 레코드는 내부적으로는 순서가 없이 뒤죽박죽으로 저장된다. 만약 인덱스가 없다면 처음부터 끝까지 다 읽어서 검색 조건과 맞는지 비교해야 한다. 하지만 인덱스 테이블은 데이터들이 정렬되어 저장되어 있기 때문에 해당 조건 (Where)에 맞는 데이터들을 빠르게 찾아낼 수 있다. 이게 인덱스(Index)를 사용하는 가장 큰 이유다.

    ```mysql
    SELECT * FROM Student
    	WHERE major_id = 1;
    # 경영학과(1)인 학생들을 모두 조회
    
    SELECT * FROM Student
    	JOIN Lecture_Record ON lecture_id = 11;
    # C언어(11)에 대한 수강기록과 학생 정보를 한 테이블로 합쳐서 조회
    ```

- **정렬**(ORDER BY) 절의 **효율성**

  인덱스를 사용하면 ORDER BY에 의한 정렬 과정을 피할 수가 있다. 인덱스에 저장된대로 가져오기만 하면 끝.

    ```mysql
    SELECT * FROM Student
    	ORDER BY id; # index인 경우 : 미리 정렬되어있어서 속도가 빠르다
    # 1 영두,2 진욱,3 성욱, 4 원석 ...
    
    SELECT * FROM Student
    	ORDER BY MBTI; 
    # index가 아닌 경우 : 따로 정렬하는 과정 필요
    ```

- **MIN, MAX**의 **효율성**

  이것 또한 데이터가 정렬되어 있기에 얻을 수 있는 장점. MIN값과 MAX값을 레코드의 시작 값과 끝 값 한 건씩만 가져오면 되기 때문에 테이블을 모두 뒤져서 작업(full scan)하는 것보다 훨씬 효율적이다.


## 단점
- **추가적인 연산 오버헤드** : DBMS는 index들을 항상 **정렬된 상태**(b-tree의 균형)로 **유지해야** 원래의 빠른 속도가 나온다. 그래서 레코드내의 데이터 값이 바뀔 때 (INSERT, UPDATE, DELETE) 정렬하기 위한 연산을 추가적으로 해줘야 하며 그에 따른 오버헤드가 발생한다.
- 인덱스 자체를 위한 추가적인 저장공간 필요 (DB의 약 10%)

## 인덱스가 필요한 경우

- **조건절**에 자주 쓰이는 속성
- **PK**와 **FK** (대부분의 DBMS는 PK, FK, unique key에 대해 자동으로 인덱스를 생성한다)
- 항상 `=` 으로 비교**되는 컬럼 (범위 검색에는 바람직하지 않음)
- 정수형, 고정 길이 문자열 등의 열 (가변 길이 문자열이나 실수형, 날짜형 열에는 바람직하지 않음)
- 갱신이 빈번하지 않은 열
- 데이터가 많은 테이블일때

# 트랜잭션

## 트랜잭션(transaction)이란?

> **한 묶음으로 처리**되도록 만든 SQL 명령문들을 묶은 **작업** **단위**


대부분의 의미 있는 서비스 처리를 하려면 SQL 명령문 한번 (SELECT, UPDATE, …)만으로는 어렵다.

**계좌이체**라는 작업을 예시로 들어보자. 만약 X의 돈을 100 감소시키는 UPDATE 문 직후에 서버가 다운되면 어떻게 될까? 더 이상 서버에선 쿼리문을 날리지 못하니, Y에겐 100만큼의 돈이 가지 않고 회사가 X의 돈을 훔친 꼴이 된다(!).

그리고 갑작스러운 서버 다운, 네트워크 오류, 데이터센터 화재 등 데이터베이스의 **일관성**을 위협하는 요소들은 생각보다 많다.

이를 해결하기 위해선 계좌이체라는 하나의 처리를 { X 잔고 UPDATE 문,  Y 잔고 UPDATE 문 } 한 단위로 묶어서 모두 실행이 끝나야만 정상 처리가 되도록 해야 한다.

![https://media.geeksforgeeks.org/wp-content/uploads/11-6.jpg](https://media.geeksforgeeks.org/wp-content/uploads/11-6.jpg)

트랜잭션 과정중에 하나의 쿼리문이라도 오류가 있으면 전체를 **취소**(ROLLBACK)해야 한다.

- COMMIT : 트랜잭션의 실행 결과를 데이터베이스에 최종적으로 반영하는 것, 즉 DB가 일관성 있는 상태이므로 정상 종료하겠다는 것
- ROLLBACK : 실행 결과를 반영하지 않고 취소하여 원래 상태로 되돌리는 것

실제 트랜잭션 코드

```mysql
START TRANSACTION;

INSERT INTO Student values(18005678,'성욱','INFP');
INSERT INTO Student values(17000000,'진욱','ISTJ');
# 여기까진 성욱, 진욱 존재함
COMMIT;
# 성욱, 진욱 데이터 삽입 확정
```

```mysql
# 트랜잭션 시작, 아래 명령문들은 하나의 묶음으로 처리됨
START TRANSACTION;

INSERT INTO Student values(17001234, '1', '스펀지밥', 'ENFP', 1);
INSERT INTO Student values(17001235, '2', '징징이', 'ISTP', 1);

UPDATE Student 
  SET MBTI='ENFJ'
  WHERE 이름='스펀지밥';

SELECT * FROM Student;
# +----------+--------------+--------------+------+----------+
# | 학번      | 주민번호       | 이름         | MBTI  | major_id |
# +----------+--------------+--------------+------+----------+
# | 17001234 | 1            | 스펀지밥       | ENFJ |        1 |
# | 17001235 | 2            | 징징이         | ISTP |        1 |
# +----------+--------------+--------------+------+----------+
# 1 row in set (0.00 sec)

# 롤백
ROLLBACK;

# 스펀지밥, 징징이 사라짐.
SELECT * FROM Student;
# Empty set (0.00 sec)
```


## 트랜잭션의 특징 - ACID

### Atomicity(원자성)

![https://pynative.com/wp-content/uploads/2018/06/python_mysql_transction_management-e1530354220769.png](https://pynative.com/wp-content/uploads/2018/06/python_mysql_transction_management-e1530354220769.png)

> all or nothing!


트랜잭션 안의 SQL 명령문들은 DB에 **모두 반영**(COMMIT)되거나, 혹은 **전혀 반영되지 않거나**(ROLLBACK) 둘 중 하나의 결과만을 낳아야 한다.

### Consistency(일관성)

트랜잭션 처리 결과로 DB는 논리적으로 항상 **일관성** 있어야 한다.

![https://images.contentful.com/po4qc9xpmpuh/6jbcyfzdVJlc6XCUbzgMzb/96b1a9f0594f1d769f2254bd1abf25c1/database-transaction-1__1_.png](https://images.contentful.com/po4qc9xpmpuh/6jbcyfzdVJlc6XCUbzgMzb/96b1a9f0594f1d769f2254bd1abf25c1/database-transaction-1__1_.png)

트랜잭션 진행중에 데이터베이스가 변경 되더라도, 업데이트된 데이터베이스로 트랜잭션이 진행되는것이 아니라, 트랜잭션을 진행 하기 위해 **처음에** **참조한 데이터베이스로 진행**된다.

이렇게 함으로써 각 사용자는 일관성 있는 데이터를 볼 수 있게 된다.

### Isolation(독립성)

커밋되기 전까지 트랜잭션의 (임시) 실행 결과들은 다른 트랜잭션에게 **공개되지 않아야 한다**. 같은 데이터를 처리하려는 다른 트랜잭션들의 **간섭 방지**를 위해서이다.

동시 실행중이며 커밋되지 않은 각 트랜잭션은 변경 내용을 **락**을 통해 고립시켜 다른 트랜잭션의 접근을 방지한다.

### Durability(지속성)

트랜잭션이 성공적으로 완료(커밋)되었으면, 그 결과는 **영구적**으로 반영되어야 한다.


# TODO
- 데이터 독립성? 응용 프로그램과 데이터의 관계
- SQL 코테 문제랑 연결해보기
- 정규형 보충(BCNF, 4NF, 5NF)
- 트랜잭션과 로그, 락
- 주기억장치, 보조기억장치란?
- 디스크 I/O 란?
---
# **RDBMS vs NoSQL**
# **RDBMS(Relational Database Management System)**

RDMS 즉 관계형 데이터베이스 관리 시스템은 데이터베이스의 한 종류로 가장 많이 사용된다. 이는 데이터베이스에 데이터를 열(Column)과 행(Row)의 테이블 형태로 저장하고, 테이블 형태의 스키마를 지켜야 한다. 가장 대표적인 RDBMS는 Oracle, MySQL 등이 있다. 그리고 이 관계형 데이터베이스를 다루기 위한 언어를 SQL이라고 한다.

관계형 데이터베이스(RDBMS)는 다른 테이블과 관계를 맺고 모여있는 집합체로 이해할 수 있다. 이러한 관계를 나타내기 위해 외래 키(foreign key)라는 것을 사용해 테이블 간 Join이 가능하다는 게 RDBMS의 가장 큰 특징이다.

---

# **NoSQL(Not Only SQL)**

NoSQL 즉 비관계형 데이터베이스는 관계형 테이블과는 다른 형식으로 데이터를 저장한다. RDBMS와 달리 테이블 간 관계를 정의하지 않는다 즉 테이블 간 Join도 불가능하다.

데이터 일관성은 포기하되 비용을 고려하여 여러 대의 데이터에 분산하여 저장하는 Scale-Out을 목표로 등장했다.

NoSQL은  다음 그림과 같이 다양한 형태의 저장 기술을 지원하고 있다.

![https://blog.kakaocdn.net/dn/lmQ97/btr1UROMNXw/G6qFcvMZbF6bnHe7gVNaSk/img.png](https://blog.kakaocdn.net/dn/lmQ97/btr1UROMNXw/G6qFcvMZbF6bnHe7gVNaSk/img.png)

### **NoSQL 저장 기술**

**1. Key-Value Database**

- **데이터가 Key와 Value에 접근하는 구조**
- 단순한 저장구조를 가지며, 복잡한 조회 연산을 지원하지 않는다.
- Key 값은 어떠한 형태의 데이터라도 담을 수 있다. 이미지나 비디오도 가능하다.
- 또한 간단한 API를 제공하는 만큼 질의의 속도가 굉장히 빠른 편이다.
- 대표적인 Model로는 , Riak, Amazon Dynamo DB 등이 있다.

  Redis


![https://blog.kakaocdn.net/dn/bbXQ7T/btr1XPQucTl/nkH7tmWhZKxPmlHY0T95UK/img.png](https://blog.kakaocdn.net/dn/bbXQ7T/btr1XPQucTl/nkH7tmWhZKxPmlHY0T95UK/img.png)

Redis의 Key-value Model

**2. Document Database**

- Key/Value 즉 1번의 확장된 형태로, value에 Document라는 타입을 저장한다. Document는 구조화된 문서 데이터(XML, Json,Yaml 등)을 말한다.
- 복잡한 데이터 구조를 표현가능하다.
- 주요한 특징으로는 객체-관계 매핑이 필요하지 않다. 객체를 Document의 형태로 바로 저장 가능하기 떄문이다.
- 단점이라면 사용이 번거롭고 쿼리가 SQL과 다르다. 도큐먼트 모델에서는 질의의 결과가 JSON이나 XML 형태로 출력되기 때문에 그 사용 방법이 RDBMS에서의 질의 결과를 사용하는 방법과 다르다.
- 대표적인 Model로는 , CouthDB 등이 있다.

  MongoDB


![https://blog.kakaocdn.net/dn/byD6W9/btr1XyuFSbt/23AUE5BkLes965xEfUnmYk/img.png](https://blog.kakaocdn.net/dn/byD6W9/btr1XyuFSbt/23AUE5BkLes965xEfUnmYk/img.png)

MongoDB의 Document Model

**3. Wide Column Database**

- 이전의 모델들이 Key-Value 값을 이용해 필드를 결정했다면, 특이하게도 이 모델은 키에서 필드를 결정한다. 키는 Row(키 값)와 Column-family, Column-name을 가진다. 연관된 데이터들은 같은 Column-family 안에 속해 있으며, 각자의 Column-name을 가진다. 관계형 모델로 설명하자면 어트리뷰트가 계층적인 구조를 가지고 있는 셈이다. 이렇게 저장된 데이터는 하나의 커다란 테이블로 표현이 가능하며, 질의는 Row, Column-family, Column-name을 통해 수행된다.
- 대표적인 Model로는 HBase, Hypertable 등이 있다.

![https://blog.kakaocdn.net/dn/cdSceO/btr2GKudvRe/mkeK4wrN1rjRcwJc2fcKS1/img.png](https://blog.kakaocdn.net/dn/cdSceO/btr2GKudvRe/mkeK4wrN1rjRcwJc2fcKS1/img.png)

**4. Graph Database**

- Graph Model에서는 데이터를 Node와 Edge, Porperty와 함께 그래프 구조를 사용하여 데아터를 표현하고 저장하는 Database이다.
- 개채와 관계를 그래프 형태로 표현한 것이므로 관계형 모델이라고 할 수 있으며, 데이터 간의 관계가 탐색의 키일 경우에 적합하다.
- 페이스북이나 트위터 같은 소셜 네트워크에서(내 친구의 친구를 찾는 질의 등) 적합하고, 연관된 데이터를 추천해주는 추천 엔진이나 패턴 인식 등의 데이터베이스로도 적합하다.
- 대표 Model로는 Neo4J가 있다.

![https://blog.kakaocdn.net/dn/bTtuGZ/btr2CvLJDUB/9RJDWyG0plaeqmSlFqsQ71/img.webp](https://blog.kakaocdn.net/dn/bTtuGZ/btr2CvLJDUB/9RJDWyG0plaeqmSlFqsQ71/img.webp)

---

![https://blog.kakaocdn.net/dn/csjCxd/btr1140PZde/Q7AGwuKd9IPlii36KnHKy0/img.png](https://blog.kakaocdn.net/dn/csjCxd/btr1140PZde/Q7AGwuKd9IPlii36KnHKy0/img.png)

### **RDBMS와 NoSQL의 장단점**

### **RDBMS**

장점

- 정해진 스키마에 따라 데이터를 저장하여야 하므로 명확한 데이터 구조를 보장한다.
- 또한 각 데이터를 중복없이 한 번만 저장할 수 있다.

단점

- 테이블간 관계를 맺고 있어 시스템이 커질 경우 JOIN문이 많은 복잡한 쿼리가 만들어질 수 있다.
- 성능 향상을 위해서는 Scale-up만을 지원한다. 이로 인해 비용이 기하급수적으로 늘어날 수 있다.
- 스키마로 인해 데이터가 유연하지 못하다. 나중에 스키마가 변경될 경우 번거롭고 어렵다.

### **NoSQL**

장점

- 스키마가 없기 때문에 유연하며 자유로운 데이터 구조를 가질 수 있다.
- 언제든 저장된 데이터를 조정하고 새로운 필드를 추가할 수 있다.
- 데이터 분산이 용이하며 성능 향상을 위한 Scale-up 뿐만이 아닌 Scale-out 또한 가능하다

단점

- 데이터 중복이 발생할 수 있으며 중복된 데이터가 변경될 경우 수정을 모든 컬렉션에서 수행해야 한다.
- 스키마가 존재하지 않기에 명확한 데이터 구조를 보장하지 않으며 데이터 구조 결정하기가 어려울 수 있다.

**※ Scale up**

![https://blog.kakaocdn.net/dn/FIBWn/btr2GLGEwkQ/KQUjQGLusnY2wyb7pj6jbk/img.png](https://blog.kakaocdn.net/dn/FIBWn/btr2GLGEwkQ/KQUjQGLusnY2wyb7pj6jbk/img.png)

스케일 업(Scale-up)은 쉽게 말하면 기존의 서버를 보다 높은 사양으로 업그레이드 하는 것으로 서버 하나의 능력을 증강하기 때문에 수직 스케일링(vertical scaling)이라고도 한다.

**※ scale-out**

![https://blog.kakaocdn.net/dn/xpGUT/btr2DDW0Jry/yQNAQVT4OMaBgBjWZxTlkk/img.png](https://blog.kakaocdn.net/dn/xpGUT/btr2DDW0Jry/yQNAQVT4OMaBgBjWZxTlkk/img.png)

스케일 아웃(Scale-out)은 장비를 추가해서 확장하는 방식을 말한다. 기존 서버만으로 용량이나 성능의 한계에 도달했을 때, 비슷한 사양의 서버를 추가로 연결해 처리할 수 있는 데이터 용량이 증가할 뿐만 아니라 기존 서버의 부하를 분담해 성능 향상의 효과를 기대할 수 있다.

서버를 추가로 확장하기 때문에 수평 스케일링(horizontal scaling)이라고도 불린다.

---

### **RDBMS, NoSQL 언제 사용해야 할까?**

RDBMS는 데이터 구조가 명확하며 변경 될 여지가 없으며 명확한 스키마가 중요한 경우. 또한 중복된 데이터가 없어(데이터 무결성) 변경이 용이하기 때문에 관계를 맺고 있는 데이터가 자주 변경이 이루어지는 시스템에 적합하다.

NoSQL은 정확한 데이터 구조를 알 수 없고 데이터가 변경/확장이 될 수 있는 경우에 사용하는 것이 좋다. 하지만 데이터 중복이 발생할 수 있으며 중복된 데이터가 변경될 시에는 모든 컬렉션에서 수정을 해야 하기 때문에 Update가 많이 이루어지지 않는 시스템이 좋다. 또한 Scale-out이 가능하다는 장점을 활용해 막대한 데이터를 저장해야 되는 시스템에 적합하다.

---


# 출처
- 데이터베이스의 정석 - 박성진
- https://www.geeksforgeeks.org/difference-between-file-system-and-dbms/
- https://towardsdatascience.com/designing-a-relational-database-and-creating-an-entity-relationship-diagram-89c1c19320b2
- https://ko.wikipedia.org/wiki/SQL
- https://victorydntmd.tistory.com/126
- http://wiki.hash.kr/index.php/%EC%88%98%ED%8D%BC%ED%82%A4
- https://www.geeksforgeeks.org/relational-model-in-dbms/
- [https://www.geeksforgeeks.org/introduction-of-database-normalization/](https://www.geeksforgeeks.org/introduction-of-database-normalization/)
- [https://gyoogle.dev/blog/computer-science/data-base/Join.html](https://gyoogle.dev/blog/computer-science/data-base/Join.html)
- [https://www.geeksforgeeks.org/sql-join-set-1-inner-left-right-and-full-joins/](https://www.geeksforgeeks.org/sql-join-set-1-inner-left-right-and-full-joins/)
- [https://fauna.com/blog/database-transaction](https://fauna.com/blog/database-transaction)
- [https://www.geeksforgeeks.org/acid-properties-in-dbms/](https://www.geeksforgeeks.org/acid-properties-in-dbms/)
- [https://inpa.tistory.com/entry/MYSQL-📚-트랜잭션Transaction-이란-💯-정리#트랜잭션_특징](https://inpa.tistory.com/entry/MYSQL-%F0%9F%93%9A-%ED%8A%B8%EB%9E%9C%EC%9E%AD%EC%85%98Transaction-%EC%9D%B4%EB%9E%80-%F0%9F%92%AF-%EC%A0%95%EB%A6%AC#%ED%8A%B8%EB%9E%9C%EC%9E%AD%EC%85%98_%ED%8A%B9%EC%A7%95)
- [https://mangkyu.tistory.com/19](https://mangkyu.tistory.com/19)
- [https://mangkyu.tistory.com/96](https://mangkyu.tistory.com/96)
- [https://coding-factory.tistory.com/746](https://coding-factory.tistory.com/746)
- [https://www.geeksforgeeks.org/indexing-in-databases-set-1/](https://www.geeksforgeeks.org/indexing-in-databases-set-1/)
- [https://tecoble.techcourse.co.kr/post/2021-10-12-scale-up-scale-out/](https://tecoble.techcourse.co.kr/post/2021-10-12-scale-up-scale-out/)
- [https://khj93.tistory.com/entry/Database-RDBMS%EC%99%80-NOSQL-%EC%B0%A8%EC%9D%B4%EC%A0%90](https://khj93.tistory.com/entry/Database-RDBMS%EC%99%80-NOSQL-%EC%B0%A8%EC%9D%B4%EC%A0%90)
- [https://pythontoomuchinformation.tistory.com/528](https://pythontoomuchinformation.tistory.com/528)