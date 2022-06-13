## 1. 싱글톤 패턴이란?

시스템 런타임, 환경 세팅에 대한 정보 등, 인스턴스가 여러 개일때, 문제가 생길 수 있는 경우가 있다. 이 때, 인스턴스를 오직 한 개만 만들어 제공하는 클래스가 필요하다. 이 때, 싱글톤 패턴을 사용하게 된다.

싱글톤 패턴은 어떤 클래스를 애플리케이션 내에서 제한된 인스턴스 개수, 주로 하나만 존재하도록 강제하는 패턴이다. 이렇게 하나만 만들어지는 클래스의 오브젝트는 애플리케이션 내에서 전역적으로 접근이 가능하다.

따라서 단일 오브젝트만 존재해야 하고, 이를 애플리케이션의 여러 곳에서 공유하는 경우에 주로 사용한다.

- 인스턴스를 오직 한 개만 제공
- 밖에서 생성자를 만들수 없어야 하므로, private한 생성자를 생성
- Global하게 매번 같은 객체에 접근할 수 있게 한다.

## 2. 싱글톤 패턴의 구현 방법

### 1) 가장 간단한 방법

```java
public class Settings {

    private static Settings instance;

    private Settings(){}

    public static Settings getInstance(){
        if (instance == null){ //thread2
						//thread1
            instance = new Settings();
        }

        return instance;
    }
}
```

해당 방법은 멀티 쓰레드 환경에서 안전하지 않다는 단점이 있다.

thread 1이 instance가 null임을 확인하고 블록 안에 들어왔고, 아직 객체를 만들지 않았을 때, thread 2 또한 instance가 null임을 확인하고 블록 안에 들어가는 경우가 생길 수 있다.

이 때, thread1, thread2 둘 다 new Settings()를 호출해서 두 쓰레드가 가지고 있는 객체가 서로 다르게 된다.

### 2) Synchronized 키워드 사용하기

```java
public class Settings {

    private static Settings instance;

    private Settings(){}

    public static synchronized Settings getInstance(){
        if (instance == null){
            instance = new Settings();
        }

        return instance;
    }
}
```

이 방법은 thread safe하지만, synchronized를 통해 동기화를 진행하면서 성능에 불이익이 생길 수 있다. 

### 3) 이른 초기화 사용하기 (Eager Initialization)

```java
public class Settings {

		//이른 초기화
    private static final Settings INSTANCE = new Settings();

    private Settings(){}

    public static Settings getInstance(){
        return INSTANCE;
    }
}
```

해당 방법도 thread safe하다. 대신, 인스턴스를 미리 만든다는 점이 단점이 될 수 있다.

해당 생성자를 만드는데 리소스가 많이 들어가고 메모리가 많이 소비되었는데, 해당 인스턴스를 한 번도 안쓰면 메모리 낭비가 되기 때문이다.

### 4) Double Checked Locking 사용하기

```java
public class Settings {

    private static volatile Settings instance;

    private Settings(){}

    public static Settings getInstance(){
        if (instance == null){
            synchronized(Settings.class){
                if (instance == null){
                    instance = new Settings();
                }
            }
        }
        return instance;
    }
} 
```

- volatile : 해당 변수를 Main memory에 저장해서 다른 쓰레드에서 읽을 때에도 값이 같도록 함

해당 방법은 instance가 null일 때만 synchronized 블록 안에 들어가기 때문에 2번 방법보다는 더 효율적이다.

다만 복잡한게 단점이다.

### 5) static inner 클래스 사용하기 (권장하는 방법 중 하나)

```java
public class Settings {
    private Settings(){}

    private static class SettingsHolder{
        private static final Settings INSTANCE = new Settings();
    }

    public static Settings getInstance(){
        return SettingsHolder.INSTANCE;
    }
}
```

해당 방법이 가장 대표적이고 권장되는 방식이다.

해당 방법은 lazy loading이 가능하다는 장점이 있다.

## 3. Spring에서 Singleton을 사용하는 이유?

스프링은 대부분 서버환경에서 사용된다. 서버환경에는 여러가지 비즈니스 로직들이 있고 서버는 해당 비즈니스 로직을 담당하는 오브젝트들이 참여하는 계층형 구조로 이뤄진 경우가 대부분이다.

그런데 매번 클라이언트에서 요청이 올 때 마다 각 로직을 담당하는 오브젝트를 새로 만들어서 사용한다면 어떻게 될까?

ex) 요청 한 번에 5개의 오브젝트가 새로 만들어지고 500개의 요청이 1초당 들어오게 된다면 초당 2500개의 오브젝트가 생성됨

→ 서버가 감당하기 힘든 부하가 걸리게 된다.

**이러한 문제를 해결하고자 스프링은 빈을 싱글톤으로 관리하여 1개의 요청이 왔을 때 여러 쓰레드가 빈을 공유하도록 했다.**

## 4. 싱글톤 패턴의 한계

- private 생성자를 갖고 있기 때문에 상속할 수 없다.
    - private 생성자를 가진 클래스는 다른 생성자가 없다면 상속이 불가능하다.
- 싱글톤은 테스트하기가 힘들다.
    - 싱글톤은 만들어지는 방식이 제한적이기 때문에, Mock object 등으로 대체하기가 힘들고 직접 오브젝트를 만들어서 사용할 수밖에 없다.
- 서버환경에서는 싱글톤이 하나만 만들어지는 것을 보장하지 못한다.
    - 서버에서는 클래스 로더의 구성에 따라 싱글톤 클래스임에도 하나 이상의 오브젝트가 만들어질 수 있다.
- 싱글톤의 사용은 전역 상태를 만들 수 있기 때문에 바람직하지 못하다.
    - 싱글톤은 애플리케이션 어디든지 사용될 수 있기 때문에, 아무 객체나 자유롭게 접근하고 수정하고 공유할 수 있고, 이는 객체지향 프로그래밍에서는 권장되지 않는 방식이다.

### + 면접 질문

- 싱글톤 패턴을 구현해보세요 (라이브 코딩)
- 스프링에서 싱글톤 패턴을 사용하는 이유는 뭘까요?
- 스프링에서 싱글톤 패턴이 기본값인데 프로토타입 스코프를 사용하는 경우는 어떤 경우일까요?
