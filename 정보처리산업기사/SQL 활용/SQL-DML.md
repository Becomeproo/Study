# SQL-DML
## DML(Data Manipulation Language, 데이터 조작어)
* DML은 데이터베이스 사용자가 저장된 데이터를 실질적으로 관리하는데 사용되는 언어이다.
* DML은 데이터베이스 사용자와 데이터베이스 관리 시스템 간의 인터페이스를 제공한다.
* DML의 유형
  * SELECT : 테이블에서 튜플을 검색함
  * INSERT : 테이블에 새로운 튜플을 삽입함
  * DELETE : 테이블에서 튜플을 삭제함
  * UPDATE : 테이블에서 튜플의 내용을 갱신함

## 삽입문(INSERT INTO~)
* 삽입문은 기본 테이블에 새로운 튜플을 삽입할 때 사용한다.
* 일반 형식
  > INSERT INTO 테이블명([속성명1, 속성명2, ...]) <br>
  > VALUES (데이터1, 데이터2, ...);
  
* 대응하는 속성과 데이터는 개수와 데이터 유형이 일치해야 한다.
* 기본 테이블의 모든 속성을 사용할 때는 속성명을 생략할 수 있다.
* SELECT 문을 사용하여 다른 테이블의 검색 결과를 삽입할 수 있다.

## 삭제문(DELETE FROM ~)
* 삭제문은 기본 테이블에 있는 튜플들 중에서 특정 튜플(행)을 삭제할 때 사용한다.
* 일반 형식
  > DELETE FROM 테이블 WHERE 조건;
* 모든 레코드를 삭제할 때는 WHERE절을 생략한다.
* 모든 레코드를 삭제하더라도 테이블 구조는 남아 있기 때문에 디스크에서 테이블을 완전히 제거하는 DROP과는 다르다.

## 갱신문(UPDATE ~ SET ~)
* 갱신문은 기본 테이블에 있는 튜플들 중에서 특정 튜플의 내용을 변경할 때 사용한다.
* 일반 형식
  > UPDATE 테이블명 <br>
  > SET 속성명 = 데이터[, 속성명 = 데이터, ...] <br>
  > [WHERE 조건];
