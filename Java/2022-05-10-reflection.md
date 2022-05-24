## 1. Reflection이란?

<aside>
💡 **구체적인 클래스 타입을 알지 못해도**, 그 클래스의 메소드, 타입, 변수들에 접근할 수 있도록 해주는 자바 API

</aside>

- 컴파일 시간 (Compile Time)이 아니라 **실행 시간(Run Time)**에 동적으로 특정 클래스의 정보를 객체화를 통해 분석 및 추출해낼 수 있는 프로그래밍 기법이다.
- 자바에서 이미 로딩이 완료된 클래스에서 (JVM위에 올라감) 또 다른 클래스를 동적으로 로딩(Dynamic Loading)하여 생성자(Constructor), 멤버 필드(Member Variables) 그리고 멤버 메서드(Member Method) 등을 사용할 수 있는 기법

### “구체적인 클래스 타입을 알지 못해도”

⇒ 구체적인 클래스 타입을 알지 못해도, 클래스 정보에 접근이 가능하다는 것이 무슨 뜻인지 알아보자.

### Example

```java
public class Customer {
    private String name;
    private int number;

    public Customer() {
    }

    public Customer(String name, int number) {
        this.name = name;
        this.number = number;
    }

    public void printIndex() {
        System.out.println("Reflection");
    }

    private void printHello() {
        System.out.println("hello world");
    }
}
```

```java
public static void main(String[] args) {
    Object obj = new Customer("CHOI", 1);
    obj.printIndex(); // 컴파일 에러
}
```

- 자바는 컴파일러를 사용한다. 즉 컴파일 타임에 타입이 결정된다.
- obj라는 이름의 객체는 컴파일 타임에 Object로 타입이 결정됐기 때문에 Object 클래스의 인스턴스 변수와 메서드만 사용할 수 있다.
- 생성된 obj라는 객체는 **Object 클래스라는 타입**만 알 뿐, Customer 클래스라는 **구체적인 타입을 모른다**.

> 결국 컴파일러가 있는 자바는 구체적인 클래스를 모르면 해당 클래스의 정보에 접근할 수 없다는 것을 알 수 있다.
⇒  Reflection **구체적인 클래스 타입을 알지 못해도 클래스 정보에 접근**할 수 있게 해준다.
>

### Relection 사용해서 접근

```java
public static void main(String[] args) throws Exception {
    Object obj = new Customer("CHOI", 1);
    Class customerClass = Customer.class;
//        Class<?> customerClass = Class.forName("devcourse.reflection.Customer");
    Method printIndex = customerClass.getMethod("printIndex");
    printIndex.invoke(obj); // invoke(메서드를 실행시킬 객체)

    // Reflection 출력

}
```

- obj라는 객체는 Object 클래스라는 타입이지만 Customer 클래스의 printIndex 메서드에 접근하고 실행시킬 수 있다.

## 2. Reflection의 클래스 정보 접근

### 클래스에 선언된 생성자, 메서드, 필드

```java
public static void main(String[] args) throws Exception {

    Class<?> customerClass = Class.forName("devcourse.reflection.Customer");

    // 클래스 내 선언된 메서드의 목록 출력
    Constructor<?>[] constructors = customerClass.getConstructors();
    for (Constructor<?> constructor : constructors) {
        System.out.println(constructor.toString());

    }
    /*
    public devcourse.reflection.Customer()
    public devcourse.reflection.Customer(java.lang.String,int)
    */
    System.out.println();

    // 클래스 내 선언된 메서드의 목록 출력
    Method[] methods = customerClass.getDeclaredMethods();
    for (Method method : methods) {
        System.out.println(method.toString());
    }
    /*
    public void devcourse.reflection.Customer.printIndex()
    private void devcourse.reflection.Customer.printHello()
     */
    System.out.println();

    // 클래스 내 선언된 필드의 목록 출력
    Field[] fields = customerClass.getDeclaredFields();
    for (Field field : fields) {
        System.out.println(field.toString());
    }
    /*
    private java.lang.String devcourse.reflection.Customer.name
    private int devcourse.reflection.Customer.number
     */
}
```

> **getMethods VS getDeclaredMethods**
getMethods
public 접근지정자 메서드만 접근 가능하다.
getDeclaredMethods
접근지정자와 상관없이 class 안의 모든 메서드에 접근 가능하다.
>

### Reflection을 이용해서 Private 필드, 메서드 접근

```java
// private field, method 접근
public static void main(String[] args) throws Exception {
    Class<?> myClass = Class.forName("devcourse.reflection.Customer");

    Constructor<?> constructor = myClass.getConstructor(); // 기본 생성자 사용
    Customer customer = (Customer) constructor.newInstance();

    Field field = myClass.getDeclaredField("name");
    field.setAccessible(true);
    field.set(customer, "CHOI");
    System.out.println(field.get(customer));
    // CHOI 출력

    Method printHello = myClass.getDeclaredMethod("printHello");
    printHello.setAccessible(true);
    printHello.invoke(customer);
    // hello world 출력
}
```

- 기본 생성자만 있다면 setter, getter 없어도 심지어 private 변수와 메서드에도 접근가능하다.

## 3. Reflection 원리

자바에서는 JVM이 실행되면 사용자가 작성한 자바 코드가 컴파일러를 거쳐 바이트 코드로 변환되어 static 영역에 저장된다.

Reflection API는 이 정보를 활용한다.

그래서 클래스 이름만 알고 있다면 언제든 static 영역을 뒤져서 정보를 가져올 수 있다.

## 4. Reflection의 단점

- 성능 오버헤드
    - 컴파일 타임이 아닌 런타임에 동적으로 타입을 분석하고 정보를 가져오므로 JVM을 최적화 할 수 없다.
- 추상화
    - 직접 접근할 수 없는 private 인스턴스 변수, 메서드에 접근하기 때문에 내부를 노출하면서 추상화가 깨진다.

> 애플리케이션 개발보다는 프레임워크나 라이브러리에서 많이 사용된다.
어떤 클래스를 만들지 예측할 수 없기 때문에 동적으로 해결해주기 위해 Reflection을 사용한다.
>

## 5. Reflection의 활용

### 1. BeanFactory (Spring Container)

- Bean은 애플리케이션이 실행한 후 런타임에 객체가 호출될 때 동적으로 객체의 인스턴스를 생성하는데 이때 BeanFactory에서 Reflection을 사용한다.

### 2. Spring JPA

- “JPA의 Entity 객체에는 기본 생성자가 있어야 한다.”
- JPA에서 Entity에 대한 객체를 생성하기 위해서 Java Reflection을 사용한다.

### 3. @Requestbody

- @RequestBody을 사용하면 ObjectMapper가 JSON Field와 Java Field (Dto)를 매칭 시킨다.
- 이때 Reflection을 사용해서 값을 주입해준다.

⇒ Dto의 setter는 필요없다, Dto에 기본 생성자가 필요하다.

> Reflection API로 가져올 수 없는 정보 중 하나가 생성자의 인자(Paramter 정보)
그래서 기본 생성자가 반드시 있어야 객체를 생성할 수 있다.
기본 생성자로 객체를 생성하고 필드 값 등은 Reflection으로 넣어준다.
>

## 예상 질문

1. Reflection이 무엇인가?
- 런타임에서 클래스 및 클래스의 생성자, 메서드, 필드를 조작하고 검사하는 역할을 맞는다. (조작 검사 시에 클래스의 구체적 타입을 알지 못한다)
1. Reflection의 활용