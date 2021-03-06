# DML-SELECT
## 일반 형식
> SELECT [PREDICATE] [테이블명.] 속성명 [AS 별칭][, [테이블명.] 속성명, ...] <br>
> [, 그룹함수(속성명) [AS 별칭]] <br>
> [, Window 함수 OVER (PARTITION BY 속성명1, 속성명2, ... <br>
>                   ORDER BY 속성명3, 속성명4, ...)] <br>
> FROM 테이블명[, 테이블명, ...] <br>
> [WHERE 조건] <br>
> [GROUP BY 속성명, 속성명, ...] <br>
> [HAVING 조건] <br>
> [ORDER BY 속성명 [ASC | DESC]];

* SELECT절
  * PREDICATE : 검색할 튜플 수를 제한하는 명령어를 기술함
  * DISTINCT : 중복된 튜플이 있으면 그 중 첫 번째 한 개만 표시함
  * 속성명 : 검색하여 불러올 속성(열) 또는 속성을 이용한 수식을 지정함
  * AS : 속성이나 연산의 이름을 다른 이름으로 표시하기 위해 사용함
  * FROM절 : 검색할 데이터가 들어있는 테이블 이름을 기술함
  * WHERE절 : 검색할 조건을 기술함
  * ORDER BY절 : 데이터를 정렬하여 검색할 때 사용함
    * 속성명 : 정렬의 기준이 되는 속성명을 기술함
    * [ASC | DESC] : 정렬 방식으로, 'ASC'는 오름차순, 'DESC'는 내림차순이다. 생략하면 오름차순으로 지정됨
  * 그룹함수 : GROUP BY절에 지정된 그룹별로 속성의 값을 집계할 수 함수를 기술함
  * WINDOW 함수 : GROUP BY절을 이용하지 않고 속성의 값을 집계할 함수를 기술함
    * PARTITION BY : WINDOW 함수의 적용 범위가 될 속성을 지정함
    * ORDER BY : PARTITION 안에서 정렬 기준으로 사용할 속성을 지정함
  * GROUP BY절 : 특정 속성을 기준으로 그룹화하여 검색할 때 사용한다. 일반적으로 GROUP BY 절은 그룹 함수와 함께 사용됨
  * HAVING절 : GROUP BY와 함께 사용되며, 그룹에 대한 조건을 지정함

## 조건 연산자
* 비교 연산자
  * = : 갇다
  * <> : 같지 않다
  * > : 크다
  * < : 작다
  * >= : 크거나 같다
  * <= : 작거나 같다
* 논리 연산자 : NOT, AND, OR
* LIKE 연산자 : 대표 문자를 이용해 지정된 속성의 값이 문자 패턴과 일치하는 튜플을 검색하기 위해 사용된다.
* % : 모든 문자를 대표함
* _ : 문자 하나를 대표함
* # : 숫자 하나를 대표함

## 그룹 함수
* 그룹 함수는 GROUP BY절에 지정된 그룹별로 속성의 값을 집계할 때 사용한다.
* COUNT(속성명) : 그룹별 튜플 수를 구하는 함수
* SUM(속성명) : 그룹별 합계를 구하는 함수
* AVG(속성명) : 그룹별 평균을 구하는 함수
* MAX(속성명) : 그룹별 최대값을 구하는 함수
* MIN(속성명) : 그룹별 최소값을 구하는 함수
* STDDEV(속성명) : 그룹별 표준편차를 구하는 함수
* VARIANCE(속성명) : 그룹별 분산을 구하는 함수
* ROLLUP(속성명, 속성명, ...)
  * 인수로 주어진 속성을 대상으로 그룹별 소계를 구하는 함수
  * 속성의 개수가 n개이면, n+1 레벨까지, 하위 레벨에서 상위 레벨 순으로 데이터가 집계됨
* CUBE(속성명, 속성명, ...)
  * ROLLUP과 유사한 형태지만 CUBE는 인수로 주어진 속성을 대상으로 모든 조합의 그룹별 소계를 구함
  * 속성의 개수가 n개이면, 2^n 레벨까지, 상위 레벨에서 하위 레벨 순으로 데이터가 집계됨

## WINDOW 함수
* WINDOWS 함수는 GROUP BY절을 이용하지 않고 함수의 인수로 지정한 속성의 값을 집계한다.
* 함수의 인수로 지정한 속성이 집계할 범위가 되는데, 이를 윈도우(WINDOW)라고 부른다.
* WINDOW 함수
  * ROW_NUMBER() : 윈도우별로 각 레코드에 대한 일련번호를 반환한다.
  * RANK() : 윈도우별로 순위를 반환하며, 공동 순위를 반환한다.
  * DENSE_RANK() : 윈도우별로 순위를 반환하며, 공동 순위를 무시하고 순위를 부여한다.

## 집합 연산자를 이용한 통합 질의
* 집합 연산자를 사용하여 2개 이상의 테이블의 데이터를 하나로 통합한다.
* 표기 형식
  > SELECT 속성명1, 속성명2, ... <br>
  > FROM 테이블명 <br>
  > UNION | UNION ALL | INTERSECT | EXCEPT <br>
  > SELECT 속성명1, 속성명2, ... <br>
  > FROM 테이블명 <br>
  > [ORDER BY 속성명 [ASC | DESC]];
* 두 개의 SELECT문에 기술한 속성들은 개수와 데이터 유형이 서로 동일해야 한다.
* 집합 연산자의 종류(통합 질의의 종류)
  * UNION(합집합)
    * 두 SELECT문의 조회 결과를 통합하여 모두 출력함
    * 중복된 행은 한 번만 출력함
  * UNION ALL(합집합)
    * 두 SELECT문의 조회 결과를 통합하여 모두 출력함
    * 중복된 행도 그대로 출력함
  * INTERSECT(교집합)
    * 두 SELECT문의 조회 결과 중 공통된 행만 출력함
  * EXCEPT(차집합)
    * 첫 번째 SELECT문의 조회 결과에서 두 번째 SELECT문의 조회 결과를 제외한 행을 출력함
