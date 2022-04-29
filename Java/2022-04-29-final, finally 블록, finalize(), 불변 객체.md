## 1. Java에서의 final

- final : 최종적이라는 뜻
- 한 번만 할당 가능하다
    
    → 해당 선언이 최종 상태이고, 결코 수정될 수 없음을 뜻함
    
    → 즉, 재할당하려고 하면 컴파일 오류가 발생하여 바로 확인이 가능하다.
    
- final 키워드는 클래스, 필드, 메소드 선언 시에 사용할 수 있다.
    
    → 어느 선언에 사용되느냐에 따라서 의미가 조금씩 달라진다.
    
<br>

## 2. final을 사용하는 이유?

- 값에 대한 검증이 필요하지 않다 → 로직 구현에 집중
- 서비스 안정성이 높아짐
    - 버그 발생 가능성이 줄어든다.
    - 버그를 찾는 시점이 빨라짐 (컴파일 타임에 확인 가능)
    - 코드 품질이 높아져 변화에 좀 더 빠르게 대응할 수 있다.
    
<br>

## 3. final 필드와 상수

- final 필드 = 최종적인 필드
- final 필드는 초기값이 저장되면 해당 값이 최종적인 값이 되어서 프로그램 실행 도중에 수정할 수 없다는 것

```java
final 타입 필드 [= 초기값];
```
    
<br>

### (1) final 필드의 초기값을 줄 수 있는 방법

final 필드의 초기값을 줄 수 있는 방법은 딱 두 가지 뿐이다.

- 필드 선언시에 주는 방법 : 단순 값인 경우

```java
final String name = "Ella";
```

- 생성자에서 주는 방법
    - 복잡한 초기화 코드가 필요하거나 객체 생성시에 외부 데이터로 초기화 해야 하는 경우
    - 만약에 생성자에서 모든 final 필드를 초기화 하지 않으면 컴파일 에러가 발생함

```java
public class Person{
	final String name;

	public Person(String name){
		this.name = name;
	}
}
```
    
<br>

## 4. final 클래스

- class 앞에 final 키워드를 붙이게 되면, 이 클래스는 최종적인 클래스이므로 상속할 수 없는 클래스가 된다.
    
    → 즉, final 클래스를 부모로 갖는 자식 클래스는 만들 수 없다.
    

```java
public final class 클래스 { ... }
```

- Java 표준 API에서 제공하는 String 클래스가 대표적인 final 클래스이다.
    
    → 따라서, String 클래스를 상속하는 자식 클래스를 만들 수 없다.
        
<br>


## 5. final 메소드

- 메소드를 선언할 때, final 키워드를 붙이게 되면, 이 메소드는 최종적인 메소드이므로 오버라이딩을 할 수 없는 메소드가 된다.
    
    → 즉, 부모 클래스를 상속해서 자식 클래스를 선언할 때, 부모 클래스에 선언된 final 메소드는 자식 클래스에서 재정의 할 수 없다
    

```java
//부모 클래스
public class Car{
	public int speed;
	
	public void speedUp() { speed += 1; }

	public final void stop(){
		System.out.println("차를 멈춤");
		speed = 0;
	}
}
```

```java
//자식 클래스
public class SportsCar extends Car{
	@Override
	public void speedUp(){ speed += 10;}

	// stop()은 오버라이딩 불가능
}
```
    
<br>

## 6. finally 블록

finally 블록은 try-catch-finally 블록의 finally 블록을 말한다.

즉, 예외 발생 여부와 상관없이 항상 실행할 내용이 있을 경우에 finally 블록을 사용한다.

finally 블록은 다음과 같은 경우에 실행되지 않는다.

- try/catch 블록 수행 중에 가상 머신이 종료됨
- try/catch를 수행하고 있던 스레드가 죽어버림

- **자바의 finally 블록은 try-catch-finally의 try블록 안에 return문을 넣어도 실행되는가?**
    - Answer
        
        실행된다. finally 블록은 try 블록이 종료되는 순간 실행된다. try 블록에서 벗어나려고 해도(return, continue, break 혹은 exception을 통해서), finally 블록은 실행될 것이다.
        
    
<br>

## 7. finalize()

참조하지 않는 객체는 GC가 힙 영역에서 자동적으로 소멸시키는데, GC는 객체를 소멸하기 직전에 마지막으로 객체의 소멸자인 finalize()를 실행시킨다.

finalize() 메소드는 Object 클래스에 존재하며, 기본적으로 실행 내용이 없다.

만약 객체가 소멸되기 전에 특정한 일을 수행하게 만들고 싶다면, Object의 finalize()를 재정의하면 된다.

```java
public class Counter{
	private int no;

	public Counter(int no){
		this.no = no;
	}

	@Overrride
	protected void finalize() throws Throwable{
		System.out.println(no + "번 객체의 finalize()가 실행됨");
	}
}
```

GC가 실행시키는 메소드이므로 finalize() 메소드가 호출되는 시점을 명확하지 않다.

→ 프로그램이 종료될 때, 즉시 자원을 해제하거나 즉시 데이터를 최종 저장해야 한다면, 일반 메소드에서 작성하고 프로그램이 종료될 때 명시적으로 메소드를 호출하는 것이 좋다.

명시적으로 finalize()를 호출하는 2가지 메소드가 있다.

- System.gc()
    - GC 수행을 명령하는 메소드
    - GC가 발생하면, 소멸의 대사이 되는 인스턴스는 결정되지만, 이것이 실제 소멸로 바로 이어지지 않고, 인스턴스의 실제 소멸로 이어지지 않는 상태에서 프로그램이 종료될 수도 있다.
        
        → 종료가 되면 어차피 인스턴스는 소멸되기 때문
        
- System.runFinalization()
    - GC에 의해서 소멸이 결정된 인스턴스를 즉시 소멸시키는 메소드
    
<br>

## 8. 불변 객체란?

- 한 번 생성되면 상태를 수정할 수 없는 객체
    
    → 생성이 된 불변 객체는 신뢰할 수 있다.
    

- 가변 객체로 사용하는 경우, 멀티쓰레드에 의해 로직에는 문제가 없어도 값이 예상과 다르게 나올 수 있다.
    
    → 스레드 동기화 문제
    
    → 불변 객체를 통해 스레드 동기화 문제 방지 가능
    
- 불변 객체는 가변 객체보다 설계하고 구현하고 사용하기 쉬우며, 오류가 생길 여지도 적고 훨씬 안전함

- 불변 객체를 사용하는 경우 객체가 가지는 값마다 새로운 객체가 필요하다. 따라서 메모리 누수와 새로운 객체를 계속 생성해야하기 때문에 성능 저하를 발생시킬 수 있다.
    
<br>

## 9. 불변객체 만들기

불변 객체를 만드는 기본적인 아이디어는 필드에 final을 사용하고, Setter를 구현하지 않는 것이지만,  불변 객체의 필드 중 참조 타입이 있는 경우엔 추가적인 작업이 필요하다.
    
<br>

### (1) 원시 타입만 있는 불변 객체

final 키워드를 사용한다.

final 필드만 있기 때문에, 당연히 setter는 구현할 수 없다.

```java
public class ExampleObject{
	private final int value;

	public ExampleObject(final int value){
		this.value = value;
	}
}
```
    
<br>

### (2) 참조 타입이 있는 불변 객체

참조 타입이 있는 경우에는 단순히 final 키워드를 사용하고, Setter를 작성하지 않는 것으로는 불변 객체를 만들 수 없다.

불변 객체의 참조 변수 또한 불변이어야 한다. 참조 변수가 일반 객체인 경우는 참조 변수도 불변 객체로 만들면 되지만, Java에서 제공하는 객체 타입을 사용하는 경우는 어떻게 해야할까?

- 참조 변수가 일반 객체인 경우
    
    ```java
    public class Animal {
        private final Age age;
    
        public Animal(final Age age) {
            this.age = age;
        }
    }
    
    //참조 변수도 불변 객체로 만들기
    class Age {
        private final int value;
    
        public Age(final int value) {
            this.value = value;
        }
    }
    ```
    
- Java가 제공하는 Array를 사용할 경우
    
    ```java
    public class ArrayObject {
    
        private final int[] array;
    
    		//전달받는 array를 그대로 array에 할당하는 것이 아닌 복사본을 할당한다
    		//-> 외부에서 변경될 가능성이 있기 때문
        public ArrayObject(final int[] array) {
            this.array = Arrays.copyOf(array,array.length);
        }
    
    		//array field를 그대로 리턴하는 것이 아닌 복사본을 리턴한다
    		//-> 외부에서 변경될 가능성이 있기 때문
        public int[] getArray() {
            return (array == null) ? null : array.clone();
        }
    }
    ```
