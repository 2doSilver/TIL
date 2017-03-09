# ORACLE


## SELECT

#### 1. 기본구조

    SELECT      [DISTINCT] {column [Alias], ...}
    FROM        TABLE
    [WHERE      condition]
    [ORDER BY   {column, expression} [ASC|DESC]];

1. SELECT : 원하는 컬럼을 선택
2. Alias  : 해당 column에 대한 다른 이름을 부여
3. DISTINCT : 중복된 행을 제거
4. FROM : 원하는 데이터가 저장된 테이블 명을 기술
5. WHERE : 조회되는 행을 제한(선택)
6. condition  : column, 표현식, 상수 및 비교연산자를 통한 제한 조건
7. ORDER BY : 정렬을 위한 옵션 (ASC : 오름차순(Default), DESC : 내림차순)

#### 2. 조회

    SELECT
    *
    FROM TAB;

- 현재 접속한 계정이 갖고 있는 모든 테이블을 보여주세요.


    DESC DEPT;

<결과표>

|이름|널?|유형|
|---|----|---|
|DEPTNO|NOT NULL|NUMBER(2)|
|DNAME|  |VARCHAR2(14)|
|LOC|  |VARCHAR2(13)|

- DEPT 테이블의 컬럼을 모두 보여주세요.
- NOT NULL : 해당 컬럼의 값은 비워져있으면 안된다.
- NUMBER(2) : 정수 2자리를 넣을 수 있다.
- VARCHAR2(14) : 문자를 14자리까지 넣을 수 있다.

> VARCHAR2 : ORACLE에서만 있는 데이터 형으로 CHAR는 문자를 바로 읽어오지만 VARCHAR2는 숨겨진 2바이트가 있어 데이터를 읽어올때 몇자리인지 확인한 후 문자를 읽어온다. 그래서 문자의 길이가 뒤죽박죽인 경우에는 VARCHAR2를 아닌 경우에는 CHAR를 사용한다.

    SELECT
    *
    FROM EMP;

- EMP테이블에 있는 모든 데이터를 보여주세요.

    SELECT
    EMPNO, ENAME, JOB, HIREDATE
    FROM EMP;

- EMP테이블에 있는 사번, 이름, 업무, 입사일을 보여주세요.


#### 3. WHERE

##### 1) 기본

    SELECT
    EMPNO, ENAME, DEPTNO
    FROM EMP
    WHERE DEPTNO = 10;

- EMP테이블에서 부서코드가 10인 경우에만 사번, 이름, 부서코드를 보여주세요.


    SELECT
    EMPNO, ENAME, SAL
    FROM EMP
    WHERE ENAME = 'FORD';

- EMP테이블에서 이름이 FORD인 사람의 사번, 이름, 급여를 보여주세요.
- 이 때 이름이 소문자인지 대문자인지 첫자만 대문자인지를 모를 경우에는 사용할 수가 없다. 그 때는 LIKE를 사용한다.


    SELECT
    ENAME, JOB, HIREDATE
    FROM EMP
    WHERE HIREDATE = '81/02/20';

- EMP테이블에서 입사일이 81년 2월 20일인 사람의 이름, 업무, 입사일을 보여주세요.
- 이 때 입사일이 기록된 양식을 모를 경우에는 사용할 수가 없다. 그 때는 LIKE를 사용한다.


    SELECT
    ENAME, JOB, SAL, (SAL+NVL(COMM,0))* 12 "연봉"
    FROM EMP
    WHERE COMM != NULL;

- COMM컬럼에는 NULL값이 포함되어 있다. 그래서 NULL값이 들어가지 않는 애들만 보고 싶은 경우에 NULL값을 제외해야 하는데 위에 처럼 !=를 사용해 하면 올바른 값이 나오지 않는다. 이 때는 IS [NOT]을 사용해 NULL값에 대해 처리해줘야 한다.


    SELECT
    ENAME, JOB, SAL, (SAL+NVL(COMM,0))* 12 "연봉"
    FROM EMP
    WHERE COMM IS NOT NULL;

- 이렇게 해주면 COMM이 NULL인 경우를 제외할 수 있다.
- 그리고 컬럼 값을 넣는 부분에서 계산식도 넣을 수 있다.

- 그리고 올바른 값과 NULL값을 계산하고 싶을 경우 NULL을 처리하는 방법은 오라클에서는 3가지가 있다.
  - NVL(COMM, 0) : COMM값이 NULL일 때 해당 값에 0을 넣어라.
  - NVL2(COMM, SAL+COMM, SAL) : COMM값이 NULL이 아니면 2번째, NULL이면 3번째 값을 넣어라.
  - COALESCE(SAL+COMM, SAL, ...) : 첫번째가 NULL이면 두번째, 두번째가 NULL이면 세번째, 이런 식으로 계속해서 NULL일 때 대체할 수 있는 값을 넣어줄 수 있다.


###### 예제

    1.
    select
    empno, ename, deptno
    from emp
    where deptno != 20;

    2.
    select
    empno, ename, sal, sal*2, sal+500
    from emp;

    3.
    select
    *
    from emp
    where ename = 'SCOTT' or sal >= 3000;

    4.
    select
    ename, sal, (sal+nvl(comm, 0))* 12
    from emp
    where (sal+nvl(comm, 0))* 12 <= 30000
          and (sal+nvl(comm, 0))* 12 >= 20000;

    5.
    select
    empno, ename, sal, comm
    from emp
    where comm is not null;



##### 2) BETWEEN a AND b

    SELECT
    ENAME, SAL
    FROM EMP
    WHERE SAL BETWEEN 1000 AND 3000;

    SELECT
    ENAME, SAL
    FROM EMP
    WHERE SAL >= 1000 AND SAL <= 3000;

- 이 둘은 같다.
- BETWEEN은 컬럼명 BETWEEN 값1 AND 값2의 구조로 해당 컬럼에 의해 값1보다 크거나 같고, 값2보다 작거나 같은 값을 구한다.
- 작은 값이 항상 앞에 와야 한다.


    SELECT
    ENAME
    FROM EMP
    WHERE ENAME BETWEEN 'K' AND 'Q';

- 문자도 BETWEEN에 사용가능하다. 대소문자를 구별해 소문자가 대문자보다 크다.(ASCII코드 값에 따라)
- 해당 SQL문에서 QUEEN은 찍히지 않는다. 그러므로 Q로 시작하는 사람도 모두 찍고 싶으면 'QZ'나 Q다음 값인 'R'로 값을 지정해줘야 한다.



##### 3) IN

    SELECT
    ENAME, DEPTNO
    FROM EMP
    WHERE DEPTNO IN (10, 30);

    SELECT
    ENAME, DEPTNO
    FROM EMP
    WHERE DEPTNO = 10 OR DEPTNO = 30;

- 이 둘은 같다.
- IN은 컬럼명 IN (값1, 값2, ...) 구조로 해당 컬럼이 값1 또는 값2 또는 ...에 해당하는 값을 골라낸다. (OR을 묶어놓은 것으로 보면 된다.)

- SELECT ENAME, DEPTNO FROM EMP WHERE DEPTNO = 10 OR 30;이런 식으로 사용해서는 안된다.



##### 3) LIKE

    SELECT
    ENAME, DEPTNO
    FROM EMP
    WHERE ENAME LIKE 'S%';

- 이름이 S로 시작하는 사람만 조회하겠다.


    SELECT
    ENAME, DEPTNO
    FROM EMP
    WHERE ENAME LIKE '%N';

- 이름이 N으로 끝나는 사람만 조회하겠다.


    SELECT
    ENAME, DEPTNO
    FROM EMP
    WHERE ENAME LIKE '%N%';

- 이름에 N이 들어가는 사람만 조회하겠다.


    SELECT
    ENAME, DEPTNO
    FROM EMP
    WHERE ENAME LIKE '____L%'

- 이름에 L이 들어가는데 L앞에 4글자가 있는 사람만 조회하겠다. (언더바의 갯수만큼 해당 문자 앞에 다른 문자가 들어가 있어야 한다.)



% 만약 %를 검색하고 싶다면 어떻게 해야 할까?

- WHERE ENAME LIKE '%%%'
  - 이렇게 할 경우 모든 값이 찍힌다.

- WHERE ENAME LIKE '%\%%'
   - 이렇게 할 경우 \가 있는 글자를 찾는다.

- WHERE ENAME LIKE '%\%%' ESCAPE '\'
   - 이렇게 할 경우 ESCAPE를 써서 \는 빼고 그 바로 뒤에 있는 %가 들어있는 값을 조회할 수 있다.
   - \대신 다른 걸 넣어줘도 된다. (WHERE ENAME LIKE '%a%%' ESCAPE 'a')
   - 이러한 방법을 %나 언더바를 검색할 때 사용한다.



#### 4) NOT

    SELECT
    ENAME, SAL
    FROM EMP
    WHERE SAL NOT BETWEEN 1000 AND 2000;

    SELECT
    ENAME, DEPTNO
    FROM EMP
    WHERE DEPTNO NOT IN (10, 30);

    SELECT
    ENAME, DEPTNO
    FROM EMP
    WHERE ENAME NOT LIKE '%N%';

- 위 처럼 NOT을 붙이면 해당 범위에 들어있지 않는 데이터를 조회할 수 있다.


###### 예제

    1.
    select
    ename, hiredate
    from emp    
    where hiredate between '81/01/01' and '82/01/01';

    2.
    select ename, hiredate
    from emp
    where hiredate like '81%';

    3.
    select
    ename, job
    from emp
    where job in('SALESMAN', 'MANAGER');

    4.
    select
    ename "이름", sal "급여", (sal+nvl(comm,0))* 12 "연봉"
    from emp
    where ename between 'C' and 'SZ';

    5.
    select
    ename
    from emp
    where ename like '%M_';

    6.
    select
    ename
    from emp
    where ename like '%#%%' escape '#';

    7.
    select
    ename, job
    from emp
    where job not in('CLERK', 'ANALYST');



##### 5) 주의

- AND와 OR가 같이 쓰일 경우, AND가 우선순위가 높아 먼저 처리된다.


    SELECT
    ENAME, SAL, JOB
    FROM EMP
    WHERE JOB = 'SALESMAN' OR JOB = 'PRESIDENT' AND SAL > 1500;

- 이 경우 OR보다 AND가 우선순위가 높기 때문에 앞에서 부터 순서대로 처리하는 것이 아닌 AND부터 처리한 후 OR로 넘어온다.
- 그래서 이는 PRESIDENT면서 급여가 1500이상인 사람 또는 SALESMAN인 사람을 조회한다.


    SELECT
    ENAME, SAL, JOB
    FROM EMP
    WHERE (JOB = 'SALESMAN' OR JOB = 'PRESIDENT') AND SAL > 1500;

- 그래서 SALESMAN이거나 PRESIDENT인 사람 중에 급여가 1500이 넘는 사람을 뽑고 싶으면 위와 같이 '()'를 통해 먼저 처리할 것을 설정해줘야 한다.




#### 4. ORDER BY

- ORDER BY는 테이블의 값을 지정한 기준에 따라 정렬하는 것으로 오름차순(ASC), 내림차순(DESC)할 수 있다.

    SELECT
    ENAME, SAL, DEPTNO
    FROM EMP
    ORDER BY SAL [ASC];

- SAL순으로 오름차순 정렬한다.
- ORDER BY는 오름차순 정렬이 DEFAULT라 뒤에 ASC를 붙여주지 않아도 알아서 오름차순으로 정렬이 된다.


    SELECT
    ENAME, SAL, DEPTNO
    FROM EMP
    ORDER BY DEPTNO DESC;

- DEPTNO순으로 내림차순 정렬한다.


    SELECT
    ENAME, SAL, DEPTNO
    FROM EMP
    ORDER BY 2;

    SELECT
    ENAME 이름, SAL 급여, DEPTNO 부서코드
    FROM EMP
    ORDER BY 급여;

- ORDER BY는 컬럼이 위치한 숫자나 별칭으로도 정렬이 가능하다. (테이블에서 이미 추출해놓은 값에 대해서 정렬을 하므로 인식이 가능하다.)

- 주의 : WHERE는 테이블 자체에서 추출해 사용하는 것이라 예를 들어 별칭의 경우 '급여'라는 것은 테이블에 존재하지 않으므로 사용이 불가능 하다.


    SELECT
    ENAME, SAL, DEPTNO, COMM
    FROM EMP
    ORDER BY COMM;

- ORACLE에서는 NULL을 미지의 큰 값으로 생각해 오름차순 정렬 시 맨 뒤로 값을 가게 한다. (MYSQL은 이와 반대다.)


    SELECT
    ENAME, SAL, DEPTNO
    FROM EMP
    ORDER BY DEPTNO DESC, SAL;

- ORDER BY에 컬럼명을 두개 이상 쓰면 앞 선 컬럼에 맞춰 정렬한 후, 동일 값에 대해 그 뒤 컬럼에 맞춰 정렬한다.
- 부서코드에 대해 내림차순 정렬하고 같은 부서에 대해서 급여순으로 오름차순 정렬한다.


##### 예제

    1.
    select
    empno, ename, sal, deptno
    from emp
    order by empno;

    2.
    select
    empno, ename, job, sal
    from emp
    where job in('MANAGER', 'CLERK')
    order by job, sal;

    3.
    select
    ename "이름", sal "급여", (sal+nvl(comm,0))* 12 "연봉"
    from emp
    order by 연봉 desc;  

    4.
    select
    ename, sal, deptno
    from emp
    where ename between 'C' and 'T'
    order by deptno, sal desc;      



#### 5. 기타 (별칭, |, DISTINCT)

    SELECT
    ENAME AS "이름", SAL "급여", DEPTNO 부서코드
    FROM EMP;

- 별칭은 '컬럼명 AS "별칭"'의 기본 구조를 가지나 AS나 ""를 생략해도 사용 가능하다.


    SELECT
    ENAME||'('||JOB||')'
    FROM EMP;

- 일단 위의 결과 값은 이름(직업)으로 보여준다.
- ||를 사용하면 문자를 붙여나오게 할 수 있다.
- ORACLE에서는 &&를 사용할 수 없고, ||는 OR의 의미를 가지지 않는다.


    SELECT
    DISTINCT DEPTNO
    FROM EMP;

- EMP테이블의 DEPTNO를 중복되지 않는 값만 조회한다.
