# SQL 학습을 위한 준비
<br/>

우리가 자주 방문하는 서점의 데이터베이스 예를 통해 SQL의 개념과 주요 명령어를 살펴보자.

마당서점은 인터넷에서 도서를 판매하는 가상의 온라인 서점이다. 주문은 웹 사이트를 통해 받고 배송은 택배로 처리한다.
개설한지 얼마 되지 않아 아직은 취급하는 도서가 10권, 고객은 5명이다. 마당서점은 경영자, 운영자, 프로그래머가 웹 사이트의 구축과 운영을 지원한다.

마당서점을 운영하는 시스템 환경은 다음 그림과 같다.
도서와 고객에 대한 정보는 데이터베이스(MySQL DBMS 소프트웨어, 이하 MySQL로 부름)에 저장하여 관리하며, 운영자와 고객은 LAN 혹은 인터넷에 연결된 컴퓨터를 통해 데이터베이스에
접속한다. 이때 운영자와 고객은 모두 웹 사이트를 통해 데이터베이스에 접속하지만, 운영자는 특별히 데이터베이스 전용 관리 프로그램을 이용하여 접속할 수도 있다.

#### [마당서점 운영 시스템 환경]
<img src="https://github.com/silxbro/cs-study/assets/142463332/dbd58773-864e-4c4b-acf2-32d893566ae5" width="580" height="300"/><br/>
<br/>

## 1. 마당서점의 데이터베이스
마당서점의 데이터베이스는 도서(Book), 고객(Customer), 주문(Orders)에 관한 데이터를 저장하는 3개의 테이블로 구성되어 있다.

#### 📌 테이블이란?
관계 데이터베이스는 수학의 릴레이션 개념에 기초한다고 배웠다. 릴레이션은 실제 데이터베이스 업무에서 '테이블'이라는 용어로 많이 사용된다.
속성도 '열', 튜플도 '행'이라고 많이 부른다. 속성은 릴레이션의 스키마 부분을 의미하고, 튜플은 릴레이션의 데이터 부분을 의미한다.
3~5장에서는 테이블, 열, 행이라는 용어를 주로 사용한다.

|릴레이션 용어|실무에서 많이 사용되는 용어|같은 의미의 파일 시스템 용어|
|:---|:---|:---|
|릴레이션(relation)|테이블(table)|파일(file)|
|속성(attribute)|열(column)|필드(field)|
|튜플(tuple)|행(row)|레코드(record)|

#### 🔳 Book 테이블
Book 테이블은 도서에 관한 데이터를 저장하며 속성으로 bookid(도서번호), bookname(도서이름), publisher(출판사), price(정가)를 갖는다.
bookid는 기본키로, 각 도서를 식별하기 위해 임의로 만든 일련번호 값을 사용한다.
보통 테이블의 구조는 '테이블 이름(속성1, 속성2, ...)'와 같이 표현하고 기본키에는 밑줄을 긋는다.
- Book(<u>bookid</u>, bookname, publisher, price)

#### [Book 테이블]
|bookid|bookname|publisher|price|
|:---:|:---|:---|:---|
|1|축구의 역사|굿스포츠|7000|
|2|축구아는 여자|나무수|13000|
|3|축구의 이해|대한미디어|22000|
|4|골프 바이블|대한미디어|35000|
|5|피겨 교본|굿스포츠|8000|
|6|역도 단계별 기술|굿스포츠|6000|
|7|야구의 추억|이상미디어|20000|
|8|야구를 부탁해|이상미디어|13000|
|9|올림픽 이야기|삼성당|7500|
|10|Olympic Champions|Pearson|13000|

#### 🔳 Customer 테이블
Customer 테이블은 고객에 관한 데이터를 저장하며 속성으로 custid(고객번호), name(이름), address(주소), phone(전화번호)을 갖는다.
custid는 기본키로, 각 고객을 식별하기 위해 임의로 만든 일련번호 값을 사용한다. 값을 알 수 없는 경우 '값이 없음'을 나타내는 'NULL'로 표시하였다.
- Customer(<u>custid</u>, name, address, phone)

#### Customer 테이블
|custid|name|address|phone|
|:---|:---|:---|:---|
|1|박지성|영국 맨체스타|000-5000-0001|
|2|김연아|대한민국 서울|000-6000-0001|
|3|장미란|대한민국 강원도|000-7000-0001|
|4|추신수|미국 클리블랜드|000-8000-0001|
|5|박세리|대한민국 대전|NULL|

#### 🔳 Orders 테이블
Orders 테이블은 고객의 주문 사항을 저장하며 속성으로 orderid(주문번호), custid(고객번호), bookid(도서번호), saleprice(판매가격), orderdate(주문날짜)를 갖는다.
orderid는 주문번호를 나타내는 기본키며 custid와 bookid는 각각 고객번호와 도서번호를 가리키는 외래키다.
saleprice는 도서의 판매가격으로, 정가보다 할인된 가격으로 실제 판매한 금액이다. orderdate는 YYYY-MM-DD(예, 2019년 05월 22일은 2019-05-22) 형식으로 데이터를 저장한다.
이를 바탕으로 다음 표의 첫 번째 튜플을 보면 1번 고객 '박지성'이 1번 도서 '축구의 역사'를 주문하였고 판매가격은 6,000원이며 2014년 7월 1일에 주문했다는 것을 알 수 있다.
- Orders(<u>orderid</u>, custid, bookid, saleprice, orderdate)

#### [Orders 테이블]
|orderid|custid|bookid|saleprice|orderdate|
|:---|:---|:---|:---|:---|
|1|1|1|6000|2014-07-01|
|2|1|3|21000|2014-07-03|
|3|2|5|8000|2014-07-03|
|4|3|6|6000|2014-07-04|
|5|4|7|20000|2014-07-05|
|6|1|2|12000|2014-07-07|
|7|4|8|13000|2014-07-07|
|8|3|10|12000|2014-07-08|
|9|2|10|7000|2014-07-09|
|10|3|8|13000|2014-07-10|

다음 그림은 세 개의 테이블 간 속성의 참조 관계를 나타낸 것이다.
#### [마당서점의 데이터 구성도]
<img src="https://github.com/silxbro/cs-study/assets/142463332/991d3f0f-5e9d-4f74-873a-3185e80f9d58" width="350" height="250"/><br/>
<br/>
<br/>
## 2. 마당서점 데이터베이스의 사용자들
마당서점 데이터베이스를 사용하는 사람은 크게 고객, 운영자, 경영자로 나눌 수 있다. 각각은 원하는 정보가 다르다.
먼저 고객은 도서이름, 가격 본인의 주문 내역 등을 확인하고 싶어한다. 운영자는 고객의 주문 현황, 주문 총액, 도서별 판매량, 남아 있는 도서의 수 등을 알기 원한다.
경영자는 운영자가 원하는 정보보다 좀 더 요약된 고부가가치의 정보인 월별 매출 동향, 도서별 판매 동향 등을 보고 싶어 한다.
고객, 운영자, 경영자 관점에서 원하는 정보를 구분해보면 다음 그림과 같다. 여기에는 물론 공통적으로 원하는 정보도 있으며, 실제 서점이라면 더 많은 요구사항이 있을 것이다.

#### [사용자 그룹별로 원하는 정보]
<img src="https://github.com/silxbro/cs-study/assets/142463332/5d8af925-2e30-4dfc-8f42-d8b401949e43" width="550" height="550"/><br/>

프로그래머는 사용자 그룹별로 원하는 정보가 무엇인지 파악해야 한다. 또 DBMS에 익숙해야 하며 일반 프로그래밍 언어와 SQL을 다룰 줄 알아야 한다.
반면 고객, 운영자, 경영자는 SQL 언어를 잘 알지 못해도 된다.
<br/>
<br/>
## 3. MySQL 샘플 데이터 설치
MySQL은 오픈소스(open source) 관계형 DBMS이다.
원래 1995년에 설립된 스웨덴의 MySQL AB사에 의하여 운영되었으나, 2008년 썬 마이크로시스템즈(Sun Microsystems), 2010년 오라클사(Oracle Corporation)로 변경되었다.
MySQL은 C와 C++로 작성되었으며 GNU GPL(General Public License)로 무료로 사용할 수 있다.
참고로 MySQL을 모체로하여 MariaDB, Percona 등의 별도의 DBMS로 분기되었으며 MySQL과 동일하게 사용할 수 있다.
2019년 MySQL Server 8.0 버전이 사용되고 있으며 기존 관계형 데이터베이스 기술에 NoSQL 등의 기술이 추가되었다.
현재 가장 많이 사용되는 MySQL 버전은 5.6 버전으로 InnoDB 엔진을 사용하며, ANSI SQL99 표준의 일부를 따르고, Stored Procedure, Trigger, 트랜잭션 등의 기능을 지원한다.

MySQL은 오픈소스이기 때문에 오픈소스 웹 응용 스택인 LAMP(Linux, Apache, MySQL, PHP)의 중요한 요소이다.
MySQL은 대용량 웹 사이트에서 많이 사용되고 있으며 구글(Google), 페이스북(Facebook), 트위터(Twitter), 유튜브(YouTube) 등에서도 사용되고 있다.

MySQL은 홈페이지(www.mysql.com) 에서 다운받을 수 있으며, 무료버전(GPL)은 'MySQL Community Edition' 버전을 선택한 후, MySQL Community 버전 중
'MySQL Community Server' 버전을 다운받으면 된다.

#### [버전별로 사용 가능한 MySQL 버전]
|MySQL 버전 / 운영체제|윈도우(32비트, 64비트)|LINUX|
|:---|:---|:---|
|MySQL Community Server<br/>(8.0)|○|○|
|MySQL Community Server<br/>(5.7) - (이전 버전)|○|○|
|MySQL Enterprise Edition<br/>(8.0, 상용)|고급 기능|고급 기능|

실습을 위해 MySQL 8.0 버전 이상을 사용한다. 보통은 8.x 버전이라고 부른다.

### 🔳 MySQL 설치
MySQL DBMS를 내려받아 설치한다. 설치 과정은 다음과 같다(윈도우 기준).
1. www.mysql.com 접속 - [DOWNLOADS] 클릭
2. MySQL Community Edition(GPL) - [Community(GPL) Downloads] 클릭
3. MySQL Community Server - [DOWNLOAD] 클릭
4. [MySQL Installer for Windows] 클릭
5. Windows(x86, 32-bit), MSI Installer - [DOWNLOAD] 클릭
    - mysql-installer-web-community-8.x.msi 버전
    - Login 하지 않고, "No thanks, just start my download" 클릭
6. 완료되면, 열기 → 설치 시작 → 설치 중 [Execute] 혹은 [Next] 버튼으로 진행

정상적으로 설치가 끝나면 C:\Program Files\MySQL 폴더에 데이터베이스 프로그램이 설치된다. 설치 중 시스템 관리자 계정은 root, 비밀번호는 root로 정하기로 한다.
설치가 완료되면 [시작]-[모든 프로그램]-[MySQL]-[MySQL Workbench]를 클릭하여 GUI 데이터베이스 관리 화면을 작동시킨다.
혹은 [시작]-[모든 프로그램]-[MySQL]-[MySQL Server]-[MySQL Command Line Client]로 명령어 모드를 실행할 수 있다.

#### [MySQL 설치 소요시간]
MySQL DBMS는 설치 파일의 용량이 300MB 이상 되기 때문에 다운로드에서 설치까지 시간이 많이 소요된다. 설치 시간을 충분히 갖는 것이 좋다.

### 🔳 실습 데이터베이스 설치
MySQL DBMS에서 제공하는 관리자 계정은 root 계정이며(비번은 사용자가 입력해서 만들지만 보통은 아이디와 동일하게 'root'로 하는 것이 나중에 비밀번호 분실에 대비하기 좋다).
초기에는 root 계정에 접속하여 사용자 계정을 만들고 필요에 따라 스키마도 생성해야 한다.

root 계정으로 madang 데이터베이스와 madang 사용자 계정을 생성하고 예제 SQL 실습을 진행한다. (madang 데이터베이스와 madang 사용자 계정 생성, 실습 데이터 설치 : 부록 참고)

마당서점 실습 데이터베이스 스크립트 파일의 내용은 다음과 같다.

```SQL
/* 이름: demo_madang.sql */
/* 설명 */
 
/* root 계정으로 접속, madang 데이터베이스 생성, madang 계정 생성 */
/* MySQL Workbench에서 초기화면에서 +를 눌러 root connection을 만들어 접속한다. */
DROP DATABASE IF EXISTS  madang;
DROP USER IF EXISTS  madang@localhost;
create user madang@localhost identified WITH mysql_native_password  by 'madang';
create database madang;
grant all privileges on madang.* to madang@localhost with grant option;
commit;

/* madang DB 자료 생성 */
/* 이후 실습은 MySQL Workbench에서 초기화면에서 +를 눌러 madang connection을 만들어 접속하여 사용한다. */
 
USE madang;

CREATE TABLE Book (
  bookid      INTEGER PRIMARY KEY,
  bookname    VARCHAR(40),
  publisher   VARCHAR(40),
  price       INTEGER 
);

CREATE TABLE  Customer (
  custid      INTEGER PRIMARY KEY,  
  name        VARCHAR(40),
  address     VARCHAR(50),
  phone       VARCHAR(20)
);

CREATE TABLE Orders (
  orderid INTEGER PRIMARY KEY,
  custid  INTEGER ,
  bookid  INTEGER ,
  saleprice INTEGER ,
  orderdate DATE,
  FOREIGN KEY (custid) REFERENCES Customer(custid),
  FOREIGN KEY (bookid) REFERENCES Book(bookid)
);

INSERT INTO Book VALUES(1, '축구의 역사', '굿스포츠', 7000);
INSERT INTO Book VALUES(2, '축구아는 여자', '나무수', 13000);
INSERT INTO Book VALUES(3, '축구의 이해', '대한미디어', 22000);
INSERT INTO Book VALUES(4, '골프 바이블', '대한미디어', 35000);
INSERT INTO Book VALUES(5, '피겨 교본', '굿스포츠', 8000);
INSERT INTO Book VALUES(6, '역도 단계별기술', '굿스포츠', 6000);
INSERT INTO Book VALUES(7, '야구의 추억', '이상미디어', 20000);
INSERT INTO Book VALUES(8, '야구를 부탁해', '이상미디어', 13000);
INSERT INTO Book VALUES(9, '올림픽 이야기', '삼성당', 7500);
INSERT INTO Book VALUES(10, 'Olympic Champions', 'Pearson', 13000);

INSERT INTO Customer VALUES (1, '박지성', '영국 맨체스타', '000-5000-0001');
INSERT INTO Customer VALUES (2, '김연아', '대한민국 서울', '000-6000-0001');  
INSERT INTO Customer VALUES (3, '장미란', '대한민국 강원도', '000-7000-0001');
INSERT INTO Customer VALUES (4, '추신수', '미국 클리블랜드', '000-8000-0001');
INSERT INTO Customer VALUES (5, '박세리', '대한민국 대전',  NULL);

INSERT INTO Orders VALUES (1, 1, 1, 6000, STR_TO_DATE('2014-07-01','%Y-%m-%d')); 
INSERT INTO Orders VALUES (2, 1, 3, 21000, STR_TO_DATE('2014-07-03','%Y-%m-%d'));
INSERT INTO Orders VALUES (3, 2, 5, 8000, STR_TO_DATE('2014-07-03','%Y-%m-%d')); 
INSERT INTO Orders VALUES (4, 3, 6, 6000, STR_TO_DATE('2014-07-04','%Y-%m-%d')); 
INSERT INTO Orders VALUES (5, 4, 7, 20000, STR_TO_DATE('2014-07-05','%Y-%m-%d'));
INSERT INTO Orders VALUES (6, 1, 2, 12000, STR_TO_DATE('2014-07-07','%Y-%m-%d'));
INSERT INTO Orders VALUES (7, 4, 8, 13000, STR_TO_DATE( '2014-07-07','%Y-%m-%d'));
INSERT INTO Orders VALUES (8, 3, 10, 12000, STR_TO_DATE('2014-07-08','%Y-%m-%d')); 
INSERT INTO Orders VALUES (9, 2, 10, 7000, STR_TO_DATE('2014-07-09','%Y-%m-%d')); 
INSERT INTO Orders VALUES (10, 3, 8, 13000, STR_TO_DATE('2014-07-10','%Y-%m-%d'));

-- 여기는 3장에서 사용되는 Imported_book 테이블
CREATE TABLE Imported_Book (
  bookid      INTEGER,
  bookname    VARCHAR(40),
  publisher   VARCHAR(40),
  price       INTEGER 
);

INSERT INTO Imported_Book VALUES(21, 'Zen Golf', 'Pearson', 12000);
INSERT INTO Imported_Book VALUES(22, 'Soccer Skills', 'Human Kinetics', 15000);
commit;
```

### 🔳 MySQL Workbench, MySQL Command Line 사용
MySQL은 GUI 방식인 Workbench와 명령어 방식인 Command Line 방식으로 접속할 수 있다. MySQL DBMS를 설치하면 두 가지 모두 설치가 된다.
초보자는 GUI 방식인 Workbehch 사용을 권장한다.

#### MySQL Workbench
MySQL 설치가 완료되면 [시작]-[모든 프로그램]-[MySQL]-[MySQL Workbench]로 Workbench를 실행한다. Workbench를 통해 데이터베이스를 **GUI 방식**으로 관리할 수 있다.
Workbench 초기 화면(Home Screen)은 다음 그림과 같다.

#### [Workbench 초기 화면 (Home Screen)]
<img src="https://github.com/silxbro/cs-study/assets/142463332/f41ef883-415d-4076-9024-b360d5e211d8" width="500" height="300"/><br/>

초기 화면에는 접속(Connection) 생성 아이콘과 root 계정용 접속 아이콘이 만들어져 있다.
root 계정 접속 아이콘을 누르면 비밀번호를 묻는 화면이 나오고, 여기서 비밀번호 'root'를 누르면 root 계정의 SQL 개발 화면으로 이동한다.

#### [Workbench SQL 개발 화면]
<img src="https://github.com/silxbro/cs-study/assets/142463332/b84543c2-089f-4034-a69e-1f273805a800" width="550" height="380"/><br/>

SQL 개발(SQL Development) 화면에서는 데이터베이스 객체 탐색과 SQL 명령어를 수행할 수 있다.
root 계정에서는 SQL 문 혹은 아이콘을 이용하여 데이터베이스 생성, 사용자 생성 등을 수행할 수 있다.
root 계정에서 demo_madang.sql을 불러온 후 스크립트를 실행하여 madang 데이터베이스와 madang 사용자를 생성한다.

#### [Workbench 기본 기능]
<img src="https://github.com/silxbro/cs-study/assets/142463332/7f277297-88a6-4231-b603-364ec6c4ea2a" width="300" height="150"/><br/>

madang 데이터베이스가 생성되면, Workbench 초기 화면에서 **⊕** 아이콘을 이용하여 접속 이름(Connection Name), 사용자 이름(Username), 비밀번호(Password) 등을 입력하여
madang 사용자용 접속(Connection)을 만든다.

#### Workbench에서 사용자 madang 접속 생성
<img src="https://github.com/silxbro/cs-study/assets/142463332/89b48b97-50a3-4517-9b02-08d161785378" width="650" height="300"/><br/>

참고로 SQL 개발 화면에서 직접 root 계정으로 madang 데이터베이스, 사용자 생성 및 권한을 부여하는 방법은 앞에서 보인 demo_madang.sql 중 아래 스크립트를 실행하면 된다.
```SQL
create database madang;
creat user madang@localhost identified by 'madang';
grant all privileges on madang.* to madang@localhost;
commit;
```
- SQL은 C, 자바와 달리 명령을 위한 예약어에 대소문자를 구분하지 않는다.
  단, 'Kim', 'kim'과 같이 데이터베이스에 저장된 내용(' ' 사이의 단어)을 검색할 때에는 대소문자를 구분한다.

#### MySQL 명령창 (MySQL Command Line Client)
SQL 명령창은 윈도우의 [시작]-[MySQL]-[MySQL Command Line Client - Unicode]를 실행하여 명령어 모드로 실행할 수 있다. 처음 실행하면 root 계정 비밀번호를 묻는다.
비밀번호 'root'를 입력하면 다음 그림과 같은 MySQL Command Line Client가 실행된다.

#### [Command Line Client에서 MySQL 실행]
<img src="https://github.com/silxbro/cs-study/assets/142463332/aeab4da2-9321-45de-9642-1ab6d7021e85" width="600" height="270"/><br/>

MySQL 명령창에서는 root 계정으로 login하고, root 권한으로 명령어들을 사용할 수 있다.
예를 들면 다음과 같다. 초기에 demo_madang.sql 스크립트를 MySQL 명령창에서 실행하면 madang 사용자와 madang 데이터베이스가 생성된다.

다음은 MySQL 명령창에서 'testdb'라는 데이터베이스를 생성하는 간략한 예제이다.
```cmd
mysql> show database;
mysql> create database testdb;
mysql> use testdb;
mysql> CREATE TABLE test
       (id smallint unsigned not null auto_increment,
        name varchar(20) not null);
mysql> show tables;
mysql> INSERT INTO test(id, name) VALUES (1, 'Sample data');
mysql> SELECT * FROM test;
```

#### [MySQL의 기본 명령어]
|기능|명령어|사용 예|
|:---|:---|:---|
|MySQL 접속<br/>(윈도우 cmd 창에서 사용 가능)|mysql -u [username] -p;|- mysql -u root -p;<br/>- 사용자 username에 접속한다.|
|데이터베이스 선택|user [database];|- database를 선택한다.|
|데이터베이스 보기|show databases;|- database에 어떤 것들이 있는지 보여준다.|
|데이터베이스 생성|create database<br/>[database];|- database를 생성한다.|
|테이블 보기|show tabes;|- database에 있는 테이블을 보여준다.|
|종료|exit;|- 명령창을 종료한다.|

#### 윈도우 명령창 (Windows Command Line Client, cmd)
윈도우 명령창으로 실행하는 방식은 윈도우 cmd 명령어 창을 열어서 사용하는 방식이다.
윈도우의 명령창(command, cmd)은 윈도우 검색창에서 'cmd' 입력 → cd 명령어로 MySQL이 설치된 C:\Program Files\MySQL\MySQL Server 8.0\bin 폴더로 이동 →
'mysql -u root -p' 명령어를 치면 mysql> 프롬프트가 나타나며, 앞에 설명한 MySQL Command Line Client의 명령어를 똑같이 사용할 수 있다.
윈도우 환경설정 PATH 변수에 C:\Program Files\MySQL\MySQL Server 8.0\bin을 추가하면 더 쉽게 사용할 수 있다.

#### [명령창(cmd)에서 MySQL 실행]
<img src="https://github.com/silxbro/cs-study/assets/142463332/82dce2dc-62a7-410d-b173-7de0bc21f4f3" width="600" height="250"/><br/>

#### [리눅스에서 MySQL 실행하기]
리눅스(Linux) 시스템에서도 MySQL을 설치하고 사용할 수 있다.
실무에서는 LAMP라고 부르는 Linux 운영체제, Apache 웹서버, MySQL 데이터베이스, PHP 프로그래밍 조합을 많이 사용한다. 이 소프트웨어의 공통점은 프리웨어(Freeware)라는 것이다.
Linux에서 MySQL을 사용하면 초보자들에게는 Linux를 따로 배워야 하는 어려움이 있기 때문에 MySQL만 집중적으로 배울 때는 윈도우 운영체제에서 배우는 것이 좋다.
아래 화면은 Linux에서 MySQL Workbench를 사용하는 모습으로 윈도우에서와 사용법이 같다.

<img src="https://github.com/silxbro/cs-study/assets/142463332/5e656bec-7235-45af-b9ef-1efb2ebdda80" width="650" height="450"/><br/>