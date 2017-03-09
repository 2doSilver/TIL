# ORACLE


## SELECT

#### TO_CHAR(데이터, 양식)
: 숫자나 문자값을 양식에 맞게 varchar2문자로 변환한다.

    select
    ename, to_char(hiredate, 'yy"년" mm"월" dd"일"')
    from emp;

- ex) 80/12/17 -> 80년 12월 17일

> 자바에서는 ''가 문자 ""가 문자열
> Oracle에서는 ''가 문자, 문자열 구분 없이 사용하고,
  ""를 인용구로(""안에 있는 내용을 그대로 표시) 사용한다.


    select
    ename, to_char(sal, '999,999')
    from emp;

- ex) 1600 -> 1,600


    select
    ename, to_char(sal, '999,999.99')
    from emp;

- ex) 1600 -> 1,600.00


    select
    ename, to_char(sal, 'L999,999.99')
    from emp;

- ex) 1600 -> \1,600.00 (원단위표시인데 여기서 역슬래시로 뜨네..)
- L을 붙이면 그 나라의 화폐단위를 표시해준다.


    select
    ename, to_char(sal, '$999,999.99')
    from emp;

- ex) 1600 -> $1,600.00


    select
    ename, to_char(sal, '###999')
    from emp;

- ex) 1600 -> 001600


#### TO_NUMBER(char)
: 숫자를 포함하는 문자 스트링을 숫자로 변화한다.


#### TO_DATE(char, 양식)
: 날짜를 나타내는 문자로 명시된 양식에 따라서 날짜로 변환한다.
: 그런데 보통은 그냥 TO_CHAR를 더 많이 사용한다.


#### 날짜 형식 요소

| 날짜 형식 | 설명 |예시|
| :------------- | :------------- |:---|
| YYYY       | 네자리 연도       |2006년 -> 2006|
|YY|두자리 연도|2006년 -> 06|
|MM|달 수|12월 -> 12|
|MON|세 자리 영문 달 이름|12월 -> DEC|
|DDD|한해 단위에서의 날짜 수|12월 30일 -> 364|
|DD|한달 단위에서의 날짜 수|3월 23일 -> 23|
|D|일주일 단위에서의 날짜 수|토요일 -> 7|
|DY|세 자리 영문 요일 이름|일요일 -> SUN|
|HH(HH12)|12시간으로 표현되는 시간|오후 2시 -> 2|
|HH24|24시간으로 표현되는 시간|오후 2시 -> 14|
|MI|분||
|SS|초||
> yyyy : 현세기 + 년도 (rr : 0~49 현세기, 50~99 전세기)


#### 숫자 형식 모델
| 요소 | 설명 |예시|결과
| :------------- | :------------- |:---|:----|
|9|9의 수는 출력 폭을 결정|999999|1234|
|0|무효의 0을 출력|099999|001234|
|$|달러 기호 |$999999|$1234|
|L|지역 화폐 기호|L999999|\1234|
|.|명시한 위치에 소수점|999999.99|1234.00|
|,|명시한 위치에 콤마|999,999|1,234|
|MI|우측에 마이너스 기호(음수 값)|999999MI|1234-|
|PR|음수를 "()"로 묶는다.|999999PR|<1234>
|EEEE|과학적인 부호 표기|99.999EEEE|1.234E+03|
|V|10을 n번 곱한다.|9999V99|123400|
|B|0을 0이 아닌 공백으로 출력|B9999.99|1234.00|
> 마지막 세개는 무슨 뜻인지 모르겠네.



#### CASE
: CASE 함수는 SQL문장 내에서 if-then-else와 같은 흐름 제어문의 역할을 한다.

    select
    ename, job, sal,
    case lower(job) when 'analyst' then sal*1.1
            when 'salesman' then sal*1.2
            when 'manager' then sal*1.3
            when 'clerk' then sal* 1.4
            else sal*1.5 end "인상된 급여"
    from emp;

- job이 when이하일 때 then이하를 해줘라.
- 꼭 마지막에 end를 붙여줘야 한다.


#### DECODE
: CASE 함수처럼 조건적 조회를 해준다.

    select
    ename, job, sal,
    decode(lower(job), 'analyst', sal*1.1, 'salesman', sal*1.2,
    'manager', sal*1.3, 'clerk', sal*1.4, sal*1.5) "인상된 급여"
    from emp;

- job이 a일때 a'를 b일때 b'를 식으로 쭉 이어진다.



#### 예제

    1.
    select
    ename "이름", job " 업무", deptno"부서코드",
    case deptno when 10 then '회계팀'
            when 20 then '연구소'
            when 30 then '영업팀'
            else '운영팀' end "부서명"
    from emp;

    2.
    select
    ename "이름", job " 업무", deptno "부서코드",
    decode(deptno, 10, '회계팀', 20, '연구소',
    30, '영업팀', '운영팀') "부서명"
    from emp;



#### EMP TABLE의 계층화

    select
    ename
    from emp
    start with ename = 'KING'
    connect by prior empno = mgr;

    KING
    JONES
    SCOTT
    ADAMS
    FORD
    SMITH
    ...

- start with : 게층적인 출력 형식을 표현하기 위한 최상위 행
- connect by prior : 계층관계의 데이터를 지정하는 컬럼

- KING을 시작으로 위에서 아래로, 왼쪽에서 오른쪽으로 출력된다. (EMP테이블 계층적인 구조 보기) 이렇게 한 테이블 내에서 하는 걸 재귀적 관계라고 한다.


    select
    lpad(ename, length(ename)+level*2-2) 이름
    from emp
    start with ename = 'KING'
    connect by prior empno = mgr;

    KING
      JONES
        SCOTT
          ADAMS
        FORD
          SMITH
      ...

- level : 계층적 질의문에서 검색된 결과에 대해 계층별 레벨을 표시하는 것으로 보는 입장에 따라 달라진다. (시작되는 사람을 1레벨로 본다.)

- 레벨에 따라 왼쪽 공백이 생셔 계층적 구조를 더 잘 볼 수 있다.


    select
    lpad(ename, length(ename)+level*2-2) 이름
    from emp
    start with ename = 'ADAMS'
    connect by prior mgr = empno;

    ADAMS
      SCOTT
        JONES
          KING

- 자식입장에서 위를 보는 경우 connect by를 반대로 해줘야한다.


    select
    lpad(ename, length(ename)+level*2-2) 이름
    from emp
    where ename != 'FORD'
    start with ename = 'KING'
    connect by prior empno = mgr;

    KING
      JONES
        SCOTT
          ADAMS
          SMITH
      ...

- FORD가 잘린 경우 where문을 이용해 제거할 노드, 즉 FORD를 제외시켜준다. 그렇게 되면 기존 FORD밑의 SMITH는 FORD와 같은 레벨에 있던 SCOTT의 아래로 들어가게 된다.


    select
    lpad(ename, length(ename)+level*2-2) 이름
    from emp
    start with ename = 'KING'
    connect by prior empno = mgr and ename != 'FORD';

    KING
      JONES
        SCOTT
          ADAMS
      BLAKE
        ...

- 트리 구조에서 'FORD'의 브랜치를 제거하고 계층형 쿼리를 진행하려면 connect by 절 뒤에 제거할 브랜치의 최고 관리자를 제외하면 된다.
