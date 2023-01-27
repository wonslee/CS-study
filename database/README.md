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

# TODO
- TCL 보충.. 트랜잭션 모름
- 데이터 독립성? 응용 프로그램과 데이터의 관계

# 출처
- 데이터베이스의 정석 - 박성진
- https://www.geeksforgeeks.org/difference-between-file-system-and-dbms/
- https://towardsdatascience.com/designing-a-relational-database-and-creating-an-entity-relationship-diagram-89c1c19320b2
- https://ko.wikipedia.org/wiki/SQL