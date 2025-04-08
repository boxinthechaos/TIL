# SQL(Structured Query Language)이란?
관계형 데이터베이스에서 주요한 특징 중 하나는 SQL이라는 구조화 질의어를 사용한다는 것이다. SQL은 RDBMS에서 사용하는 프로그래밍 언어라고 보면 된다.
SQL을 통해 RDBMS에서 데이터를 검색하고, 추가하고, 업데이트하고, 삭제하는 작업 등 데이터를 관리한다. 
SQL 종류는 아래와 같이 존재한다
* 데이터 정의 언어
* 데이터 조작 언어
* 데이터 제어 언어

# 대표적인 SQL문법
![스크린샷](https://github.com/boxinthechaos/Link1/blob/main/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202024-09-12%20174804.png)

# 데이터 정의어(DDL)
* CREATE TABLE : 테이블 구성, 속성과 속성에 관한 제약 정의, 기본키 및 외래키를 정의한다.
* ALTER TABLE : 생성된 테이블의 속성과 속성에 관한 제약을 변경하며, 기본키 및 외래키를 변경함
* DROP TABLE : DROP TABLE 문은 테이블을 삭제하는 명령어 DROP 문은 테이블의 구조와 데이터를 모두 삭제하므로 사용에 주의해야 함

# 데이터 조작어(DML)
* INSERT : 테이블에 새로운 투플을 삽입하는 명령어
* UPDATE : 테이블에 저장된 투플에서 특정 속성의 값을 수정하는 명령
* DELETE : 테이블에 있는 기존 투플을 삭제하는 명령

# 데이터 제어어(DCL)
* GRANT : 권한을 부여하는 명령
* REVOKE : 권한을 제거하는 명령

# SQL사용 방법
* CREATE TABLE
  > 다음과 같은 속성을 가진 NewOrders 테이블을 생성하시오.
  > - orderid(주문번호) - INTEGER, 기본키
  > - custid(고객번호) - INTEGER, NOT NULL 제약조건, 외래키(NewCustomer.custid, 연쇄삭제)
  > - bookid(도서번호) - INTEGER, NOT NULL 제약조건
  > - saleprice(판매가격) - INTEGER
  > - orderdate(판매일자) – DATE
  > ```
  > CREATE TABLE NewOrders (
  > orderid INTEGER,
  > custid INTEGER NOT NULL,
  > bookid INTEGER NOT NULL,
  > saleprice INTEGER,
  > orderdate DATE,
  > PRIMARY KEY (orderid),
  > FOREIGN KEY (custid) REFERENCES NewCustomer(custid) ON DELETE CASCADE );
  > ```

* ALTER TABLE
  > - ADD, DROP은 속성을 추가하거나 제거할 때 사용함
  > - MODIFY는 속성의 기본값을 설정하거나 삭제할 때 사용함
  > - ADD <제약이름>, DROP <제약이름>은 제약사항을 추가하거나 삭제할 때 사용함
  > - ALTER TABLE 기본 문법
  > ```
  > ALTER TABLE 테이블이름
  >  [ADD 속성이름 데이터타입]
  >  [DROP COLUMN 속성이름]
  >  [MODIFY 속성이름 데이터타입]
  >  [MODIFY 속성이름 [NULL┃NOT NULL]]
  >  [ADD PRIMARY KEY(속성이름)]
  >  [[ADD┃DROP] 제약이름]
  > ```

* DROP TABLE
  > – 데이터만 삭제하려면 DELETE 문을 사용함
  > ```
  > NewBook 테이블을 삭제하시오.
  > DROP TABLE NewBook;
  > ```

* INSERT
  > - INTO 키워드와 함께 투플을 삽입할 테이블의 이름과 속성의 이름을 나열
  > - VALUES 키워드와 함께 삽입할 속성 값들을 나열
  > ```
  >  Book 테이블에 새로운 도서 ‘스포츠 의학’을 삽입하시오. 스포츠 의학은 한솔의학서적
  >  에서 출간했으며 가격은 90,000원이다
  > INSERT INTO Book(bookid, bookname, publisher, price)
  > VALUES (11, '스포츠 의학', '한솔의학서적', 90000);
  > ```

* UPDATE
  > - SET 키워드 다음에 속성 값을 어떻게 수정할 것인지를 지정
  > - WHERE 절에 제시된 조건을 만족하는 투플에 대해서만 속성 값을 수정
  > ```
  > Book 테이블에서 14번 ‘스포츠 의학’의 출판사를 imported_book 테이블의 21번
  > 책의 출판사와 동일하게 변경하시오.
  >
  > UPDATE Book
  > SET publisher = (SELECT publisher
  > FROM imported_book
  > WHERE bookid = '21')
  > WHERE bookid = '14' ;
  > ```

  * DELETE
    > – WHERE 절에 제시한 조건을 만족하는 투플만 삭제
    > ```
    > Book 테이블에서 도서번호가 11인 도서를 삭제하시오.
    > DELETE FROM Book
    > WHERE bookid = 11;
    > ```

* GRANT
  > - GRANT SELECT ON 테이블 TO 사용자;
  > - GRANT INSERT ON 테이블 TO 사용자;
  > - GRANT UPDATE ON 테이블 TO 사용자;
  > - GRANT DELETE ON 테이블 TO 사용자;
  > - 테이블의 SELECT, INSERT, UPDATE, DELETE 권한을 사용자에게 부여한다
  > ```
  > GRANT SELECT ON hr.employees TO scott;
  > GRANT INSERT ON hr.employees TO scott;
  > GRANT UPDATE ON hr.employees TO scott;
  > GRANT DELETE ON hr.employees TO scott;
  > ```

* REVOKE
  > - REVOKE SELECT ON 테이블 FROM 사용자;
  > - REVOKE INSERT ON 테이블 FROM 사용자;
  > - REVOKE UPDATE ON 테이블 FROM 사용자;
  > - REVOKE DELETE ON 테이블 FROM 사용자;
  > - 사용자에게 부여한 권한을 수거한다
  > ```
  > GRANT SELECT ON hr.employees TO scott;
  > GRANT INSERT ON hr.employees TO scott;
  > GRANT UPDATE ON hr.employees TO scott;
  > GRANT DELETE ON hr.employees TO scott;
  > ```
