## [2.5] persistence.xml 설정

JPA는 persistence.xml을 사용해서 필요한 설정 정보를 관리한다. 이 설정 파일이 META-INF/persistence.xml 클래스 패스 경로에 있으면 별도의 설정 없이 JPA가 인식할 수 있다.

#### [JPA 환경설정 파일 persistence.xml]
```xml
<?xml version="1.0" encoding="UTF-8"?>
<persistence xmlns="http://xmlns.jcp.org/xml/ns/persistence"
        version="2.1">
  <persistence-unit name="jpabook" >
    <properties>

      <!-- 필수 속성 -->
      <property name="javax.persistence.jdbc.driver"
              value="org.h2.Driver"/>
      <property name="javax.persistence.jdbc.user" value="sa"/>
      <property name="javax.persistence.jdbc.password" value=""/>
      <property name="javax.persistence.jdbc.url"
              value="jdbc:h2:tcp://localhost/~/test"/>
      <property name="hibernate.dialect"
              value="org.hibernate.dialect.H2Dialect" />

      <!-- 옵션 -->
      <property name="hibernate.show_sql" value="true" />
      <property name="hibernate.format_sql" value="true" />
      <property name="hibernate.use_sql_comments" value="true" />
      <property name="hibernate.id.new_generator_mappings" value="true" />

    </properties>
  </persistence-unit>
</persistence>
```
위 persistence.xml 내용을 차근차근 분석해보자.
```
<persistence xmlns="http://xmlns.jcp.org/xml/ns/persistence"
versiont="2.1">
```
설정 파일은 persistence로 시작한다. 이곳에 XML 네임스페이스와 사용할 버전을 지정한다. JPA 2.1을 사용하려면 이 xmlns와 version을 명시하면 된다.
```
<persistence-unit name="jpabook" >
```
JPA 설정은 영속성 유닛(persistence-unit)이라는 것부터 시작하는데 일반적으로 연결할 데이터베이스당 하나의 영속성 유닛을 등록한다.
그리고 영속성 유닛에는 고유한 이름을 부여해야 하는데 여기서는 jpabook이라는 이름을 사용했다.

다음으로 설정한 각각의 속성 값을 분석해보자.
```
<properties>
    <property name="javax.persistence.jdbc.driver"
      value="org.h2.Driver"/>
    ...
```
사용한 속성은 다음과 같다.

- JPA 표준 속성
  - javax.persistence.jdbc.driver : JDBC 드라이버
  - javax.persistence.jdbc.user : 데이터베이스 접속 아이디
  - javax.persistence.jdbc.password : 데이터베이스 접속 비밀번호
  - javax.persistence.jdbc.url : 데이터베이스 접속 URL

- 하이버네이트 속성
  - hibernate.dialect : 데이터베이스 방언(Dialect) 설정

이름이 `javax.persistence`로 시작하는 속성은 **JPA 표준 속성**으로 특정 구현체에 종속되지 않는다.
반면에 hibernate로 시작하는 속성은 하이버네이트 전용 속성이므로 하이버네이트에서만 사용할 수 있다.

사용한 속성을 보면 데이터베이스에 연결하기 위한 설정이 대부분이다. 여기서 가장 중요한 속성은 **데이터베이스 방언을 설정**하는 `hibernate.dialect` 다.

<br/>

### [2.5.1] 데이터베이스 방언
JPA는 특정 데이터베이스에 종속적이지 않은 기술이다. 따라서 다른 데이터베이스로 손쉽게 교체할 수 있다. 그런데 각 데이터베이스가 제공하는 SQL 문법과 함수가 조금씩 다르다는 문제점이 있다.
예를 들어 데이터베이스마다 다음과 같은 차이점이 있다.
- **데이터 타입** : 가변 문제 타입으로 MySQL은 VARCHAR, 오라클은 VARCHAR2를 사용한다.
- **다른 함수명** : 문자열을 자르는 함수로 SQL 표준은 SUBSTRING()를 사용하지만 오라클은 SUBSTR()을 사용한다.
- **페이징 처리** : MySQL은 LIMIT을 사용하지만 오라클은 ROWNUM을 사용한다.

이처럼 SQL 표준을 지키지 않거나 특정 데이터베이스만의 고유한 기능을 JPA에서는 방언(Dialect)이라 한다.
애플리케이션 개발자가 특정 데이터베이스에 종속되는 기능을 많이 사용하면 나중에 데이터베이스를 교체하기가 어렵다.
하이버네이트를 포함한 대부분의 JPA 구현체들은 이런 문제를 해결하려고 다양한 데이터베이스 방언 클래스를 제공한다.

개발자는 JPA가 제공하는 표준 문법에 맞추어 JPA를 사용하면 되고, 특정 데이터베이스에 의존적인 SQL은 데이터베이스 방언이 처리해준다.
따라서 데이터베이스가 변경되어도 애플리케이션 코드를 변경할 필요 없이 데이터베이스 방언만 교체하면 된다. 참고로 데이터베이스 방언을 설정하는 방법은 JPA에 표준화되어 있지 않다.

#### [방언]
<img src="https://github.com/user-attachments/assets/994a2cf8-20f9-422a-83cb-6b7ababf733c" width="450"/><br/>

하이버네이트는 다양한 데이터베이스 방언을 제공한다. 대표적인 것만 살펴보자.
- H2 : org.hibernate.dialect.H2Dialect
- 오라클 10g : org.hibernate.dialect.Oracle10gDialect
- MySQL : org.hibernate.dialect.MySQL5InnoDBDialect

여기서는 H2 데이터베이스를 사용하므로 hibernate.dialect 속성을 org.hibernate.dialect.H2Dialect로 설정했다.

#### [참고]
> 하이버네이트는 현재 45개의 데이터베이스 방언을 지원한다. 상세 항목은 다음 URL을 참고하자.
>
> http://docs.jboss.org/hibernate/orm/4.3/manual/en-US/html_single/#configuration-optional-dialects

지금까지 persistence.xml 설정을 살펴보았다. 이제 이 정보를 바탕으로 실제 JPA를 사용해보자.

#### [참고]
> 사용된 하이버네이트 전용 속성은 다음과 같다.
>
> - hibernate.show_sql: 하이버네이트가 실행한 SQL을 출력한다.
> - hibernate.format_sql: 하이버네이트가 실행한 SQL을 출력할 때 보기 쉽게 정렬한다.
> - hibernate.use_sql_comments: 쿼리를 출력할 때 주석도 함께 출력한다.
> - hibernate.id.new_generator_mappings: JPA 표준에 맞춘 새로운 키 생성 전략을 사용한다.

#### [참고]
> JPA 구현체들은 보통 엔티티 클래스를 자동으로 인식하지만, 환경에 따라 인식하지 못할 때도 있다.
> 그때는 persistence.xml에 다음과 같이 \<class\>를 사용해서 JPA에서 사용할 엔티티 클래스를 지정하면 된다.
> 참고로 스프링 프레임워크나 J2EE 환경에서는 엔티티를 탐색하는 기능을 제공하므로 이런 문제가 발생하지 않는다.
>
>```
> <persistence-unit name="jpabook">
>    <class>jpabook.start.Member</class>
>    <properties>
>    ...
>```