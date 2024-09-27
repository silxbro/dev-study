## [1.1] SQL을 직접 다룰 때 발생하는 문제점

관계형 데이터베이스는 가장 대중적이고 신뢰할 만한 안전한 데이터 저장소다. 그래서 자바로 개발하는 애플리케이션은 대부분 관계형 데이터베이스를 데이터 저장소로 사용한다.

데이터베이스에 데이터를 관리하려면 SQL을 사용해야 한다.
자바로 작성한 애플리케이션은 JDBC API를 사용해서 SQL을 데이터베이스에 전달하는데, 자바 서버 개발자들에게 이것은 너무나 당연한 이야기이고 대부분 능숙하게 SQL을 다룰 줄 안다.

#### [JDBC API와 SQL]
<img src="https://github.com/user-attachments/assets/14905772-2e6b-4aae-9150-19bd6c5919bc" width="330"/><br/>

<br/>

### [1.1.1] 반복, 반복 그리고 반복
SQL을 직접 다룰 때의 문제점을 알아보기 위해 자바와 관계형 데이터베이스를 사용해서 회원 관리 기능을 개발해보자.
회원 테이블은 이미 만들어져 있다고 가정하고 회원을 CRUD(등록, 수정, 삭제, 조회)하는 기능을 개발해보자. 먼저 자바에서 사용할 회원(Member) 객체를 만들자.

#### [회원 객체]
```java
public class Member {

  private String memberId;
  private String name;
  ...
}
```
다음으로 회원 객체를 데이터베이스에 관리할 목적으로 회원용 DAO(데이터 접근 객체)를 만들자.

#### [회원용 DAO]
```java
public class MemberDAO {

  public Member find(String memberId) {...}
}
```
이제 MemberDAO의 find() 메소드를 완성해서 회우너을 조회하는 기능을 개발해보자. 보통 다음 순서로 개발을 진행할 것이다.

- [1] 회원 조회용 SQL을 작성한다.

  ```sql
  SELECT MEMBER_ID, NAME FROM MEMBER M WHERE MEMBER_ID = ?
  ```

- [2] JDBC API를 사용해서 SQL을 실행한다.

  ```java
  ResultSet rs = stmt.executeQuery(sql);
  ```

- [3] 조회 결과를 Member 객체로 매핑한다.

  ```java
  String memberId = rs.getString("MEMBER_ID");
  String name = rs.getString("NAME")

  Member member = new Member();
  member.setMemberId(memberId);
  member.setName(name);
  ...
  ```

회원 조회 기능을 완성했다. 다음으로 회원 등록 기능을 만들어보자.

#### [회원 등록 기능 추가]
```java
public class MemberDAO {

  public Member find(String memberId) {...}
  public void save(Member member) {...}  // 추가
}
```
- [1] 회원 등록용 SQL을 작성한다.

  ```java
  String sql = "INSERT INTO MEMBER(MEMBER_ID, NAME) VALUES (?, ?)";
  ```

- [2] 회원 객체의 값을 꺼내서 등록 SQL에 전달한다.

  ```java
  pstmt.setString(1, member.getMemberId());
  pstmt.setString(2, member.getName());
  ```

- [3] JDBC API를 사용해서 SQL을 실행한다.

  ```java
  pstmt.executeUpdate(sql);
  ```

회원을 조회하는 기능과 등록하는 기능을 만들었다. 다음으로 회원을 수정하고 삭제하는 기능도 추가해보자. 아마도 SQL을 작성하고 JDBC API를 사용하는 비슷한 일을 반복해야 할 것이다.

회원 객체를 데이터베이스가 아닌 자바 컬렉션에 보관한다면 어떨까? 컬렉션은 다음 한 줄로 객체를 저장할 수 있다.
```java
list.add(member);
```
하지만 데이터베이스는 객체 구조와는 다른 데이터 중심의 구조를 가지므로 객체를 데이터베이스에 직접 저장하거나 조회할 수는 없다.
따라서 개발자가 객체지향 애플리케이션과 데이터베이스 중간에서 SQL과 JDBC API를 사용해서 변환 작업을 직접 해주어야 한다.

문제는 객체를 데이터베이스에 CRUD하려면 너무 많은 SQL과 JDBC API를 코드로 작성해야 한다는 점이다.
그리고 테이블마다 이런 비슷한 일을 반복해야 하는데, 개발하려는 애플리케이션에서 사용하는 데이터베이스 테이블이 100개라면 무수히 많은 SQL을 작성해야 하고 이런 비슷한 이을 100번은 더 반복해야 한다.

데이터 접근 계층(DAO)을 개발하는 일은 이렇듯 지루함과 반복의 연속이다.

<br/>

### [1.1.2] SQL에 의존적인 개발
앞에서 만든 회원 객체를 관리하는 MemberDAO를 완성하고 애플리케이션의 나머지 기능도 개발을 완료했다. 그런데 갑자기 회원의 연락처도 함께 저장해달라는 요구사항이 추가되었다.

#### ▼ 등록 코드 변경
회원의 연락처를 추가하려고 아래와 같이 회원 테이블에 TEL 컬럼을 추가하고 회원 객체에 tel 필드를 추가했다.

#### [회원 클래스에 연락처 필드 추가]
```java
public class Member {

  private String memberId;
  private String name;
  private String tel;  // 추가
  ...
}
```
연락처를 저장할 수 있도록 INSERT SQL을 수정했다.

```java
String sql = "INSERT INTO MEMBE (MEMBER_ID, NAME, TEL) VALUES (?, ?, ?)";
```

그 다음 회원 객체의 연락처 값을 꺼내서 등록 SQL에 전달했다.

```java
pstmt.setString(3, member.getTel());
```

연락처를 데이터베이스에 저장하려고 SQL과 JDBC API를 수정했다. 그리고 연락처가 잘 저장되는지 테스트하고 데이터베이스에 연락처 데이터가 저장된 것도 확인했다.

#### ▼ 조회 코드 변경
다음으로 회원 조회 화면을 수정해서 연락처 필드가 출력되도록 했다. 그런데 모든 연락처의 값이 null로 출력된다. 생각해보니 조회 SQL에 연락처 컬럼을 추가하지 않았다.

다음처럼 회원 조회용 SQL을 수정한다.
```java
SELECT MEMBER_ID, NAME, TEL FROM MEMBER WHERE MEMBER_ID = ?
```
또한 연락처의 조회 결과를 Member 객체에 추가로 매핑한다.
```
String tel = rs.getString("TEL");
member.setTel(tel);  // 추가
...
```
이제 화면에 연락처 값이 출력된다.

#### ▼ 수정 코드 변경
기능이 잘 동작한다고 생각했는데 이번에는 연락처가 수정되지 않는 버그가 발견되었다. 자바 코드를 보니 MemberDAO.update(member) 메소드에 수정할 회원 정보와 연락처를 잘 전달했다.
MemberDAO를 열어서 UPDATE SQL을 확인해 보니 TEL 컬럼을 추가하지 않아서 연락처가 수정되지 않는 문제였다. UPDATE SQL과 MemberDAO.update()의 일부 코드를 변경해서 연락처가 수정되도록 했다.

만약 회원 객체를 데이터베이스가 아닌 자바 컬렉션에 보관했다면 필드를 추가한다고 해서 이렇게 많은 코드를 수정할 필요는 없을 것이다.
```java
list.add(member);  // 등록
Member member = list.get(xxx);  // 조회
member.setTel("xxx");  // 수정
```

#### ▼ 연관된 객체
회원은 어떤 한 팀에 필수로 소속되어야 한다는 요구사항이 추가되었다. 팀 모듈을 전담하는 개발자가 팀을 관리하는 코드를 개발하고 커밋했다. 코드를 받아보니 Member 객체에 team 필드가 추가되어 있다.

아래와 같이 회원 정보를 화면에 출력할 때 연관된 팀 이름도 함께 출력하는 기능을 추가해보자.

#### [회원 클래스에 연관된 팀 추가]
```java
class Member {

  private String memberId;
  private String name;
  private String tel;
  private Team team;  //추가
  ...
}

// 추가된 팀
class Team {
  ...
  private String teamName;
  ...
}
```

다음 코드를 추가해서 화면에 팀의 이름을 출력했다.

```
이름 : member.getName();
소속 팀 : member.getTeam().getTeamName();  // 추가
```

코드를 실행해보니 member.getTeam()의 값이 항상 null이다. 회원과 연관된 팀이 없어서 그럴 것으로 생각하고 데이터베이스를 확인해보니 모든 회원이 팀에 소속되어 있다.
문제를 찾다가 아래와 같이 MemberDAO에 findWithTeam()이라는 새로운 메소드가 추가된 것을 확인했다.

#### [MemberDAO에 추가된 findWithTeam()]
```java
public class MemberDAO {

  public Member find(String memberId) {...}
  public Member findWithTeam(String memberId) {...}
}
```
MemberDAO 코드를 열어서 확인해보니 회원을 출력할 때 사용하는 find() 메소드는 회원만 조회하는 다음 SQL을 그대로 유지했다.
```
SELECT MEMBER_ID, NAME, TEL FROM MEMBER M
```
또한 새로운 findWithTeam() 메소드는 다음 SQL로 회원과 연관된 팀을 함께 조회했다.
```
SELECT M.MEMBER_ID, M.NAME, M.TEL, T.TEAM_ID, T.TEAM_NAME
FROM MEMBER M
JOIN TEAM T
    ON M.TEAM_ID = T.TEAM_ID
```
결국 DAO를 열어서 SQL을 확인하고 나서야 원인을 알 수 있었고, 회원 조회 코드를 MemberDAO.find()에서 MemberDAO.findWithTeam() 으로 변경해서 문제를 해결했다.

SQL과 문제점을 정리해보자. Member 객체가 연관된 Team 객체를 사용할 수 있을지 없을지는 전적으로 사용하는 SQL에 달려 있다.
이런 방식의 가장 큰 문제는 데이터 접근 계층을 사용해서 SQL을 숨겨도 어쩔 수 없이 DAO를 열어서 어떤 SQL이 실행되는지 확인해야 한다는 점이다.

Member나 Team처럼 비즈니스 요구사항을 모델링한 객체를 엔티티라 하는데, 지금처럼 SQL에 모든 것을 의존하는 상황에서는 개발자들이 엔티티를 신뢰하고 사용할 수 없다.
대신에 DAO를 열어서 어떤 SQL이 실행되고 어떤 객체들이 함께 조회되는지 일일이 확인해야 한다. 이것은 진정한 의미의 계층 분할이 아니다.
물리적으로 SQL과 JDBC API를 데이터 접근 계층에 숨기는 데 성공했을지는 몰라도 논리적으로는 엔티티와 아주 강한 의존관계를 가지고 있다.
이런 강한 의존관계 때문에 회원을 조회할 때는 물론이고 회원 객체에 필드를 하나 추가할 떄도 DAO의 CRUD 코드와 SQL 대부분을 변경해야 하는 문제가 발생한다.

애플리케이션에서 SQL을 직접 다룰 때 발생하는 문제점을 요약하면 다음과 같다.
- 진정한 의미의 계층 분할이 어렵다.
- 엔티티를 신뢰할 수 없다.
- SQL에 의존적인 개발을 피하기 어렵다.

<br/>

### [1.1.3] JPA와 문제 해결
JPA는 이런 문제들을 어떻게 해결할까? JPA가 문제를 어떻게 해결하는지 간단히 알아보자.

JPA를 사용하면 객체를 데이터베이스에 저장하고 관리할 때, 개발자가 직접 SQL을 작성하는 것이 아니라 JPA가 제공하는 API를 사용하면 된다.
그러면 JPA가 개발자 대신에 적절한 SQL을 생성해서 데이터베이스에 전달한다.

JPA가 제공하는 CRUD API를 간단히 알아보자.

#### ▼ 저장 기능
```java
jpa.persist(member);  // 저장
```
persist() 메소드는 객체를 데이터베이스에 저장한다. 이 메소드를 호출하면 JPA가 객체와 매핑 정보를 보고 적절한 INSERT SQL을 생성해서 데이터베이스에 전달한다.
매핑정보는 어떤 객체를 어떤 테이블에 관리할지 정의한 정보이다.

#### ▼ 조회 기능
```java
String memberId = "helloId";
Member member = jpa.find(Member.class, memberId);  // 조회
```
find() 메소드는 객체 하나를 데이터베이스에서 조회한다. JPA는 객체와 매핑정보를 보고 적절한 SELECT SQL을 생성해서 데이터베이스에 전달하고 그 결과로 Member 객체를 생성해서 반환한다.

#### ▼ 수정 기능
```java
Member member = jpa.find(Member.class, memberId);
member.setName("이름변경");  // 수정
```
JPA는 별도의 수정 메소드를 제공하지 않는다. 대신에 객체를 조회해서 값을 변경만 하면 트랜잭션을 커밋할 때 데이터베이스에 적절한 UPDATE SQL이 전달된다.

#### ▼ 연관된 객체 조회
```java
Member member = jpa.find(Member.class, memberId);
Team team = member.getTeam();  // 연관된 객체 조회
```

JPA는 연관된 객체를 사용하는 시점에 적절한 SELECT SQL을 실행한다. 따라서 JPA를 사용하면 연관된 객체를 마음껏 조회할 수 있다.

지금까지 JPA의 CRUD API를 간단히 알아보았다. 수정 기능과 연관된 객체 조회에서 설명한 것처럼 JPA는 SQL을 개발자 대신 작성해서 실행해주는 것 이상의 기능들을 제공한다.