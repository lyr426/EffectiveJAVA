### item3. private 생성자나 열거 타입으로 싱글턴임을 보증하라. 
---

__싱글턴이란?__</br>
: 인스턴스를 오직 하나만 생성할 수 있는 클래스를 말함. 전형적인 예로 함수와 같은 무상태 객체나 설계상 유일해야 하는 시스템 컴포넌트를 들 수 있음 

>클래스를 싱글턴으로 만들면 이를 사용하는 클라이언트를 테스트하기 어려워질 수 있음.
<br>


#### public static final 필드 방식의 싱글턴 

```java
public class Example{
  public static final Example INSTANCE = new Example();
  private Example(){...}
  
  public void leaveTheBuilding(){...}
}
```

- private 생성자는 Example.INSTANCE를 초기화할 때 딱 한 번 호출되며, public이나 protected 생성자가 없으므로 Example 클래스가 초기화될 때 만들어진 인스턴스가 전체 시스템에서 하나뿐 임을 보장한다. 
- public 필드 방식의 장점은 해당 클래스가 싱글텀임이 API에 명백히 드러난다. => public static 필드가 final이기 때문에 절대로 다른 객체를 참조할 수 없다. 
- 간결하게 코드를 작성할 수 있다. 

#### 정적 팩터리 방식의 싱글턴 
```java
public class Example{
  public static final Example INSTANCE = new Example();
  private Example(){...}
  public static Example getInstance() {return INSTANCE;}
  
  public void leaveTheBuilding(){...}
}
```

- Example.getInstance는 항상 같은 객체의 참조를 반환하므로 인스턴스가 하나 뿐이다.
- 정적팩터리 방식의 장점은 API를 바꾸지 않아도 싱글턴이 아니게 변결할 수 있다. => 유일한 인스턴스를 반환하던 팩터리 메서드가 호출하는 스레드 별로 다른 인스턴스를 넘겨주게 할 수 있다. 
- 정적팩터리를 제네릭 싱글턴 팩터리를 만들 수 있다.
- 정적팩터리의 메서드 참조를 공급자로 사용할 수 있다. 

#### 열거 타입 방식의 싱글턴 - 바람직한 방법  
```java
public class Example{
  INSTANCE; 
  public void leaveTheBuilding(){...}
}
```

-대부분의 상황에서 원소가 하나뿐인 열거 타입이 싱글턴을 만드는 가장 좋은 방법이다. 
