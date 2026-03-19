# RDBMS와 NoSQL
## DB와 DBMS와 SQL
Databse 일반적으로 컴퓨터 시스템에 전자 방식으로 저장된 구조화 된 정보 또는 데이터의 체계적인 집합을 의미한다

DBMS란(DataBase Management System) 사용자와 DB사이에서 사용자의 요구에 따라 정보를 생성하고 관리하는 소프트웨어 입니다

SQL이란 (Strucured Query Language) DBMS의 데이터를 관리하기 위해 설계된 특수한 언어로 관계형 데이터베이스 관리 시스템에서 자료 검색과 관리, 데이터베이스 스키마 생성과 수정, 데이터베이스 객체 접근 조정 관리를 위해 고안되었습니다.

## RDBMS(Relational Database Management System)
RDBMS는 관계형 데이터베이스 관리 시스템을 말합니다 이름과 같이 RDBMS는 RDB를 관리하는 시스템이며 RDB는 관계형 데이터 모델을 기초로 두고 모든 데이터를 2차원 테이블 형태로 표현하는 데이터베이스 입니다

관계형 데이터베이스(RDMBS)는 아래와와 같이 구성된 테이블이 다른 테이블들과 관계를 맺고 모여있는 집합체로 이해할 수 있습니다

RDBMS에서는 이러한 관계를 나타네기 위해 외래 키(foreign key)라는 것을 사용합니다<br>
이런한 테이블간의 관계에서 외래 키를 이용한 테이블 간 Join이 가능하다는게 RDBMS의 가장 큰 특징 입니다

### [회원테이블]
|회원 번호(PK)|회원 이름|휴대폰 번호|
|---------|----------|----------|
| 1111111 |  김희진   |010-xxxx-yyyy|
| 2222222 | 김또깡 | 010-YYYY-XXXXX |

### [주문 Table]
|주문 번호(PK)|주문 회원 번호(FK)|주문 상품|
|---------|----------|----------|
| 20200207xxxxxxxx |  1111111   |컴퓨터|
| 20200207yyyyyyy | 2222222 | 키보드 |
| 20200207zzzzzzz | 2222222 | 마우스 |


## NoSQL
NoSQL이란(Not Only SQL)의 약자로 말 그대로 위에서 설명한 RDB 형태가 아닌 다른 헝태의 데이터 저장 기술을 의미한다 또한 NoSQL은 RDB와 다르게 JOIN이 불가능 하다

NoSQL은 빅데이터의 등장으로 데이터 트래픽이 기하급수적으로 증가함에 따라 RDBMS의 단점인 성능을 향상시키기 위해 장비가 좋아야 하는 Scale - Up의 특징이 비용을 기하급수적으로 증가시키기 때문에 일관성을 포기하되 비용을 고려하여 여러대의 데이터를 분산하여 저장하는 Scale-Out을 목표로 등장하였다

NoSQL을 하면 가장 유명한 Document 기반의 MongoDB를 많이 떠올리지만 MongoDB는 NoSQL한 종류로 NoSQL은 하기와 같이 다양한 형태의 저장 기술을 지원하고 있습니다<br>
이 다양한 형태의 저장기술은 RDBMS 스키마에 맞추어 데이터를 관리해야 된다는 한계를 극복하고 수평적 확장성(Scale-out)을 쉽게 할 수 있다는 장점을 가지고 있습니다

### 1. Key-Value Database
- Key-Value Database는 데이터가 Key와 Value가 쌍으로 저장된다 Key는 value에 접근하기 위해 사용되며 값은 어떤 형태의 데이터를 담을 수 있다 심지어 이미지나 비디오도 가능하다 또한 간단한 API를 제공하는 만큼 질의 속도도 엄청 빠르다
- 대표적인 NoSQL Key-Value Model로는 Redies, Riak, Amazon Dynamo DB등이 있다

### 2. Document Database
Documnet Database 데이터는 Key와Document의 형태로 저장된다 Key-Value 모델과 다른 점이라면 Value가 계층적인 형태인 도큐먼트로 저장된다는 것이다

 객체지향에서의 객체와 유사하며, 이들은 하나의 단위로 취급되어 저장된다  다시 말해 하나의 객체를 여러 개의 테이블에 나눠 저장할 필요가 없어진다는 뜻이다

주요한 특징으론 객체 관계 매핑이 필요 없다 객체를 Document의 형태로 바로 저장 가능하기 때문이다 또한 검색에 최적화 되어 있는데 이는 key-value 모델의 특징하고 동일하다 

단점이라면 사용이 번거롭고 쿼리가 SQL과 다르다는 점이다

대표적인 NoSQL Document Model로는 MongoDB, CouthDB 등이 있다

### 3. Wide Column Database
이 모델은 키에서 필드를 결정하는 모델이다 키는 ROW와 Column-family, Column-name을 가진다 연관된 데이터들은 같은 Column-family 안에 속해 있으며 각자의 Column-name을 가진다 

이렇게 저장된 데이터는 하나의 커다란 테이블로 표현이 가능하며 질의는  Row, Column-family, Column-name을 통해 수행한다

대표적인 NoSQL Column-family Model로는 HBase, Hypertable 등이 있다

### 4. Graph Database 
Graph Model Model에서는 데이터를 Node와 Edge, Property와 함께 그래프 구조를 사용하여 데이터를 표현하고 저장하는 Database이다

개체와 관계를 그래프로 표현한 것이므로 관계형 모델이라고 할 수 있으며 데이터 간의 관계가 탐색의 키일 경우 적합하다

연관된 데이터를 추천해주는 추천 엔진이나 패턴 인식등의 데이터베이스로도 적합하다

대표적인 NoSQL Graph Model로는 Neo4J가 있다