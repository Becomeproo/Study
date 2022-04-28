# SQL-DDL
## DDL(Data Define Language, 데이터 정의어)
* DDL은 DB 구조, 데이터 형식, 접근 방식 등 DB를 구축하거나 수정할 목적으로 사용하는 언어이다.
* 번역한 결과가 데이터 사전(Data Dictionary)이라는 특별한 파일에 여러 개의 테이블로 저장된다.
* DDL의 3가지 유형
  * CREATE
    SCHEMA, DOMAIN, TABLE, VIEW, INDEX를 정의함
  * ALTER
    TABLE에 대한 정의를 변경하는 데 사용함
  * DROP
    SCHEMA, DOMAIN, TABLE, VIEW, INDEX를 삭제함
 
## CREATE SCHEMA
* CREATE SCHEMA는 스키마를 정의하는 명령문이다..
* 표기 형식
  > CREATE SCHEMA 스키마명 AUTHORIZATION 사용자_id;

  EX) 소유권자의 사용자 ID가 '홍길동'인 스키마 '대학교'를 정의하는 SQL문은 다음과 같다.
      CREATE SCHEMA 대학교 AUTHORIZATION 홍길동;
      
## CREATE DOMAIN
* CREATE DOMAIN은 도메인을 정의하는 명령문이다.
* 표기 형식
> CREATE DOMAIN 도메인명 [AS] 데이터_타입 <br>
>          [DEFAULT 기본값] <br>
>          [CONSTRAINT 제약조건명 CHECK (범위값)];

* 데이터 타입 : SQL에서 지원하는 데이터 타입
* 기본값 : 데이터를 입력하지 않았을 때 자동으로 입력되는 값
EX) '성별'을 '남' 또는 '여'와 같이 정해진 1개의 문자로 표현되는 도메인 SEX를 정의하는 SQL문은 다음과 같다. <br>
-> CREATE DOMAIN SEX CHAR(1) <BR>
          DEFAULT '남' <BR>
          CONSTRAINT VALID_SEX CHECK(VALUE IN ('남', '여')); <BR>
          // 다중제약 CONSTRAINT VALID_DEGREE CHECK(DEGREE>=1 AND DEGREE<=6); <- 1학년부터 6학년 사이
  

## CREATE TABLE
* CREATE TABLE은 테이블을 정의하는 명령문이다.
* 표기 형식
  > CREATE TABLE 테이블명 <BR>
  >        [속성명 데이터_타입 [DEFAULT 기본값][NOT NULL], <BR> 
  >        [PRIMARY KEY(기본키_속성명, ...)] <BR>
  >        [UNIQUE(대체키_속성명, ...)] <BR>
  >        [FOREIGN KEY(외래키_속성명, ...)] <BR>
  >                 REFERENCES 참조테이블(기본키_속성명, ...)] <BR>
  >                 [ON DELETE 옵션] <BR>
  >                 [ON UPDATE 옵션] <BR>
  >          [ CONSTRAINT 제약조건명][CHECK(조건식)]);
* 기본 테이블에 포함될 모든 속성에 대하여 속성명과 그 속성과 데이터 타입, 기본값, NOT NULL 여부를 지정한다.
* PRIMARY KEY : 기본키로 사용할 속성을 지정함
* UNIQUE : 대체키로 사용할 속성을 지정함, 중복된 값을 가질 수 없음
* FOREIGN KEY ~ REFERENCES ~ : 외래키로 사용할 속성을 지정함
  * ON DELETE 옵션 : 참조 테이블의 튜플이 삭제되었을 때 기본 테이블에 취해야 할 사항을 지정함
  * ON UPDATE 옵션 : 참조 테이블의 참조 속성 값이 변경되었을 때 기본 테이블에 취해야 할 사항을 지정함
* CONSTRAINT : 제약 조건의 이름을 지정함
* CHECK : 속성 값에 대한 제약 조건을 정의함
  
## CREATE VIEW
* CREATE VIEW는 뷰(VIEW)를 정의하는 명령문이다.
* 표기 형식
  > CREATE VIEW 뷰명[(속성명[, 속성명, ...])]
  > AS SELECT문;

## CREATE INDEX
* CREATE INDEX는 인덱스를 정의하는 명령문이다.
* 표기 형식
  > CREATE [UNIQUE] INDEX 인덱스명
  > ON 테이블명(속성명 [ASC | DESC] [, 속성명[ASC | DESC]])
  > [CLUSTER];
  
* UNIQUE
  * 사용된 경우 : 중복 값이 없는 속성으로 인덱스를 생성한다.
  * 생략된 경우 : 중복 값을 허용하는 속성으로 인덱스를 생성한다.
* 정렬 여부 지정
  * ASC : 오름차순 정렬
  * DESC : 내림차순 정렬
  * 생략된 경우 : 오름차순으로 정렬됨
* CLUSTER : 사용하면 인덱스가 클러스터드 인덱스로 설정됨
  
## ALTER TABLE
* ALTER TABLE은 테이블에 대한 정의를 변경하는 명령문이다.
* 표기 형식
  > ALTER TABLE 테이블명 ADD 속성명 데이터_타입 [DEFAULT '기본값'];
  > ALTER TABLE 테이블명 ALTER 속성명 [SET DEFAULT '기본값'];
  > ALTER TABLE 테이블명 DROP COLUMN 속성명 [CASCADE];
* ADD : 새로운 속성(열)을 추가할 때 사용한다.
* ALTER : 특정 속성의 DEFAULT 값을 변경할 때 사용한다.
* DROP_COLUMN : 특정 속성을 삭제할 때 사용한다.

## DROP
* DROP은 스키마, 도메인, 기본 테이블, 뷰 테이블, 인덱스, 제약 조건 등을 제거하는 명령문이다.
* 표기 형식
  > DROP SCHEMA 스키마명 CASCADE | RESTRICT;
  > DROP DOMAIN 도메인명 CASCADE | RESTRICT;
  > DROP TABLE 테이블명 CASCADE | RESTRICT;
  > DROP VIEW 뷰명 CASCADE | RESTRICT;
  > DROP INDEX 인덱스명 CASCADE | RESTRICT;
  > DROP CONSTRAINT 제약조건명;
* CASCADE : 제거할 요소를 참조하는 다른 모든 개체를 함께 제거한다.
* RESTRICT : 다른 개체가 제거할 요소를 참조중일 때는 제거를 취소한다.
