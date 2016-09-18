---
layout: post
title: "오라클 강좌 - Oracle Fundamental"
description: "오라클 DBMS 기본 및 SQL문 "
comments: true
category: Study
tags: [oracle, database]
---



##개요

데이터베이스는 모든 프로그램의 핵심이며 데이터를 효율적으로 관리하는 방법으로 **DBMS(DataBase Management System)** 라는 소프트웨어를 사용하게 된다.

데이터베이스를 사용하는 목적은 다음과 같다.

- 데이터 중복의 최소화
- 데이터의 공유
- 데이터의 보안유지
- 데이터의 무결성 유지
- 데이터의 독립성

다양한 DBMS 중에서 가장 잘 알려진 것이 **관계형 데이터베이스 시스템(RDBMS)** 이다. RDBMS로는 Oracle, MS-SQL, MySQL 등이 있다.
RDBMS는 정형화된 데이터 항목들의 집합체로서 확장이 쉽다는 장점이 있다. 처음 데이터베이스를 만든 후 관련되는 응용 프로그램들을 변경하지 않고도 새로운 데이터 항목을 데이터베이스에 추가할 수 있다.
RDBMS는 2차원 테이블 구조로 데이터를 관리하는데, 열을 칼럼(column)이라고 하며 행을 레코드(record)라고 한다. 데이터의 중복을 피하면서 무결성을 보장하기 위해서 다양한 제약 조건을 지정할 수 있다.


##사용자 계정 관련

커맨드 창에서

**sqlplus 아이디/비밀번호** 혹은 **sqlplus 아이디** 입력 후 비밀번호 입력하여 로그인

*이 후 모든 예제는 오라클 설치시 기본적으로 생성되는 scott 아이디로 실습한다. 이 아이디에는 실습을 위한 테이블들이 자동적으로 생성되어 있다.*

**conn 사용자/비밀번호**
- connection의 약자로 sql에 접속된 상태에서 다른 사용자로 로그인

**show user;**
- 접속한 user 정보를 출력

**alter user 사용자 identified by 비밀번호**
- 사용자 비밀번호를 변경

**alter user 사용자 account unlock**
- 사용자의 lock을 해제

**alter user 사용자 identified by 비밀번호 account unlock**
- 사용자 비밀번호를 변경 후 계정의 lock을 해제

##select 문

**select * from tab;**
- 접속된 계정이 소유한 테이블 목록을 출력

**desc 테이블**
- 테이블 구조를 출력한다.

**select * from 테이블**
- 테이블의 모든 데이터 출력

**select 칼럼, 칼럼 from 테이블**
- 테이블의 특정 칼럼의 데이터 출력

**select deptno as "부서번호" from emp**
- depno를 별칭으로 출력 가능하며 이때 as는 생략이 가능하다.

**select distinct job from emp**
- distinct 키워드로 중복을 제거한 job의 정보 출력

**select empno, ename, sal from emp where sal>=3000**
- 급여가 3000 이상인 사원 정보 출력

**select * from emp where ename='SCOTT'**
- 이름이 scott인 사원의 정보 출력, *sql문은 대소문자를 구분 안하지만 데이터는 대소문자를 구분하므로 주의해야한다.*

**select * from emp where deptno <> 10**
- 부서 번호가 10이 아닌 사원

**select * from emp where sal >= 1000 and sal <= 3000** 혹은
**select * from emp where sal between 1000 and 3000**
- 급여가 1000~3000사이인 사원

**select * from emp where sal in (1300, 1500, 1600)**
- 급여가 1300 또는 1500 또는 1600인 사원 정보

**select * from emp where ename like 'K%'**
- 이름이 'K'로 시작하는 사원

**select * from emp where ename like '_A%'**
- 2번째에 'A'가 들어가는 사원

**select * from emp where comm is not null**
- 커미션을 받는 사람

**select * from emp order by empno asc**
- 사번의 오름차순 정렬, 오름차순은 기본이기 때문에  asc 생략이 가능하다.

**select * from emp where order by empno desc**
- 사번의 내림차순 정렬

**select ename, sal, sal*12 "연봉" from emp**
- 사원의 연봉 계산

**select ename, sal, comm, nvl(comm, 0), sal*12+nvl(comm,0) "연봉" from emp**
- 커미션을 포함한 연봉 계산, comm에 null이 있는 경우 0으로 대체해서 계산해준다.

##Group 함수

합계: `sum()`
**select sum(sal) from emp**

카운트: `count()`
**select count(*) from emp**

평균: `avg()`
**select avg(sal) from emp**

최대, 최소: `max()`, `min()`

###Group by

직업별 급여 평균
**select job, avg(sal) from emp group by job**

###Having

직업별 급여 평균(단, 평균 2000이상)
**select job, avg(sal) from emp group by job having avg(sal) >= 2000**

##내장 함수

샘플 테이블 `dual`
- select 1234*12 from dual

`round()` : 반올림
- select deptno, round(sal, -3) from emp where deptno=30;

`lower()` : 모든 문자를 소문자로 변환

`upper()` : 모든 문자를 대문자로 변환

`initcap()` : 첫자만 대문자로 변환

`concat()` : 문자열 연결
- select concat('a', 'bc') from dual;

`length()`, `lengthb()` : 문자열 길이

`substr()`, `substrb()` : 문자열 추출

`instr()` : 문자열 시작 위치

`lpad()`, `rpad()` : 자리 채우기

`trim()` : 컬럼이나 대상 문자열에서 특정 문자가 첫번째 글자이거나 마지막 글자이면 잘라내고 남은 문자열만 반환

`abs()` : 절대값

`floor()` : 소수점 이하 버리기

`trunc()` : 특정 자리 자르기

`mod()` : 나머지 연산

`sysdate()` : 현재 날짜

`months_between()` : 개월 수 구하기

`add_months()` : 개월 수 더하기
- select add_months(sysdate, 4) from dual;

`next_day()` : 다가올 요일에 해당하는 날짜
- select next_day(sysdate, '일요일') from dual;

`last_day()` : 해당 달의 마지막 일 수
- select last_dat(sysdate) from dual;

`to_char()` : 문자열로 반환
- select to_char(sysdate, 'yyyy-mm-dd') from dual;

`to_date()` : 날짜형으로 변환
- select to_date('2015/3/2', 'yyyy/mm/dd') from dual;

`nvl()` : NULL인 데이터를 다른 데이터로 변경
- select ename, nvl(comm, 0) from emp;

`decode()` : switch 문과 같은 기능

`case()` : if elseif else

##테이블 생성/수정/삭제

테이블 생성: `create`

```
create table exam01
(
	num number(2),
	name varchar2(20),
	sal number(7, 2)
);
```

기존 테이블과 동일하게 테이블 만들기
- create table exam02 as select * from emp;

기존 테이블에서 새로운 컬럼 추가 `alter add`
- alter table exam01 add (job varchar2(10));

테이블 구조 수정: 필드 수정 `alter modify`
- alter table exam01 modify (job varchar2(20));

테이블 구조 수정: 필드 삭제 `alter drop column`
- alter table exam01 drop column job;

테이블 삭제 `drop`
- drop table exam02

임시 테이블 삭제
- purge recyclebin;

처음부터 테이블 완전 삭제
- drop table exam02 purge;

테이블 이름 변경 `rename`
- rename exam01 to exam02;

테이블 내의 모든 데이터(레코드) 삭제 `truncate`
- truncate table exam01;

##오라클 자료형

1. 정수형 타입 : number(2)는 총 자리수가 2자리인 정수형 값이 필드에 저장된다.
2. 실수형 타입 : number(6, 2)는 소수점을 포함한 총 자리수가 6자리이고, 2는 소수점 둘째자리까지 있는 실수형 값이 저장된다.
3. 가변형 문자열 : varchar2 - 입력 데이터가 실제 크기를 넘어서면, 넘어선 크기만큼 자료형 크기가 늘어나지 않으며 그 이하면 그 크기만큼 줄어들어 저장된다.
4. 고정형 문자열 : char - char(10)에서 5크기 만큼 삽입해도 10만큼 저장된다.

##테이블명, 컬럼명 명명 규칙

1. 반드시 문자로 시작해야됨.
2. 1~30자까지 가능.
3. A~Z까지의 대소문자와 0~9까지의 숫자 조합, 특수기호는 (_ $ #)만 가능.
4. 오라클에서 사용되는 예약어나 다른 객체명과 중복 불가.
5. 공백 허용 안됨.

##데이터 삽입/수정/삭제

데이터 입력 : `insert into`
- insert into exam02(deptno, dname, loc) values (10, 'account', 'korea');

데이터 입력 : 행 생략(테이블에 저장된 데이터 순서대로 넣어야한다.)
- insert into exam02 values(20, 'account', 'korea');

null 값 입력
- insert into exam02 values(30, 'research', null);

부서 번호 변경 : 필드의 데이터 변경 `update set`
- update exam03 set deptno=30

급여 10% 인상
- update exam03 set sal=sal*1.1;

부서번호가 10인 사원의 부서번호를 20으로 변경
- update exam03 set deptno=20 where deptno=10;

사원 이름이 SCOTT인 자료의 부서번호를 10, 직급을 MANAGER로 변경
- update exam03 set deptno=10, job='MANAGER' where ename='SCOTT';

30번 부서 사원을 삭제 `delete`
- delete from exam03 where deptno=30

모든 자료 삭제
- delete from exam03;

##join

두 개 이상의 테이블에 나뉘어져 있는 데이터를 한 번의 sql문으로 원하는 결과를 얻을 수 있는 기능
[Join 자세히](http://mirwebma.tistory.com/17)

### Cross Join

2개 이상의 테이블이 조인될 때 where절에 의해 공통되는 컬럼에 의한 결함이 발생되지 않는 경우를 의미한다. 아무런 의미 없는 Join이다.

`select * from emp, dept;`

### Equi Join

가장 많이 사용되는 방법으로 조인대상이 되는 두 테이블에서 공통적으로 존재하는 컬럼을 연결하여 결과를 생성 하는 조인 방법

`select * from emp,dept where emp.deptno=dept.deptno`

`select ename, dname from emp e, dept d where e.deptno=d.deptno and ename='SCOTT'`

### Non-Equi Join

값을 비교하면서 두 개의 테이블을 연결하는 조인방법
`select ename,sal,grade from emp,salgrade where sal between losal and hisal;`

### Self Join

하나의 테이블 내에서 조인을 해야 원하는 자료를 얻는 join 방법

```
select employee.ename, manager.ename
from emp employee, emp manager
where employee.mgr = manager.empno;
```

### Outer Join

```
select employee.ename, manager.ename
from emp employee, emp manager
where employee.mgr = manager.empno(+)
```

## ANSI Join

여러 DBMS 간의 공통된 sql 명령어 규악

### Cross join

select * from emp *cross join* dept;

### Inner join(Equi join)

select ename, dname from emp *inner join* dept *on* emp.deptno = dept.deptno where ename='SCOTT'

select emp.ename, dept.dname from emp *inner join* dept *using (deptno)*;

select ename, dname from emp *natural join* dept;

### Outer join

## 서브쿼리

- 서브쿼리는 하나의 select 문장의 절 안에 포함된 또 하나의 select 문이다.
- 메인쿼리/서브쿼리
- 서브쿼리는 비교 연산자의 오른쪽에 기술해야하고 반드시 괄호로 둘러쌓아야야한다.
- 서브쿼리는 메인 쿼리가 실행되기 이전에 한 번만 실행이된다.
- 단일행 서브쿼리, 다중행 서브쿼리

[문제] SCOTT와 동일한 직급(job)을 가진 사원을 출력하는 sql문을 작성해 보세요.

```
 SQL>select ename, job
     from emp
     where job = ( select job
                   from emp
                   where ename = 'SCOTT'
                  );
```

[문제] SCOTT의 급여와 동일하거나 더 많이 받는 사원명과 급여를 출력해보세요.

```
SQL>select ename, sal
    from emp
    where sal >= ( select sal
                   from emp
                   where ename = 'SCOTT'
                  );
```


###서브쿼리 & 그룹 함수

[문제] 평균 급여보다 더 많은 급여를 받는 사원 출력하세요.

```
SQL>select ename, sal
    from emp
    where sal > ( select avg(sal)
                  from emp
                 );
```

###다중행 서브쿼리

- IN연산자 - 서브쿼리의 출력결과중 하나라도 일치하면 참이 된다.
- ALL 연산자
	- 서브쿼리의 출력결과와 모두 일치하면 참이된다.
	- > all은 '모든 비교값보다 크냐'고 묻는 것이 되므로 최대값보다 더 크면 참이 된다.
- ANY, SOME 연산자
	- 서브쿼리의 출력결과와 하나이상의 조건이 만족하면 참이 된다.
	- > any 는 찾아진 값에 대해서 하나라도 크면 참이 된다.(최소값)
- EXIST 연산자 - 서브쿼리의 결과중에서 만족하는 값이 하나라도 존재하면 참이 된다.


[문제] 급여를 3000이상 받는 사원이 소속된 부서와 동일한 부서에서 근무하는 사원들의 정보를 출력하세요.

```
SQL>select ename, sal, deptno
	from emp
	where deptno in (select deptno
					 from emp
					 where sal >= 3000
					);
```

[문제] 부서별로 가장 급여를 많이 받는 사원의 정보를 출력하세요.

```
SQL>select empno, ename, sal, deptno
	from emp
	where sal in (
					select max(sal)
					from emp
					group by deptno
				 );
```

[문제] 영업사원들보다 급여를 많이 받는 사원들의 이름과 급여를 출력하되 영업사원은 출력하지 않게 명령문을 작성해보세요.

```
SQL>select ename, sal
    from emp
    where sal > all ( select sal
                      from emp
                      where job='SALESMAN'
                     );
```

부서 번호가 30번인 사원들의 급여 중 가장 낮은 값(950)보다 높은 급여를 받는 사원의 이름, 급여를 출력하는 명령문을 작성해 보세요.

[단일행 서브쿼리 & 그룹함수]

```
SQL>select ename, sal
    from emp
    where sal > ( select min(sal)
                  from emp
                  where deptno = 30
                 );
```

[다중행 서브쿼리]

```
SQL>select ename, sal
    from emp
    where sal > any (
                     select sal
                     from emp
                     where deptno = 30
                     );
```

[문제] 영업 사원들의 최소 급여보다 많이 받는 사원들의 이름과 급여와 직급을 출력하되 영업 사원은 출력하지 않습니다.

```
SQL>select ename, sal, job
	from emp
	where sal > any (
					 select sal
					 from emp
					 where job='SALESMAN'
					 )
	and job <> 'SALESMAN';
```

##연습 문제

1. 10번 부서 소속의 사원 중에서 커미션을 받는 사원의 수를 구해보세요.
	- select count(*) from emp where deptno=10 and comm > 0;

2. 가장 최근에 입사한 사원의 입사일과 입사한 지 가장 오래된 사원의 입사일을 출력해보세요.
	- select max(hiredate) "최근입사일", min(hiredate) "오래된 입사일" from emp ;

3. SCOTT과 동일한 근무지에서 근무하는 사원의 이름을 출력하세요.
	- select ename from emp where deptno=(select deptno from emp where ename='SCOTT');
	- select e.ename from emp e, emp d where e.deptno=d.deptno and d.ename='SCOTT';

4. 직급이 'SALESMAN'인 사원이 받는 급여들의 최소 급여보다 많이 받는 사원들의 이름과 급여를 출력하되
부서 번호가 20번인 사원은 제외한다.
	- select ename, sal from emp where sal > any (select sal from emp where job='SALESMAN') and deptno<>20;

5. 직급이 'SALESMAN'인 사원이 받는 급여들의 최대 급여보다 많이 받는 사원들의 이름과 급여를 출력하되
부서번호가 10번인 사원은 제외한다.
	- select ename, sal from emp where sal > all (select sal from emp where job='SALESMAN') and deptno<>10;

6. 사원테이블(emp)과 부서 테이블(dept)을 join하여 사원 이름과 부서번호와 부서명을 출력하세요.
단 40번 부서의 부서 이름도 출력 되도록 하세요.
	- select ename, d.deptno, dname from emp e, dept d where e.deptno(+)=d.deptno;

7. 뉴욕에서 근무하는 사원의 이름과 급여를 출력하세요(join 이용)
	- select ename, sal from emp e, dept d where e.deptno=d.deptno and loc='NEW YORK';
	- select ename, sal from emp inner join dept using(deptno) where loc='NEW YORK';
	- select ename, sal from emp natural join dept where loc='NEW YORK';
