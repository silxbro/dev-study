# SQL 개요
<br/>

마당서점 고객이 '축구의 역사'라는 도서의 출판사와 가격을 알고 싶어한다. 프로그래머는 어떻게 해야 할까?
다행히 데이터베이스에서는 몇 가지 사실만 DBMS에 알려주면 원하는 답을 얻을 수 있다. 즉 '도서', '축구의 역사', '출판사', '가격'과 같은 단어를 적절히 알려주면 된다.
이와 같이 DBMS에게 원하는 내용을 알려주고 결과를 얻는 데 사용하는 데이터베이스 전용 언어가 SQL이다. 위의 예를 SQL로 표현하면 다음과 같다.

```SQL
SELECT  bookname, publisher, price
FROM    Book
WHERE   bookname LIKE '축구의 역사';
```

DBMS는 SQL 문을 해석하고 프로그램으로 변환하여 실행한 후 결과를 알려준다. SQL은 데이터를 다루는 데 있어서 자바, C++, C 같은 일반 프로그래밍 언어보다 훨씬 쉽고 편리하다.

SQL(Structured Query Language)은 1970년대 후반 IBM이 SEQUEL(Structured English QUEry Language)이라는 이름으로 개발한 관계형 데이터베이스 언어이다.
이후 1986년 ANSI(American National Standards Institute)에 의해 관계형 데이터베이스 표준 언어로 승인되었다.
SQL의 후속 버전은 1992년에 SQL2, 1999년에 SQL3로 확장되었으며 SQL3는 객체지향의 개념을 일부 포함하고 있다.

SQL은 자바나 C 같은 완전한 프로그래밍 언어는 아니다.
대신 데이터 부속어(data sublanguage)라고 부르는데, 그 이유는 데이터베이스의 데이터와 메타 데이터(데이터 구조에 관한 데이터)를 생성하고 처리하는 문법만 갖고 있기 때문이다.
SQL은 DBMS에 직접 입력해 사용할 수 있고, 자바나 C로 작성된 클라이언트/서버 응용 프로그램에 삽입하여 사용할 수도 있다.
또 HTML 웹 페이지 문서에 삽입할 수 있고, 보고서나 데이터 추출 프로그램에서도 사용할 수 있다. 또한 Visual Studio.NET이나 다른 개발 도구에서 직접 실행할 수도 있다.

#### [SQL과 일반 프로그래밍 언어의 차이점]
|구분|SQL|일반 프로그래밍 언어|
|:---|:---|:---|
|용도|데이터베이스에서 데이터를 추출하여 문제 해결|모든 문제 해결|
|입출력|입력은 테이블, 출력도 테이블|모든 형태의 입출력 가능&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|
|번역|DBMS|컴파일러|
|문법|SELECT *<br/>FROM  Book;|int main()<br/>{...}|

SQL은 기능에 따라 데이터 정의어(DDL, Data Definition Language)와 데이터 조작어(DML, Data Manipulation language), 데이터 제어어(DCL, Data Control Language)로 나뉜다.
- **데이터 정의어(DDL)**
    - 테이블이나 관계의 구조를 생성하는 데 사용하며 CREATE, ALTER, DROP 문 등이 있다.
- **데이터 조작어(DML)**
    - 테이블에 데이터를 검색, 삽입, 수정, 삭제하는 데 사용하며 SELECT, INSERT, UPDATE, DELETE 문 등이 있다. 여기서 SELECT 문은 특별히 질의어(query)라고 부른다.
- **데이터 제어어(DCL)**
    - 데이터의 사용 권한을 관리하는 데 사용하며 GRANT, REVOKE 문 등이 있다.

#### [데이터 정의어와 데이터 조작어의 주요 명령어]
<img src="https://github.com/silxbro/cs-study/assets/142463332/2756d359-3431-4d6d-a2a0-6925937fbec6" width="350" height="200"/><br/>

데이터 정의어와 데이터 조작어에 대해 자세히 살펴보기 전에 SELECT 문의 예를 들어 SQL 문을 이해해보자. SELECT 문의 문장 프레임워크는 다음과 같다.
```
  SELECT    : 질의 결과 추출되는 속성 리스트를 열거한다.
  FROM      : 질의에 어느 테이블이 사용되는지 열거한다.
  WHERE     : 질의의 조건을 작성한다.
```
- 예를 들어 Customer 테이블에서 '김연아 고객의 전화번호를 찾으시오.'라는 질의를 SQL 문으로 표현하면 다음과 같다.
  ```
    SELECT    phone
    FROM      Customer
    WEHRE     name = '김연아';
  ```

SQL 문은 실행 순서가 없는 비절차적인(non procedural) 언어다. 즉 찾는 데이터만 기술하고 어떻게 찾는지 그 절차(실행 순서)는 기술하지 않는다.
그렇지만 위 SQL 문은 내부적으로 DBMS에 의하여 ① FROM 절에 쓰인 테이블을 가져와서 ② WHERE 절 조건에 의하여 튜플을 선택한 다음 ③ SELECT 절에 있는 속성들을 결과로 출력한다.

#### [SQL 문의 내부적 실행 순서]
<img src="https://github.com/silxbro/cs-study/assets/142463332/79cd57e6-f2ce-4c0b-ae54-d9f5381f6fba" width="550" height="140"/><br/>