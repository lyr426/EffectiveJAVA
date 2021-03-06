
### item1. 생성자 대신 정적 팩터리 메서드를 고려하라 
---
#### __정적 팩터리 메서드를 사용했을 때의 장점__
1. 이름을 가질 수 있다. 
    - 생성자 자체로는 반환될 객체의 특성을 제대로 설명하기가 어렵다.
    - 한 클래스에 같은 시그니처가 같은 생성자가 여러 개 필요하다면 정적팩터리 메서드로 각각의 차이를 표현할 수 있다. 

2. 호출될 때마다 인스터스를 새로 생성하지는 않아도 된다. 
    - 불변 클래스는 인스턴스를 미리 만들어 놓거나 불필요한 객체 생성을 피할 수 있다. 
    - 생성 비용이 큰 같은 객체가 자주 요청되는 상황에 성능을 끌어올릴 수 있다. 
    - 인스턴스를 통제할 수 있다. => 클래스를 싱글턴/인스턴스화 불가로 만들 수 있음 

3. 반환 타입의 하위 타입 객체를 반환할 수 있는 능력이 있다. 
    - 반환할 객체의 클래스를 자유롭게 선택할 수 있는 유연성을 가진다. 
    - API를 만들 때 구현 클래스를 공개하지 않고도 그 객체를 반환할 수 있어 API를 작게 유지할 수 있다. 
    - 그 예시로 java.util.Collections가 있으며 이 클래스는 45개의 클래스를 공개하지 않아 API외견을 훨씬 작게 만들었으며 쉽게 사용이 가능하도록 하였다. 
    
4. 입력 매개변수에 따라 매번 다른 클래스의 객체를 반환할 수도 있다. 
    - 반환타입의 하위타입이기만 하면 어떤 클래스의 객체를 반환하든 상관없다. 
    - 클라이언트는 입력 매개변수에 따른 다른 두 클래스의 존재를 알 필요없이 하위 클래스이기만 하면 된다. => 다음 릴리스때에 성능을 개선하여 클래스를 추가하거나 삭제할 수 있다.

5. 정적 팩터리 메서드를 작성하는 시점에는 반환할 객체의 클래스가 존재하지 않아도 된다. 
    - 이러한 유연성으로 서비스 제공자 프레임워크를 만드는 근간이 된다. 
    -  대표적인 예로 JDBC가 있다.
    -  서비스 제공자 프레임워크에서의 제공자는 서비스의 구현체이다.
    -  이 구현체들은 클라이언트에 제공하는 역할을 프레임워크가 통제하여, 클라이언트를 구현체로부터 분리해준다.
    - |서비스 제공자 프레임워크 핵심 컴포넌트|
      |------|
      |서비스 인터페이스|
      |제공자 등록 API|
      |서비스 접근 API|
    - 클라이언트는 서비스 접근 API를 사용할 때 원하는 구현체의 조건을 명시할 수 있다. => 유연한 정적 팩터리의 실체 
    - 종종 서비스 제공자 인터페이스라는 네 번째 컴포넌트가 쓰이기도 함 -> 서비스 인터페이스의 인스턴스를 생성하는 팩터리 객체를 설명 



#### __정적 팩터리 메서드를 사용했을 때의 단점__
1. 상속을 하려면 public이나 protected 생성자가 필요하니 정적 팩터리 메서드만 제공하면 하위 클래스를 만들 수 없다. 
    - 앞선 java.util.Collections의 구현 클래스들은 상속할 수 없다.
    - 이 제약은 상속보다 컴포지션 사용을 유도(item18)하고 불변타입(item17)으로 만들려면 이 제약을 지켜야 한다는 점에서 오히려 장점이 될 수도 있다. 
2. 정적 팩터리 메서드는 프로그래머가 찾기 어렵다. 
    - 생성자 처럼 API 설명에 명확히 드러나지 않는다. 
    - API 문서를 잘 써놓고 메서드 이름도 널리 알려진 규약을 따라 짓는 식으로 문제를 완화 시켜야 함
