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

### 커맨드 패턴 (Command Pattern)

- 요청을 캡슐화하여 호출자와 수신자를 분리한다.
- 구성
    - Command: 명령 수행하는 인터페이스. execute()같은 작업 정의
    - Concrete Command: 명령을 구현하는 클래스. Receiver 객체를 가지며 Receiver의 메서드를 사용하여 명령 수행
    - Receiver: 실제 작업 수행자
    - Invoker: 명령을 실행하는 역할. Command 인터페이스의 execute() 메서드 호출
- 게임의 캐릭터가 움직이는 방법을 예시로 들면 다음과 같다. 움직임에는 걷기, 달리기, 날기가 있다.
    - Command: 이동을 수행하는 인터페이스.
    - Concrete Command: 걷기, 달리기, 달기
    - Receiver: 이동을 수행하는 캐릭터
    - Invoker: 캐릭터에게 이동하라고 요청하는 컨트롤러
    - 움직임이 추가되거나 변경되어도 Invoker(호출자)는 명령 수행 방식에 대해 영향을 받지 않는다.
- 자바의 Runnable 인터페이스가 대표적인 커맨드 패턴이다.

### 플라이웨이트 패턴 (Flyweight Pattern)

- 데이터 공유를 통해 객체를 효율적(메모리 사용량 최소화)으로 사용하게 해준다.
    - 한마디로 캐시를 두고 재사용한다.
- 구성
    - Flyweight: 대상 클래스. 공유 가능한 불변 객체여야 한다.
    - FlyweightFactory: 대상 클래스를 제공(관리)하는 클래스. 별도로 분리하지 않고 Flyweight 클래스 내에 정적 메서드로 만들기도 한다.
        - Factory를 통해 사용하여 객체를 얻으면 한 번 생성한 객체를 재사용함으로써 최적화가 가능하다.
- 예) 자바의 Integer.valueOf()에서 -128부터 127까지의 숫자 인스턴스를 캐시해둔다.    
    ```java
    System.out.println(Integer.valueOf(127) == Integer.valueOf(127)) // true
    System.out.println(Integer.valueOf(128) == Integer.valueOf(128)) // false
    ```

### 메멘토 패턴 (Memento Pattern)

- 객체의 상태를 저장(snapshot)해서 복원 가능하도록 하는 패턴. 메멘토는 기념품을 의미한다.
- 구성
    - Originator: 작성자, 저장하고자 하는 상태를 갖고있는 클래스
    - Memeto: 기념품, 작성자의 상태를 갖는 클래스. Originator만 변경 가능하도록 구성한다.
    - Caretaker: 관리자, Memento 목록을 갖는다. 추후 상태 복원이 필요하면 Originator에게 Memento를 제공한다.
- 예) 메멘토 패턴을 활용하면 에디터의 되돌리기(cmd + z) 기능을 구현할 수 있다.
    - Originator: 에디터. 커서 위치와 본문 내용 등을 갖는다. Memento를 받아 상태를 복구, Memento를 생성(snaphot)하는 기능을 갖는다.
    - Memento: 저장하고자 하는 상태인 커서 위치와 본문 내용 필드를 가지며 Originator에 의해 생성된다.
    - Caretaker: Memento 목록을 가지며 되돌리기 기능이 실행될때마다 Originator에 최근 Memento를 전달해서 내용을 복구한다.
- 자바나 스프링에서 실제 사용중인 예시는 찾지 못했는데 패턴의 컨셉인 스냅샷과 복구라는 개념만 사용중이지 않을까 싶다.
    - 객체를 버저닝하여 데이터베이스에 저장하는 경우, 데이터베이스를 caretaker, 버저닝하여 저장한 내용을 memento, 실제 사용중인 객체를 originator 느낌으로 바라볼 수 있을 것 같다.

### 퍼사드 패턴 (Facade Pattern)

- 간단한 인터페이스 제공을 통해 복잡한 세부 내용을 정리하여 보여주는 패턴. Facade는 프랑스어로 정면, 외관이라는 의미다.
- 클라이언트가 의존해야 하는 클래스, 메서드 호출 순서, 세부 과정을 이해하는 대신 퍼사드(또는 파사드)를 의존해 간단하게 사용하도록 돕는다.
- 구성
    - Facade: 시스템을 구성하는 여러 기능을 모아 제공하는 역할
    - 여기에 추가로 각 기능을 제공하는 클래스들이 있다. 클라이언트는 이 기능들을 Facade를 통해 사용한다.
- 하나의 요청 Controller에서 다양한 Service를 의존하는 경우 Facade를 두기도 한다. Facade가 Service1, Service2를 사용하여 서비스 역할을 명확히 하고 단계를 구분한다.
    - Controller → Facade → Service
- 초기화 → 사용 → 정리 순으로 해야하는 작업이 있는 경우 프로세스를 순차적으로 수행하는 클래스(또는 메서드)를 제공하면 클라이언트가 실수할 여지를 줄이고 코드로 순서를 명확히 표현한다.

### 메디에이터 패턴 (Mediator Pattern)

- 중재자를 통해 연관 객체들에게 변경을 전파하여 객체간 결합도를 낮추는 패턴
- 구성
    - Mediator: 중재자 인터페이스. 메디에이터 → 조정자, 중개자, 중재자.
    - Concrete Mediator: 중재자 구현체. 동료 객체들을 관리하는 역할을 수행한다. 멤버는 중재자에게만 보고하고 중재자만 멤버에게 지시를 내린다.
    - Colleague: 멤버(동료). 객체간 통신이 필요한 클래스 인터페이스
    - Concrete Collegue: 멤버 인터페이스 구현체. 각 멤버는 메디에이터를 갖고 자신의 상태가 변경되면 메디에이터에게 알린다.
- 각 멤버들은 서로 모른다. 메디에이터를 의존하여 요청을 전달하고, 메디에이터는 멤버들에게 변경을 전파한다. 객체가 너무 많아지면 메디에이터가 복잡해질 수 있다.
    - 열차 관제소를 상상하면 이해하는데 도움이 된다. 관제소는 열차간의 간격 조절과 각 열차의 상태 확인, 열차의 역 진입 등을 관리하는 메디에이터이다.

### 브릿지 패턴 (Bridge Pattern)

- 기능의 클래스 계층과 구현의 클래스 계층을 연결하는 디자인 패턴
- 구성
    - 여기서 등장하는 추상화는 추상 클래스와 다른 의미다.
    - Abstraction: 기능 클래스 계층의 최상위 클래스. 클라이언트 코드에 제공하는 인터페이스를 정의한다. 구현자(Implementor)를 참조하여 메서드를 호출한다.
    - RefinedAbstraction: Abstraction의 기능을 확장한다
    - Implementor: 구현자. Abstraction 역의 API를 구현하기 위한 메서드를 정한다. interface라고 이해하면 쉽다.
    - ConcreteImplementor: 구체적인 구현자. interface 구현체
- 예) WEB 알림이 필요하면 ConcreteImplementor3을 만들면 된다.
    - Implementor: 알림 API
    - ConcreteImplementor1: macOS에서 작동하는 알림 API
    - ConcreteImplementor2: Windows OS에서 작동하는 알림 API
    - Abstraction: 알림 시스템
    - RefinedAbstraction: 퇴근시간 알림 시스템
- 구성은 좀 어렵게 느껴지지만 의존성 주입을 통한 유연한 구조를 가져가는게 핵심이고, 서로 다른 두 계층(관심사)를 분리하여 각각 확장해 나갈 수 있다.
