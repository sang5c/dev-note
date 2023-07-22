### Composite 패턴

- 구조적 디자인 패턴
- “전체”와 “부분”이 같은 방식으로 작동하도록 하는 디자인 패턴. 객체들 간의 계층 구조를 구성하는 데 사용된다.
    - 클라이언트가 단일/복합 객체를 같은 방식으로 취급할 수 있게 해 준다.
    - 그릇과 내용물을 동일시한다 (Java 언어로 배우는 디자인 패턴 입문)
- 세 가지 요소로 구성된다.
    - Component: 모든 객체가 공유하는 인터페이스. 공통 기능(Operation)을 정의한다.
    - Leaf: Component를 구현하는 가장 기본적인 단일 객체. 자식을 가질 수 없다. (Operation 구현)
    - Composite: Component를 구현하는 복합 객체. Component 객체를 자식으로 가질 수 있다. 인퍼테이스에 정의된 기능을 자식에게 위임하여 계층을 구성한다. (Operation 구현, Add, Remove 등의 자식 관리 기능 제공)
- 대표적인 예시는 파일시스템이다.
    - 파일 시스템의 요소→Component, 파일→Leaf, 폴더→Composite
    - 폴더는 파일 또는 파일을 갖는다. 클라이언트는 파일과 폴더를 파일 시스템의 요소로 동일하게 취급한다.

### State 패턴

- “상태”를 개별 클래스로 나타내는 패턴. 각 상태 클래스가 각자의 역할을 갖는다.
- 구성
    - State: 공통 인터페이스
    - Concrete State: 상태를 표현하는 구체 클래스
    - Context: 현 상태를 유지하며 상태에 대한 접점을 제공하는 클래스
- 예: 토글 버튼사용
    - State: 버튼, Concrete State: 버튼 활성화 / 버튼 비활성화, Context: 버튼을 조작하는 클래스
    - “버튼 비활성화” 상태에서는 버튼이 눌렸을 때 기능 활성화가 작동한다.
    - “버튼 활성화” 상태에서는 버튼이 눌렸을 때 기능 비활성화가 작동한다.
- Context에서의 if-else 대신 클래스가 각자의 책임(SRP)을 갖는 장점이 있다. 상태가 추가되어도 기존 기능에 영향을 미치지 않으며(OCP), 추가된 상태가 구현해야 하는 인터페이스를 구현하지 않으면 컴파일 에러로 알 수 있다.
    - “변경 중” 상태가 추가되어도 활성화/비활성화 버튼이 수정되지 않는다.

### 옵저버 패턴 (Observer Pattern)

- 객체의 상태 변경될때 관찰자 모두에게 상태 변화를 알리는 패턴.
- 구성
    - Observer: 관찰자들은 해당 인터페이스를 구현해야 한다.
    - Concrete Observer: 관찰자. 옵저버 인터페이스 구현체.
    - Subject: 옵저버 관리(저장/추가/삭제) 및 알림 보내는 클래스
    - Concrete Subject: Subject를 확장하는 옵저버의 관심 대상 클래스. 상태 변경시 옵저버들에게 알림을 보내는 메서드를 호출한다.
- 객체 간 의존성을 최소화하고 상태 변화에 대해 핸들링이 가능해진다. 직접 상태 변화를 지켜보는게 아니라 알림을 받는 관찰자 라는 느낌으로 이루진다.
- 스프링에서는 EventListener 어노테이션을 가진 메서드가 옵저버 패턴으로 알림을 받는다.
    - Subject: AbstractApplicationEventMulticaster
    - Concrete subjecct: SimpleApplciationEventMulticaster
    - Observer: @EventListener 어노테이션
 
### 어댑터 패턴 (Adapter Pattern)

- 인터페이스가 호환되지 않는 클래스들을 함께 사용할 수 있도록 만들어주는 디자인 패턴
- 구성
    - Target: 클라이언트가 사용하고자 하는 인터페이스
    - Adapter: Target 인터페이스를 구현하고 Adaptee의 메서드를 호출하는 클래스. 상속과 조합으로 구현할 수 있다.
    - Adaptee: 클라이언트와 호환되지않는, 원하는 기능을 구현한 클래스.
    - 클라이언트는 Target 인터페이스 사용을 통해 Adaptee를 사용한다.
- 예) 220V 전원 →  맥북에 라이트닝 포트 연결해서 전원 공급
    - Client: 맥북
    - Target: 라이트닝 포트
    - Adapter: 220V ↔ 라이트닝 전환
    - Adaptee: 220V
    - Concrete observer: @EventListener 어노테이션이 달린 메서드
