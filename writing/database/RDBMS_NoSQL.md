
## 데이터베이스

데이터베이스는 컴퓨터 시스템에 저장된 구조화된 정보의 집합을 의미합니다. 데이터베이스는 대개 DBMS(Database Management System)이라는 소프트웨어에 의해 관리되며 이 둘을 합쳐 일반적으로 데이터베이스라고 부릅니다.

현재 가장 많이 쓰이는 데이터베이스는 RDBMS(Relational Database Management System)이고 이 소프트웨어와 소통하는 방법은 SQL(Structured Query Language)를 사용합니다. 

하지만 최근에 들어서 NoSQL(Not only SQL)이라는 관계형 데이터 모델을 탈피한 데이터 모델을 지향하는 DBMS가 인기를 끌고 있습니다. 해당 글에서는 RDBMS와 NoSQL 중 문서형 데이터 모델의 차이를 알아보도록 하겠습니다.

## 데이터 모델링

데이터 모델링은 현실의 정보를 인간과 컴퓨터가 이해할 수 있도록 추상화하여 표현하는 과정입니다. 데이터와 데이터의 관계, 데이터의 의미와 일관성, 제약조건을 기술하여 정보를 실제로 이용할 수 있는 형태로 표현할 수 있습니다.

데이터 모델링의 기본 구성요소는
1. Entity(개체): 개념, 정보 단위 같은 현실 세계의 대상체
2. Attribute(속성) : 개체를 구성하는 항목
3. Relation(관계) : 개체나 속성간의 관계
   으로 이루어져 있습니다.

데이터 모델링 과정은
1. 개념적 데이터 모델링 : 데이터의 주요 개체와 속성을 정의하고, 이들 간의 관계로 전체적인 구조를 설계
2. 논리적 데이터 모델링 : 개념적 모델을 기반으로, 데이터베이스 구조를 논리적으로 설계
3. 물리적 데이터 모델링 : 논리적 모델을 실제 데이터베이스 관리 시스템(DBMS)에 맞게 구현

의 과정을 걸칩니다. 이 때 논리적 데이터 모델링은 데이터 간의 관계를 표현하는 방식에 따라 관계 모델(Relational Model), 계층 모델(Hierarchical Model), 네트워크 모델(Network Model) 등의 다양한 모델이 있습니다.

## RDBMS

### 관계형 데이터 모델의 제안

초기의 DBMS는 계층형 모델로 이루어져 있었습니다. 그러다 1970년, Edgar Codd는 데이터를 테이블 형태로 저장하고 이를 관계(relation)와 튜플(tuple)로 표현하는 관계형 모델을 제안했습니다. (Excel이 1980년에 나왔으니 굉장히 획기적인 방식의 데이터 모델이었습니다.)

IBM은 이 관계형 모델을 실질적으로 구현하고 활용하기 위해 SQL이라는 선언형 질의 언어를 개발합니다. 선언형 질의 언어는 선언을 수행할 방법을 따로 명시하지 않습니다.

`SELECT * FROM STUDENT` 라는 SQL문에는 통상의 프로그래밍 언어처럼 변수를 선언한다던가, 제어문과 반복문으로 어떻게 데이터를 저장하고 변형할지에 대한 구체적인 방법이 적혀있지 않습니다.

따라서 RDBMS는 SQL의 사용으로 내부 구현을 감출 수 있게 되었고 선언이 실현되는 구체적인 방식을 사용자는 알 필요가 없게 되었습니다.
(RDBMS의 옵티마이저가 어떤 식으로 쿼리 수행 계획을 짜는지, 그 계획이 어떻게 실행되는지 사용자는 몰라도 됩니다)

1980년대 부터 30년간 RDBMS와 SQL은 데이터베이스로 가장 많이 선택되는 도구였고 현재도 큰 인기를 얻고 있습니다.

### 관계형 데이터베이스의 특징

데이터 베이스에서 데이터의 정합성은 중요합니다(데이터베이스의 데이터가 잘못되었다면 이에 기반을 둔 모든 처리 결과가 부정확해지기 때문입니다). 관계형 데이터베이스가 주류가 된 이유는 데이터의 정합성을 높이기 위한 설계 노하우가 매우 발달했기 때문입니다.

관계형 데이터베이스는 모든 데이터를 행열 테이블 형태로 저장합니다. 이 때 테이블은 공통적인 개념의 집합이어야 하며 각각의 행은 유일하게 식별할 수 있어야 합니다.

이 두가지의 합의를 지키지 않는다면 매우 제멋대로인 테이블들이 생성될 수 있고 이러한 테이블들은 데이터가 중복되며 이에 따른 이상현상이 발생하기 쉬워집니다. 
이를 방지하기 위해 관계형 데이터베이스는 '테이블은 이렇게 정의해야 한다'라는 테이블 설계 이론 `정규화` 에 따라 설계됩니다.

정규화는 간단하게 말해서 `x -> y`라는 관계를 지키며(이를 함수 종속성이라 합니다) 데이터의 중복을 최소화하게 테이블을 분리하는 것을 의미합니다. 자세한 정규화의 방법은 [링크](https://en.wikipedia.org/wiki/Database_normalization)에서 볼 수 있습니다.

또다른 관계형 데이터베이스의 큰 특징은 데이터의 일관성을 굉장히 중요시 여긴다는 것입니다. 이를 위해 관계형 데이터베이스는 데이터 갱신시 트랜잭션을 지원합니다.

트랜잭션은 데이터베이스에서 여러개의 작업을 하나의 단위로 묶어서 처리하는 기술이며 이는 ACID특성을 가집니다.

1. Atomicity : 트랜잭션은 모두 성공 or 모두 실패
2. Consistentcy : 트랜잭션 이전과 이후 DB 스키마는 그대로여야 합니다.
3. Isolation : 동시에 실행되는 트랜잭션은 서로 격리된다
4. Durability : 트랜잭션이 성공했다면 결함이나 장애가 발생해도 데이터는 손실되지 않는다.

## `#NoSql`

NoSql은 현재 관계형 데이터베이스가 아닌 데이터베이스를 일컫는 말이지만 원래는 2010년 경 오픈소스, 분산환경, 관계형 데이터베이스 탈피 밋업의 해시태그 였습니다.

이러한 바람이 일어난 이유는 애플리케이션이 다루어야 하는 데이터의 급증입니다. 따라서 대규모 데이터셋의 높은 쓰기량을 맞추기 위한 확장성이 필요했고, 관계형 데이터베이스의 엄격한 스키마에 대한 불만과 유연한 데이터 모델에 대한 열망이 NoSQL 오픈소스 발달의 배경에 있었습니다.

### RDBMS의 한계

- 스키마의 엄격함과 유연성 부족

RDBMS는 설계시의 테이블이 지속될 것을 가정합니다. 예를 들어 상품에 신규 이벤트 A가 적용되었는지 여부를 기존의 테이블에 적용하기 위해서는 모든 테이블의 행을 바꿔야 하며 적재된 데이터가 많을수록 많은 시간과 비용이 듭니다.

- 수평적 확장의 어려움

관계형 데이터 모델은 정규화를 시행하여 데이터의 중복을 줄이지만 이는 동시에 하나의 객체에 대한 연관된 데이터들이 여러 테이블로 나뉘어 지는 것을 의미합니다. 이 테이블들은 단일 서버에서는 효율적으로 처리되지만 서버가 분산되어 있다면 관리가 어렵고 성능에 많은 영향을 미칩니다.

관계를 유지하며 데이터를 연산하기 위해 조인연산이 빈번하게 발생하며 데이터가 여러 서버에 분산되어 있을 경우 네트워크 오버헤드가 추가로 발생합니다.

트랜잭션을 지원하는 것 또한 여러 서버에서 동시에 트랜잭션을 수행하고 모든 트랜잭션의 성공을 검증해야 하는 방식으로 바뀌기 때문에 관리가 복잡해질 수 있습니다.

- 객체지향 프로그래밍 언어와 RDBMS의 패러다임 불일치

객체지향 언어는 데이터와 기능을 함께 묶어 객체로 표현하며, 객체 간의 관계를 통해 프로그래밍 로직을 구현합니다. 반면, RDBMS는 데이터를 테이블 형태로 저장하며 테이블 간의 관계를 통해 데이터 모델을 구성합니다.

이러한 차이로 인해 객체지향 언어와 RDBMS 간의 데이터 변환이 필요하며, 이를 위해 ORM(Object-Relational Mapping) 도구를 사용하지만, ORM 도구의 사용은 추가적인 복잡성과 성능 오버헤드를 초래할 수 있습니다.

Java의 Hibernate를 예로 들자면 N+1문제는 이러한 패러다임의 불일치에 의한 사이드이펙트라고 할 수 있습니다. (객체내의 정보에 접근한다는 객체 지향언어에서는 자연스러운 접근이 테이블끼리의 접근에 join을 사용해야만 하는 RDBMS에서는 불필요한 쿼리와 성능저하를 가져옵니다)

### NoSQL의 특징

NoSQL은 위와 같은 RDBMS의 한계를 탈피하기 위해 등장했습니다. NoSQL은 RDBMS의 일부 기능에 대한 집착을 버림으로써 확장성과 유연성을 획득합니다.

-  스키마에 대한 집착을 버리다

RDBMS의 한계에서 말했던 것처럼 고정된 스키마와 데이터의 중복을 제거하기 위한 정규화에 따른 다수의 테이블은 수많은 join의 사용이 필요했습니다.
```
public class Post {
	String title;
	String content;
	List<Comment> comments;
	List<View> views;
}
```
RDBMS는 위와 같은 객체를 Posts, Comments, Views 테이블 3개로 나누어서 저장할 것입니다. 따라서 Post라는 하나의 행의 정보를 온전히 얻기 위해서는 적어도 3개 테이블의 join이 필요합니다.

문서형 데이터 모델을 사용하는 MongoDB는 해당 객체를 json형태로 저장합니다.
```json
{ "title": "제목", "content": "내용", "comments": [ { "id": 1, "text": "댓글 내용 1" }, { "id": 2, "text": "댓글 내용 2" } ], "views": [ { "viewerId": 1, "viewedAt": "2024-07-03T10:00:00Z" }, { "viewerId": 2, "viewedAt": "2024-07-03T11:00:00Z" } ] }
```

이제 Post에대한 하나의 질의로 Post객체에 대한 정보를 온전히 얻을 수 있습니다.
다른 객체에 의존하지 않기 때문에 분산 환경에서 Post가 어느 서버에 저장되어도 다른 서버에 있는 데이터에 대한 join을 수행할 필요가 없어지며 이는 NoSQL이 수평적 확장을 쉽게 하는 이유가 됩니다.

- ACID보다 BASE

RDBMS의 트랜잭션은 ACID를 충실히 지킴으로써 다중 사용자 요청시 각 데이터의 즉각적인 일관성을 보장합니다. NoSQL은 트랜잭션에 대해 BASE(Basically available, Soft state, Eventual Consistency)를 추구합니다.

1. Basically Available : 사용자는 언제든 데이터베이스 동시에 엑세스할 수 있다.
2. Soft State : 데이터는 일시적, 임시 상태를 가질 수 있다.
3. Eventual Consistency : 로컬 데이터는 일시적으로 변경을 반영하지 않을 수 있지만 변경 내용은 전파되고 최종적으로 일관성을 획득한다.

즉각적인 일관성에 대한 집착을 포기함으로써 ACID가 가지는 단점(레코드의 일관성을 유지하기 위해 잠금이 필요 혹은 무결성 제약으로 잠금이 전파되는 등)을 극복하여 더 나은 사용자 요청 처리를 할 수 있습니다.

### 문서형 데이터 모델의 한계

위에서 Post를 json으로 쉽게 문서형 데이터 모델로 표현할 수 있다고 했습니다. 하지만 User가 추가되면서 이들이 다수의 Post를 가지는 경우는 어떨까요?
```java
public class User {
	String name;
	List<Post> posts;
}
```

NoSQL은 하나의 객체에 대해 join 없는 하나의 질의로 수평적 확장의 이점을 가진다고 했습니다. 이 User 객체도 완전한 정보를 가지게 하려면 Post의 모든 데이터를 User라는 객체도 가지고 있어야 합니다.

그럼 이 User가 Team에 속한다면? 그리고 Team이 User를 다수 가질 수 있다면,  Team은 User에 대한 모든 정보와 각 User가 가지는 모든 Post에 대한 정보를 또! 가지게 되고. 중복 데이터를 여러 객체가 가지게 될 것입니다. 

이러한 중복 데이터는 모두 한꺼번에 변경되어야 하며 이는 쓰기 오버헤드의 증가와 일부 갱신의 위험이 있습니다.

MongoDB는 이러한 다대일 관계의 데이터에 대응하기 위해 문서 참조 기능을 제공하고 있지만 이는 RDBMS의 join과 다를바가 없습니다. 즉, 데이터가 다대다 관계로 상호 연결될수록 문서형 데이터 모델이 가지고 있던 장점이 희석됩니다.

이러한 문서형 데이터 모델의 한계는 관계형 모델이 제안되기 전 계층형 모델이 가지던 한계와 비슷합니다. 계층형 모델은 부모 노드가 자식 노드를 가지고 있는 형태였고 조인을 지원하지 않아 다대다 관계 표현이 어려웠습니다.

관계형 모델은 이러한 문제를 테이블로 대표되는 데이터 구성과 각 테이블 사이의 join으로 (다대다의 경우 일대다 + 다대일 매핑 테이블의 삽입)으로 풀어냈고 선풍적인 인기를 끌었습니다. 즉, 지금 문서형 데이터 모델이 가지는 단점을 80년대에 관계형 모델이 풀어냈던 것입니다.

### MySQL과 MongoDB

MySQL은 대표적인 RDBMS이고 MongoDB는 문서형 데이터 모델을 사용하는 NoSQL 데이터베이스입니다.
MySQL은 5.7버전부터 json타입의 데이터를 추가했고 MongoDB는 문서 참조와 각 노드에 대한 분산 트랜잭션 처리를 지원합니다.

MySQL은 자체적으로 복제와 파티셔닝(서버 내에서 큰 테이블 -> 작은 테이블로)을 지원하고 있고 샤딩(여러 서버에 나누어 저장)은 애플리케이션 구현에 따라 할 수 있으므로 수평적 확장을 아예 하지 못하는 것도 아닙니다.

우아한 형제들의 [발표](https://www.youtube.com/watch?v=704qQs6KoUk)는 주문 시스템이 대량의 데이터를 어떻게 처리하는지에 대해 말하고 있습니다. RDBMS에 저장되던 주문이라는 객체가 가져야하는 정보가 늘어나면서 정규화된 테이블이 늘어나게 되고 이로 인한 다수 join으로 인해 성능이 저하되었고, 이를 해결하기 위해 MongoDB를 도입하여 주문 객체를 하나로 모아 조회 성능을 높였다고 합니다.

이 때 쓰기 질의에 쓰이는 DB는 RDBMS로, 읽기 질의에 쓰이는 DB는 MongoDB로 사용하여 데이터 갱신 시에는 RDBMS의 데이터 정합성의 이점을 가져가고 읽기 시에는 MongoDB의 이점을 가져가는 방식을 택했습니다. 또한 쓰기시의 RDBMS는 샤딩을 구현하여 수평적으로 확장했다고 말하고 있습니다.

결국은 애플리케이션에서 사용하는 데이터가 어떠한 모델인지, 서로간에 어떠한 관계를 맺고 있는지에 따라 더 알맞은 도구를 선택해야 합니다.

---
참고

https://www.oracle.com/database/what-is-database/

데이터 중심 애플리케이션 설계, 마틴 클레프만 저

데이터베이스 첫걸음, 미크 기무라 메이지 저

https://www.mongodb.com/resources/compare/relational-vs-non-relational-databases

https://stackoverflow.com/questions/8729779/why-nosql-is-better-at-scaling-out-than-rdbms
