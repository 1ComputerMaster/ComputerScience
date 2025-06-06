# 1. 문제 및 해답

## 문제 1: 관계형 모델에서는 튜플(tuple)과 속성(attribute)의 개념이 핵심이다. 다음 중 관계형 모델의 특징으로 옳은 것을 고르시오.

1. 테이블의 행(row)은 순서를 보장한다.

2. 기본키(primary key)는 튜플을 고유하게 식별한다.

3. NULL 값은 기본적으로 허용되지 않는다.

4. 스키마 변경은 런타임 시 자유롭게 이루어진다.

### 해답: 2) 기본키는 튜플을 고유하게 식별한다. 

(1번) 행은 순서를 보장하지 않으며, (3번) NULL 허용 여부는 제약에 따라 달라진다, (4번) 스키마 변경은 일반적으로 비용이 크다.

## 문제 2: 문서형 모델(MongoDB 등)에서 JSON 문서를 사용해 데이터 구조를 표현할 때 이점이 아닌 것은?

1. 스키마 유연성 증가

2. 조인이 필요 없는 데이터 중첩(nesting)

3. 분산 트랜잭션 지원 강화

4. 버전 간 호환성 관리 비용 절감

### 해답: 3) 분산 트랜잭션 지원 강화 – 문서 DB는 일관성 수준이 낮을 수 있으며, 복잡한 분산 트랜잭션을 강력히 지원하지 않는다. 

## 문제 3: Cypher나 Gremlin 같은 그래프 질의 언어가 SQL과 다른 주된 특징은 무엇인가?

1. 선언형 언어라는 점

2. 경로 기반의 패턴 매칭 지원

3. 테이블 간 조인의 자동 최적화

4. 완전한 ACID 트랜잭션 보장

### 해답: 2) 경로 기반의 패턴 매칭 – 그래프 DB는 노드와 간선의 경로 패턴을 직관적으로 탐색할 수 있는 문법을 제공한다. 

## 문제 4: 다음 중 “선언형 언어(declarative language)”의 특징으로 옳은 것은?

1. 변수 할당과 루프 구조가 필수적이다.

2. 어떻게 수행할지보다 무엇을 원하는지를 기술한다.

3. 실행 계획을 개발자가 직접 작성해야 한다.

4. 상태 변화를 단계별로 기술해야 한다.

### 해답: 2) 선언형 언어는 “무엇을 원하는가”에 집중하고 실행 계획(어떻게)은 런타임에 결정된다. 

## 문제 5: 다음 중 데이터 모델과 질의 언어의 관계 설명으로 옳은 것을 고르시오.

1. 데이터 모델이 먼저 결정되고, 그에 맞춰 질의 언어가 정의된다.

2. 질의 언어가 먼저 설계된 후 데이터 모델이 부수적으로 등장한다.

3. 데이터 모델과 질의 언어는 상호 독립적으로 발전하여 아무런 영향이 없다.

4. 질의 언어는 특정 데이터 모델의 추상화된 표현을 기반으로 만들어진다.

### 해답: 4) 질의 언어는 데이터 모델(관계형·문서·그래프)의 추상화된 개념(테이블·문서·노드·간선)을 바탕으로 설계된다. 

