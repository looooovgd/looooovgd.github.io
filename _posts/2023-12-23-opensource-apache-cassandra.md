---
title: [번역] 아파치 카산드라란? (개요, 동작, 기능 및 사례) Apache Cassandra
date: 2023-12-23 01:53:13 +0900
categories: [Bloggings, opensource]
tags: [카산드라, Cassandra, 오픈소스, 분산, db, noSql, DHT, sql, Apache, Distributed]    # TAG names should always be lowercase
---

> 이 글은 [Spiceworks 블로그의 글](https://www.spiceworks.com/tech/big-data/articles/what-is-cassandra/amp/)을 
> 제가 번역한 내용입니다. 
> 풀어서 쓴 스타일이고, 글의 내용이 쉬운 편입니다. (학부생 용)

// 글 중간에 있는 괄호들은 제가 추가한 내용들이 대부분입니다.
// 좀 길어서... 제 깃헙 블로그 ToC 기능을 잘 이용하시면 편합니다!

<div style="background-image: url('https://cassandra.apache.org/assets/img/eye-texture.svg');    background-repeat: no-repeat;">
<img src="https://cassandra.apache.org/assets/img/logo-white-r.png">
</div>

---

---

---

# Apache 카산드라 란? (개요, 동작, 기능 및 사례)

- 글쓴이: [Chiradeep BasuMallick](https://www.spiceworks.com/user/about/chiradeepbasumallick)
- 기술 포스팅 작성가
- 최근 수정날짜: 2023년 8월 17일
---
Cassandra(카산드라)는 오픈소스 NoSQL 데이터 스토리지 시스템입니다. 
높은 가용성과 신뢰성을 가능케 하기 위해 분산 구조를 극대화한 구조입니다.
Apache(아파치) 비영리 재단에서 운용되고 있습니다.

이 글에서는 카산드라의 (어느정도의) 상세한 기능들을 설명합니다. 
기업의 실사용 사례 top 6개를 다루기도 할 것입니다.

<b>ToC Table of Contents(목차)</b>

* 카산드라란 무엇인가요?
* 카산드라는 어떻게 동작하나요? (아키텍쳐, 파티셔닝 시스템, 복제성)
* 카산드라의 핵심 기능들 (9가지)
* 실사용 사례 6개
---

## 카산드라란 무엇인가요?

카산드라는 오픈소스 NoSQL 데이터 저장 스토리지입니다.
다음과 같은 특징이 있습니다. 분산 구조를 극대화하여 고가용성,
확장성, 신뢰성들이 이루어 질 수 있도록 합니다. Apache 비영리 재단에서 관리되고 있습니다.

<div>
<img src="https://www.aquafold.com/wp-content/themes/Divi-Child/assets/diagrams-img/aquadatastudio_apache_cassandra_schema_browser_476x300.png">
</div>

그림 1. 아파치 카산드라에서의 작업 이미지. [출처](https://www.aquafold.com/dbspecific/apache_cassandra_client)

현대의 (최근의) 초연결사회는 데이터로 차고 넘치며, 거기에는 항상 기록하고 이용 극대화 할 
새로운 정보가 있습니다. 언제나 기업들이 자신들의 앱을 통해서 처리하고 인용해야 할 새로운 정보들을 필요로 합니다. 
이는 기업의 의사결정에 같은 방식으로 사용되기도 합니다. 그런데 누군가는 반드시 데이터를 저장해야합니다.
이 때, 이 기업용 데이터 스토리지는 database(데이터베이스)라고 일컬어집니다.

데이터베이스라고 하는 것은 저장된 데이터의 조직화된 모음이며, 이는 전자기기에서 마음대로 데이터를 두거나 
데이터에 액세스 할 수 있게합니다. 데이터를 저장하는 것을 넘어서, 데이터에 대해 어떤 치명적인 조작과 동작이 이 시스템(데이터베이스)을
통해서 수행됩니다. 이는 데이터베이스를 갖는데에 가장 중요하며 데이터베이스 관리 시스템(DBMS)도 마찬가지입니다.

데이터베이스 관리 시스템 또는 DBMS라고 불리는 프로그램은 특정 데이터 셋을 
분석하기 위해 데이터베이스나 다른 소프트웨어들과 상호작용이 가능합니다.
데이터베이스 생태계는 데이터베이스, 관리 시스템, 그리고 관련 애플리케이션들을 포함합니다.
이것(데이터베이스에 관한 기본정보)은 우리가 카산드라를 이해하는 데에 있어서 도움을 줍니다.
왜냐하면 카산드라는 모두 데이터 저장과 데이터가 어떻게 관리되는지에 관한 게 전부이기 때문입니다.

카산드라는 오픈소스 NoSQL 분산 DB이며 막대한 양의 데이터가 저장된 상업 서버의 데이터들을 관리합니다.
분산 (decentralized)되어 있으며, 확장가능한 저장 시스템은 상업용 서버에 저장되는
막대한 볼륨의 데이터들은 다루기 위해 고안되었는데, 고가용성을 단 한 점의 오류,실패 없이 제공합니다.
(중요!!)

---

## 카산드라의 역사

처음에 카산드라는 FaceBook이 오픈소스로 만들었지만 후에 아파치의 프로젝트로 변하기 위해 2008년에 릴리즈 되었습니다.
프로젝트는 미국 비영리 아파치 소프트웨어 재단에 의해 유지보수됩니다. 2010년에도 최우선 프로젝트로
생각되었으며 지금은 세계 최고의 NoSQL DB 시스템이 되었습니다. 카산드라는 신뢰 받을만하며
수천개의 기업들에 의해 사용되는데, 이는 당연히 손쉬운 확장뿐만 아니라 더 중요한 장점인
단 한 건의 오류,실패(쿼리)가 없다는 것입니다. 현재, 이 솔루션은 넷플릭스, 트위터(현 X), 레딧 등
등의 기업에서 데이터베이스를 다루기 위해 배포되어 있습니다.

2010년에는 카산드라의 0.6.13 버전이 개시되었습니다. 열 두 번의 iteration을 거쳐, 가장 최신 버전은 4.1.1이고,
2023 상반기에 개시되었습니다. 각 버전은 핵심 업데이트들을 갖고 해결책으로 제시했으며, 셀프튜닝 memtable의 추가 라던지
카산드라 쿼리 언어(CQL), 향상된 컴팩션, 컴프레션, 그리고 읽기수행능력 등이 그 중에서도 두드러집니다.

더 보기 : [데이터 카탈로그란 무엇인가요? 정의, 예제, 가장 좋은 실사용들](https://www.spiceworks.com/tech/big-data/articles/what-is-a-data-catalog-definition-examples-and-best-practices/)

---

## 카산드라는 어떻게 동작하나요?

아파치 카산드라는 분산 데이터베이스 관리 시스템이며, 몇개의 클라우드 데이터 센터들에 대한
많은 양의 데이터들에 대해서 관리하기 위해 만들어졌습니다. 카산드라의 동작을
이해하는 것은 시스템의 3가지 절차를 이해하는 것과 같습니다. 이것들은(3가지 절차는) 이미 만들어진
아키텍쳐 구성요소, 파티셔닝 시스템, 그리고 복제 가능성에 대한 것입니다.

### 1. 카산드라의 아키텍쳐

카산드라의 주된 아키텍쳐는 노드들로 구성된 클러스터로 이루어져 있습니다. 아파치 카산드라는
peer-to-peer 시스템으로 구조화 되어 있으며 DynamoDB와 Google Bigtable을 닮았습니다.

카산드라의 모든 노드는 동일하며 동일한 중요도를 갖는데, 이는 카산드라의 근본적인 구조입니다.
각 노드는 특정 데이터가 저장되는 정확한 포인트입니다. 한 그룹의 노드, 서로 연관된 그 그룹이 데이터
센터를 만들어냅니다. 완전한 데이터 센터들의 세트(모음)는 프로세싱을 위한 데이터 저장에 대해 수용
가능한 수준이며 이 것들이 클러스터를 구성합니다.(데이터 센터들이 있고 이것들이 모이면 클러스터가 된다+
데이터센터의 데이터는 바로 가공 가능하다)

카산드라 아키텍쳐에서 가장 아름다운 것은 데이터를 보관하기 위한 확장이 굉장히 쉽게
이루어 질 수 있다는 것입니다. 노드를 추가함으로써, 과부화 없이 시스템이 감당하는 데이터의
양을 두 배로 늘릴 수 있습니다. 이 동적 스케일링 능력은 양방향으로 갑니다. 노드를 제거함으로써,
개발자들은 필요시 데이터베이스 크기를 줄이는 것이 가능합니다. 이전의 SQL과 증가하는 데이터 적재
용량에 대한 복잡성을 생각하며 비교해 보았을 때, 카산드라의 아키텍쳐는 상당한 이점을 제공합니다.

또 다른 카산드라 아키텍쳐가 가지는 기능성은, 데이터 보안의 향상과 데이터 손실로부터의
보호에 일조한다는 것입니다.

### 2. 파티셔닝 시스템(Partitioning System)

카산드라에서, 데이터는 파티셔닝 시스템을 통해서 저장하거나 되찾아집니다.
파티셔너는 어디에 데이터셋의 primary copy(주 복사본)을 저장할지에 대한 것(주체)입니다. 이것은 
nodal tokens을 이용한 직접적인 포맷(in a direct format)으로 작동합니다. 
모든 노드는 토큰셋을 갖거나 그에 대한 역할이 있습니다. 이 토큰셋은 파티션 키에 기반합니다.
파티션 키는 데이터가 어디 저장되는지 결정하는 역할을 합니다.

데이터가 클러스터에 들어가는 즉시, 해쉬 함수가 파티션 키에 추가됩니다. Coordinator node(요청을 가지고 클라이언트가 연결하는 노드)
는 노드로 그 파티션에 대한 토큰과 데이터를 함께 전송하는 역할을 합니다. 

### 3. 카산드라의 복제성(replicability)

또 다른 카산드라의 동작은 노드를 거쳐 데이터를 복제하는 것입니다. 
이 2차 노드들은 replica node(복제 노드)라고 불리우며, 주어진 데이터셋을 위해
많은 수의 복제 노드들이 복제 요인(복제 지수;replication factor)에 기반합니다.
복제지수 3 (a replication factor of 3)은 세 개의 노드가 같은 토큰 range(범위)를
커버한다는 뜻이고, 같은 데이터를 저장한다는 뜻입니다. 
복수의 복제본들은 카산드라 신뢰성의 핵심이 되는 부분입니다.

일시적 또는 영구적으로 한 노드가 기능을 중지하게 되더라도, 다른 노드들이 같은
데이터들 가지고 있습니다. 이 말은 데이터가 어떻게 되더라도 완전히 없어지기 어렵다는 뜻이죠.
더 나아가서, 만약 일시적으로 기능상실 노드가 다시 원상태로 돌아왔을 때, 누락했을 뻔한 data action들을
업데이트로 하여금 따라잡을 수 있게 되고, 기능 재개에 속도를 올릴 수 있게 되는겁니다.

더보기 : [데이터 거버넌스(data governance)란 무엇인가요? 정의, 중요성, 좋은 실사용사례들](https://www.spiceworks.com/tech/big-data/articles/what-is-data-governance-definition-importance-and-best-practices/)

---

## 카산드라의 핵심 기능들

카산드라는 독특한 데이터베이스 시스템이며, 핵심기능을 다음과 같은 것들을 가지고 있습니다:


<div>
<img src="https://images.spiceworks.com/wp-content/uploads/2022/07/15150609/Key-Features-of-Cassandra.png">
</div>
그림2. 카산드라의 핵심 기능들

### 1. 오픈 소스 가능 (Open-Source Available)
손쉬운 제품을 무료로 얻는것만큼 흥미진진한 것도 없습니다. 이것은 아마 카산드라의 
광범위한 인기와 수용성 다음으로 오는 가장 중요한 요인들 중 하나일 겁니다.
아파치가 다루는 오픈 소스 제품들중 하나이고 이용하기 원하는 모든 이들에게 무료입니다.

### 2. 분산 확장성(Distributed Footprint)
카산드라의 또 다른 특징은 잘 분산되고 중앙형 시스템과 다르게 복수개의 노드들로
가동되도록 정해져있다는 점입니다. 모든 노드들은 중요도에 있어서 평등하고, 
마스터 노드 없이, 병목현상의 미발현이 프로세스의 속도를 줄입니다.(no bottleneck slows the process down.)
이것은 매우 중요한데요, 왜냐면 카산드라를 이용하는 회사는 지속적으로 정확한 데이터를
운용해야하고, 데이터 손실은 용납할 수 없기 때문입니다. 노드를 걸쳐서 이루어지는 카산드라데이터의
평등하고 넓게 퍼진 분포는 하나의 노드가 다른 시스템의 평소 퍼포먼스에 상당하게 
영향을 미치지는 않는다는 소리입니다.

### 3. 확장 가능성 (Scalability) 
카산드라는 탄력적인 확장성이 있습니다. 이것은 심한 여려움이나 저항없이 스케일 업
하거나 다운 할 수 있다는 뜻입니다. 카산드라의 확장가능성은 다시금 언급되는 nodal 아키텍쳐에
의한 것입니다. 개발자나 회사가 수평적으로 성장하고자 함에 따라 카산드라가 커질 수 있는 것이죠.
카산드라에서의 스케일 업은 매우 쉬우며 위치에 제약을 받지 않습니다. 동적인 당신의 수요에 따라 
데이터베이스에 대하여 추가적인 노드추가 또는 노드제거 작업을 할 수 있습니다.

스케일링(노드를 추가하거나 제거하는 것)에 있어서 가장 재밌는 포인트는 속도 저감,
일시정지, hitch in the system(뭔가 걸려서 진행이 안되는 것)이 없다는 점입니다.
이 말은 사용자들은 뭔 일이 발생했는지 느끼지 못한다는 의미이며, 네트워크에 연결된
모든 개인이 매끄러운 서비스를 보장받는다는 말 이기도 합니다.

### 4. 카산드라 쿼리 언어(Cassandra Query Language)
카산드라는 관계형 데이터베이스가 아니며 표준쿼리언어,SQL을 사용하지 않습니다. 
카산드라 쿼리 언어(CQL)을 사용하죠. admin사용자가 하나의 언어를 통으로 마스터
해야 했을 수도 있기 때문에 문제가 되어 왔었습니다. - 하지만 카산드라쿼리언어의 좋은 점은
SQL과 매우 유사하다는 점입니다. 행과 열, 테이블 기반 데이터들을 다루기 위해 구조가 잘 짜여있습니다.

그럼에도, SQL의 수정된 스키마에 대해서는 유연성에서 부족함을 보입니다. CQL은
정형(표;tabular) 데이터베이스 시스템과 키 값을 조합했습니다. 데이터타입 오퍼레이션, 정의 오퍼레이션,
데이터 정의 오퍼레이션, 트리거 오퍼레이션, 보안 오퍼레이션, 수학적 오퍼레이션 등을 사용합니다.

### 5. 실패 견고성 (실패 내성; Fault tolerance) 

카산드라는 우선적 실패-견고의 특성이 있습니다. 왜냐하면 카산드라의 복제가능 기능때문입니다.
데이터 복제는 시스템이 같은 정보에 대해서 여러 위치나 노드에 저장하는 것을 나타냅니다.
이것은 매우 가용적이고(바로 가져다 쓸 수 있고) 시스템에서 실패에 대해 내성이 있습니다.
데이터 센터나 단일 노드에서의 실패는 데이터베이스의 정지를 만들어내지는 않는데 이는 데이터가 
킅러스터의 다른 노드들에 거쳐서 복제되고 저장되어있기 때문입니다. 데이터 복제는 높은 수준의
백업과 복구능력을 이끌어내게 되었습니다.

더보기: [기업 데이터관리 (EDM) 란 무엇인가요?](https://www.spiceworks.com/tech/big-data/articles/what-is-enterprise-data-management-edm-definition-importance-and-best-practices/)

### 6. 스키마 제약 없음 (Schema free)

SQL은 고정된 스키마 데이터베이스 언어로, 엄격하고 정해진 형태로 만들도록 합니다.
그러나, 카산드라는 선택적 스키마 데이터 모델(schema-optional data model)로 관리자가
행과 열을 필요에 맞게 생성 할 수 있도록 허용합니다.

### 7. 조정 가능한 일관성(Tunable consistency) 

카산드라는 두 종류의 인관성을 가지고 있습니다. 하나는 최종 일관성(the eventual consistency)과
세팅 일관성입니다. 스트링 일관성은 관련된 데이터를 가진 모든 노드들에게
정보나 어떠한 업데이트라도 브로드캐스트 하는 타입입니다. 최종 일관성에서는, 클라이언트는
클러스터가 write를 받자마자 즉시 승인해야합니다.

카산드라의 조정가능한 일관성은 개발자가 수행될 함수에 따라 두 가지중 어떤 것을 사용할 지 
정하는걸 허락해주는 기능입니다. 개발자는 어느쪽이건 아니면 양쪽다의 일관성기능을 어떤 상황에서도
사용 할 수 있습니다.

### 8. 빠른 작성

카산드라는 사이즈에 상관없이 수행되는 매우 방대한 처리량으로 잘 알려져있습니다. 
빠른 작성의 기능은 데이터 처리 프로세스에서의 기능입니다. 처음으로 밟게된 스텝은 
커밋 로그에 작성하기입니다. 이것은 내구성을 위한것인데, 데미지나 노드 기능다운과 같은 상황에 
데이터를 보존하기 위함입니다. 커밋로그에 작성하는 것은 이 툴을 사용함에 있어서 빠르고
효율적인 프로세스입니다.

그 다음 스텝은 "Memtable"에 작성하기 아니면 메모리에 작성하기 입니다.
멤테이블에 작성을 하고 나면, 노드는 성공적인 데이터 작성에 대해 ACK합니다. 
멤테이블은 데이터베이스 메모리에서 찾을 수 있으며, 인메모리에 작성하는 것은 
디스크에 작성하는 것보다 훨씬 빠릅니다. 이 모든 설명이 카산드라의 빠른 write에 대해
충분한 설명이 되었을겁니다.


### 9. P2P 아키텍쳐 (Peer-to-peer architecture)

카산드라는 p2p 모든 노드들이 평등한 아키텍쳐 모델에 만들어졌습니다.
이는 어떤 "슬레이브 마스터" 관계 모델의 데이터베이스들과는 사뭇 다릅니다.
이것(마스터-슬레이브)은 하나의 유닛이 다른 유닛들의 기능들과 직접 연결되어 있다는 것인데,
마스터 유닛을 제외한 나머지 유닛들은 오직 중앙유닛(또는 마스터라고 불리우는 녀석)과 통신합니다.
카산드라에서는, 서로다른 유닛은 gossiping이라는 프로세스의 과정 중에 서로 피어로써 통신할 수 있습니다.
이 p2p 통신은 단 한 건의 실패쿼리 일지라도 (용납 하지않고) 제거하게 되며, 카산드라의
유일무이 중요한 기능입니다.

---
## 카산드라의 한계

이러한 모든 장점에도 불구하고, 카산드라는 몇몇의 한계점을 가집니다.

* 첫째로, 카산드라는 아파치에서 제공하는 공식문서가 없습니다.(2023월 12월 기준 [아파치 공식 문서](https://cassandra.apache.org/doc/4.1/index.html)가 있습니다.) 써드파티 툴들에 의존하는 행위들이 꼭 필요하게 됩니다. 이는 리스크를 야기합니다. 
* 카산드라는 어떠한 형태의 관계형 데이터베이스와 ACID 데이터베이스 자산에 대한 지원을 해주지 않습니다. 
* 카산드라는 쓰기 작업(operations)에서 매우 뛰어난 성능을 보여주지만, 읽기 작업은 최적화되지 않았습니다.(sub-optimal)
* 지연율 이슈는 막대한 양의 데이터와 요청을 다룰 때에 나타나는 정말 일반적인 문제입니다.
* 카산드라는 집계와 서브쿼리에 대해서도 지원을 하지 않습니다.
* 쿼리들에 기반함에 따라, 카산드라는 같은 데이터를 여러 번 저장합니다. 이는 결국 자바 메모리 모델(JVM; java virtual machine 말하는거 같습니다.)에서의 이슈를 야기합니다. 

더보기: [2021 top8 데이터 보안 최고 사례들](https://www.spiceworks.com/it-security/data-security/articles/top-big-data-security-best-practices/)

---

## Top 6 카산드라의 이용 사례 

카산드라는 오픈소스 NoSQL 데이터베이터로 많은 이점과 다른 수많은 
시스템들에 견줄 수 있는 실용적인 기능성을 가지고 있습니다.
이는 전세계의 몇몇의 크고 작은 회사들에서 사용됩니다. 

카산드라의 탑 어플리케이션(적용사례)에는 다음과 같은 사례들이 있습니다:

### 1. E-commerce 

이커머스는 모든 지역과 나라에 거쳐 영향을 미치는 극도로 민감한 영역입니다.
금융 시장의 성격이라 하면, 다운타임(하락세;피크타임의 반대성격)의 만큼 피크타임이 기대된다는
뜻입니다. 재무 운용에 있어서, 어떤 소비자도 하락세 또는 "이득이 생기려 할 때의 접근불가로 인해 홀드 할 기회 놓치기" 들을 마주하고
싶지는 않을 것 입니다. 이커머스 회사들은 이 다운타임이나 잠재적 블랙아웃을 예방할 수있습니다. 
카산드라와 같은 전적으로 의존가능한 시스템을 통해서 말이죠. 실패 견고성은 센터 자체가 손상을 입은 상태에서도,
시스템이 조금 또는 아예 장애 없이 계속 가동할 수 있도록 합니다.

카산드라의 손쉬운 확장성 덕에, 특히 피크시즌 때에 이커머스와 창고관리 또한 카산드라의
중요한 적용입니다. 마켓러쉬 때에는, 기업은 증가하는 데이터의 운용을 위해 데이터베이스의 성능을
향상시킬 필요가 있습니다. 급격한 이커머스 성장을 감당 가능하고 시스템 재시작을 하지 않는 그런 시즌에 따른 것들은
단순하게 봐도 회사에 있어서 (카산드라가) 딱 맞습니다.

이머커스 웹사이트들은 카산드라로부터 이득을 얻습니다. 방문자의 활동을 기록하고 저장하기 때문이죠.
그러고 나면 방문자의 행위를 조율하는 분석적 툴들을 허가하게 되고, 더 나아가 웹사이트에 더 머물게 합니다.


### 2. 오락 웹사이트 (Entertainment Website)

카산드라의 도움으로 영화, 게임, 음악을 위한 웹사이트들은 소비자의 행동과 선호요소들의
발자취를 밟을 수 있습니다. 데이터베이스들은 각 방문자에 대해 어떤 것이
클릭되었는지, 다운했는지, 시간을 보냈는지 등에 대해 기록합니다. 이 정보는 
분석돼서 나중에 사용자의 오락요소 추천에 사용됩니다.

이 카산드라의 적용은 개인화, 추천, 그리고 사용자경험 유스케이스에 해당합니다.
그저 오락 사이트들에 국한되는 것이 아니라, 온라인 쇼핑 플랫폼들과 소셜미디어 추천에도 해당합니다.
이것은 왜 유저들이 시간을 써서 브라우징 하는 상품들과 비슷한 것들의 알림을
받게 되는지에 대한 답입니다.

### 3. IoT와 엣지컴퓨팅

오늘날의 세계는 IoT에 대한 급부상을 목격중에 있습니다. 우리는 꾸준히 
수천에 달하는 새 정보들이나 데이터셋에 휘말리고 있죠. 모든 웨어러블 기기, 웨어러블
센서, 교통센서, 아니면 모바일와 같은 것들은 계속 날씨, 교통, 에너지사용, 토양 상태 등에
대한 정보들을 추적하고 전송합니다. 이 정보의 홍수는 과열, 유실되기 쉽습니다.

그러나, IoT장치들로부터 정보를 저장하고 분석하는것은, 그 크기가 얼마나 클지라도,
카산드라의 기술을 이용한다면 훨씬 효율적으로 이루어집니다. 이는 다음의 이유들에 인해 그렇습니다: (본문: 꼭 이것에 국한되는 것은 아닙니다)

* 카산드라는 각 노드에게 읽기와 쓰기 작업의 수행을 허용합니다.
* 카산드라는 막대한 양의 데이터를 다루고 저장 가능합니다.
* 카산드라는 실시간 데이터 분석을 지원합니다.

### 4. 인증과 이상거래탐지 (Authentication and Fraud detection)

이상거래탐지는 수많은 기업, 수많은 은행, 보험사, 다른 금융기업들의 신뢰성(reliability)과 보안에 있어서 가장 핵심이 되는 부분입니다.
모든 부분에서, 이 기업들은 악의적집단(fraudesters)들에 의한 새롭고 개편된 데이터를 훔치는 방법들을 모두
타파 할 수 있다고 안심시켜야 합니다. 금융기업들은 악의적집단이나 해커들을 그들의
시스템으로부터 쫓아내는 것에 초점을 두고 있습니다. 카산드라가 바로 이곳에서 
연속적이고, 실시간의, 빅데이터 분석 등과 같은 강점을 적용하기 좋습니다.
카산드라는 광범위한 인터넷 활동에 대한 데이터의 소스(원천)가 되어 패턴과 비정상적 요소가
감지 될 때 fraud(기만적행위자)보다 앞서서도 알람을 발생시킬 수 있습니다.

다른 한편으로는, 이 같은 금융 기관들은 매끄러운 신원 인증 절차를 수행해야 할 의무를 지닙니다.
유저 로그인 인증 절차를 consumer(고객)들이 쉽게 이용하면서도 진짜 고객만 허용하기 위해,
카산드라가 실시간 분석을 할 수 있도록 도와줍니다. 이 분석들 뿐만 아니라,
카산드라가 있으면 끊임없는 접근성도 확신 할 수 있기 때문에, 꼭 필요합니다.

일반적으로, 카산드라는 데이터베이스들 중에서 이상거래를 감지하고 인증 절차를
수행하는 데에 있어서 가장 탑 옵션입니다. 왜냐하면 다음 이유들 때문입니다:

* 인공지능을 이용한 실시간 분석을 가능하게 합니다.
* 활발히 성장하고 있는 어마어마한 양의 데이터를 호스트 할 수 있습니다.
* 서로 다른 타입의 데이터들을 프로세싱 할 수 있는 유연한 스키마를 가지고 있습니다.

### 5. 메시징

현재 사용되고 있는 몇몇의 메시징 어플리케이션들이 있고, 끊임없이 성장하는 몇
개인들이 이들을 사용하고 있죠. 이는 안정적인 데이터베이스 시스템에 대한 수요를
만들어냅니다. 끊임없이 흐르는 정보 볼륨들을 저장하기 위해서입니다. 카산드라는
메시징 서비스를 제공하는 기업들을 위해 저장용량과 안정성을 제공합니다.

### 6. 물류 및 자산관리 (Logistics and asset management)

카산드라는 물류와 자산관리에서 운송 될 모든 아이템의 물류추적을 위해 사용됩니다.
구매부터 최종 배달까지, 어플리케이션은 각 처리를 로그로 남기기 위해 카산드라를
이용 할 수 있습니다. 이것은 특히 정기적으로 막대한 양의 물류를 처리하는 거대한 물류 회사들
에게 적용될 수 있습니다. 카산드라는 그러한 어플리케이션의 백엔드 개발에서 
건장한 유스케이스들을 찾았었습니다. [어플리케이션 퍼포먼스](https://www.spiceworks.com/it-security/application-security/articles/is-application-performance-monitoring-key-to-protecting-critical-infrastructure-against-cyberattacks/)에 영향을 
주지 않으면서도 데이터를 저장하고 분석합니다.

---

## Takeaway (글쓴이 강조내용)

카산드라의 강력한 기능과 분산된 아키텍쳐는 개인 개발자와 거대 기업들에게 
가장 선호하는 데이터베이스 관리 시스템으로 만들었습니다. 가장 큰 기업들 중
고속 정보 릴레이를 필요로 하는 몇  이들은 카산드라에 의존하는데, 여기에는 Netflix 
같은 미디어 플랫폼 뿐만 아니라 Facebook이나 Twitter(현 X)같은 기업들도 포함됩니다.

더 나아가, 카산드라는 첫 오픈소스화가 진행된 2008년부터 지속적인 업데이트를
해왔습니다. Apache Cassandra version 4.1은 2022년 7월에 릴리즈 예정이
잡혀있으며, 추가비용 없는 신기술과 지속적인 전문 기술 지원을 확신해 볼
수 있을 것 같습니다.