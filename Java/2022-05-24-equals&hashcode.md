## equals()

### (1) equals 규약

- 반사성 : `A.equals(A) == true`
- 대칭성 : `A.equals(B) == B.equals(A)`
- 추이성 : `A.equals(B) && B.equals(C), A.equals(C)`
- 일관성 : `A.equals(B) == A.equals(B)`
- null-아님 : `A.equals(null) == false`

### (2) equals() Overriding 방법

```java
public class User {
    private String name;
    private int age;

    @Override
    public boolean equals(Object o) {
				// 1. 자기 자신의 참조인지 확인한다.
        if (this == o) {
            return true;
        }

				// 2. 올바른 타입인지 확인한다. (User Class인지 확인)
        if (!(o instanceof User)) {
            return false;
				}

				// 3. 입력된 값을 올바른 타입으로 형변환 한다.
        User user = (User) o;

				// 4. 입력 객체와 필드가 일치하는지 확인한다.
        return age == user.age && Objects.equals(name, user.name);
    }
}
```

- equals 구현 방법 4단계
    - == 연산자를 사용해 자기 자신의 참조인지 확인한다.
    - instanceof 연산자로 올바른 타입인지 확인한다.
    - 입력된 값을 올바른 타입으로 형변환 한다.
    - 입력 객체와 자기 자신의 대응되는 핵심 필드가 일치하는지 확인한다.

## hashCode()

### (1) hashCode 규약

- equals 비교에 사용하는 정보가 변경되지 않았다면 **hashCode는 매번 같은 값을 리턴**해야 한다. (변경되거나, 애플리케이션을 다시 실행했다면 달라질 수 있다.)
- **equals가 두 객체를 같다고 판단했다면, 두 객체의 hashCode의 값도 같아야 한다.**
- 두 객체에 대한 equals가 다르더라도, **hashCode의 값은 같을 수 있지만** 해시 테이블 성능을 고려해 다른 값을 리턴하는 것이 좋다. (필수 X)
    - 성능이 느려지지만 문제는 없다.

### (2) hashCode 구현

```java
public class User {
    private String name;
    private int age;

    @Override
    public int hashCode() {
        int result = Integer.hashCode(age);
        result = 31 * result + (name == null ? 0 : name.hashCode());

        return result;
    }
}
```

- 핵심 필드 하나의 값을 해쉬값을 계산해서 result 값을 초기화 한다.
- hashCode 계산
    - 기본 타입은 Type(Wrapper class).hashCode
        - `Integer.hashCode(age)`
    - 참조 타입은 해당 필드의 hashCode
        - `name.hashCode()`
    - 배열은 Arrays.hashCode
        - `Arrays.hashCode(list)`
    - `result = 31 * result + 해당 필드의 hashCode` 계산
- result 리턴

> 왜 31 인가?
홀수 선택, 31을 썻을 때 해쉬 충돌이 가장 적었다.
>

## “equals와 hashCode를 같이 재정의 해야 하는 이유”

### 1. equals만 재정의 하는 경우

```java
public class User {
    private String name;
    private int age;

    @Override
    public boolean equals(Object o) {
        if (this == o) {
            return true;
        }

        if (!(o instanceof User)) {
            return false;
				}

        User user = (User) o;

        return age == user.age && Objects.equals(name, user.name);
    }
}
```

### Test

```java
public class HashMapTest {
    public static void main(String[] args) {
        Set<User> set = new HashSet<>();

        User user1 = new User("name", 20);
        User user2 = new User("name", 20);

        // 같은 인스턴스, 다른 hashCode 
        System.out.println(user1.equals(user2)); // true 출력
        System.out.println(user1.hashCode());
        System.out.println(user2.hashCode()); // 다른 hashcode 출력

        set.add(user1);
        set.add(user2);

        System.out.println(set.size()); // 2 출력
    }
}
```

- Collection에 중복되지 않은 User 객체만 넣으라고 요구사항을 받아 Set 자료구조를 사용해야 한다는 상황
- 추가된 두 User 객체의 필드 값이 모두 같지만 각 객체의 hashcode 값이 다르다.

> 즉, hash 값을 사용하는 Collection (HashSet, HashMap)을 사용할 대 문제가 발생 한다.
>

## 2. 발생 이유

- hash 값을 사용하는 Collection (HashMap, HashSet, HashTable)은 객체가 논리적으로 같은지 비교할 때 아래 그림과 같은 과정을 거친다.

![hash](https://tecoble.techcourse.co.kr/static/c248e8d79140c18ed9895d1c95dd7ad0/54e75/2020-07-29-equals-and-hashcode.png)

- hashCode 메서드의 리턴 값을 비교한 후 일치하면 equals 메서드의 리턴 값이 true여야 논리적으로 같은 객체라고 판단한다.
- HashSet에 User 객체를 추가할 때 다른 hashcode에 의해 다른 객체 판단하고 HashSet에 각각 추가되었다.
    - User 객체에 hashCode 메서드가 재정의 되어있지 않아서 Object 클래스의 hashCode 메서드가 사용되었다.

> Object 클래스의 hashCode 메서드는 **객체의 고유한 주소 값**을 int 값으로 변환한다.
>

## 3. 결론 “항상 같이 재정의 해주어야 할까?”

- hash 값을 사용하는 Collection을 사용하지 않는다면, equals와 hashCode를 같이 재정의하지 않아도 된다고 생각할 수 있다.
- 하지만 요구사항은 항상 변할 수 있다.
- 또한 협업 환경이라면 동료는 당연히 equals와 hashCode를 같이 재정의 했을 것이라고 생각하고 hash 값을 사용하는 Collection을 사용할 수 있다.
- **equals와 hashCode를 항상 같이 재정의** 해주는 것이 좋다.

### 1. Intellij의 Generate 사용

```java
public class User {
    private String name;
    private int age;

    @Override
    public boolean equals(Object o) {
        if (this == o) {
            return true;
        }
        if (o == null || getClass() != o.getClass()) {
            return false;
        }
        User user = (User) o;
        return age == user.age && Objects.equals(name, user.name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(name, age);
    }
}
```

- Object.hash 메서드를 호출하는 로직으로 hashCode 메서드가 재정의 되었다.

```java
// Objects.class
public static int hash(Object... values) {
    return Arrays.hashCode(values);
}
```

```java
// Arrays.class
public static int hashCode(Object a[]) {
    if (a == null)
        return 0;

    int result = 1;

    for (Object element : a)
        result = 31 * result + (element == null ? 0 : element.hashCode());

    return result;
}
```

- Objects.hash() 사용 시 hashCode 함수를 단 한 줄로 작성할 수 있다. (장점)
- Objects.hash 메서드 **속도가 느리다.** (단점)
    - 입력 인수를 담기 위한 배열이 만들어진다.
    - 입력중 기본 타입이 있다면 박싱과 언방식도 거쳐야 한다.

> - 성능에 아주 민감하지 않은 대부분의 프로그램은 간편하게 Objects.hash 메서드를 사용해서 hashCode 메서드를 재정의해도 문제 없다.
- 민감한 경우에는 **직접 재정의**해주는 것이 좋다.
>

### 2. Lombock 사용

```java
@EqualsAndHashCode
public class User {
    private String name;
    private int age;
}
```

```java
public class User {
    private String name;
    private int age;

    public User() {
    }

    public boolean equals(Object o) {
        if (o == this) {
            return true;
        } else if (!(o instanceof User)) {
            return false;
        } else {
            User other = (User)o;
            if (!other.canEqual(this)) {
                return false;
            } else if (this.age != other.age) {
                return false;
            } else {
                Object this$name = this.name;
                Object other$name = other.name;
                if (this$name == null) {
                    if (other$name != null) {
                        return false;
                    }
                } else if (!this$name.equals(other$name)) {
                    return false;
                }

                return true;
            }
        }
    }

    protected boolean canEqual(Object other) {
        return other instanceof User;
    }

    public int hashCode() {
        int PRIME = true;
        int result = 1;
        int result = result * 59 + this.age;
        Object $name = this.name;
        result = result * 59 + ($name == null ? 43 : $name.hashCode());
        return result;
    }
}
```

- 장점
    - 필드 추가시 코드 수정이 필요 없다.

필요에 의해 직접 정의 해야 하는 상황이 아니라면 lombok 혹은 Intellij generate를 사용하자.

## 참조

**Java.doc (Class Object)**

https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/Object.html

**Java equals() 및 hashCode()**

https://www.baeldung.com/java-equals-hashcode-contracts

**Guide to hashCode()**

https://www.baeldung.com/java-hashcode