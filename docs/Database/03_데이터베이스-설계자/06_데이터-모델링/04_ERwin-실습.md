# ERwin 실습
<br/>

데이터 모델링을 하기 위한 프로그램으로는 ERwin과 ER STUDIO, DA# 등이 있는데, ERwin이 가장 많이 사용된다. ERwin은 미국 CA 테크놀로지사에서 개발했으며 IE 표기법을 지원한다.
여기에서는 ERwin Data Modeler를 기준으로 실습한다. ERwin의 설치는 부록 D를 참고하기 바란다.

이 절에서는 ERwin을 이용하여 아래의 ER 다이어그램을 만들어본다.
실습 시 주의할 사항은 지금까지는 개념적 모델링을 한 후 논리적 모델링을 위한 사상을 진행했지만, ERwin은 개념적 모델링을 별도로 지원하지 않고 직접 논리적 모델링을 진행한다는
점이다.

#### [마당서점의 ER 다이어그램]
<img src="https://github.com/silxbro/cs-study/assets/142463332/b3321f09-98ea-455c-ac38-188787fea70d" width="450" height="270"/><br/>

#### [Workbench의 모델링 도구]
지금까지 실습에서 사용하고 있는 Workbench 역시 모델링 도구를 내장하고 있다.<br/>
[File]-[New Model]을 선택하면 다음과 같이 ERD를 작성할 수 있다.

<img src="https://github.com/silxbro/cs-study/assets/142463332/cb5e4008-ee25-45d5-ac1a-498830341092" width="650" height="330"/><br/>

Workbench Modeling 도구의 경우 논리 모델을 지원하지 않아 논리 모델 및 물리 모델을 모두 지원하는 ERwin으로 실습을 진행한다.
테이블 설계를 위한 용도로 사용한다면 Workbench의 모델링 도구도 충분히 사용할 수 있다.
기존에 만들어진 데이터베이스 테이블을 기준으로 ERD를 작성해주는 Reverse Engineer([Database]-[Reverse Engineer]) 등 유용한 기능을 제공한다.

#### [ERwin Data Modeler 시험판 받기]
ERwin Data Modeler의 Academic License(교육용 시험판)이나 Trial 버전은 ERWIN사의 웹사이트(https://erwin.com) 에서 받을 수 있다.
Academic License는 ERWIN사의 웹사이트에 접속한 후 [Services]-[erwin Academic Program]-[erwin DM ACademic Edition]-[Apply Now]를 클릭하고 순서에 따라
본인 메일, 수업 과목명, 교수 이름 등을 입력하면 바로 다운받을 수 있다.
신청 후 수신받은 메일을 따라 라이센스 신청 메일을 보내면 1~3일 후에 1년 동안 유효한 License Key(18자리 숫자)를 메일로 보내준다.
이후 ERwin을 실행하고 License 키를 입력하면 된다.

Trial 버전은 ERWIN사의 웹사이트 메인 페이지의 Try for Free을 클릭한 후 절차에 따라 신청하면 다운로드 받을 수 있다.
신청 시 Academic License는 학교 메일을, Trial 버전은 기업, 단체 등의 이메일을 사용해야만 한다(naver, daum, gmail 등은 신청할 수 없다).
<br/>
<br/>
## 1. ERwin 기본 화면 및 툴 둘러보기
본격적으로 ER 다이어그램을 작성하기에 앞서 ERwin의 기본 화면을 살펴보자. 아래와 같이 모든 프로그램 목록에서 ERwin을 실행시킨다.

#### [ERwin Data Modeler 실행]
<img src="https://github.com/silxbro/cs-study/assets/142463332/0c319925-fe96-4180-814f-4a66232bdab1" width="300" height="400"/><br/>

ER 다이어그램 작성을 위한 기본 화면은 다음과 같다.

#### [ERwin의 기본 화면]
<img src="https://github.com/silxbro/cs-study/assets/142463332/d9a6ce89-a138-4aca-b994-457c7c27bdd2" width="650" height="350"/><br/>

#### ❶ 메뉴
ERwin의 전반적인 기능을 수행한다.
#### ❷ 툴바
ER 다이어그램을 그리기 위해 필요한 도구 및 메뉴들이 모여 있다.
#### ❸ 다이어그램 작성 영역
ER 다이어그램을 그리는 작업 영역이다.
#### ❹ 모델 탐색기
다이어그램의 속성을 확인하거나 변경한다.

다이어그램 작성 시 본 실습에서 사용하는 IE 표기법의 툴 박스는 다음과 같다. 툴 박스는 툴바에서 사용하거나 드래그하여 바깥으로 분리시킨 후 사용할 수 있다.

#### [툴 박스]
<img src="https://github.com/silxbro/cs-study/assets/142463332/8cf8cb1e-fc56-4f57-8753-1992e77acaa6" width="250" height="150"/><br/>

#### ❶ 개체
개체 타입의 이름, 식별자, 속성을 표현한다.
#### ❷ SUB 타입
ISA 모델의 슈퍼클래스와 서브클래스처럼 부모, 자식 관계에서 자식 개체가 서로 배타적인 관게를 가지는 여러 서브 개체 타입을 표현한다.
#### ❸ 1:N(식별), M:N(식별), 1:N(비식별)
1, N, M은 두 개체 간의 관계에서 관계 대응 수를 말한다.
식별 관계는 두 개체가 부모(1), 자식(N) 관계일 때 부모의 기본키가 자식의 기본키가 되거나 기본키의 구성원으로 사용되는 관계로 실선으로 나타낸다.
비식별 관계는 부모의 기본키가 자식의 기본키가 아닌 속성의 일부로 전이되는 관계로 점선으로 나타낸다. 관계의 필수(1)와 선택(0)은 관계선의 옵션을 통해 선택할 수 있다.
- 식별 관계는 약한 개체를 표현하며 비식별 관계는 강한 개체를 표현한다. 대부분의 개체는 강한 개체에 해당되기 때문에 비식별 관계를 더 많이 사용한다.
  <br/>

## 2. ERwin 실습을 위한 기본 환경 설정하기
ERwin으로 ER 다이어그램을 작성하려면 우선 목적과 대상에 맞는 모델, DBMS, 표기법을 선택해야 한다. 다음과 같이 설정한다.

#### [ERwin 실습 기본 환경 설정]
- **모델 타입** : Logical/Physical
- **DBMS** : MySQL 5.x
- **표기법** : IE 표기법

#### [Logical, Physical, Logical/Physical 타입]
ERwin은 Logical, Physical, Logical/Physical의 세 가지 타입으로 모델링 작업을 수행할 수 있다.
- **Logical**
    - 작업 대상인 현실 세계의 객체를 엔티티와 속성으로 추출하고 그들 간의 관계를 다이어그램을 표현한다.
- **Physical**
    - 실제 사용하는 DBMS의 특징에 맞게 엔티티, 속성, 관계 등을 테이블, 컬럼, 제약관계 등으로 풀어내 테이블을 생성한다.
- **Logical/Physical**
    - 하나의 모델을 Logical 부분과 Physical 부분으로 구성하여 함께 작업하는 타입이다. Logical 부분과 Physical 부분의 작업을 즉시 연계하여 진행할 수 있다.
- Logical 타입 또는 Physical 타입을 선택하면 상호 간섭 없이 독립적으로 모델링을 진행하고, Logical/Physical 타입을 선택하면 논리 모델에서부터 최종 테이블 생성까지
  하나의 모델로 진행한다. 일반적으로 Logical/Physical 타입으로 진행하는 경우가 많으며, 때에 따라 논리 모델에는 엔티티가 필요하지만 물리 모델에서는 테이블로 생성할
  필요가 없는 경우 별도의 타입을 선택하여 진행한다.
  <br/>

기본 환경 설정은 다음의 순서대로 진행한다.
#### ❶ 모델 타입, DBMS 선택하기
[FILE]-[NEW] 메뉴를 선택한다. [New Model] 창이 나타나면 Type에서 'Logical/Physical'을 선택하고 Target Server에서 'MySQL', '5.x'를 선택한 후 <OK>를 클릭한다.
#### [모델 타입, DBMS 선택]
<img src="https://github.com/silxbro/cs-study/assets/142463332/75fe510c-69bc-4dc9-bed0-04eff949f856" width="600" height="300"/><br/>

#### ❷ IE 표기법으로 변경하기
모델 타입과 DBMS가 선택되었으면 표기법을 선택해야 한다. 선택을 위해 [Model]-[Model Properties] 메뉴를 선택한다.
#### [모델 속성 메뉴 선택]
<img src="https://github.com/silxbro/cs-study/assets/142463332/fc276669-e469-4192-8590-eeaa42c03389" width="550" height="280"/><br/>

Notation을 'Information Engineering'으로 선택한 후 <Close>를 클릭한다.
#### [IE 표기법으로 변경]
<img src="https://github.com/silxbro/cs-study/assets/142463332/9c8b2c51-48fb-4034-a7ea-33c44f3a928a" width="430" height="300"/><br/>

- ERwin은 IE(Information Engineering) 표기법과 미국방성 프로젝트 표준안인 IDEF1X(Integration DEFinition for Information Modeling) 표기법을 지원한다.
  일반적으로 산업계에서는 IE 표기법을 주로 사용한다.

표기법까지 설정했다면 기본적인 모델링 준비는 완료되었다.

#### ❸ 툴바에 메뉴 추가하기(선택사항)
실습을 위해서는 툴바에 Alignment(정렬)와 Transformation(변환) 메뉴를 추가해야 한다. 툴바에 이미 해당 메뉴가 있는 경우에는 생략한다.
툴바에 메뉴를 추가하기 위해서 아래와 같이 툴바의 빈 영역에서 마우스 오른쪽 버튼을 누른 후 [Alignment]와 [Transformations]를 선택한다.

#### [툴바에 메뉴 추가]
<img src="https://github.com/silxbro/cs-study/assets/142463332/3d1fc803-485a-4057-807d-fce37e3244d1" width="650" height="300"/><br/>

#### [테마 변경]
다이어그램 작성 시 글자 크기, 엔티티 색깔, 배경 색깔 등을 변경하려면 Theme을 이용하면 된다.
[Diagram]-[Diagrams] 메뉴를 선택한 후 Diagram Editor의 [General] 탭의 Optional Theme 부분에서 원하는 테마를 선택한다. 참고로 이후의 실습은 모두 Classic Theme로 진행한다.

<img src="https://github.com/silxbro/cs-study/assets/142463332/66037731-7972-4d49-ada2-5c337323b9d1" width="670" height="360"/><br/>

테마는 [View]-[Themes] → [Theme Editor] 창에서 설정값 확인이나 변경을 진행할 수 있다.

<img src="https://github.com/silxbro/cs-study/assets/142463332/2117492a-1b90-4da2-98c6-87f367f72557" width="670" height="370"/><br/>
<br/>

## 3. 마당서점 설계 실습
ER 다이어그램 작성을 위한 기본적인 준비가 완료되었다. 마당서점의 요구사항을 확인하고 요구사항에 맞게 ER 다이어그램을 작성한 후 테이블을 만들어보자.

### 마당서점의 논리적 모델링
마당서점은 출판사에서 도서를 공급받아 판매한다. 판매 내역은 매일 기록으로 남기며, 고객 서비스와 매출 관리에 활용한다. 마당서점의 요구사항은 다음과 같다.
- 도서 목록에는 도서번호, 도서이름, 출판사이름, 도서단가를 기록한다(개체).
- 출판사 목록에는 출판사이름, 담당자이름, 전화번호를 기록한다(개체).
- 고객 목록에는 고객번호, 고객이름, 주소, 전화번호를 기록한다(개체).
- 마당서점은 출판사에서 공급한 도서만 등록하여 관리한다(출판사와 도서의 관계는 1:N).
- 고객은 여러 번에 걸쳐 여러 권의 도서를 구입할 수 있다(고객과 도서의 관계는 M:N).
- 마당서점은 고객이 도서를 구입한 날(주문일자)와 구매한 가격(주문금액)을 따로 저장한다(고객과 도서의 관계에 속성이 존재함).

#### 🔳 개체 만들기
마당서점의 요구사항을 살펴보면 개체는 총 3개, 즉 도서 개체, 출판사 개체, 고객 개체가 필요하다.
각 개체 사이의 관계를 살펴보면 먼저 출판사는 여러 권의 도서를 공급할 수 있으므로 출판사와 도서 개체는 1:N의 관계가 성립한다.
그리고 고객은 여러 권의 도서를 구입할 수 있으므로 고객과 도서는 M:N의 관계가 성립한다.
또한 고객이 도서를 주문하면 주문금액과 주문일자가 등록되는데, 이를 통해 고객과 도서 개체의 관계에 속성이 존재하는 것을 알 수 있다
(주문 관계에 관한 내용은 이후 M:N 관계에 대한 교차 테이블에서 다룬다).

ER 다이어그램을 그리기에 앞서 위의 요구사항을 통해 나타난 개체와 속성을 정리하면 다음 표와 같다.
#### [마당서점의 개체와 속성]
|개체|키 속성(PK)|속성|
|:---|:---|:---|
|출판사|출판사이름|담당자이름, 전화번호|
|도서|도서번호|도서이름, 도서단가, 출판사이름(FK)|
|고객|고객번호|고객이름, 주소, 전화번호|

출판사, 도서, 고객 개체를 만들어보자. 툴 박스에서 Entity(<img src="https://github.com/silxbro/cs-study/assets/142463332/30a35efe-f1e8-4731-94ba-c27241451a90" width="20" height="15"/>)를
선택하고 다이어그램 작성 영역의 원하는 위치에 클릭한다.
#### [개체(Entity) 선택]
<img src="https://github.com/silxbro/cs-study/assets/142463332/a230a6d5-5787-41c7-9eea-02994a213332" width="400" height="110"/><br/>

개체는 다음과 같이 세 단락으로 나누어진다. 맨 위에는 개체 이름, 가운데에는 기본키, 아래에는 기본키를 제외한 나머지 속성을 입력한다.
단락 간 이동은 [Tab] 키를, 단락 내 속성을 입력할 때는 [Enter] 키를 이용한다. 입력이 완료되면 [Esc] 키를 누르거나 작성 영역의 빈 공간을 클릭한다.
#### [개체의 구성]
<img src="https://github.com/silxbro/cs-study/assets/142463332/5d7de0a0-b7c8-4bf1-918a-a432bfd2a2ef" width="160" height="80"/><br/>

아래는 출판사 개체를 만들고 일반 속성을 입력하는 모습이다. 같은 방법으로 도서 개체와 고객 개체도 만든다.
이때 도서 개체에 '출판사이름'은 입력하지 않아도 된다('출판사이름' 속성은 출판사 개체와 도서 개체의 관계를 표현할 때 자동으로 생성된다).
#### [출판사 개체 생성]
<img src="https://github.com/silxbro/cs-study/assets/142463332/08aaec5b-e8a6-4b87-b99a-bccc9a8d4665" width="350" height="130"/><br/>

#### 🔳 관계 표현하기
출판사, 도서, 고객 개체를 만들었다면 각 개체 간의 관계를 표현해야 한다.
- **1:N 비식별 관계 표현**
    - 한 출판사는 여러 권의 도서를 서점에 공급할 수 있으므로 출판사와 도서의 관계는 1:N이다. 이는 서로 독립된 개체(강한 개체) 간의 관계이므로 비식별자 관계이다.
      정리하면 출판사와 도서 개체의 관계는 '1:N 비식별 관계'이다.

      1:N 비식별 관계를 표현하기 위해 툴 박스에서 1:N(비식별)(<img src="https://github.com/silxbro/cs-study/assets/142463332/51d0c082-9b28-44c5-9723-4c456d3edc0c" width="15" height="15"/>)을 선택한다.
      출판사 개체를 먼저 클릭하고 화살표를 도서 개체 위로 가져간 후 클릭하면 출판사에서 도서 개체로 1:N 관계가 표시된다.
      이때 처음 클릭한 개체가 부모(Parent)가 되고 나중에 클릭한 개체가 자식(Child)이 된다.
      #### [출판사, 도서 개체의 관계 설정(1:N 비식별)]
      <img src="https://github.com/silxbro/cs-study/assets/142463332/fee3d26f-a892-41f0-a0de-3a368e898877" width="400" height="240"/><br/>

- **M:N 식별 관계 표현**
    - 도서는 여러 명의 고객에게 판매될 수 있고 고객은 여러 권의 도서를 구입할 수 있다. 따라서 도서와 고객 개체는 M:N 관계이다.
      M:N 관계를 표현하려면 툴 박스에서 M:N(식별)(<img src="https://github.com/silxbro/cs-study/assets/142463332/0a6e1cce-9b01-40ef-8c96-7d45586d49d0" width="15" height="15"/>)을
      선택한 후 도서 개체를 먼저 클릭하고 화살표를 고객 개체로 가져간 후 클릭한다. 양쪽 개체 사이에 M:N 관계가 설정된다.
      이때 1:N과는 다르게 양쪽 개체 모두에 새발(crow`s foot) 모양의 N 관계수가 표시된 것을 볼 수 있다.
      #### [도서, 고객 개체의 관계 설정(M:N 식별)]
      <img src="https://github.com/silxbro/cs-study/assets/142463332/70087811-7fe6-4f4f-b090-fc63cb9ec5d5" width="400" height="240"/><br/>

#### 🔳 M:N 관계 해소하기
도서 개체와 고객 개체의 관계가 생성되었으나 M:N 관계는 실제 테이블로 변환하기 어렵다.
또한 요구사항에 나타난 주문금액과 주문일자 기록을 남기기 위해서는 또 다른 개체가 필요하다.
두 개체 사이의 M:N 관계를 2개의 1:N 관계로 해소시키려면 새로운 개체를 생성해야 한다.

M:N 관계를 해소가히 위해 아래와 같이 두 개체의 관계 실선을 클릭한 후 툴 박스의 Resolve Many to Many Relationship(<img src="https://github.com/silxbro/cs-study/assets/142463332/5fa1a129-7190-4dba-a681-924330fbc528" width="17" height="15"/>)을
클릭한다.
- [Actions]-[Transformations]-[Resolve Many to Many Relationship] 메뉴를 선택해도 된다.

#### [M:N 관계 해소]
<img src="https://github.com/silxbro/cs-study/assets/142463332/f153ac90-7132-4698-9487-da12ee85bb19" width="650" height="300"/><br/>

'도서 고객' 개체가 생성된다. 기본키는 도서 개체와 고객 개체의 기본키를 가져와 사용한다.
도서 고객 개체는 고객이 도서를 주문한 내용을 담고 있으므로 개체의 이름을 '주문'으로 변경하는 것이 좋다. 또한 주문일자와 주문금액 등의 정보를 기록해야 한다.
이를 위해 도서 고객 개체를 선택하여 개체 이름에 '주문'을 입력하고 속성에 '주문일자'와 '주문금액'을 입력한다.
#### [도서 고객 개체 생성 및 변경]
<img src="https://github.com/silxbro/cs-study/assets/142463332/04804eec-ca86-4bb1-bd71-4b93afa0547b" width="400" height="450"/><br/>

#### [M:N 관계를 해소하는 또 다른 방법]
M:N 관계를 해소하는 개체를 물리적 모델에서는 교차 테이블이라고 한다. M:N 관계에서 작업 중인 논리적 모델을 물리적 모델로 바꾸면 자동으로 교차 테이블이 생성될 것이다.

#### 🔳 식별 관계 및 관계 대응 수 변경하기
지금까지 실습으로 네 개의 개체와 세 개의 관계선이 만들어졌다. 이제 만들어진 개체 간의 관계가 요구사항에 맞는지 살펴보자.

우선 출판사와 도서 개체 간의 관계에 대한 요구사항을 보면 '출판사에서 공급한 도서만 등록하여 관리한다'이다.
그런데 생성된 관계를 살펴보면 '도서 개체의 출판사 이름은 출판사 개체에 있거나 없어도 된다'라는 의미의 '<img src="https://github.com/silxbro/cs-study/assets/142463332/828f7087-05c2-4fdf-9f94-8fc218464f83" width="17" height="17"/>'
기호로 연결되어 있다. 이는 요구사항에 위배된다. 이 상태에서는 출판사 이름이 없는 도서를 등록할 수 있으므로 요구사항에 맞게 변경해야 한다.

다음은 M:N 관계를 해소하기 위하여 만든 주문 개체를 살펴보자. 주문 개체를 생성하기 전에는 도서와 고객 개체 사이에서 한 고객이 같은 도서를 여러 번 주문할 수 있었다.
하지만 주문 개체는 도서와 고객 개체의 기본키를 자신의 기본키로 상속 받아 사용하기 때문에 동일한 고객이 동일한 도서를 주문하면 기본키의 중복이 발생하여 주문 사항을
등록할 수 없다. 이 문제는 '주문번호'라는 인조키를 생성하여 기본키로 삼고 상속받은 도서번호(FK)와 고객번호(FK)를 일반 속성으로 변경하면 해결할 수 있다.
주문번호를 달리하면 한 고객이 같은 책을 여러 번 주문할 수 있게 된다.
- 인조키는 고객번호, 도서번호와 같이 실제로 존재하지는 않지만 필요에 따라 자료를 식별하기 위해 생성하는 가상의 키이다.

앞의 두 가지를 수정하기 위해서는 관계 속성을 변경해야 한다.
관계 속성을 변경하기 위해 관계선을 선택한 후 마우스 오른쪽 버튼을 누르고 [Properties]를 선택한다(관계선을 더블 클릭해도 된다).
[Relationship Editor] 창이 나타나면 아래 표와 같이 [General] 탭의 [Type Properties]에서 각 관계를 수정한다.
주의할 점은 관계들의 Name이 기본적으로 R/숫자(R/1, R/6, R/7)로 되어 있으므로 Parent와 Child 사이의 관계를 확인하면서 관계 속성 설정을 변경해야 한다는 점이다.
숫자 값은 순차적으로 부여되므로 실습순서에 따라 다르게 나타난다. Name 역시 향후 작업을 위해 변경한다.

#### [관계 속성 설정]
|Parent|Child|Name<br/>(기존 → 수정)|Type Properties<br/>(Type/Null Option)|
|:---|:---|:---|:---|
|출판사|도서|R/1 → BookPublisher|Non-Identifying / Nulls Not Allowed|
|도서|주문|R/6 → OrdBook|Non-Identifying / Nulls Not Allowed|
|고객|주문|R/7 → OrdCustomer|Non-Identifying / Nulls Not Allowed|

Type 중 'Non-Identifying'은 비식별 관계로 점선으로 표현되고, 'Identifying'은 식별 관계로 실선으로 표현된다.
Null Options 중 'Nulls Not Allowed'는 Null 입력을 허가하지 않는다는 의미로 Parent 개체 방향의 관계선을 교차하는 '-'로 표시되고, 'Nulls Allowed'은 Null 허가 표시로
'○'로 표시된다.
#### [개체 간의 식별 관계 변경]
<img src="https://github.com/silxbro/cs-study/assets/142463332/627914ee-2fb9-4d2a-999c-a7fd46418a08" width="350" height="250"/><br/>
<img src="https://github.com/silxbro/cs-study/assets/142463332/48eda0a6-c7c0-495b-b571-59c54c78d934" width="750" height="400"/><br/>

개체 간의 관계 속성을 정리했으면 주문 개체에 기본키가 비어 있고 도서번호(FK)와 고객번호(FK)가 일반 속성으로 변경되어 있을 것이다.
비어 있는 기본키에 앞서 생성하기로 한 '주문번호'를 입력한다. 마우스로 관계선 등을 정리하면 다음과 같이 마당서점 데이터베이스의 ER 다이어그램이 완성된다.
#### [마당서점의 ER 다이어그램]
<img src="https://github.com/silxbro/cs-study/assets/142463332/3c16037e-8589-4e8b-97ef-164882f57c60" width="400" height="500"/><br/>

#### [관계 대응 수 선택]
[Relationship Editor] 창의 'Cardinality Properties'에서는 관계 대응 수를 선택할 수 있다.
- <img src="https://github.com/silxbro/cs-study/assets/142463332/92448a00-0840-44d1-9af5-95f1320bf00b" width="80" height="3"/> : 단순 관계선만 표현
- Zero, One or More : 0 이상의 값, 선택 참여를 의미함 <img src="https://github.com/silxbro/cs-study/assets/142463332/f79bd5ee-c731-42f5-ba7c-38ee2cbf3fed" width="30" height="20"/>
- One or More : 하나 이상의 값, 필수 참여를 의미함 <img src="https://github.com/silxbro/cs-study/assets/142463332/00c9b716-6d8b-4714-9d11-22296a8edd01" width="25" height="20"/>
- Zero or One : 0 또는 하나의 값, 선택 참여를 의미함 <img src="https://github.com/silxbro/cs-study/assets/142463332/7175b5c9-709a-41c9-827d-7c1bba52bb71" width="30" height="20"/>

### 도메인 정의하기
ER 다이어그램이 완성되면 논리 모델링의 중요한 단계 중 하나인 속성의 도메인을 정의해야 한다. 도메인이란 속성이 가질 수 있는 값을 뜻한다.
지금까지 생성한 개체와 속성은 다음과 같다.
#### [마당서점의 개체와 속성]
<img src="https://github.com/silxbro/cs-study/assets/142463332/b6788c37-1893-40d0-94c3-318532e41e37" width="490" height="150"/><br/>

위 속성들을 살펴보면 각 개체의 기본키를 구성하는 '번호', 도서, 고객, 출판사 등에 사용하는 '이름', 판매액을 나타내는 '금액', 권당 '단가', 날짜를 나타내는 '일자' 등과 같이
공통으로 사용되는 속성이 있다. 이렇게 공통으로 사용되는 속성을 도메인으로 미리 정의해놓으면 향후 데이터 타입의 변경에 쉽게 대처할 수 있다.
도메인별 관련 데이터 타입은 자료의 용도에 맞게 적절하게 선택해야 한다. 아래 표는 마당서점의 도메인별 데이터 타입을 정의한 것이다.
#### [마당서점의 도메인별 데이터 타입 정의표]
|구분(기본 도메인)|도메인|데이터 타입|
|:---|:---|:---|
|숫자(Number)|번호<br/>금액<br/>단가|INTEGER<br/>INTEGER<br/>INTEGER|
|문자(String)|이름<br/>주소<br/>전화번호|VARCHAR(40)<br/>VARCHAR(40)<br/>VARCHAR(30)|
|날짜시간(Datetime)|일자|DATE|

도메인은 ERwin의 [Model Explorer]-[Domains]에서 입력한다.
[Domains]에는 기본적으로 Number(숫자), String(문자), Datetime(날짜시간) 등이 있으며, 각각 자신의 특징에 맞는 데이터 타입이 설정되어 있다.
- 도메인을 생성하는 방법에는 다음과 같이 두 가지가 있다.
    - [Model Explorer]-[Domains]에서 마우스 오른쪽 버튼을 눌러 [New]를 선택한 후 모든 값을 새롭게 설정하여 생성한다.
    - 기존에 생성된 도메인명(Number, String 등) 위에서 마우스 오른쪽 버튼을 눌러 [New]를 선택한 후 선택된 도메인의 속성값을 기본으로 하여 생성한다.

먼저 '번호' 도메인을 설정해보자.
번호는 Number형을 사용하기로 하였으므로 아래와 같이 Number에서 마우스 오른쪽 버튼을 누른 후 [New]를 선택하고 도메인 이름을 '번호'라고 입력한다.
#### [도메인 설정]
<img src="https://github.com/silxbro/cs-study/assets/142463332/5ca3c72c-81a7-4b23-91df-fbcf65a85a87" width="520" height="350"/><br/>

나머지 Number형 역시 도메인 정의표를 참조하여 입력한다. Number형 입력이 끝나면 String형, Datetime형의 도메인을 모두 생성한다.
아래는 도메인 정의표에 따라 모든 도메인을 생성한 모습이다.
#### [마당서점의 도메인]
<img src="https://github.com/silxbro/cs-study/assets/142463332/e54abedf-658a-4965-9cbd-0a4289563169" width="500" height="230"/><br/>

모든 도메인 이름을 생성했으면 각 도메인의 데이터 타입 크기 및 기타 속성을 설정해야 한다.
속성을 설정하려면 도메인 이름에서 마우스 오른쪽 버튼을 누른 후 [Properties]를 선택한다.
[Domain Editor] 창이 나타나면 아래와 같이 각 도메인별로 정의한 데이터 타입을 Logical Data Type에서 선택하여 입력한다.
'금액' 도메인은 NUMBER형으로 정의하였으므로 해당되는 Logical Data Type에 'INTEGER'를 선택한다.
'이름' 도메인의 자료형 VARCHAR(40)과 같이 크기가 지정된 경우에는 VARCHAR()을 선택한 후 괄호() 사이에 '40'을 입력한다.
모든 도메인 데이터 타입의 입력이 완료되면 <Close>를 클릭한다.
#### [도메인별 데이터 타입 설정]
<img src="https://github.com/silxbro/cs-study/assets/142463332/84a0d0fa-dbc1-45ba-b0ed-2474835a7f32" width="750" height="430"/><br/>

- 참고로 [Domain Editor] 창의 Attribute Name은 도메인을 이용하여 개체에 새로운 속성을 만들 때 속성 이름의 형식을 정하는 메뉴이다.
  예를 들어 '%EntityName%AttDomain'과 같이 설정하면 '개체이름+도메인이름'의 형태로 속성 이름이 생성된다.
  도메인 생성 후 새로운 속성을 추가할 경우 [Model Explorer]-[Domain]에서 해당 도메인 이름을 선택한 후 드래그하여 추가하고자 하는 개체에 끌어다 놓으면 된다.
  '이름' 도메인을 클릭한 후 드래그하여 '도서' 개체에 놓으면 '도서 이름'이라는 속성이 자동으로 생성된다.

이제 정의된 도메인을 개체의 각 속성들에 적용해야 한다. 아래와 같이 개체 위에서 마우스 오른쪽 버튼을 눌러 [Attribute Properties]를 선택한다.
[Attribute Editor] 창이 나타나며 각 개체 속성에 대해 도메인을 지정해준다.
예를 들어 도서 개체의 도서번호를 '번호'로 설정하려면 Entity를 '도서'로 선택하고 도서번호 속성의 Parent Domain을 '번호'로 선택한다.
같은 방식으로 도서의 나머지 속성과 주문, 고객 개체의 속성에 대해서도 도메인을 적용한다. 모든 속성의 설정을 마쳤다면 <Close>를 클릭해 작업을 종료한다.
#### [개체 속성 설정]
<img src="https://github.com/silxbro/cs-study/assets/142463332/fb052d17-22de-4d72-9f1d-ffb74683dfbd" width="630" height="400"/><br/>
- 속성값 설정 시 도메인 정의 없이 속성의 Data Type을 일일이 설정하는 식으로 작업하는 것은 비효율적이다.
  예를 들어 여러 사람이 참여하는 대규모 모델에서 도메인을 일일이 설정한다면 반복적인 중복 작업으로 효율성이 떨어질 것이다.
  뿐만 아니라 완성된 결과물 역시 설계자에 따라 달라질 수 있어 전체 데이터베이스의 질을 저하시킬 수도 있다.

마당서점의 논리적 모델링까지 완료하였다. 완성된 ER 다이어그램은 다음 실습을 위해 [File]-[Save] 메뉴를 선택해 저장해둔다.

### 마당서점의 물리적 모델링
앞에서 마당서점의 논리적 모델(logical model)을 작성했다면 여기서부터는 마당서점의 물리적 모델(physical model)을 작성한 후 실제 테이블로 만들어본다.

#### ❶ ER 다이어그램 불러오기
[File]-[Open] 메뉴를 선택해 논리적 모델 작업까지 완료하고 저장해둔 ER 다이어그램을 불러온다.

#### ❷ Physical 타입으로 변경하기
[View]-[Physical Model] 메뉴를 선택해 ER 다이어그램을 물리적 모델로 변경한다.
물리적 모델로 변경되면 [Model Explorer]는 물리적 모델을 위한 메뉴로 변경되며, 논리적 모델에서 작성한 ER 다이어그램상의 각 개체의 이름 역시 물리적 모델에 맞게
공백 사이에 밑줄(_)이 추가되어 보인다.
#### [모델 타입의 변경]
<img src="https://github.com/silxbro/cs-study/assets/142463332/7dda8490-d8a9-47ea-b6f9-1f0b8ccbe317" width="700" height="500"/><br/>

#### ❸ 물리적 모델링
물리적 모델은 테이블을 만들기 위한 과정으로 논리적 모델과는 별도의 모델로 본다. 실제 논리적 모델에서는 개체를 Entity라고 불렀으나 물리적 모델에서는 테이블이라고 부른다.
또한 논리적 모델에서 '도서 고객'과 같이 개체 이름에 공백을 넣어서 사용할 수 있었으나 테이블 이름에는 공백을 넣을 수 없다.
이와 같은 이유로 물리적 모델 단계에서는 기존 논리적 모델에서 생성한 개체의 이름과 속성의 이름을 실제 사용할 테이블 이름, 컬럼 이름으로 변경해주어야 한다.
아래 표는 마당서점의 개체별로 논리적 모델과 물리적 모델에서 사용하는 이름을 정리한 것이다. 각 테이블 이름 앞에 New를 기존 테이블 이름과 충돌을 피하기 위하여 붙이기로 하자.

#### [마당서점의 논리적 모델, 물리적 모델에서의 개체별 대응표]
<img src="https://github.com/silxbro/cs-study/assets/142463332/028848dc-6c42-414e-aa7d-9bfd0c1c87ca" width="570" height="400"/><br/>

위 표를 참고하여 논리적 모델 단계에서 만든 개체 이름과 속성 이름을 아래와 같이 실제 데이터베이스에서 사용할 테이블 이름과 컬럼 이름으로 변경한다.
#### [마당서점의 테이블]
<img src="https://github.com/silxbro/cs-study/assets/142463332/1f3d5201-ecbd-44a5-8770-76a1740fdcc6" width="400" height="230"/><br/>

#### ❹ 컬럼의 속성 확인하기
각 테이블 컬럼의 속성이 제대로 설정되었는지 확인하기 위해 아래와 같이 테이블 이름 위에서 마우스 오른쪽 버튼을 눌러 [Column Properties]를 선택한다.
앞서 논리적 모델을 작성할 때 도메인을 정상적으로 적용하였다면 물리적 모델의 각 테이블별 컬럼의 속성(데이터 타입, 제약조건 등) 역시 지정된 도메인에 맞게 설정되어 있을 것이다.
이 단계에서 설정한 Physical Data Type은 실제 DBMS에서 테이블을 생성할 때 적용된다.
#### [컬럼 속성 확인]
<img src="https://github.com/silxbro/cs-study/assets/142463332/0b2c80a5-ed8d-4169-aea2-1e594c85f94a" width="650" height="420"/><br/>

여기까지 작업을 마쳤다면 마당서점의 물리적 모델링까지 완료한 것이다.
<br/>
<br/>
## 4. DBMS에 접속하여 테이블 생성하기
ERwin은 완성된 ER 다이어그램을 DBMS의 테이블로 직접 생성할 수 있는 기능을 제공한다. 본 실습을 진행하기 위해서는 부록 C.3의 ODBC 설정이 완료되어 있어야 한다.

#### ❶ DBMS에 접속하기
ERwin을 MySql 데이터베이스에 연결하기 위해 아래와 같이 [Actions]-[Database Connection] 메뉴를 선택하고, MySQL을 선택하고 창에서 다음과 같이 설정한 후 <Connect>를 클릭한다.
#### [데이터베이스에 연결]
<img src="https://github.com/silxbro/cs-study/assets/142463332/105266a2-72e5-46fb-a0d0-c49c1d88b91f" width="650" height="320"/><br/>

#### ❷ 테이블 생성하기
테이블을 생성하기 위해 [Actions]-[Forward Engineer]-[Schema] 메뉴를 선택한다.
#### [스키마 메뉴 선택]
<img src="https://github.com/silxbro/cs-study/assets/142463332/15ad695f-bede-42d7-a7e4-42210d3a3e5a" width="670" height="250"/><br/>

창이 나타나면 각 대상별로 DB에 적용할 내용을 설정해준다.
이 실습에서는 테이블만 생성하므로 아래와 같이 Option Selection에서 Table, Column, Referential Integrity만 그림과 같이 선택하고 나머지 모든 부분의 체크를 해제한다.
선택 후 <Next>를 클릭해서 Preview까지 진행한다.
#### [스키마 생성 마법사]
<img src="https://github.com/silxbro/cs-study/assets/142463332/64792b39-8148-43bb-8022-3b915a55fc78" width="900" height="280"/><br/>
<img src="https://github.com/silxbro/cs-study/assets/142463332/5be09a83-99ac-40fb-a700-f8ccff62147b" width="900" height="280"/><br/>
<img src="https://github.com/silxbro/cs-study/assets/142463332/aecbb059-de69-45a5-9651-edcc82768b6a" width="900" height="280"/><br/>
<img src="https://github.com/silxbro/cs-study/assets/142463332/1c63b639-c163-4fe2-8bcc-74d2c58da4c7" width="900" height="280"/><br/>
<img src="https://github.com/silxbro/cs-study/assets/142463332/76d8596f-4ae0-4012-9ad3-f1c8aeb2443a" width="900" height="280"/><br/>

모든 설정을 마쳤으면 Preview를 클릭하여 테이블 생성에 관련된 SQL 스크립트가 만들어진 것을 확인한다. 확인 결과에 문제가 없으면 <Generate>를 클릭하여 MySQL에 테이블을 생성한다.
#### [데이터베이스 스키마 생성]
<img src="https://github.com/silxbro/cs-study/assets/142463332/e02c6300-93d2-45be-a6a8-34c783642125" width="850" height="310"/><br/>

테이블이 성공적으로 생성되었다면 Workbench를 실행하여 madang 계정으로 접속해본다.
아래와 같이 기존의 마당서점 테이블과 별도로 NewBook, NewCustomer, NewOrders, NewPublisher 테이블이 생성된 것을 확인할 수 있다.
#### [madang 데이터베이스에 추가된 테이블]
<img src="https://github.com/silxbro/cs-study/assets/142463332/4f4bc909-5a51-42bc-9142-3f4152dd8fee" width="650" height="380"/><br/>