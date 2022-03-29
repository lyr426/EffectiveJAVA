### item2. 생성자에 매개변수가 많다면 빌더를 고려하라 
----

__클래스의 매개변수가 많아지면 생성자 혹은 정적 팩터리는 어떤 모습일까??__ <br>
- 대부분 이러한 경우에는 점층적 생성자 패턴(telescoping constructor pattern)을 즐겨 사용한다. <br>

#### 점층적 생성자 패턴
```java
public class Example{
  private final int a;
  private final int b;
  private final int c;
  private final int d;
 
  public Example(int a, int b){
    this(a,b,0);
  }
  public Example(int a, int b, int c){
    this(a,b,c,0);
  }
  public Example(int a, int b, int c, int d){
    this.a = a;
    this.b = b;
    this.c = c;
    this.d = d;
  }
}

```
 - 이러한 경우 사용자가 설정하길 원치 않는 매개변수까지 포함하기 쉽다. -> 어쩔 수 없이 그런 매개변수에도 값을 지정해줘야 한다. 
 - 점층적 생성자 패턴은 매개변수 개수가 많아지면 클라이언트 코드를 작성하거나 읽기 어렵다. 

#### 자바빈즈 패턴(JavaBeans pattern)
- 매개변수가 없는 생성자로 객체를 만든 후, 세터 메서드들을 호출해 원하는 매개변수의 값을 설정하는 방식이다. <br>

```java
public class Example{
  // 매개변수들은 기본값으로 초기화 됨 
  private final int a = 1;
  private final int b = 1;
  private final int c = 0;
  private final int d = 0;
  
  public Example() { }
 
  public void setA(int val) { a = val; }
  public void setB(int val) { b = val; }
  public void setC(int val) { c = val; }
  public void setD(int val) { d = val; }
}
```
 - 점층적 생성자 패턴의 단점을 보완하였다. -> 인스턴스를 만들기 쉽고, 더 읽기 쉬운 코드가 되었다. 
 -  __자바빈즈 패턴에서는 객체 하나를 만들려면 메서드를 여러개 호출해야 하며 객체가 완전히 생성되기 전까지 일관성이 무너진 상태에 놓이게 된다.__
 -  일관성이 무너지는 문제 때문에 자바빈즈 패턴에서는 클래스를 불변으로 만들 수 없다. 
 -  이러한 단점을 완화 하려면 생성이 끝난 객체를 수동으로 얼리고 얼리기전에는 사용할 수 없도록 해야한다. -> 이 방법은 다루기 어려워 거의 사용하지 않음 

#### 빌더 패턴 (Builder pattern) 
- 클라이언트는 필요한 객체를 직접 만드는 대신, 필수 매개변수만으로 생성자를 호출해 빌더 객체를 얻는다. 
- 그 후에 빌더 객체가 제공하는 일종의 세터 메서드들로 원하는 선택 매개변수를 설정한다. 
- 매개변수가 없는 build 메서드를 호출해 필요한 객체를 얻는다. 

```java
public class Example{
  // 매개변수들은 기본값으로 초기화 됨 
  private final int a;
  private final int b;
  private final int c;
  private final int d;
  
  public static class Builder{
      private final int a;
      private final int b; //필수 매개변수 
      
      private int c = 0;
      private int d = 0; // 선택 매개 변수 -> 기본값으로 초기화 
      
      public Builder(int a, int b){
          this.a = a;
          this.b = b;
      }
      
      public Builder c(int val){ 
          c = val;
          return this; 
      }
      public Builder d(int val){ 
          d = val;
          return this; 
      }
      public Exam build(){
          return new Exam(this);
      }
  }
  
  private Exam(Builder builder) {
      a = builder.a;
      b = builder.b;
      c = builder.c;
      d = builder.d;
  }
}
```

- Exam 클래스는 불변이며, 모든 매개변수의 기본값들을 한 곳에 모아뒀다.
- 빌더의 세터 메서드들은 빌더 자신을 반환하기 때문에 연쇄적으로 호출한다. => 플루언트 API, 메서드 연쇄라 함.
- 이 클라이언트 코드는 쓰기 쉽고, 읽기 쉽다. 
- 빌더 패턴은 계층적으로 설계된 클래스와 함께 사용하기 좋다. 
