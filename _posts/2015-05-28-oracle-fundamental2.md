---
layout: post
title: "오라클 강좌 - Oracle Fundamental#2"
description: "오라클 DBMS 기본 및 SQL문 "
comments: true
category: Study
tags: [oracle, database]
---



##테이블 무결성 제약조건

**데이터 무결성 제약 조건(Data Integrity Constraint Rule)**
테이블에 부적절한 자료(데이터)가 입력되는 것을 방지하기 위해서 테이블을 생성할 때
각 컬럼에 대해서 정의하는 여러 가지 규칙을 의미.

- Not null/null : null을 허용할 지 아니면 반드시 입력받게 할 건지의 조건.
- Unique : 지정된 컬럼에 중복되지 않고 유일한 값만 저장되는 조건.
- Primary key(기본키) : unique + not null
- Check : 특정한 값만 저장되는 필드 조건.
- Default : 기본값으로 특정 값이 저장되도록 설정하는 조건.
- Foreign key(외래키) : 다른 테이블의 컬럼에 들어있는 값만 저장을 허용하는 조건.

**USER_CONSTRAINTS 데이터 딕셔너리 뷰 : 제약 조건에 관한 정보를 알려 줌**

- owner : 제약 조건을 소유한 사용자명을 저장하는 컬럼.
- constraint_name : 제약 조건명을 저장하는 컬럼.
- constraint_type : 제약 조건 유형을 저장하는 컬럼.
	- P : Primary key
    - R : Foreign key
    - U : Unique
    - C : Check, Not null
- table_name : 각 제약 조건들이 속한 테이블의 이름.
- search_condition : 어떤 내용이 조건으로 사용되었는지 설명.
- r_constraint_name : 제약 조건이 foreign key인 경우 어떤 primary key를 참조했는지에 대한 정보를 갖음.

```
SQL>desc user_constraints;   
SQL>column constraint_type format a15
SQL>select constraint_name, constraint_type, table_name from user_constraints;
```
**user_cons_columns 데이터 딕셔너리 뷰 : 어떤 컬럼에 어떤 제약 조건이 지정되었는지 알려주는 데이터 딕셔너리**

```
SQL>column owner format a10
SQL>column constraint_name format a15
SQL>column table_name format a15
SQL>column column_name format a15
SQL>select * from user_cons_columns;
```

###컬럼 레벨 제약 조건 지정 방법

####NOT NULL 제약 조건을 설정하지 않고 테이블 생성

```
SQL>drop table emp01;
SQL>create table emp01
        (
            empno number(4),
            ename varchar2(10),
            job varchar2(10),
            deptno number(2)
        );

SQL>insert into emp01
        values (null, null, 'SALEMAN', 40);
```

####not null 제약 조건을 걸고 테이블 생성

```
SQL>drop table emp02;
SQL>create table emp02
        (
            empno number(4) not null,
            ename varchar2(10) not null,
            job varchar2(10),
            deptno number(2)
        );
SQL>insert into emp02
        values (null, null, 'SALEMAN', 40);--[error]
SQL>insert into emp02
        values (null, 'SCOTT', 'SALEMAN', 40);--[error]
SQL>insert into emp02
        values (7499, 'ALLEN', 'SALEMAN', 30);
```

####Unique 제약 조건을 설정하여 테이블 생성

```
SQL>drop table emp03;
SQL>create table emp03
        (
            empno number(4) unique,
            ename varchar2(10) not null,
            job varchar2(10),
            deptno number(2)
        );
SQL>insert into emp03
        values (7499, 'ALLEN', 'SALEMAN', 30);
SQL>insert into emp03
        values (7566, 'JONES', 'MANAGER', 20);
SQL>insert into emp03
        values (null, 'SMITH', 'CLERK', 20);
SQL>insert into emp03
        values (null, 'JAMES', 'CLERK', 20);
SQL>insert into emp03
        values (null, 'JAMES', 'CLERK', 20);
```

####not null & unique 제약 조건 : 컬럼 레벨로 제약 조건명 명시하기.

- 사용자가 제약 조건명을 지정하지 않고 제약 조건만을 명시할 경우 오라클 서버가 자동으로 제약 조건명을 부여한다.
- 오라클이 부여하는 제약 조건명은 SYS_ 다음에 숫자를 나열한다.
- 어떤 제약 조건을 위배했는지 알 수 없기 때문에 사용자가 의미있게 제약 조건명을 명시할 수 있도록 오라클은 제공.

```
SQL>drop table emp04;
SQL>create table emp04
        (
            empno number(4) constraint emp04_empno_uk unique,
            ename varchar2(10) constraint emp04_ename_nn not null,
            job varchar2(10),
            deptno number(2)
        );
SQL>insert into emp04
        values (7499, 'ALLEN', 'SALEMAN', 30);
SQL>insert into emp04
        values (7499, 'JONES', 'MANAGER', 20);

SQL>select table_name, constraint_name
        from user_constraints
        where table_name in ('EMP04');
```

#### Primary key 제약 조건 설정하기

```
SQL>drop table emp05;
SQL>create table emp05
        (
            empno number(4) constraint emp05_empno_pk primary key,
            ename varchar2(10) constraint emp05_ename_nn not null,
            job varchar2(10),
            deptno number(2)
        );
SQL>insert into emp05
        values (7499, 'ALLEN', 'SALEMAN', 30);
SQL>insert into emp05
        values (7499, 'JONES', 'MANAGER', 20);--[error]
SQL>insert into emp05
        values (null, 'JONES', 'MANAGER', 20);--[error]
```

#### 참조 무결성을 위한 Foreign key(외래키) 제약 조건

부모 키가 되기 위한 컬럼은 반드시 부모 테이블의 기본키(primary key)이거나 유일키(unique)로 설정되어 있어야 한다.

```
SQL>drop table dept01;
SQL>create table dept01
        (
             deptno number(2) constraint dept01_deptno_pk primary key,
             dname varchar2(14),
             loc varchar2(13)
        );
SQL>insert into dept01
        values (10, 'ACCOUNTING', 'NEW YORK');
SQL>insert into dept01
        values (20, 'RESEARCH', 'DALLAS');
SQL>insert into dept01
        values (30, 'SALES', 'CHICAGO');

SQL>drop table emp06;
SQL>create table emp06
        (
            empno number(4) constraint emp06_empno_pk primary key,
            ename varchar2(10) constraint emp06_ename_nn not null,
            job varchar2(10),
            deptno number(2) constraint emp06_deptno_fk references dept01(deptno)
        );
SQL>insert into emp06
        values (7499, 'ALLEN', 'SALEMAN', 30);
SQL>insert into emp06
        values (7566, 'JONES', 'MANAGER', 40);--[error]
```

####Check 제약 조건 설정하기

- 급여 컬럼을 생성하되 값은 500~5000  사이의 값만 저장 가능.
- 성별 저장 컬럼으로 gender를 정의하고 'M', 'F' 둘 중 하나만 저장 가능.

```
SQL>drop table emp07;
SQL>create table emp07
        (
               empno number(4) constraint emp07_empno_pk primary key,
               ename varchar2(20) constraint emp07_ename_nn not null,
               sal number(7, 2) constraint emp07_sal_ck check(sal between 500 and 5000),
               gender varchar2(1) constraint emp07_gender_ck check(gender in ('M', 'F'))
        );
SQL>insert into emp07
        values (7499, 'ALLEN', 200, 'M');--[error]
SQL>insert into emp07
        values (7499, 'ALLEN', 700, 'M');
SQL>insert into emp07
        values (7499, 'JONES', 900, 'A');--[error]
```

#### Default 제약 조건 설정하기

지역(loc) 컬럼에 아무 값도 입력하지 않을 때 디폴트 값인 'SEOUL'  입력되도록 디폴트 제약 조건 지정.

```
SQL>drop table dept02;
SQL>create table dept02
        (
               deptno number(2) primary key,
               dname varchar2(14),
               loc varchar2(12) default 'SEOUL'
        );
SQL>insert into dept02(deptno, dname)
        values (10, 'ACCOUNTING');
SQL>insert into dept02
        values (20, 'RESEARCH', 'NEW YORK');
```

#### 컬럼 레벨로 제약 조건명을 명시해서 제약 조건 설정하기

```
SQL>create table emp10
        (
            empno number(4) constraint emp06_empno_pk primary key,
            ename varchar2(10) constraint emp06_ename_nn not null,
            job varchar2(10),
            deptno number(2) constraint emp06_deptno_fk references dept01(deptno)
        );
```

#### 테이블 레벨 방식으로 제약 조건 설정하기

**not null 제약 조건은 테이블 레벨 방식으로 제약 조건을 지정할 수 없다.**

```
SQL>create table emp11
        (
            empno number(4),
            ename varchar2(10) constraint emp06_ename_nn not null,
            job varchar2(10),
            deptno number(2),
            primary key(empno),
            unique(job),
            foreign key(deptno) references dept01(deptno)
        );

SQL>create table emp11
        (
            empno number(4),
            ename varchar2(10) constraint emp06_ename_nn not null,
            job varchar2(10),
            deptno number(2),
            constraint emp11_empno_pk primary key(empno),
            constraint emp11_job_uk unique(job),
            constraint emp11_deptno_fk foreign key(deptno) references dept01(deptno)
        );
```

####제약 조건 추가하기

```
SQL>alter table emp01
        add constraint emp01_empno_pk primary key(empno);

SQL>alter table emp01
        add constraint emp01_deptno_fk foreign key(deptno) references dept(deptno);

SQL>insert into emp01
        values (null, null, 'SALEMAN', 40);
```

####not null 제약 조건 추가 하기

```
SQL>alter table emp01
        modify ename constraint emp01_ename_nn not null;
```

####제약 조건 제거하기

```
SQL>alter table emp01
        drop primary key;
SQL>alter table emp01
        drop constraint emp01_ename_nn;
```

####제약 조건(외래키) 삭제

1. 제약 조건의 비활성화
	- 자식 테이블인 사원테이블(emp06)은 부모테이블(dept01)에 기본키인 부서번호를 참조하고 있다.
	- 부서 테이블의 30번 부서는 사원 테이블에 근무하는 30번 사원이 존재하기 때문에 삭제할 수 없다.
	- 부모테이블의 부서번호 30번이 삭제되면 자식테이블에서 자신이 참조하는 부모를 잃어버리게 되므로 삭제할 수 없는 것이다.
	- 어떻게 삭제?
		1. 부서 테이블의 30번 부서에서 근무하는 사원을 삭제한 후 부서 테이블에서 30번 부서를 삭제.
		2. 참조 무결성 때문에서 삭제가 불가능하므로 emp06 테이블의 외래키 제약 조건을 제거한 후 30번 부서를 삭제.
	- 제약조건의 비활성화
		1. 테이블에서 제약 조건을 삭제하지 않고 일시적으로 적용시키지 않도록하는 방법으로 제약 조건을 비활성화 하는 방법.
		2. DISABLE_CONSTRAINT : 제약  조건의 일시 비활성화
		3. ENABLE_CONSTRAINT : 비활성화된 제약 조건을 해제하여 다시 활성화.

2. cascade 옵션
	- 부모테이블(dept01)과 자식 테이블(emp06) 간의 참조 설정(외래키)이되어 있을 때 부모테이블의 제약 조건을 비활성화하면
이를 참조하고 있는 자식테이블의 제약 조건까지 비활성화 시켜주는 옵션.

```
SQL>delete from dept01
        where deptno=30;

//제약 조건의 비활성화
SQL>alter table emp06
        disable constraint emp06_deptno_fk;
SQL>delete from dept01
        where deptno=30;
SQL>alter table emp06
        enable constraint emp06_deptno_fk;
SQL>insert into dept01
        values (30, 'SALES', 'CHICAGO');

//cascade 옵션
SQL>alter table dept01
         disable primary key cascade;
SQL>alter table dept01
         drop primary key;
SQL>alter table dept01
         drop primary key cascade;
```

##시퀀스(Sequence)

- 유일(UNIQUE)한 값을 생성해주는 오라클 객체이다.
- 시퀀스를 생성하면 기본키와 같이 순차적으로 증가하는 컬럼을 자동적으로 생성 할 수 있다.
- 보통 PRIMARY KEY 값을 생성하기 위해 사용 한다.
- 메모리에 Cache되었을 때 시퀀스값의 액세스 효율이 증가 한다.
- 시퀀스는 테이블과는 독립적으로 저장되고 생성된다.

테이블 생성 후 시퀀스(일련번호)를 따로 만들어야 한다.

####샘플테이블 생성

```
SQL>create table memos
        (
            num number(4) primary key,
            name varchar2(20) not null,
            postDate Date default (sysdate)
        );
```

###해당 테이블의 시퀀스 생성

```
SQL>create sequence memos_seq
    start with 1 increment by 1 ;
```

####데이터 입력 : 일련번호 포함

```
SQL>insert into memos(num, name)
    values (memos_seq.nextval, '홍길동');
SQL>insert into memos(num, name)
    values (memos_seq.nextval, '홍길서');
SQL>insert into memos(num, name)
    values (memos_seq.nextval, '홍길남');
SQL>insert into memos(num, name)
    values (memos_seq.nextval, '홍길북');
```

#### 현재 시퀀스가 어디까지 증가되어져 있는지 확인

```
SQL>select memos_seq.currval from dual;
```

###시퀀스 수정 : 최대 증가값을 6까지로 제한

```
SQL>alter sequence memos_seq maxvalue 6;
SQL>insert into memos(num, name)
    values (memos_seq.nextval, '이길동');
SQL>insert into memos(num, name)
    values (memos_seq.nextval, '박길동');
```

###시퀀스 삭제

```
SQL>drop sequence memos_seq;

cf) 권한 부여
   - system으로 접속 후
      SQL>grant create sequence to 사용자명;
```

##트랜잭션(transaction)

[개념](http://www.gurubee.net/lecture/1403)

- commit : 명령어 완전 실행.
- rollback : 명령어 되돌리기(실행 해제)

####테이블 생성

```
SQL>create table trans
        (
              num number(2) primary key,
              name varchar2(20) not null,
              email varchar2(50),
              title varchar2(150),
              postDate Date default sysdate,
              postIP varchar2(20)
        );
```

####시퀀스(일련번호) 생성

```
SQL>create sequence trans_seq
    start with 1 increment by 1;

SQL>insert into trans
    values (trans_seq.nextval, '홍길동', 'a@a.com', '안녕하세요, 홍길동입니다.',
    sysdate, '127.0.0.1');
SQL>insert into trans
    values (trans_seq.nextval, '홍길서', 'b@b.com', '안녕하세요, 홍길서입니다.',
    sysdate, '127.0.0.1');
```

##뷰(View)

- 물리적인 테이블에 근거한 논리적인 가상 테이블.
- 가상이란 단어는 실질적으로 데이터를 저장하고 있지 않기 때문에 붙인 것이고, 테이블이란 단어는 실질적으로 데이터를 저장하고 있지는 않지만 사용자는 마치 테이블을 사용하는 것과 동일하게 뷰를 사용할 수 있기 때문에 붙인 것.
- 뷰는 기본 테이블에서 파생된 객체로서 기본 테이블에 대한 하나의 쿼리문임.
- 실제 테이블에 저장된 데이터를 뷰를 통해서 볼 수 있도록 함.
- 사용자에게 주어진 뷰를 통해서 기본 테이블을 제한적으로 사용하게 됨.
- 뷰는 이미 존재하고 있는 테이블에 제한적으로 접근하도록 한다.
- 뷰를 생성하기 위해서는 실질적으로 데이터를 저장하고 있는 물리적인 테이블이 존재해야 하는데 이 테이블을 기본 테이블이다.

#### 뷰의 기본 테이블 생성

```
//dept 테이블의 복사본을 dept_copy 으로 생성함.
SQL>create table dept_copy
    as
    select * from dept;

//emp 테이블의 복사본을 emp_copy으로 생성함.
SQL>create table emp_copy
    as
    select * from emp;
```

### 뷰 정의하기

- 뷰를 생성하기 위해서는 create view로 시작함. as 다음은 마치 서브쿼리문과 유사함.
- 서브쿼리에는 우리가 지금까지 사용했던 select 문을 기술하면 됨.

```
// 만일 30번 부서에 소속된 사원들의 사번과 이름과 부서번호를 자주 검색한다고 한다면
SQL>select empno, ename, deptno
    from emp_copy
    where deptno = 30;

SQL>create view emp_view30
    as
    select empno, ename, deptno
    from emp_copy
    where deptno = 30;

SQL>select * from emp_view30;
```

#### 뷰의 내부구조와 user_views 데이터 딕셔너리

```
SQL>desc user_views;
SQL>select view_name, text from user_views;
```

### 뷰의 동작 원리

1. 사용자가 뷰에 대해서 질의를 하면 user_views에서 뷰에 대한 정의를 조회한다.
2. 기본 테이블에 대한 뷰의 접근 권한을 살핀다.
3. 뷰에 대한 질의를 기본 테이블에 대한 질의로 변환한다.
4. 기본 테이블에 대한 질의를 통해 데이터 검색한다.
5. 검색된 결과를 출력한다.

#### 뷰와 기본 테이블 관계 파악

1. 뷰를 통한 데이터 저장이 가능?

```
SQL>insert into emp_view30 values (8000, 'ANGEL', 30);
SQL>select * from emp_view30;
SQL>select * from emp_copy;
```

2. insert 문에 뷰(emp_view30)를 사용하였지만, 뷰는 쿼리문에 대한 이름일 뿐 새로운 레코드는 기본 테이블(emp_copy)에 실질적으로 추가되는 것이다.

3. 뷰는 실질적인 데이터를 저장한 기본 테이블을 볼 수 있도록 한 투명한 창이다. 즉, 기본 테이블의 모양이 바뀐 것이고 그 바뀐 내용을 뷰라는 창을 통해서 볼 뿐이다.
뷰에 insert 뿐만 아니라, update, delete 모두 사용할 수 있는데, 이 명령문 역시 뷰의 텍스트에 저장되어 있는 기본 테이블을 변경하는 것이다.


####뷰를 사용하는 이유

1. 복잡하고 긴 쿼리문을 뷰로 정의하면 접근을 단순화 시킬 수 있다.
2. 보안에 유리하다. - 사용자마다 특정 객체만 조회할 수 있도록 권한을 부여할 수 있기 때문

###뷰의 종류

뷰는 뷰를 정의하기 위해서 사용되는 기본 테이블의 수에 따라 두 가지로 나뉜다.

1. 단순뷰(simple view)
	- 하나의 테이블로 생성
	- 그룹 함수의 사용이 불가능
	- distinct 사용이 불가능
	- insert/update/delete(DML) 등의 사용 가능
2. 복합뷰(complex view)
	- 여러 개의 테이블로 생성
	- 그룹 함수의 사용이 가능
	- distinct 사용이 가능
	- DML 사용 불가능

####단순뷰

*단순 뷰에 대한 데이터 조작*

```
SQL>insert into emp_view30 values(8010, 'HONG', 30);
```

*단순 뷰의 컬럼에 별칭 부여하기*

```
SQL>create view emp_view_copy(사원번호, 사원명, 급여, 부서번호)
	as
	select empno,ename,sal,deptno
	from emp_copy;

SQL>select * from emp_view_copy;

SQL>select * from emp_view_copy where 부서번호=30;
```

*그룹 함수를 사용한 단순 뷰*
뷰를 작성할 때 select절 다음에 sum이라는 그룹함수를 사용하면 결과를 뷰의 특정 컬럼처럼 사용하게 된다. 따라서 물리적인 컬럼이 존재하지 않는 가상 컬럼이기에 컬럼명도 상속 받을 수 없다.
뷰를 생성할 때 가상 컬럼을 사용하려면 사용자가 반드시 별칭을 따로 설정해야한다.

```
SQL>create view view_sal
	as
	select deptno, sum(sal) sum, avg(sal) avg
	from emp_copy
	group by deptno;
```

**단순 뷰의 경우 DML 명령어의 조작이 불가능한 경우**

1. 뷰 정의에 포함되지 않은 컬럼 중에 기본 테이블의 컬럼이 not null 제약 조건이 지정되어 있는 경우 insert문 사용이 불가능하다.
왜냐하면 뷰에 대한 insert문은 기본 테이블에 null 값을 입력하는 형태가 되기 때문이다.
2. sal * 12  와 같이 산술 표현식으로 정의된 가상 컬럼이 뷰에 정의되면 insert/update가 불가능하다.
3. distinct을 포함한 경우에도 DML(data manipulation language) 명령을 사용할 수 없다.
4. 그룹 함수나 group by절을 포함한 경우에도 DML 명령을 사용할 수 없다.

####복합뷰

```
SQL>select empno,ename,sal,e.deptno,dname,loc
	from emp e, dept d
	where  e.deptno = d.deptno
	order by empno desc;

SQL>create view emp_view_dept
	as
	select empno,ename,sal,e.deptno,dname,loc
	from emp e, dept d
	where  e.deptno = d.deptno
	order by empno desc;
```

###뷰 삭제

```
SQL>drop view emp_view;
```

###뷰 생성에 사용되는 다양한 옵션

1. **or replace**
존재하지 않는 뷰이면 새로운 뷰를 생성하고 기존에 존재하는 뷰이면 그 내용을 변경한다.

2. force/noforce
force - 기본 테이블이 존재하지 않을 때도 뷰를 생성해야 되는 경우 사용하는 옵션.
noforce - 기본 테이블이 존재하는 경우만 뷰가 생성(default)

3. with check option
뷰를 생성할 때 조건 제시에 사용된 컬럼 값을 변경하지 못하도록 하는 기능을 제공
뷰를 설정할 때 조건으로 설정한 컬럼 이외의 다른 컬럼의 내용은 변경할 수 있음

4. **with read only**
기본 테이블의 어떤 컬럼에 대해서도 뷰를 통한 내용 수정을 불가능하게 만드는 옵션

##Top Query

상위 몇 개의 데이터만을 출력하고 싶을 때 사용하는 쿼리
Top-N을 구하기 위해 rownum & 인라인뷰가 사용

```
SQL>select rownum, empno, ename, hiredate from emp;
SQL>select rownum, empno, ename, hiredate
	from emp
	order by hiredate

SQL>create or replace view view_hire
    as
    select empno, ename, hiredate
    from emp
    order by hiredate;
SQL>select rownum, empno, ename, hiredate
    from view_hire;
SQL>select rownum, empno, ename, hiredate
	from view_hire
	where rownum <= 5

//인라인 뷰를 사용 - 중요!!!
SQL>select rownum, empno, ename, hiredate
	from (select empno, ename, hiredate
    	  from emp
    	  order by hiredate)
	where rownum <= 5;

```

##사용자(User) 권한(Rule)

###권한의 역할과 종류

- 권한 : 사용자가 특정 테이블을 접근할 수 있도록 하거나 해당 테이블에 sql(select/insert/update/delete)문을 사용 할 수 있도록 제한을 두는 것.
- 데이터베이스 보안을 위한 권한은 시스템 권한과 객체 권한으로 나눌 수 있다.
- 시스템 권한: 사용자의 생성과 제거, DB 접근 및 각종 객체를 생성할 수 있는 권한 등 DBA에 부여됨.
- 객체 권한 : 객체를 조작할 수 있는 권한.

###실습

#### user01 계정 생성

```
SQL>conn system/manager
SQL>create user user01 identified by tiger;
```

#### 데이터베이스 접속 권한

```
SQL>grant create session to user01;
SQL>create table emp01
        (
             empno number(4),
             ename varchar2(10),
             job varchar2(10),
             deptno number(2)
        );--[error]
```

#### 테이블 생성 권한

```
SQL>grant create table to user01;
SQL>create table emp01
        (
             empno number(4),
             ename varchar2(10),
             job varchar2(10),
             deptno number(2)
        );
SQL>insert into emp01 values(7326,'SMITH','CLERK',20);
```

#### 테이블 스페이스 확인
테이블 스페이스는 디스크 공간을 소비하는 테이블과 뷰 그리고 그 밖의 다른 데이터베이스 객체들이 저장되는 장소

```
SQL>alter user user01 quita 2m on users;
SQL>insert into emp01 values(7326,'SMITH','CLERK',20);
```

#### with admin option
사용자에게 시스템 권한을 with admin option과 함께 부여하면 그 사용자는 데이터베이스
관리자가 아닌데도 불구하고 부여받은 시스템 권한을 다른 사용자에게 부여할 수 있는 권한을 함께 부여받게 된다.

```
SQL>create user user02 identified by tiger;
SQL>grant create session to user02 with admin option;
SQL>create user user03 identified by tiger;
SQL>conn user02/tiger
SQL>grant create session to user03;
```

#### 테이블 객체에 대한 select 권한 부여

```
//scott/emp -> user01
SQL>conn scott/tiger 계정 접속
SQL>grant select on emp to user01;
SQL>conn user01/tiger;
SQL>select * from emp;
```

#### 스키마(SCHEMA)

객체를 소유한 사용자 명을 의미한다.

```
SQL>select * from scott.emp;
```

#### 사용자에게 부여된 권한 조회

- user_tab_privs_made : 현재 사용자가 다른 사용자에게 부여한 권한 정보를 알려줌
- user_tab_privs_recd : 자신에게 부여된 사용자 권한을 알고 싶을 떄

```
SQL>select * from user_tab_privs_made;
SQL>select * from user_tab_privs_recd;

//한번에 권한 부여
SQL>grant create session, create table, create view to user01;
```

#### 객체 권한 제거하기

```
SQL>conn scott/tiger
SQL>select * from user_tab_privs_made;
SQL>revoke select on emp from user01;
```

#### with grant option

사용자에게 객체 권한을 with grant option과 함께 부여하면 사용자는 객체를 접근할 권한을 부여 받으면서 그 권한을 다른 사용자에게 부여할 수 있는 권한도 함께 부여받게 된다.

```
SQL>conn scott/tiger
SQL>grant select on scott.emp to user02 with grant option;
SQL>conn user02/tiger
SQL>grant select on scott.emp to user01 with grant option;
```

#### 사용자 계정 제거

```
SQL>conn system/manager
SQL>drop user user03;
```

#### 롤(Role)

- 사용자에게 보다 효율적으로 권한을 부여할 수 있도록 여러 개의 권한을 묶어 놓은 것.
- 사용자를 생성했으면 그 사용자에게 각종 권한을 부여해야만 생성된 사용자가 데이터베이스를 사용할 수 있음.

1. connect role
	- 사용자가 데이터베이스에 접속 가능하도록 하기 위해서 다음과 같이 가장 기본적인 시스템 권한 8가지 묶어 놓은 권한.
	- alter session, create cluster, create database link, create sequence, create session, create synonym, create table, create view

2. resource role
	- 사용자가 객체 (테이블, 뷰, 시퀀스)를 생성할 수 있도록 시스템 권한을 묶어 놓은 것.
	- create cluster, create procedure, create sequence, create table, create trigger

3. DBA role
	- 사용자들이 소유한 데이터베이스 객체를 관리하고 사용자들이 작성하고 변경하고 제거 할 수 있도록 하는 모든 권한을 가짐.

```
SQL>conn system/manager
SQL>create user user03 identified by tiger;
SQL>grant connect, resource to user03;
SQL>conn user03/tiger
SQL>select * from dict
    where table_name like '%ROLE%';
```

##연습문제

[문제1] 기본테이블은 EMP_COPY로 합니다. 20번 부서에 소속된 사원들의 사번과 이름, 부서번호와 상관의 사번을 출력하기 위한 select문을 emp_view20이라는 이름의 뷰로 정의해 보세요.

```
SQL>create or replace view emp_view20
	as
	select empno, ename, deptno, mgr
	from emp_copy
	where deptno=20;
```

[문제2] 각 부서별 최대 급여와 최소 급여를 출력하는 뷰를 sal_view 라는 이름으로 작성하세요.

```
SQL>create or replace view sal_view
	as
	select deptno, max(sal) "최대급여", min(sal) "최소급여"
	from emp_copy
	group by deptno
	order by deptno;
```

[문제3] 인라인뷰를 이용하여 급여를 많이 받는 순서대로 3명만 출력하는 뷰(sal_top3_view)를 작성하세요.

```
SQL>select rownum, empno, ename, sal
	from (select empno, ename, sal
	  	  from emp_copy
		  where sal is not null
	  	  order by sal desc)
	where rownum <= 3;
```

[문제4] acorn/tiger 계정을 생성해서 순번/사원번호/이름/직업/근무부서(컬럼)의 항목을 갖는 테이블을 만들어서 데이터를 저장하되 순번 데이터 입력은 sequence를 이용하여 저장하고, view_acorn 뷰를 생성해서 순번/사원번호/이름만 출력할 수 있도록 만들어보세요.

```
SQL>conn system/manager

SQL>create user acorn identified by tiger;

SQL>grant create session, create table, create sequence, create view to acorn;

SQL>conn acorn/tiger

SQL>create table emp_acorn(
	num number(4) primary key,
	empno number(4),
	ename varchar2(10),
	job varchar2(10),
	dname varchar(10));

SQL>create sequence emp_seq start with 1 increment by 1 ;

SQL>insert into emp_acorn values(emp_seq.nextval, 10, 'IRONMAN', 'HERO', 'SHIELD');
SQL>insert into emp_acorn values(emp_seq.nextval, 10, 'HULK', 'HERO', 'SHIELD');

SQL>create view view_acorn
	as
	select num, empno, ename
	from emp_acorn;
```

## 인덱스(Index)

- 조회를 빠르게(빠른 검색) 하도록 도와준다.
- sql 명령문의 처리 속도를 향상시키기 위해서 컬럼에 생성하는 오라클 객체
- 장점
	- 검색 속도가 빨라진다.
	- 시스템에 걸리는 부하를 줄여서 시스템 전체 성능을 향상시킨다.
- 단점
	- 인덱스를 위한 추가적인 공간이 필요하다.
	- 인덱스를 생성하는데 시간이 걸린다.
	- 데이터의 변경작업(DML)이 자주 일어날 경우에는 오히려 성능이 떨어진다.

### 인덱스 정보 조회

```
SQL>select index_name, table_name, column_name
    from user_ind_columns
    where table_name in ('EMP', 'DEPT');
```

#### 조회 속도 비교하기

```
SQL>create table emp01
	as
	select * from emp;

SQL>select index_name, table_name, column_name
    from user_ind_columns
    where table_name in ('EMP', 'EMP01');
//emp 테이블에는 empno 컬럼에 인덱스가 존재하지만 emp를 서브 쿼리로 복사한 emp01 테이블에는 인덱스가 존재하지 않는다.
서브쿼리문으로 복사한 테이블은 구조와 내용만 복사할 뿐 제약조건은 복사되지 않기 때문이다.

SQL>insert into emp01
	select * from emp01;
//자신의 데이터를 그대로 덧붙여서 추가한다. 수행할때마다 2배씩 늘어나는 셈이다.
많은 양의 데이터를 추가한 후 실습을 해본다.

SQL>insert into emp(empno, ename)
	values(8000, 'ANGEL');
SQL>set timing on	//시간 측정
SQL>select empno, ename
	from emp01
	where ename='ANGEL';

```

### 인덱스 생성 & 제거

기본키(primary key)나 유일키(unique)가 아닌 컬럼에 대해서 인덱스를 지정하려면 `create index` 를 사용한다.

```
//인덱스 생성 후 다시 조회하면 시간이 단축된걸 알 수 있다.
SQL>create index idx_emp01_ename
	on emp01(ename);
SQL>select empno, ename
	from emp01
	where ename='ANGEL';

//인덱스 제거
SQL>drop index idx_emp01_ename;

//인덱스의 물리적인 구조과 인덱스의 재생성(최적화)
SQL>alter index idex_emp01_ename rebuild;
```

####인덱스 사용 기준

- 인덱스를 사용해야 하는 경우
	- 테이블에 행(레코드)의 수가 많을 때
	- where문에 해당 컬럼이 많이 사용될 때
	- 검색 결과가 전체 데이터의 2~4% 정도 일 때
	- join에 자주 사용되는 컬럼이나 null을 포함하는 컬럼이 많은 경우

- 인덱스를 사용하지 말아야 하는 경우
	- 테이블에 행(레코드)의 수가 적을 때
	- where문에 해당 컬럼이 자주 사용되지 않을 때
	- 검색 결과가 전체 데이터의 10~15% 이상 일 때
	- 테이블에 DML 작업이 많은 경우

### 인덱스 종류

1. 고유 인덱스(Unique Index)
	- 기본키나 유일키처럼 유일한 값을 갖는 컬럼에 대해서 생성하는 인덱스
	- `create unique index`
2. 비고유 인덱스(Nonunique Index)
	- 중복된 데이터를 갖는 컬럼에 대해서 인덱스를 생성(default)
3. 단일 인덱스(Single Index)
	- 한 개의 컬럼으로 구성한 인덱스
4. 결합 인덱스(Composite Index)
	- 두 개 이상의 컬럼으로 인덱스를 구
5. 함수 기반 인덱스(Function Based Index)


##저장 프로시저(stored procedure)

- 복잡한 쿼리문들을 필요할 때 마다 다시 입력할 필요없이 간단하게 호출만 해서 복자한 쿼리문의 생행결과를 얻어올 수 있다.
- 성능도 향상되고, 호환성 문제도 해결된다.
- 여러 번 반복 호출해서 사용할 수 있다.

```
SQL>create table emp01
        as
        select * from emp;
SQL>ed proc01;

SQL>@proc01
SQL>execute del_all
SQL>ed proc01
SQL>show error
```

*proc01*

```
create or replace procedure DEL_ALL
is
begin
delete from emp01;
end;
/
```

### 저장프로시저 조회하기

```
SQL>desc user_source
SQL>select name, text
        from user_source;
```

### 저장 프로시저의 매개변수

```
SQL>ed proc02;
SQL>@proc02
SQL>execute del_ename('SMITH');
```

*proc02*

```
create or replace procedure del_ename(vename emp01.ename%type)
is
begin
delete from emp01 where ename = vename;
end;
/
```

### IN, OUT, INOUT 매개변수

```
SQL>ed proc03;
SQL>@proc03;

SQL>variable var_ename varchar2(15);
SQL>variable var_sal number;
SQL>variable var_job varchar2(9);

SQL>execute sel_empno(7788, :var_ename, :var_sal, :var_job);

SQL>print var_ename;
SQL>print var_sal;
SQL>print var_job;
```

*proc03*

```
create or replace procedure sel_empno
( vempno in emp.empno%type,
  vename out emp.ename%type,
  vsal out emp.sal%type,
  vjob out emp.job%type
)
is
begin
select ename, sal, job into vename, vsal, vjob
from emp where empno = vempno;
end;
/
```
