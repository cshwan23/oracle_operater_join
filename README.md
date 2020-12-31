# oracle_operater_join
오라클 연산자, 조인 문제 


    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    -- Q37. employee 테이블에서 입사일이 1990~1999년 사이인 직원 검색?
    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm

    select*from EMPLOYEE
    where HIRE_DATE >= to_date('1990-1-1','YYYY-MM-DD')
      and HIRE_DATE < to_date('2000-1-1','YYYY-MM-DD');

    select * from EMPLOYEE
    where extract(year from HIRE_DATE) >= 1990
      and extract(year from HIRE_DATE) <= 1999;

    select * from EMPLOYEE
        where extract(year from HIRE_DATE) between 1990 and 1999;

    select * from EMPLOYEE
        where substr(to_char(hire_date,'YYYY'),1,3)='199';

    select * from EMPLOYEE
        where to_char(HIRE_DATE,'YYYY') like '199%';
    -- %의 의미는 무엇이 와도 좋아. 길이 제한 없어.
    -- like 연산자의 형태와 기능
    -- where 컬럼명 like '문자패턴' => 컬럼명 안의 데이터가 문자패턴에 맞으면 그 행을 보이라.


    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    -- Q38. employee 테이블에서 부서번호가 10번 또는 30번인 직원 중에 연봉이 3000미만이고
    --      입사일이 '1996-01-01' 이전 인 직원은 ?
    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    select * from EMPLOYEE
        where
          (DEP_NO = 10 or DEP_NO = 30)
          and SALARY < 3000
          and HIRE_DATE<to_date('1996-01-01','YYYY-MM-DD');

    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    -- Q39. employee 테이블에서 부서번호가 10번인 직원 중에 연봉이 2000미만
    --      또는 부서번호 20번인 직원중에 연봉이 3500 이상인 직원은?
    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    select * from EMPLOYEE
        where (DEP_NO = 10 and SALARY < 2000) or (DEP_NO = 20 and SALARY >= 3500);


    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    -- Q40. employee 테이블에서 성이 김씨 직원은?
    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    select * from EMPLOYEE
        where substr(EMP_NAME,1,1)='김';

    select * from EMPLOYEE
        where EMP_NAME like '김%';


    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    -- Q41. employee 테이블에서 직원 이름이 '라'로 끝나는 직원 검색
    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm

    select * from EMPLOYEE
        where substr(EMP_NAME,length(EMP_NAME),1)='라';

    select * from EMPLOYEE
        where EMP_NAME like '%라';


    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    -- Q42. employee 테이블에서 성이 김씨이고 3글자인 직원 검색
    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm

    select * from EMPLOYEE
        where EMP_NAME like '김%' and length(EMP_NAME)=3;
    select * from EMPLOYEE
        where substr(EMP_NAME,1,1) ='김' and length(EMP_NAME)=3;
    select * from EMPLOYEE
        where EMP_NAME like '김__';
    -- like 연산자 오른쪽에 나오는 '_' 의 의미 => 무엇이 나와도 좋은데 글자 1개야! 라는 뜻


    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    -- Q43. employee 테이블에서 이름에 김이 들어간 직원을 검색하면? 즉 이름에 김을 포함하는 직원은?
    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    select * from EMPLOYEE
        where EMP_NAME like '%김%';
    -- 칼럼명 like '%x%' => 컬럼에 'x' 를 포함하는 행은 나와라

    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    -- Q44. employee 테이블에서 이름이 2자인 직원을 검색하면?
    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    select * from EMPLOYEE where length(emp_name)=2;
    select * from EMPLOYEE where emp_name like '__';
    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    -- Q45. employee 테이블에서 이름이 중간에만 김이 들어간 직원을 검색하면?
    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    -------------------------
    -- like 반대 => not like
    -------------------------
        select * from EMPLOYEE where EMP_NAME
        like '%김%' and substr(EMP_NAME,1,1)='김' and substr(EMP_NAME,length(EMP_NAME),1)='김';

        select * from EMPLOYEE where EMP_NAME
        like '%김%' and EMP_NAME not like '%%김' and EMP_NAME not like '김%%';

        select * from EMPLOYEE where regexp_like(EMP_NAME,'^[^김][김]+[^김]$');


    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    -- Q46. employee 테이블에서 여자 직원을 검색하라
    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
        select *from EMPLOYEE
        where substr(JUMIN_NUM,7,1)='2' or substr(JUMIN_NUM,7,1)='4';

        select * from EMPLOYEE
        where substr(JUMIN_NUM,7,1) in('2','4');

        select * from EMPLOYEE
        where JUMIN_NUM like '______2%' or JUMIN_NUM like '______4%';


    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    -- Q47. 아래 select 문은 무슨 요구사항을 반영한 것인가?
    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
        select * from EMPLOYEE
        where JUMIN_NUM like'______1%'or JUMIN_NUM like '______2%';
        -- 즉 1900년대 생을 검색해라


    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    -- Q48.employee 테이블에서 1960년대, 1970년대 출생자 중 남자만 검색하라.
    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm

        select * from EMPLOYEE
        where (JUMIN_NUM like '6%' or JUMIN_NUM like'7%') and
              (JUMIN_NUM like '______1%' or JUMIN_NUM like '______3%');

        select * from EMPLOYEE
        where (substr(JUMIN_NUM,1,1)='6' or substr(JUMIN_NUM,1,1)='7') and
              (substr(JUMIN_NUM,7,1)='1' or substr(JUMIN_NUM,7,1)='3');

        select * from EMPLOYEE
        where (substr(JUMIN_NUM,1,1) in('6','7') )and
              (substr(JUMIN_NUM,7,1)='1' or substr(JUMIN_NUM,7,1)='3');

        select * from EMPLOYEE
        where (JUMIN_NUM like'6_____1%' or  JUMIN_NUM like'7_____1%');


    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    -- Q49.(함수문제) 직원번호, 직원명, 주민번호, 다가올생일(xxx-xx-xx(월)), 생일까지 남은 일수를 검색하면?
    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    select EMP_NO, EMP_NAME, JUMIN_NUM,
    --     sysdate 2020-12-29 2020-12-31 2020-12-30
    --     올해 내생일에서 오늘 날짜를 빼서 양수가 나오면 올해 내생일
    --     올해 내생일에서 오늘 날짜를 빼서 음수가 나오면 내년 내생일
    case when
        to_date(
            to_char(sysdate,'YYYY')||substr(JUMIN_NUM,3,4),'YYYYMMDD'
        )
        - sysdate
        > 0
        then to_char(
                to_date(
                    to_char(sysdate,'YYYY')||substr(JUMIN_NUM,3,4)
                    ,'YYYYMMDD'
                )
            ,'YYYY-MM-DD(dy)','NLS_DATE_LANGUAGE=Korean'
            )

        else to_char(
                to_date(
                    (to_number(to_char(sysdate,'YYYY'))+1)
                    ||substr(JUMIN_NUM,3,4)
                    ,'YYYYMMDD'
                )
            ,'YYYY-MM-DD(dy)','NLS_DATE_LANGUAGE=Korean'
            )
        end"다가올 생일 기념일"


    ,case
        when to_date(
            to_char(sysdate,'YYYY')||substr(JUMIN_NUM,3,4),'YYYYMMDD'
        )
        - sysdate
        > 0

        then
        ceil(
            to_date(
                    to_char(sysdate, 'YYYY') || substr(JUMIN_NUM, 3, 4)
                , 'YYYYMMDD'
                )
            - sysdate
            )
    else ceil(
            to_date(
                        (to_number(to_char(sysdate,'YYYY'))+1)
                        ||substr(JUMIN_NUM,3,4)
                        ,'YYYYMMDD'
                    )
            - sysdate
          )

    end "생일 D-day"

    from EMPLOYEE
    order by 4 asc;


    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    -- Q49. 아래 처럼 테이블을 만들고 데이터를 입력한 상태에서
    --      제품 총 개수를 내림차순으로 제일 많은 것부터 정렬하여 보이면?
    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm

        create table product (
            p_no number(3)
            ,p_name varchar2(20)    not null unique
            , tot_cnt varchar2(20)  not null
        );

        insert into product values(1,'컴퓨터1','20');
        insert into product values(2,'컴퓨터2','2');
        insert into product values(3,'컴퓨터3','4');
        insert into product values(4,'컴퓨터4','2');
        insert into product values(5,'컴퓨터5','16');
        insert into product values(6,'컴퓨터6','60');
        insert into product values(7,'컴퓨터7','30');
        insert into product values(8,'컴퓨터8','27');
        insert into product values(9,'컴퓨터9','25');
        insert into product values(10,'컴퓨터10','22');
        insert into product values(11,'컴퓨터11','34');
        insert into product values(12,'컴퓨터12','50');
        insert into product values(13,'컴퓨터13','8');
        insert into product values(14,'컴퓨터14','9');
        insert into product values(15,'컴퓨터15','10');

    select * from PRODUCT order by to_number(tot_cnt) desc;
    ---------------------------------------------------------
    -- 1. 길이가 긴걸 먼저 위로 올린다. 2. 아스키코드값 순서~
    select * from PRODUCT order by length(tot_cnt) desc, tot_cnt desc;

    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    -- join 문제 *******
    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    -- oracle 4대천왕 [1.함수 2.join 3.subquery 4.group by] => 모두 select 안에 나온다.
    -- 두개이상 테이블에서 칼럼을 복사해서 행으로 붙여 보여주는것을 join 조인이라고 한다

    -- 두개이상 테이블에서 칼럼을 복사해서 위아래로 붙여 보여주는것을 union 유니온이라고 한다
    select EMP_NAME"이름", JUMIN_NUM "주민번호" ,'직원' "구별" from EMPLOYEE
        union
    select CUS_NAME, JUMIN_NUM,'고객' from CUSTOMER;



    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    -- Q50. 직원번호, 직원명, 직급, 소속부서명"을 출력하면? (employee/dept)
    --      단 소속부서가 있는 놈만 나와라.(inner join)
    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    -- 서로 다른 칼럼을 좌우로 붙인다.
    --join은 where 절을 조심해야한다.

    ----------------------------
    -- 정답 1) 오라클 방식의 조인
    ----------------------------
        select
              EMPLOYEE.EMP_NO   "직원번호"
             ,EMPLOYEE.EMP_NAME "직원명"
             ,EMPLOYEE.JIKUP    "직급"
             ,DEPT.dep_name     "소속부서명"
        from EMPLOYEE,DEPT
        where EMPLOYEE.DEP_NO=DEPT.DEP_NO;
    -- where : 칼럼이 붙을 조건이 나오면 된다.
    ----------------------------
    -- 정답 2) 오라클 방식의 조인
    ----------------------------
        select
              e.EMP_NO      "직원번호"
             ,e.EMP_NAME    "직원명"
             ,e.JIKUP       "직급"
             ,d.dep_name    "소속부서명"
        from EMPLOYEE e , DEPT d  -- 테이블이름에 컬럼 별칭을 주면(변수처럼 사용가능.)
        where e.DEP_NO = d.DEP_NO; --칼럼이 붙을 조건이 나오면 된다.
    ----------------------------
    -- 정답 3) ANSI 방식의 조인
    ----------------------------
        select
              e.EMP_NO      "직원번호"
             ,e.EMP_NAME    "직원명"
             ,e.JIKUP       "직급"
             ,d.dep_name    "소속부서명"

        from EMPLOYEE e inner join DEPT d  -- 테이블이름에 컬럼 별칭을 주면(변수처럼 사용가능.)

        on e.DEP_NO = d.DEP_NO; --칼럼
        --inner join 조건에 맞는 놈만 나온다.

    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    -- Q51. 고객명, 고객전화번호, 담당직원명, 담당직원직급 을 출력하면? <조건> 담당직원이 있는 고객만 출력
    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm


    ----------------------------
    -- 정답 1) 오라클 방식의 Join
    ----------------------------
    select
        c.CUS_NAME  "고객명"
        ,c.TEL_NUM  "고객전화번호"
        ,e.EMP_NAME "담당직원명"
        ,e.JIKUP    "담당직원직급"
    from CUSTOMER c , EMPLOYEE e
    where c.EMP_NO = e.EMP_NO;
    ----------------------------
    -- 정답 2) ANSI 방식의 Join
    ----------------------------
    select
        CUSTOMER.CUS_NAME   "고객명"
        ,CUSTOMER.TEL_NUM   "고객전화번호"
        ,EMPLOYEE.EMP_NAME  "담당직원명"
        ,EMPLOYEE.JIKUP     "담당직원직급"
    from CUSTOMER inner join EMPLOYEE
    on CUSTOMER.EMP_NO = EMPLOYEE.EMP_NO;

    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    -- Q52. 고객명, 고객전화번호, 담당직원명, 담당직원직급 출력하면?
    --      <조건> 10번부서의 담당직원이 있는 고객만 출력
    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm

    ----------------------------
    -- 정답 1) 오라클 방식의 Join
    ----------------------------
    select
    c.CUS_NAME
    ,c.TEL_NUM
    ,e.EMP_NAME
    ,e.JIKUP
    from CUSTOMER c, EMPLOYEE e
    where c.EMP_NO=e.EMP_NO and e.DEP_NO=10;
    -- 오라클 조인에서 where 절에 join 조건과 행을 골라내는 조건이 같이 나온다.

    ----------------------------
    -- 정답 2) ANSI 방식의 Join
    ----------------------------
    select
    c.CUS_NAME
    ,c.TEL_NUM
    ,e.EMP_NAME
    ,e.JIKUP

    from CUSTOMER c inner join EMPLOYEE e
    on c.emp_no = e.EMP_NO
    where e.DEP_NO=10;
    -- [ansi 방식의 join 에서는] 조인 조건은 on 뒤에 나온다.
    -- [ansi 방식의 join 에서는] 행을 골라 내는 조건은 where 뒤에 나온다.

    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    -- Q53. 직원번호, 직원명, 직원직급, 직원소속부서명, 담당고객명, 고객전화번호 를 출력하면?
    --      <조건> 조건에 맞는 직원만 나오기, 직원이름 오름차순 정렬 (삼중inner조인)
    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    ----------------------------
    -- 정답 1) 오라클 방식의 Join
    ----------------------------
    select
     e.emp_no         "직원부서명"
    ,e.EMP_NAME       "직원명"
    ,e.JIKUP          "직원직급"
    ,d.DEP_NAME       "직원소속부서명"
    ,c.CUS_NAME       "담당고객명"
    ,c.TEL_NUM        "고객전화번호"

    from EMPLOYEE e , CUSTOMER c, DEPT d
    where c.EMP_NO= e.EMP_NO and e.DEP_NO = d.DEP_NO
    order by e.EMP_NAME asc;

    ----------------------------
    -- 정답 2) ANSI 방식의 Join
    ----------------------------

    select
     e.emp_no         "직원부서명"
    ,e.EMP_NAME       "직원명"
    ,e.JIKUP          "직원직급"
    ,d.DEP_NAME       "직원소속부서명"
    ,c.CUS_NAME       "담당고객명"
    ,c.TEL_NUM        "고객전화번호"

    from (EMPLOYEE e inner join DEPT d on e.DEP_NO = d.DEP_NO)
            inner join CUSTOMER c on c.EMP_NO= e.EMP_NO

    order by e.EMP_NAME asc;

    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    -- Q54. 직원번호e, 직원명e, 직원직급e, 연봉등급e 을 출력하면?
    -- <inner join>
    -- <2중조인>
    -- order by 연봉등급 오름차순, 직급높은순서 오름차순, 나이많은순서 내림 유지 요망.
    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm

    select
         e.emp_no         "직원번호"
        ,e.EMP_NAME       "직원명"
        ,e.JIKUP          "직원직급"
        ,s.SAL_GRADE_NO   "연봉등급"

    from EMPLOYEE e , SALARY_GRADE s
    where
        e.SALARY >= s.MIN_SALARY and e.SALARY <= s.MAX_SALARY

    order by s.SAL_GRADE_NO asc ,
             decode(e.JIKUP,'사장',1,'부장',2,'과장',3,'대리',4,'사원',5,6) asc ,
             case
                 when substr(e.JUMIN_NUM,7,1) in('1','2') then '19'
                 else '20'
            end || substr(e.JUMIN_NUM,1,6) asc;
    --   asc      /     desc
    -- 낮은거 먼저       많은거 먼저
