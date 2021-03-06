# NoSQL
Not Only SQL
1998년도에 SQL을 사용하지 않는 경량 데이터베이스의 의미로 처음 등장하였다.
일반적으로 통용되는 정의가 없으며, 그 정의를 내리는 권위자도 존재하지 않는다.

# NoSQL?
빅데이터와 사물인터넷의 발전으로 다양한 형태의 데이터를 효율적으로 저장하고 처리하기 위한 새로운 형태의 데이터베이스 구조가 필요했다.
- 관계형 모델을 사용하지 않음
- 대부분 오픈소스
- 21세기 웹 환경을 위해 구축됨
- 스키마가 없음

NoSQL을 전통적인 관계형 데이터베이스가 해결할 수 없는 웹 기반 애플리케이션의 성능과 확장성 요구사항을 해결하기 위해 개발된 시스템들이기 때문에, 분산 처리 구조를 지니고 있다. 따라서 분산 시스템에 적용되는 CAP 이론에 대하여 이해하는 것도 필요하다. CAP 이론은 Eric Brew가 주장한 것으로, 분산 시스템에서 보장해야 하는 측면을 다음과 같이 정의하고 있다.
## CAP
- 일관성(consistency): 모든 클라이언트들은 항상 데이터들에 대한 같은 뷰를 가져야 한다. 즉 모든 노드들은 동시에 같은 데이터를 보아야 한다.
- 가용성(availability): 각각의 클라이언트들은 항상 읽고 쓸 수 있다. 모든 요청은 실패하든 성공하든 응답을 받는 것을 보장한다.
- 분할 용인(partition tolerance): 네트워크 실패로 인하여 임의의 분할이 발생해도 시스템은 계속적으로 동작해야 한다. 임의의 메시지들의 손실 또는 시스템의 부분 실패에도 불구하고, 시스템은 지속적으로 동작한다.
CAP 이론에 따르면 C와 A는 동시에 만족할 수 없기 때문에, 일반적으로 C+P 전략 혹은 A+P 전략을 절충해서 취한다.
일반적인 SQL 시스템의 경우 트랜잭션이 지녀야 하는 4가지 속성인 ACID 속성을 만족하는데, CAP의 P 속성을 기본적인 측면으로 취하기 때문에 BASE 속성을 따른다.

## BASE
- Basically Available: 확장성
- Soft State: 소프트 상태
- Eventually Consistent: 결국 일관성

기본적으로 확장적은 NoSQL 데이터베이스 시스템이 확장성을 보장한다는 것으로, 애플리케이션은 항상 동작을 해야 한다는 것을 의미한다.
소프트 상태는 애플리케이션들이 항상 일관적인 상태에 있을 필요가 없음을 의미한다. 점진적인 일관성은 리더(reader)들이 시간이 지남에 따라 쓰기 결과를 본다는 뜻이다. 안정 상태에서 시스템은 결국 마지막 쓰기 결과를 반환한다. 이는 클라이언트들은 변경이 진행중인 상황에서는 데이터의 불일치 상태를 겪을 수 있다.

많은 수의 NoSQL 데이터베이스가 샤딩(sharding)을 특징으로 내세우고 있다.
샤딩 기법을 사용할 때 중요한 문제중의 하나는 데이터 아이템을 어느 물리적인 노드에 배치하는 지를 결정하는 것이다.
가장 단순한 방법은 NodeID = hash(key) % TotalNodes 식을 사용하여 노드의 위치를 찾는 것이다.
새로운 노드의 추가나 기존 노드의 삭제가 야기하는 전체 데이터의 재정리(reshuffling)가 발생하는데, 하지만 나머지 연산은 이런 클러스터의 재조정을 효율적으로 지원할 수 없고 결과적으로 복제와 장애조치(failover)를 다루기 매우 어렵다.
이를 해결하기 위한 방법으로 NoSQL 데이터베이스들 중 일부는 consistent hashing을 사용한다.
Consistent hashing의 기본 아이디어는 데이터 아이템과 노드들이 같은 해시 함수를 사용한다는 것이다.
노드들에게 간격(interval)을 매핑하는데, 각 간격은 많은 수의 데이터 아이템 해시들을 저장한다.
노드가 클러스터에서 제거되면, 그 노드의 간격은 이웃한 간격을 지닌 노드가 가져간다.
다른 노드들에게는 아무런 변화가 발생하지 않는다.

# Types of NoSQL Systems
- Key-Value Stores: 가장 기본적인 형식으로 대부분의 NoSQL이 지원하는 저장 개념인 키/값 형식을 지원한다. Memcached, Redis, Dynamo, Voldemort, BerkeleyDB, 등이 지원한다.
- Wide Column Stores: 키/값 저장소의 확장 개념으로, 값으로 컬럼 패밀리 형태를 사용한다. Cassandra, Hbase, Hypertable, Accumulo, 등의 오픈소스가 존재한다.
- Document Stores: 키/값 저장소의 확장된 개념으로, 값의 형태로 XML과 JSON 형태의 문서 타입을 사용한다. MongoDB, CouchDB, Couchbase, Dynamo, OrientDB, 등의 솔루션이 있다.
- Graph Database: 테이블간의 JOIN 연산이 아니라 그래프에서의 경로 탐색 형태의 연산이 많이 사용되는 소셜 네트워크 분석 같은 그래프 응용 프로그램에 주로 사용된다. Neo4j, InfiniteGraph, Titan, ArangoDB, Trinity, 등의 솔루션이 있다.

# Acknowledgements
- NoSQL 데이터베이스 최신 동향, 권준호(부산대학교), 정보처리학회지 제 22권 제 4호(2015. 7)
