# ORACLE


## SELECT

#### JOIN의 조건

- JOIN이란? 둘 이상의 테이블을 연결해 데이터를 검색하는 방법

- 여러 테이블에서 특정 열을 선택한다.
  - Where절 안에서 두 테이블의 공통되는 컬럼을 비교한다.

- 둘 이상의 테이블을 쿼래해 결과 집합을 생성한다.
  - 기본키(Primary Key)와 외래키(Foreign Key)를 조인의 조건의로 사용한다.
  - 테이블을 조인하려면 지정한 테이블에서 공통적으로 사용하는 열을 사용한다.


- JOIN의 종류 Equijoin, Non-Equijoin, Self join, Outer JOIN


###### Alias(별칭)

- 테이블 alias로 컬럼을 단순화, 명확화 할 수 잇다.
- 현재의 SELECT 문장에서만 유효하다.
- 별칭의 길이는 30자까지 가능하지만 짧을 수록 좋다.
- 의미없는 단어보다는 의미있는 단어를 별칭으로 지정해야한다.
- FROM절에서 테이블 별칭을 설정할 때, 이를 해당 SELECT문에서 테이블 이름 대신 사용한다.

      select
      emp.ename, emp.job, dept.deptno, dept.dname
      from emp, dept
      where emp.deptno = dept.deptno;

      select
      e.ename, e.job, d.deptno, d.dname
      from emp e, dept d
      where e.deptno = d.deptno;

      select
      ename, job, d.deptno, dname
      from emp e, dept d
      where e.deptno = d.deptno;

- 둘 다 같은 결과를 보여주지만 아래가 보기 편하다.
- 그리고 조회하는 테이블에 공통으로 있는 데이터가 아닌 경우에는 별칭을 붙이지 않아도 된다.


###### Cartesian Product(카티션 곱)

- 검색하고자 했던 데이터 뿐 아니라 조인에 사용된 테이블의 모든 데이터가 검색되는 현상이다.

- 카티션 곱이 발생하는 경우
  - 조인 조건을 정의하지 않았을 경우
  - 조인 조건이 잘못된 경우
  - 첫 번째 테이블의 모든 행들이 두 번째 테이블의 모든 행과 조인이 되는 경우
  - 테이블의 갯수가 N이라면 카티션 곱을 피하기위해 적어도 N-1개를 등가해야한다.


            select
            ename, e.deptno, dname
            from emp e, dept d;


- 이 경우 조인 조건이 정의되지 않았기 때문에 모든 데이터가 검색된다.
  > where e.deptno = d.deptno를 써줘야 한다.
