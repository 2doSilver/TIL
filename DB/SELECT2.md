# ORACLE


## SELECT

#### LOWER()
: ()안의 값을 모두 소문자로 나타내라.

    select
    *
    from emp
    where lower(job) = 'analyst';

- emp 테이블에서 job은 대문자로 표기되어있다. 하지만 이 때 해당 데이터가 모두 대문자로 입력되어있다는 것을 모른다면 원하는 데이터 값을 조회할 수 없다. 그래서 lower를 사용해 임의적으로 해당 데이터 값을 모두 소문자로 바꿔 검색하면 원래 값의 형태를 알지 못해도 검색을 잘 할 수 있다.


#### UPPER()
: ()안의 값을 모두 대문자로 나타내라.
- ex) smith -> SMITH

#### INITCAP()
: ()안의 값을 첫글자만 대문자로, 나머지는 소문자로 나타내라.
- ex) smith -> Smith


#### CONCAT(컬럼명A, 컬럼명B)
: A와 B값을 붙여서 나타내라.

    select
    concat(ename, sal)
    from emp;

- 이름과 급여가 붙여져서 조회된다.


    select
    concat(ename, '(', sal, ')')
    from emp;

- 이렇게도 하면 좋겠지만 concat은 두개밖에 못 받기 때문에 이 경우에는 그냥 '||'를 사용하면 좋다.


#### substr(컬럼명, n1, n2)
: 해당 컬럼의 n1번째 글자부터 n2개를 뽑아내라.

    select
    ename, substr(ename, 2, 3)
    from emp;

- ename에 들어있는 데이터를 2번째 글자부터 3개씩만 뽑아내 조회한다.
- ex) SMITH -> MIT / JONES -> ONE


#### length(컬럼명)
: 해당 컬럼이 몇 글자인지 나타내라.
- SMITH -> 5

#### instr(컬럼명, 컬럼에서찾고자하는것, n1, n2)
: 해당 컬럼의 n1번째 부터 n2번째의 찾고자하는 것을 검색해라. (찾고자하는 문자가 있는 자릿수를 나타낸다.)

- instr(ename, 'L') : ename의 데이터에서 (가장 처음에 나오는)L을 찾으세요. ex) MARTIN -> 0 / CLART -> 2 / MILLER -> 3

- instr(ename, 'L', 1, 1) : ename의 데이터 1번째자리부터 시작해 1번째로 나오는 L을 찾으세요. ex) MARTIN -> 0 / CLART -> 2 / MILLER -> 3

- instr(ename, 'L', 1, 2) : ename의 데이터 1번째자리부터 시작해 2번째로 나오는 L을 찾으세요. ex) MARTIN -> 0 / CLART -> 0 / MILLER -> 4

- instr(ename, 'L', 4, 1) : ename의 데이터 4번째자리부터 시작해 1번째로 나오는 L을 찾으세요. ex) MARTIN -> 0 / CLART -> 0 / MILLER -> 4

- instr(ename, 'L', 4, 2) : ename의 데이터 4번째자리부터 시작해 2번째로 나오는 L을 찾으세요. ex) MARTIN -> 0 / CLART -> 0 / MILLER -> 0


#### lpad(컬럼명, n, 빈칸에 채울 문자)
: 해당 컬럼을 총 n글자로 맞추고, 부족하면 왼쪽에 지정한 문자를 넣어라.
#### rpad(컬럼명, n, 빈칸에 채울 문자)
: 해당 컬럼을 총 n글자로 맞추고, 부족하면 오른쪽에 지정한 문자를 넣어라.

    select
    ename, lpad(ename, 7, '$')
    from emp;

- ex) SMITH -> $$SMITH / KING -> $$$KING


#### ltrim(컬럼명, 지우고자 하는 문자)
: 왼쪽에 지정한 문자가 있으면 지워라.
#### rtrim(컬럼명, 지우고자 하는 문자)
: 오른쪽에 지정한 문자가 있으면 지워라.

    select
    ename, rtrim(ename, 'N')
    from emp;

- ex) MARTIN -> MARTI / ALLEN -> ALLE


#### translate(컬럼명, A, B)
: 해당 컬럼에 있는 A를 B로 바꿔라.(1:1대응)

    select
    ename, sal, translate(sal, '0123456789', '영일이삼사오육칠팔구')
    from emp;

- ex) 1600 -> 일육영영 / 2450 -> 이사오공


#### replace(컬럼명, A, B)
: 해당 컬럼에 있는 A를 B로 바꿔라.

    select
    ename, replace(ename, 'A', '대박')
    from emp;

- ex) ALLEN -> 대박LLEN / CLART ->CL대박RT


#### ceil(올림), floor(내림), round(반올림), trunc(절삭)

    select
    sal/3, ceil(sal/3), floor(sal/3), round(sal/3), trunc(sal/3)
    from emp;

- 위에 순서대로 266.66...., 267, 266, 267, 266
- 양수 값을 나눌 때는  floor와 trunc의 값이 같다. 하지만 반대로 음수 값을 나눌 때는 ceil과 trunc의 값이 같다.

- round와 trunc는 값 뒤에 반올림/절삭할 위치까지 지정할 수 있다.

    select
    sal/3, round(sal/3, 2), round(sal/3), round(sal/3, -2)
    from emp;

- 위에 순서대로 266.66...., 266.67, 267, 300
- round(sal/3, 2) : 소숫점 두번째 자리에서 반올림하시오.
- round(sal/3, -2) : 두번째 자리에서 반올림하시오.


#### mod(컬럼명, n)
: 컬럼 값을 n으로 나눈 나머지
- sal = 800 / mod(sal, 7) = 2

#### power(컬럼명, n)
: 컬럼 값의 n승
- sal = 800 / power(sal, 2) = 640000



#### 예제

    1.
    select
    ename, sal, job
    from emp
    where lower(job) in ('manager', 'analyst');

    2.
    select
    upper(job), lower(job), initcap(job)
    from emp;

    3.
    select
    ename, sal, lpad(sal, 6, '* ')
    from emp;

    4.
    select
    ename, job, comm, round((sal+nvl(comm,0))* 12, -3)
    from emp
    order by 4 desc;

    5.
    select
    ename, sal, job
    from emp
    where lower(job) in ('manager', 'analyst') and comm is null
    order by job, sal desc;





### Oracle의 날짜 형식
    1. 세기, 년, 월, 일, 시간, 분, 초의 내부 숫자 형식을 가진다.
    2. 기본 날짜 형식 : 'DD-MON-YY'
    3. 날짜 범위 : January 1, 4712 B.C와 December 91, 9999 A.D의 사이
    4. sysdate : 현재의 날짜와 시간을 리턴 (mysql에서는 now())
    5. Dual 테이블(Oracle에만 있다.)
      - SYS에 의해 소유되며, 모든 사용자가 액세스 가능하다.
      - DUMMY라는 하나의 열과 x값을 가지는 하나의 행이 존재한다.
      - 오직 한번만 값을 리턴하고자 할 때 유용하다.
      - select 2+3 from dual; (dual테이블을 꼭! 지정해줘야한다.)





#### 근무일수, 근무주수, 근무월수 구하기

    select
    ename, hiredate, round(sysdate - hiredate) "근무일수"
    from emp;

- 오늘 날짜(sysdate)에서 입사일을 빼면 오늘까지의 근무일수를 구할 수 있다.


    select
    ename, hiredate, round((sysdate - hiredate)/7) "근무주수"
    from emp;

- 근무일수에서 7을 나눠주면 오늘까지의 근무주수를 구할 수 있다.


    select
    ename, hiredate, round(months_between(sysdate, hiredate)) "근무월수"
    from emp;

- 매 달 일이 달라서(28 or 30 or 31) 근무 월수는 months_between()를 통해 입사일부터 오늘까지의 월수를 구해야한다.


>추가
add_months(날짜, n) : 해당 날짜에서 n개월 후만큼의 날을 구할 수 있다.
ex) 81/12/17 -3개월-> 81/03/17 // 81/09/08 -3개월-> 81/12/08


##### ?

    select
    sysdate - to_date('90/05/28', 'yy/mm/dd')
    from dual;

- 이렇게 지정한 날에서 오늘까지의 일수를 구하고 싶다. 하지만 값이 '-'가 되서 나온다. 왜냐하면 년도를 1990이 아닌 2090으로 보고 계산하기 때문이다.

    select
    sysdate - to_date('1990/05/28', 'yyyy/mm/dd')
    from dual;

- 해서 이처럼 년도를 확실하게 1990이라고 보여줘야 제대로된 값이 나온다.
