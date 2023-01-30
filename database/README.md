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

# 릴레이션(Relation)과 키(Key)

## 관계형 데이터 모델

관계형 데이터베이스 개념은 관계형 데이터 모델에 기반하고 있다.  
관계형 데이터 모델은 이론적으로는 릴레이션(relation)이라는 **수학적 집합** 개념에 기초하고 있다.  


> 관계 모델에서 **릴레이션**(relation)은 동일한 구조로 이뤄진 **튜플(tuple)의 집합**을 말한다.   

쉽게 표현하면 릴레이션은 **2차원 테이블(table)** 형태의 단순한 구조다.  

<img src="https://odinuv.cz/slides/relational-database/relation.svg" width="50%" height="50%">

테이블 개념은 내부 저장 구조에 대한 추상적인 표현일 뿐이고, 물리적으로는 복잡한 구조 속에 데이터가 저장된다.  

관계형 데이터베이스에서 데이터베이스는 전체 릴레이션들의 모임이다.

## 릴레이션 용어 정리
- 속성(**attribute**) : 테이블의 열(column). 데이터를 표현하는 **가장 작은 논리적 단위**
- 속성 집합(attribute **domain**) : 각 속성이 취할 수 있는 모든 값들의 집합을 정의한 것. 데이터 타입과 비슷!  
  <img src="https://people.cs.pitt.edu/~chang/156/images/fig41.gif" width="50%" height="50%">
- 튜플(tuple), record : 테이블의 각 행(row)
- 차수(degree) : 릴레이션을 구성하는 전체 속성의 개수. 각 튜플이 가지는 속성값의 개수는 해당 릴레이션의 차수와 같다.

## 키(Key)
튜플의 **유일성** 규칙을 충족시키기 위해 모든 릴레이션은 **키(Key)** 를 갖는다.  
키는 릴레이션이 단순한 테이블이 아님을 보여주는 대표적 개념이다.  
데이터베이스에서 키는 여러 **무결성** 제약 조건에 대해 중요한 역할을 한다.  

### 후보키(Candidate Key)
> 튜플을 유일하게 식별할 수 있는 속성들의 최소(부분) 집합.  
후보키는 유일성과 최소성 조건을 모두 만족해야 한다.  

- **유일성** : Key로 하나의 튜플을 유일하게 식별할 수 있음
- **최소성** : 각 튜플들을 유일하게 식별하기 위해 꼭 필요한 최소한을 만족

예를 들어, 속성 중 '이름'을 기본키로 사용한다면 **동명이인**이 있을 수 있기 때문에 **유일성**을 만족할 수 없다.    

모든 릴레이션은 최소 하나 이상의 후보키를 가진다.

<img src="http://wiki.hash.kr/images/thumb/d/da/%EB%8C%80%EC%B2%B4%ED%82%A4_%EC%98%88%EC%8B%9C_%EC%88%98%EC%A0%95%EB%B3%B8.PNG/900px-%EB%8C%80%EC%B2%B4%ED%82%A4_%EC%98%88%EC%8B%9C_%EC%88%98%EC%A0%95%EB%B3%B8.PNG"  width="50%" height="50%">

### 기본키(Primary Key)
> 튜플을 **대표**하도록 선정된 후보키.  

대부분의 DB에서는 `id`로 튜플을 식별한다.  

### 대체키(Alternate Key)
> 기본키로 **선정되지 못한** 후보키. 

### 슈퍼키(Super Key)
> **유일성**은 만족하지만, 최소성과는 상관 없는 키.  

투플을 유일하게 식별할 수 있는 속성들의 집합.  
꼭 필요한 속성이 아니어도 포함된다.  

<img src="https://velog.velcdn.com/images/duck-ach/post/8b2ea421-f688-4d37-896c-8eecdf1bb68b/image.png" width="50%" height="50%">


### 외래키(Foreign Key)
> 외부 릴레이션의 키를 **참조**하는 키

외래키의 값은 참조될 다른 릴레이션의 **기본키(PK)** 중에서 하나를 골라 정한다.  


<img src="http://wiki.hash.kr/images/8/8b/%EC%99%B8%EB%9E%98%ED%82%A4_%EC%98%88%EC%8B%9C.jpg" width="50%" height="50%">

<details>
<summary>외래키 포함한 테이블 생성 (sql)</summary>

```sql
CREATE TABLE table_name (
  id    INTEGER PRIMARY KEY,
  col3  INTEGER FOREIGN KEY 
      REFERENCES other_table(other_id),
  ... )
```  
</details>


## 무결성 제약 조건
> 데이터 **무결성(integrity)** 은 데이터베이스에 저장된 데이터의 **일관성**과 **정확성**을 지키는 것을 말한다.
### 개체 무결성 제약 조건(PK constraint)
> 기본키(PK)로 지정한 모든 속성은 **NULL값을 가질 수 없고** 릴레이션 안에서 중복되지 않는 **유일한 값**을 가지도록 하는 제약 조건

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F996030495B3646B627" width="50%" height="50%">

개체의 **유일성**을 위해, 왠만하면 릴레이션마다 PK를 정의해야 한다.  
보통은 테이블을 생성할 때 `PRIMARY KEY(기본키)`를 선언함으로써 적용된다.  


<details>
<summary>SQL PRIMARY KEY</summary>

```sql
CREATE TABLE Persons (
   ID int NOT NULL PRIMARY KEY,
   LastName varchar(255) NOT NULL,
   FirstName varchar(255),
   Age int
);
```  
</details>


### 참조 무결성 제약 조건(FK constraint)
> 외래키(FK)로 지정한 속성은 참조하는 릴레이션의 **기본키(PK) 값과 일치**하거나 **NULL값**을 가지도록 하는 제약 조건

어떤 개체의 외래키가 NULL 값을 갖는다는건 관련된 개체가 없음을 의미한다.   
반면에 어떤 값을 갖는다면, 그 값은 반드시 관련된 개체의 기본키값과 일치해야 한다.  

만약 관계된 개체를 삭제하려면, 그걸 참조중인 개체를 먼저 삭제하거나 관계를 끊어야 한다.   
외래키의 값이 존재하지 않는 기본키라면 PK 제약 조건을 위배하기 때문이다.   


<details>
<summary>SQL FOREIGN KEY</summary>

```sql
CREATE TABLE Orders (
  OrderID int NOT NULL PRIMARY KEY,
  OrderNumber int NOT NULL,
  PersonID int FOREIGN KEY REFERENCES Persons(PersonID)
);
```  
</details>


### 도메인 무결성 제약 조건(domain constraint)
> 튜플의 모든 속성 값이 각 속성 **도메인에 속한 값만을 취하도록** 하는 제약 조건. 

SQL에서 테이블 생성시 각 열의 타입을 명시하거나  
`NULL` 혹은 `NOT NULL`, `DEFAULT(디폴트 값)`, `CHECK(값 범위 체크 조건)` 등의 키워드 설정을 써서 제약을 명시한다.  

### 유일성 제약 조건(uniqueness constraint)
> **키** 속성 값이 서로 중복되지 않고 **유일하도록** 하는 제약 조건.

SQL에서 테이블 생성시에 `UNIQUE(유일 조건)` 키워드 설정을 써서 명시한다.  


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
